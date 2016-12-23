---
layout: post
title: How to solve slow rrdtool graph creation
category: digital
tags: howto os_x cacti
---

After upgrading my cacti server to Mac OS X Lion, the graphs in my Cacti installation needed a very long time to render and the CPU usage of rrdtool spiked – somehow blocking the whole machine.

The rrdtool which is needed by cacti has been installed via Fink, which is a fine project maintaining all the usual \*nix tools, that don’t come with Mac OS X. Fink gives you the convenience of a Debian package manager and makes installing additional software a breeze.

Because I didn’t know exactly which program was responsible for the slow rendering, I dumped the whole fink tree which is normally located at ```/sw``` and started over with a fresh install. After fink completed all the compiling an installation I had a fast cacti again. Since I didn’t update the fink packages regulary, I was sure, that some bugfixes in the newer versions resolved the slowness problem.

After the recent update of Lion to 10.7.2, I had to realize, that the rendering was totally slow again. So I thought, that an update of rrdtool and the other needed tools should do the trick again. Unfortunately there was no newer version of the tools as those which where already installed.

After trying some things (which didn’t help), I looked for a way to circumvent the dump-everything-and-spend-another-two-hours-reinstalling thing. Since there was no newer version for 10.7.2, there must be a different cause.

I figured out, that one of the following tools had to be the offending one:

* librrd4-shlibs
* rrdtool
* pango1-xft2-ft219 (dev, shlibs & the main one)
* freetype219 (main & shlibs)
* fontconfig2

So find their package name and remove them (example for ‘freetype’):

![](/media/remove_package.png)

When you delete or remove a package via fink and then reinstall it again, fink makes use of the binary packages it produced during the former installation. (I tried the command-line switches to avoid this behaviour but had no luck.)

What helped, was to delete the packages from the cache directories (on my system this is at ```/sw/fink/10.7/stable/main/binary-darwin-x86_64/```) and starting the fink install command again.

I think it’s safe to delete them all:

![](/media/NewImage.png)


fink grabs the source and – since the binary package is no longer available – compiles everything from scratch.

Install the packages. It’s best to start with the main package (the one you were initially interested in), because the package management of fink will install additional ones automatically when needed:

![](/media/NewImage1.png)


After that, the rrdtool graph rendering was fast again. So my conclusion is, that even during minor updates of Mac OS X, underlying things can change that way, that programs don’t work as expected, if they were compiled on a different version of the system.

So, if your tools behave different after an update or upgrade, try a fresh compile of them first.
