---
title: open graphic application on romote machine through SSH
date: 2019-07-20 16:48:50
tags: [Linux, SSH]
---
When open a graphic application on remote machine, through a SSH connection,
usually it shows like this:
```
$ gitk
Application initialization failed: no display name and no $DISPLAY environment
variable
Error in startup script: no display name and no $DISPLAY environment variable
    while executing
"load /usr/lib64/tk8.5/../libtk8.5.so Tk"
    ("package ifneeded Tk 8.5.13" script)
    invoked from within
"package require Tk"
    (file "/bin/gitk" line 10)
```
It is because the environment DISPLAY is not set up correctly.

So set it, and then:
```
$ export DISPLAY=172.16.20.152:0.0
$ gitk
Application initialization failed: couldn't connect to display
"172.16.20.152:0.0"
Error in startup script: couldn't connect to display "172.16.20.152:0.0"
    while executing
"load /usr/lib64/tk8.5/../libtk8.5.so Tk"
    ("package ifneeded Tk 8.5.13" script)
    invoked from within
"package require Tk"
    (file "/bin/gitk" line 10)

```

After some research, I find it lack parameter -X when connecting with SSH, so
```
ssh -X adamas@172.16.20.201
```
solves this problem, without DISPLAY variable set manually.
