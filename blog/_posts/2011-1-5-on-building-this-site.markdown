---
title: On Building This Site
layout: post
category: blog
---

<h2 id="design">Design</h2>

This website underwent a *lot* of changes during the designing stage, but I never deviated from the overall feel of what I wanted. 

I wanted it to read and feel like a book. I don't know if I've achieved that or even come close to it, but I'm satisfied. 


<h2 id="the-setup">The Setup</h2>
### Choosing a framework
I had a hard time deciding on which engine or framework to use. I was contemplating between [Wordpress][], [Tumblr][], [a Django blogging engine](https://github.com/agiliq/django-blogango), [Hyde][hyde] and [Posterous][]. 
In the end, I went with Hyde. At first, it seemed limited compared to the others. But after a lot of thought, I realized that it had everything I needed, and nothing I didn't need. 
So I went ahead and downloaded Hyde, checked it's docs and looked for tutorials. There wasn't much, and I had some trouble getting up to speed, but I managed. 

I'm also using [BlueprintCSS][] as my CSS framework, to make working with CSS *less* painful. 

[Wordpress]: http://wordpress.org/
[Tumblr]: http://tumblr.com
[Posterous]: http://posterous.com
[BlueprintCSS]: http://blueprintcss.org
### Essentials
When working on *any* Python project, I suggest using [virtualenv](http://virtualenv.openplans.org/). It makes managing libraries much easier. And while you're at it, go ahead and get [virtualenvwrapper](http://www.doughellmann.com/projects/virtualenvwrapper/) as well. Or, if you insist on not using virtualenv, at least have [pip](http://pypi.python.org/pypi/pip) installed. *(This post won't go into details on how to install and use other libraries. Their respective docs should be enough for setting them up though. )*

<h2 id="turning-into-hyde">Turning into Hyde</h2>

Like I've said, [Hyde][hyde] didn't come with a lot of tutorials. The docs weren't very detailed either. This post will serve as a reminder for me, and maybe as a guide for others.

### Installing Hyde
Installing Hyde wasn't as smooth as I hoped it would be. First, get the source by cloning it from [Github](https://github.com/lakshmivyas/hyde), or, if you don't know git, download the [zip](https://github.com/lakshmivyas/hyde/zipball/master) or [tarball](https://github.com/lakshmivyas/hyde/tarball/master). 
Install Hyde using 
{% highlight sh %}$ python path/to/hyde/setup.py install{% endhighlight %} 
Afterwards, install it's requirements found in the `requirements.txt` file. Install them using pip by 
{% highlight sh %}$ pip install -r path/to/hyde/requirements.txt{% endhighlight  %}
If all goes well, Hyde should be installed in your virtual environment (or system). Test it by creating a new Hyde project with the command 
{% highlight sh %}$ hyde -i -s path/to/your/site{% endhighlight %}

On my first attempt, Hyde gave me an error complaining about not finding templates. It turned out that somehow setup.py failed to copy the templates folder into my virtual environment. I don't know if this was just a one time thing, but if the same thing happens to you simply copy the templates folder (`hyde/templates/`) into your site-packages. 

With Hyde installed and your project initialized, it's time to get working. 

<h2 id="getting-to-know-hyde">Getting to Know Hyde</h2>

### Interlude
Before I continue, I'd like to point your attention to this [blog post][stevelosh]. That post (and the website's [source][sjl]) served as my primary guide when working on this site. So go ahead and check it out when you can, it contains much more substance and covers a lot more ground. 

### What is Hyde?
Hyde is a static site generator. Basically, you write your site's content using the tools supported by Hyde (Markdown, Django's templating system, etc.) and let Hyde generate your final site with proper markup. You then host the generated static content, which are just plain .html pages. Static pages have the advantage of being lightweight and fast when you don't need dyncamic capabilities.

### Listing Pages
Hyde uses "listing" pages for archives and similar features. Listing pages are basically pages which inherit `layout/skeleton/_listing.html` in the default template. They are placed in folders, called "nodes" in Hyde, and they are automatically populated with a list of all the the other pages in the node (e.g. blog posts in a blog folder). Very nifty. 

### Markdown
My favorite feature of Hyde has got to be the built-in support for [Markdown](http://daringfireball.net/projects/markdown/). It makes writing articles a breeze. The [sytax docs](http://daringfireball.net/projects/markdown/syntax) in the Markdown homepage should be enough to get you started. Just install Markdown with pip 
{% highlight sh %}
$ pip install markdown
{% endhighlight %}
and you are good to go. 

### Clean URL's
To enable clean URL's on your site, just set `GENERATE_CLEAN_URLS` to `True` in `settings.py`. You need to run your site on a server though for clean URL's to work, but Hyde has that covered as well. 
Hyde uses [CherryPy][] as a testing server, so go ahead and install that as well. 
{% highlight sh %}
$ pip install cherrypy
{% endhighlight %}
Then run the server via
{% highlight sh %}
$ hyde -g -w -s path/to/your/site -k
{% endhighlight %}
and point your browser to `http://localhost:8080/` to see your site running on CherryPy. Just make sure the paths in your `settings.py` are correct. 
(`-g` means to generate the site, `-w` runs the CherryPy development server, `-s path/to/your/site` is your site source and `-k` tells Hyde to regenerate everything. )

### Other Features
Hyde has a lot more features but I am not using them on this site, so I won't be discussing them. For that, Hyde's [wiki][hydewiki] should help to get started. And the [post][stevelosh] I referenced above is a very helpful resource as well. 

<h2 id="deploying">Deploying</h2>

I originally thought that deploying a Hyde website would need a server with Python installed. How wrong I was. 
Just host the `/deploy` directory created by `hyde -g` and that's it. 

<h2 id="missing-parts">Missing Parts</h2>

### Comments
Right now I haven't enabled any comments on the blog yet. I'll try using [Disqus](http://disqus.com) for that. 
### Categories
Hyde has support for [categories](https://github.com/lakshmivyas/hyde/wiki/Site-Preprocessors) but I don't really need it yet as I only have one blog post and I don't really write all that much. But it's definitely something I'll add in the future. 

<h2 id="in-closing">In Closing</h2>

I hope this post helps you in getting started with [Hyde][hyde]. It's a great project and I've learned a lot from it. Thanks for reading. 

[hyde]:http://ringce.com/hyde
[Markdown]:http://daringfireball.net/projects/markdown/
[markdownsyntax]:http://daringfireball.net/projects/markdown/syntax
[stevelosh]:http://stevelosh.com/blog/2010/01/moving-from-django-to-hyde/
[sjl]:https://github.com/sjl/stevelosh
[CherryPy]:http://www.cherrypy.org/
[hydewiki]:http://wiki.github.com/lakshmivyas/hyde

