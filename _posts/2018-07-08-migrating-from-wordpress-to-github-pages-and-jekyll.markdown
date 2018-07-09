---
layout: post
title:  "Migrating my blog from WordPress to GitHub Pages and Jekyll"
date:   2018-07-08 14:21:43 +0200
categories: Blog, GitHub
permalink: /2018/07/08/migrating-from-wordpress-to-github-pages-and-jekyll/
---
Taking a look a what I was using and paying WordPress.com for I felt it no longer made sense to keep using the service. After looking at some alternatives I decided upon using GitHub Pages to host my blog. It will force me to use a certain toolset (ways around it ofc) and it's free! :-)

You can easily export your old blogs via WordPress.com and you'll get them in XML format, as GitHub Pages with Jekyll uses Markdown you need to convert that XML first. There are many tools available, just Google "wordpress xml to markdown" and you'll get a bunch of hits. I tried a couple of things but felt too much manual cleanup was needed still. For example, I found no way to link directly to a YouTube video (with preview screen) from markfown so that is something I had to manually change. (You can have a screenshot preview linking to the video on YouTube.com). 

As I'm using Cloudflare for DNS/CDN/DDoS/... I needed to change the A records to point to GitHub Pages, more info on how to do that can be found [here.](https://help.github.com/articles/setting-up-an-apex-domain/)

Another thing I needed to do manually was provide a permalink for each page so I could maintain the links WordPress created for each blog post, I get a bunch of hits each day from people linking so that was paramount to get right. Let me know if something doesn't work. I'm also chaning DNS registrar at the same time so that might cause some minor hickups in a few days.

As my editor I'm using [Visual Studio Code](https://code.visualstudio.com/), and I have [GitHub Desktop](https://desktop.github.com/) installed on my MacBook, I also installed [Jekyll](https://jekyllrb.com/) locally to preview changes before pushing stuff to GitHub Pages. GitHub Pages support Jekyll out of the box for blogging. 

I'm still debating to use another theme, but I'm kinda digging the simplicity and speed of the default minima one. Another thing I'll probably add is [pagination](https://jekyllrb.com/docs/pagination/) and maybe see if I can use [disqus](http://sgeos.github.io/jekyll/disqus/2016/02/15/adding-disqus-to-a-jekyll-blog.html) for commenting. I also still need to look at Google indexing/SEO. 

Overall still some work to do, but happy with the migration so far. 