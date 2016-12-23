---
layout: post
title: Install HeaderDoc on Ubuntu
category: digital
tags: howto dev
---

HeaderDoc is a very versatile document generation system by Apple, which can handle a wide variety of languages. The latter was the reason for me to give it a chance, since other generator either work only with a specific language or produce (for me) unusable results.
HeaderDoc works fine with PHP, JavaScript etc. which makes it a perfect tool for web projects - otherwise you'll have to deal with different tools, which can't produce combined docs.

HeaderDocs comes with Mac OS X (at least when you have Xcode installed). But I wanted to use it on my Ubuntu server where Jenkins does all the integration stuff.

Apple has open-sourced HeaderDoc and you can find it on Apple's open source website at <http://www.opensource.apple.com/>

####Downlaod & unpack
- The most recent version is in the Mac OS X 10.7.3 tree; copy this link and download the archive into a directory on your Linux machine: <http://www.opensource.apple.com/tarballs/headerdoc/headerdoc-8.8.38.tar.gz>
 - ```wget http://www.opensource.apple.com/tarballs/headerdoc/headerdoc-8.8.38.tar.gz```

- unpack with ```tar -xvzf headerdoc-8.8.38.tar.gz``` and ```cd``` into the created directory

####Requirements
HeaderDocs is basically a PERL script, so you'll need a recent PERL installation. This should be the case on every 'normal' system, so I won't dive into installing PERL here. Besides that, HeaderDoc needs some other libraries. If they're not installed, run the following commands:

- FreezeThaw
 - ```sudo apt-get install libfreezethaw-perl```

- libxml2-dev
 - ```sudo apt-get install libxml2-dev```

- xmllint (from the libxml2-utils)
 - ```sudo apt-get install libxml2-utils```

- [checkinstall](http://wiki.debian.org/CheckInstall) (not necessary for this installation, but you should alway use checkinstall, when manually installing software, which circumvents the Ubuntu/Debian package system!)
 - ```sudo apt-get install checkinstall```

####Build & install
- ```make clean```
- ```make``` - This will actually compile the software & libraries and performs a lot of tests. Three of them ('class 3', 'header 5', 'template 1') failed during my install, but I didn't notice any false behaviour using HeaderDoc.
- ```sudo checkinstall make realinstall```

Ready. You should have now two files in ```/usr/bin/```:

- headerdoc2html - the processor itself
- gatherheaderdoc - a utility to combine the docs in an overview

You'll find the documentaion for HeaderDoc on [Apple's developer site](https://developer.apple.com/library/mac/#documentation/DeveloperTools/Conceptual/HeaderDoc/)