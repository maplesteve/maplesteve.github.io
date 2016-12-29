---
layout: post
title: maplesteve.com now powered by Jekyll
category: digital
tags: tech jekyll
---

I write blog post now for more than five years. One thing that I observe - like any other person on the net who blogs - is that I blog less often than I want to.

Why is that? I assume it's when the 'process' is cumbersome, you'll blog less. Especially when it comes to write short posts about this and that. (Amateur) blogging should be easy and not a multi-step login-create-publish-admin job.

I tried different engines in the past; from hand-made HTML, via Wordpress to [secondcrack](https://github.com/maplesteve/secondcrack) lately. Now I reached the next stage: [Jekyll](http://jekyllrb.com).

Jekyll brings everything I want:

* *understands Markdown files:*
[Markdown](http://daringfireball.net/projects/markdown/) makes WYSYWIG editors obsolete. All you need is a simple text editor and you can write blog posts. In my setup it's the really great [MarkdownPro](http://www.markdownpro.com).

* *generates static pages:*
In the not too distant past, *dynamic* web sites where state of the art. Today we know, that (at least) for blogs you don't need all the stuff that PHP gives you. And for the rest, there is still JavaScript. Static web sites do their job without putting any pressure on the system. You don't have to fear memory limits, CPU load or the like.

* *processes text files:*
I don't want to use a browser to write a blog post. I want to write a text file and save it to a synced directory. Since I run an [owncloud](http://owncloud.org) instance for various other stuff, it was really easy to create a new directory, let owncloud do the syncing and tell Jekyll to take this as its source directory. BTW: images etc. are handled exactly the same way. Can't be more simple.

* *automatically updates the site on changes:*
When I'm finished with writing a post, I want it to be live within seconds. I don't want to ```push``` a repository. I don't want to login somewhere just to hit the publish button. Too many steps. Jekyll has a ```--watch``` command which is all you need, to have it constantly watch for changes of anything in the source directory and it'll start to (re-)generate your site.

* *provides a simple template system:*
If you want a unique layout for your Wordpress blog, you'll face a steep learning curve. It's no surprise, that there's a whole industry selling *professional* templates for WP. Even with secondcrack - which is definitely more simple than the most other systems - I found myself investing way too much into developing scripts. Jekyll bases it's output on [Liquid](http://docs.shopify.com/themes/liquid-basics) tags and though I didn't know anything about this only some days ago, I quickly felt comfortable and produced results in little time.

Jekyll is so simple and quick. Setup is done literally in minutes. Just type ```gem install jekyll``` into your terminal and you're basically done. No database setup, no user management. Now make one or two changes in the ```_config.yml``` file, point your web-servers document root to the output directory (or vice versa), save a markdown file in the ```_posts``` directory and you're live.

If my theory works, you'll see some more posts on this site than in the past. So stay tuned.
