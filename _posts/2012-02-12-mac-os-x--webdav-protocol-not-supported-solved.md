---
layout: post
title: Mac OS X&#58; WebDAV protocol not supported (solved)
category: digital
tags: os_x
related: ["Create Jira issues from Jenkins;/2013/03/24/create-jira-issues-from-jenkins", "PMD analysis for Objective-C with Jenkins and OCLint;/2013/03/10/jenkins-pmd-analysis-for-objective-c-with-oclint"]
---

When you see in your console log messages like

```
11.02.12 17:36:41,009 webdavfs_agent: network_mount: WebDAV protocol not supported; file: /SourceCache/webdavfs/webdavfs-322/	mount.tproj/webdav_network.c; line: 3131
```

this doesn't necessarily mean, that Mac OS X suddenly stopped understanding the WebDAV server it could mount minutes ago. In fact it's more likely, that the problem lies on the server side and the DAV config is incomplete.

Since the Mac OS web DAV client isn't really chatty about what's going (wr)on(g) here, you should use a unix WebDAV client (e.g. [cadaver](http://www.webdav.org/cadaver/)) for debugging.

The client can properly connect to the DAV server which you can see by the result code ("200") in the apache log file:
	
```"OPTIONS / HTTP/1.1" 200 250 "-" "cadaver/0.23.3 neon/0.29.0"```

But the client tells you, that there's something wrong anyhow, which you can verify again in the apache log:

```"PROPFIND / HTTP/1.1" 405 508 "-" "cadaver/0.23.3 neon/0.29.0"```

The result code "405" means "Method not allowed". Does the client try to talk to the DAV server via incorrect command and Mac OS therefore reports the "not supported" message? No, it's just that the DAV server doesn't know how to handle the DAV protocol properly.

So go and check your server config. When I upgraded Plesk from version 10.2 to 10.4, the upgrade process didn't catch all the relevant vhost.conf files and therefore left me with a half configured DAV server. The problem is, that Plesk 10.4 uses a different directory layout under ```/var/www/vhosts/``` and I had to manually copy and configure the ```vhost.conf``` files to their new places.