---
title: "Sunrise: A WordPress Theme"
date: 2011-6-26
layout: post
permalink: /blog/sunrise-a-wordpress-theme
category: blog
tags: wordpress
---

<h2 id="about-sunrise">About Sunrise</h2>

Sunrise is my first attempt at a [WordPress][] theme.

It started from a [design][forrst] I had made about a year ago, which remained unused until today. I really liked the design so I set to work on converting it to a WordPress theme. 


<h2 id="features">Features</h2>

### Full Background Photos ###
The bulk of the development time was taken up by figuring out how to do proper full image backgrounds by pure CSS (along with sticky footers!), and still it isn't perfect in other browsers (see "Limitations" section below), but it will have to do for now. 

### Use "Featured Image" for your photos ###
Using WordPress' built-in "Featured Image" feature, you can easily attach full sized photos to your posts. The Featured Image will automatically be used to fill the background of the post.

### "Show/Hide Info" Toggle ###
A button at the footer allows you to toggle the text overlay for a better view of your photos.

<h2 id="limitations">Limitations</h2>

### Not Perfect ###
Some browsers don't play too well with the CSS trickery I had to do for the full background images. [Opera][], in particular, doesn't preserve the aspect ratio of the photo when the window is resized too small. 

And Internet Explrorer fallbacks to a non-transparent text background.

### No Comments ###
I've decided to not include comments and commenting in the theme. I couldn't find a space to place them and I think pictures don't need much discussion anyway. 

<h2 id="screenshots">Screenshots</h2>


![Homepage]({{site.data.global.static.images}}{{page.url}}/index.png)

![Post with info]({{site.data.global.static.images}}{{page.url}}/single-with-info.png)

![Post with info hidden]({{site.data.global.static.images}}{{page.url}}/single-no-info.png)

![Sample page]({{site.data.global.static.images}}{{page.url}}/page.png)

<h2 id="links">Links</h2>
[Project page][project].

[Download][].

[Source][].

[Demo][].

[Tweet me][] or submit an issue at [Bitbucket][] or [Github][] for comments, suggestions or bugs.


[WordPress]: http://wordpress.com
[forrst]: http://forrst.com/posts/Untitled-Ej
[Opera]: http://opera.com
[Tweet me]: http://twitter.com/john2x
[Bitbucket]: {{site.data.global.links.bitbucket}}/sunrise
[Github]: {{site.github.owner_url}}/sunrise
[Download]: {{site.data.global.links.bitbucket}}/sunrise/get/v1.1.0.zip
[project]: {{site.url}}/projects/sunrise
[Source]: {{site.data.global.links.bitbucket}}/sunrise
[Demo]: {{site.url}}/wordpress

