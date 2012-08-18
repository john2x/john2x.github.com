---
title: Learning Vimscript
layout: post
category: blog
---

I wrote my first [Vim plugin][x-marks-the-spot] a few days ago. Although the usefulness of said plugin is up for debate, I'm pretty glad I made it. 

I’ve learned a good deal about the quirky language which is Vimscript, and a great deal more about my favorite editor. 

If you are unfamiliar with Vimscript or with Vim plugin development in general, and would like to get started quickly, then I recommend you check out [Learn Vimscript the Hard Way][lvsthw]. It’s a great resource and should get you up and running in no time. 

Another indispensable resource was Vim's very own `:help` section. Although it's kind of difficult to find what you want if you don't know what you're looking for in the first place. For such occasions I head over to `#vim` on IRC. The folks there are really helpful (if you ask nicely). 

Here are some of the things which caused quite a bit of head scratching and aren't very well documented. 

<h2 id="==">The result of `==` depends on the user's settings</h2>

This really didn't catch me by surprise, since it was mentioned in [Chapter 22][lvsthw-22] of LVSTHW, but in case you didn't go through it (which I strongly suggest), I'll reiterate it here. 

Apparently, when using the equality (`==`) operator on strings, the result will depend on Vim's `ignorecase` setting. If it's set, then `==` will be case-insensitive. Or if `noignorecase` is set, it will be case-sensitive. 

Fortunately there are ways to force case-sensitive/insensitive comparisons. Just append `#` to the operator to force case-sensitive comparison and `?` otherwise. 

e.g.: 

{% highlight vimscript %}
"foo" ==# "foo" " true
"foo" ==# "FOO" " false
"foo" ==? "FOO" " true
{% endhighlight %}

Note that it works for the other comparison operators too, like `<#`, `>?`, `!=#`, `!=?`, etc. 

<h2 id="dynamic-type-casting">Dynamic type casting &emdash; Not!</h2>

This one was more of a hair-puller than a head-scratcher when I encountered it. 

Dynamic type casting is mentioned (but not yet revisited) in [Chapter 19][lvsthw-19] of LVSTHW, demonstrating that the following is legal:
 
{% highlight vimscript %}
let foo = "bar"
echo foo
let foo = 5
echo foo
{% endhighlight %}

From this you would probably assume that Vimscript has no problem casting any type to another (I definitely did, missing the brief statement about it from Chapter 19), but apparently it only works with strings to numbers and vice-versa. 

The following is not legal, for example:

{% highlight vimscript %}
let list = [1, 2, 3]
let list = "foo"

let dict = {"a": 1, "b": 2, "c": 3}
let dict = [3, 2, 1]
{% endhighlight %}

If you want to reuse a variable as another type, you can `unlet` the variable and declare it again. 

<h2 id="true-or-false">True or false?</h2>

Vim doesn't have a boolean datatype, instead it uses numbers to check for truthiness. Basically, `0` is false, and everything else is true, including negative numbers. 

Remember, it uses numbers, so a non-empty string is not true. To check if a string is true (e.g. check if it has a value), use the `len()` function to get its length, now you have a number which Vim can evaluate as either true or false. 

[x-marks-the-spot]: https://github.com/john2x/x-marks-the-spot.vim
[lvsthw]: http://learnvimscriptthehardway.stevelosh.com
[lvsthw-22]: http://learnvimscriptthehardway.stevelosh.com/chapters/19.html
[lvsthw-19]: http://learnvimscriptthehardway.stevelosh.com/chapters/22.html
