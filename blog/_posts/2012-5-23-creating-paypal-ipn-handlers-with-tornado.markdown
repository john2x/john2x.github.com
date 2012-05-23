Creating PayPal IPN Handlers with Tornado
=========================================

What is IPN?
------------

Instant Payment Notification (IPN) is PayPal's message service that sends a 
notification when a transaction is affected.

Basically, once a PayPal transaction happens, it sends a notification to a
handler you specify, with all the information and variables you need to handle
that that event in your application.

Check out PayPal's official [docs on IPN][] for more information. 

[docs on IPN]: https://www.paypal.com/ipn

Preparing the PayPal buttons
----------------------------

Before we write our handler, we need a way to send the IPNs. The easiest way
to do this is to use one of PayPal's Payments Standard Buttons. For this
article, we'll be using the "Subscribe" button. 

Follow the [instructions here][] to create your button.

[instructions here]: https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&content_ID=developer/e_howto_html_subscribe_buttons#id08ADF1008HS

Writing the handler
-------------------

Once we have the button in a page in our app, we can begin writing our IPN
handler. 

I'll be using the [Tornado][] web framework for this example, but it should be
very similar for other frameworks. Also, I'm deploying on [Google App Engine][],
so there might be some App Engine specific code. 

First, determine the URL your handler will use. For example,
`http://example.com/ipn`. You'll need to modify your PayPal button so it knows
where to send IPN messages. Just add

    notify_url=http://example.com/ipn

to the "Advanced variables" textbox at the bottom of the modify button page.
Don't forget to update the HTML in your app's subscribe page as well.

![Advanced variables]({{site.repo}}images{{page.url}}/advanced_variables.png)

Once that's set up, we can begin writing the code for our IPN handler. I'll
just show the entire handler below and explain the parts.

	import logging
	import os.path
	import urllib
	import urllib2
	import wsgiref.handlers
	
	import tornado.web
	import tornado.escape
	import tornado.wsgi
	
	RECEIVER_ID = 'xxxxxxxx'
	RECEIVER_EMAIL = 'name@example.com'
	
    class IPNHandler(tornado.web.RequestHandler):
        def verify_ipn(self, data, sandbox=''):
            # prepares provided data set to inform PayPal we wish to validate the response
            data["cmd"] = "_notify-validate"
            params = urllib.urlencode(data)
    
            if sandbox:
    			# sends the data and request to the PayPal Sandbox
                paypal_url = 'https://www.sandbox.paypal.com/cgi-bin/webscr'
            else:
                paypal_url = 'https://www.paypal.com/cgi-bin/webscr'
    
            req = urllib2.Request(paypal_url, params)
            req.add_header("Content-type", "application/x-www-form-urlencoded")
            # reads the response back from PayPal
            response = urllib2.urlopen(req)
            status = response.read()
    
            # If not verified
            if not status == "VERIFIED":
                return False
    
            # if not the correct receiver ID
            if not sandbox and data['txn_type'] == 'subscr_payment'\
            and not data["receiver_id"] == RECEIVER_ID:
                logging.info('Incorrect receiver_id')
                logging.info(data['receiver_id'])
                return False
    
            # if not the correct receiver email
            if not sandbox and data['txn_type'] != 'subscr_payment'\
            and not data["receiver_email"] == RECEIVER_EMAIL:
                logging.info('Incorrect receiver_email')
                logging.info(data['receiver_email'])
                return False
    
            # if not the correct currency
            if not sandbox and not data.get("mc_currency") == "USD":
                logging.info('Incorrect mc_currency')
                return False
    
            # otherwise...
            return True
    
        def subscr_signup(self, data):
    		# handle a 'Signup' IPN message
    		# you can create a User object, for example,
            # or set a user's plan
    		pass
    
        def subscr_payment(self, data):
    		# handle a 'Payment' IPN message
    		# this message gets sent when you receive a recurring payment
            # you can re-set your user's plan here
    		pass
    
        def subscr_modify(self, data):
    		# handle a 'Modify' IPN message
    		# the Subscribe button has an option to allow users to modify
            # their subscription plan
            # you can upgrade your user's plan here
    		pass
    
        def subscr_eot(self, data):
    		# handle a 'End of Transaction' IPN message
    		# at the end of the subscription period, this message gets sent
            # you can disable a user here
    		pass
    
        def subscr_cancel(self, data):
    		# handle a 'Cancel' IPN message
    		# when a user cancels his subscription (either in his PayPal page or
            # in your website), this message gets sent
            # you can disable a user here
    		pass
    
        def subscr_failed(self, data):
    		# handle a 'Failed' IPN message
    		# sometimes something goes wrong while the IPN is being sent
            # you can log the error here
    		pass
    
        def post(self, sandbox=''):
            data = {}
    
			# the values in request.arguments are stored as single value lists
			# we need to extract their string values
            for arg in self.request.arguments:
                data[arg] = self.request.arguments[arg][0]
    
            # If there is no txn_id in the received arguments don't proceed
            if data['txn_type'] == 'subscr_payment'\
            and not 'txn_id' in data:
                logging.info('IPN: No Parameters')
                return
    
            # Verify the data received with Paypal
            if not self.request.headers['User-Agent'] == 'python-requests/0.11.2'\
            and not self.verify_ipn(data, sandbox):
                logging.info('IPN: Unable to verify')
                return
    
            logging.info('IPN: Verified!')
    
            # Now do something with the IPN data
            if data['txn_type'] == 'subscr_signup':
                # initial subscription
                self.subscr_signup(data)
            elif data['txn_type'] == 'subscr_payment':
                # subscription renewed
                self.subscr_payment(data)
            elif data['txn_type'] == 'subscr_modify':
                # subscription plan modified
                self.subscr_modify(data)
            elif data['txn_type'] == 'subscr_eot':
                # subscription expired
                self.subscr_eot(data)
            elif data['txn_type'] == 'subscr_cancel':
                # subscription canceled
                self.subscr_cancel(data)
            elif data['txn_type'] == 'subscr_failed':
                # subscription failed
                self.subscr_failed(data)
    
            return
    
    settings = {
        'template_path': os.path.join(os.path.dirname(__file__), 'templates'),
        'autoescape': None,
        'debug': os.environ.get('SERVER_SOFTWARE', '').startswith('Development/'),
    }
    app = tornado.wsgi.WSGIApplication([
        (r'/ipn', IPNHandler),
        (r'/ipn/(sandbox)', IPNHandler),
    ], **settings)
    
    def main():
        wsgiref.handlers.CGIHandler().run(app)
    
    if __name__ == '__main__':
        main()
    
The "Subscribe" button sends 6 types of transactions (subscr_signup,
subscr_payment, subscr_modify, subscr_eot, subscr_cancel, subscr_failed).
Each of those transaction types has its own handler. I've placed comments in
each of them as guides on what you can do once you receive a specific
transaction type. 

When the handler receives an IPN (which is a POST request from PayPal), it
stores the request's arguments in the `data` dict. 

We first verify the `data` by passing it to the `verify_ipn` method. What it
does is resend the exact same arguments to PayPal in order to verify it. Once
we receive a "VERIFIED" message (plus various other arguments checks), it is
safe to proceed.

We then send `data` to the specific transaction type handler to make use of.

For a list of all the arguments included in `data` and the transaction types
for other buttons, refer to PayPal's [docs on IPN variables][].

[Tornado]: http://tornadoweb.org
[Google App Engine]: https://developers.google.com/appengine/
[docs on IPN variables]: https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&content_ID=developer/e_howto_html_IPNandPDTVariables

Testing
-------

### Using the Sandbox Tool

PayPal provides a Sandbox tool which simulates sending IPN messages to your
IPN handler. Visit [http://sandbox.paypal.com][https://www.sandbox.paypal.com/]
to get started. You'll need to sign up for a separate Sandbox account, then go
to "Test Tools" > "Instant Payment Notification (IPN) simulator". Fill in the
URL (use `/ipn/sandbox` since the Sandbox uses a different URL) 
to your IPN handler, select the transaction type you want, then click 
submit. 

Unfortunately, the IPN simulator doesn't have the option to send subscription
IPNs. I resorted to testing it "live", by actually clicking on the subscribe
button and going through a full cycle. This can be a pain, since you'll have
to wait for a full cycle to test the other transaction types (payment, eot).

### Testing on localhost

You can also test you handler on localhost, but we'll have to skip the
`verify_ipn()` part, and jump right to handling the transaction type, just
assuming the IPN was verified.

Check out my [IPN-tester][] project. It's a script which sends a POST to
`http://localhost:8080/ipn` with arguments taken from logs from real IPNs. It
only has data for `subscr_signup` and `subsr_cancel` for now, but I'll add
other types soon (it's open source, so anyone can add :P).

It's the reason why the code above has

    if not self.request.headers['User-Agent'] == 'python-requests/0.11.2'

since the script uses the [Requests][] library. So if the request came from
Requests, we skip calling `verify_ipn()`.

[IPN-tester]: https://bitbucket.org/john2x/ipn-tester
[Requests]: http://docs.python-requests.org/en/latest/index.html


