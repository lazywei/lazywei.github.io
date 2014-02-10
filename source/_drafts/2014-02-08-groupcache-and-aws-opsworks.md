title: GroupCache and AWS OpsWorks
tags: ["groupcache", "opsworks", "golang"]
---

I'm working on a project which uses golang and groupcache to process images on the fly. After finishing the basic structure, we decide to put this project on-line to validate the performance. By "put it on-line", I mean put it on AWS.

So the next task is to build a custom OpsWorks cookbooks to deploy this project. In my last post [Monit and golang web server](http://blog.lazywei.com/2014/01/27/monit-and-golang-web-server/) I described how to 
