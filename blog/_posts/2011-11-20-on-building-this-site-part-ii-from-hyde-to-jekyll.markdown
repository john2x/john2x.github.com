---
title: "On Building This Site: Part II"
layout: post
category: blog
---

<h2 id="design">Why?</h2>

I was bored the past couple of days so I decided to redesign my site. And while I was at it, I thought about giving [Jekyll][] a try. I thought it would be cool to just be able to push to Github any site updates. I already had a nice little [fabfile][] to publish changes on Hyde, but that was getting old. 

Also, I was getting tired of the design. But I still tried to keep the same
look and feel of reading a book. I have a soft spot for minimalism (okay, maybe
a little more than a soft spot). Or maybe I just suck at design and this is the best I can do. I like to think it's the former. 

<h2 id="the-setup">From Hyde to Jekyll</h2>
### What's different

Well, the most notable difference is Jekyll is written with Ruby, although I don't think the language has anything to do with it. In fact, I didn't even need to touch Ruby while converting (as far as I can tell). Unlike Hyde where I had to define a `settings.py` file, but I don't think that counts either. 

Jekyll had much better documentation, but still pretty sparse. It had a lot more example sites though, so I had more references which made it pretty easy to pick up. 

### Markdown. Yey!

Jekyll supports several markup syntaxes, including Markdown. I'm finding it hard to not write everything in Markdown nowadays, despite its [shortcomings][]. I only had to make a few changes to the markup of my posts, due to different custom tags of the rendering engine. 

Markdown support wasn't perfect, though. It ran into some problems parsing Vim key bindings, mistaking them for HTML tags. Changing the default Markdown engine to `rdiscount` fixed it, although I still get errors when generating the pages locally. But it seems to work fine on Github. 

### What's missing

Some links aren't working due to redirecting to Github Pages. I'm not sure yet how to get around that (yet). But at least there's still some pretty pictures to look at.

[Jekyll]: http://jekyllrb.com/
[fabfile]: http://fabfile.org
[shortcomings]: 

