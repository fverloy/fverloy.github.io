---
layout: post
title:  "Migrating my blog from WordPress to GitHub Pages and Jekyll"
date:   2018-07-08 14:21:43 +0200
categories: Blog, GitHub
permalink: /2018/07/08/migrating-from-wordpress-to-github-pages-and-jekyll/
---
Taking a look a what I was using and paying WordPress.com for I felt it no longer made sense to keep using the service. After looking at some alternatives I decided upon using GitHub Pages to host my blog. It will force me to use a certain toolset (ways around it ofc) and it's free! :-)

You can easily export your old blogs via WordPress.com and you'll get them in XML format, as GitHub Pages with Jekyll uses markdown you need to convert that XML first. There are many tools available, just Google "wordpress xml to markdown" and you'll get a bunch of hits. I tried a couple, inlcuding [exitwp](https://github.com/thomasf/exitwp) but felt too much manual cleanup was needed still simply because some of the constructs are not available in markdown. For example, I found no way to link directly to a YouTube video (with a preview screen) from markfown so that is something I had to manually change. (You can have a screenshot preview linking to the video on YouTube.com itself). 

As I'm using Cloudflare for DNS/CDN/DDoS/... I needed to change the A records to point to GitHub Pages, more info on how to do that can be found [here.](https://help.github.com/articles/setting-up-an-apex-domain/)

Another thing I needed to do manually was provide a permalink for each page so I could maintain the links WordPress created for each blog post, I get a bunch of hits from external places each day so that was paramount to get right. Let me know if something doesn't work. I'm also changing DNS registrar at the same time so that might cause some minor hickups in a few days.

As my editor I'm using [Visual Studio Code](https://code.visualstudio.com/), and I have [GitHub Desktop](https://desktop.github.com/) installed on my MacBook for easy committing, I also installed [Jekyll](https://jekyllrb.com/) locally to preview changes before pushing stuff to GitHub Pages. GitHub Pages supports Jekyll out of the box for blogging so that requires nothing special on their end. 

I'm still debating using another theme, but I'm kinda digging the simplicity and speed of the default one called Minima. Another thing I'll probably add is [pagination](https://jekyllrb.com/docs/pagination/) and maybe see if I can use [disqus](http://sgeos.github.io/jekyll/disqus/2016/02/15/adding-disqus-to-a-jekyll-blog.html) for commenting. I also still need to look at Google indexing/SEO. 

Overall still some work to do, but happy with the migration so far. 

UPDATE 1: Small issue found with [AMP](https://www.ampproject.org/) links. Apparently historical Twitter links automatically point to (now non-existent) AMP versions of my blog, which leads to 404's right now. An AMP plugin doesn't seem to be available with [GitHub Pages and Jekyll](https://help.github.com/articles/configuring-jekyll-plugins/) right now. 
