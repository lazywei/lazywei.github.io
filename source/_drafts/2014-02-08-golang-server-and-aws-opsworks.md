title: Golang Server and AWS OpsWorks
tags:
---

- Introduction
- Install golang cookbook
- Pull and build your project
  - set GOPATH
  - Integrate with deployment procedure of OpsWorks, copy then build.
  - notice: pull private repo first
  - template and symlink: write in custom json and default attributes.
- Run server with monit
- Deal with groupcache's HTTPPool
  - A peer manager HTTP endpoint for receiving notification
  - Use private IP to communicate between instances
  - Use curl to notify peer manager
- Add ELB
