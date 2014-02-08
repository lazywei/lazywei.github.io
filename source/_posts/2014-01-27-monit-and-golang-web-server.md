title: Monit and golang web server
tags: ["Linux", "Golang", "Monit"]
date: 2014-01-27 15:33:40
---

I wrote a golang web server, and I want to use monit to monitor it. If you haven't heard monit yet, you can check it out here: [Monit](http://mmonit.com/monit/).

The main problem is that my golang web server is not daemonable. So, the first step is to write a wrapper for monit.


## Wrapper for monit

Suppose we use `go build -o ./my_server` to build our server, then we will have a executable binary at `./my_server`.

In the wrapper, we should echo PID to a file so monit can use the PID to monitor the process.

```bash
#!/bin/bash

case $1 in
  start)
    cd /home/deploy/source/current;
    echo $$ > /home/deploy/source/shared/pids/my_server.pid;
    exec /home/deploy/source/current/my_server > /tmp/my_server.out 2>&1;
    ;;
  stop)
    kill `cat /home/deploy/source/shared/pids/my_server.pid`;;
  *)
  echo "usage: knife-knife-server-daemon {start|stop}" ;;
esac
exit 0
```

The reason I use `/home/deploy/source/current/my_server > /tmp/my_server.out 2>&1` is because I want to redirect the output of `my_server` to `/tmp/my_server.out` so that I can use it to debug.


## Make sure your wrapper work

After creating the wrapper, you should be able to use `your_wrapper.sh start` to run your process. Then press `ctrl` + `z` to pause the program and get back to the shell. Secondly, use `bg` to run it in background. Now you can use `ps aux | grep my_server` and `cat /home/deploy/source/shared/pids/my_server.pid` to check there is nothing weird.


## Monit configuration

This step is relatively simple.

The configuration file should locate at `/etc/monit/conf.d/my_server.monitrc`

```
check process my_server with pidfile /home/deploy/source/shared/pids/my_server.pid
  start = "/home/deploy/source/current/my_wrapper.sh start"
  stop = "/home/deploy/source/current/my_wrapper.sh stop"
```

## Finale

Simply use `monit reload`, `monit start my_server` and `monit summary` to make sure everything is all right.

## Troubleshooting

I spent a lot of time on fix a weird bug. I can use wrapper to run the process. However, when I use `monit start my_server` the process can't be executed. It turns out the problem is I have:

```
bufio.NewScanner(os.Stdin)
```

and the process will waiting the standard input. This will make monit wait for the process until timeout. After removing this line, everything works fine.
