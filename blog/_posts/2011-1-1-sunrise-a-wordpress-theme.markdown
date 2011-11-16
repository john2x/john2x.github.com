---
title: "Sunrise: A WordPress Theme"
layout: post
category: blog
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
![Homepage]({{site.url}}/media/images/projects/sunrise/index.png)

![Post with info]({{site.url}}/media/images/projects/sunrise/single-with-info.png)

![Post with info hidden]({{site.url}}/media/images/projects/sunrise/single-no-info.png)

![Sample page]({{site.url}}/media/images/projects/sunrise/page.png)

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
[Bitbucket]: {{links.bitbucket}}/sunrise
[Github]: {{links.github}}/sunrise
[Download]: {{links.bitbucket}}/sunrise/get/v1.1.0.zip
[project]: {{site.url}}/projects/sunrise
[Source]: {{links.bitbucket}}/sunrise
[Demo]: {{site.url}}/wordpress

