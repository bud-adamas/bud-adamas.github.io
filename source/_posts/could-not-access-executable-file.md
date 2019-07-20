---
title: how to deal with error "Could not access executable file" (mainly for developer)
date: 2019-07-19 13:06:46
tags: [EsgynDB, Trafodion]
---
After a lot of code has been modified, like merging two branches, we usually
meet the problem like this in sqlci:
```
*** ERROR[2012] Server process tdm_arkesp could not be created on kylin-44.esgyn.cn
 - Operating system error 4022, TPCError = 53, error detail = 0. Could not access executable file.
```
and we can find nothing in the log files.

---

To diagnose this problem, we can replace that executable binary with a bash
script, and within that script, we can check the linkage and then invoke that
executable binary, as the following:

1. change the binary name, like tdm_arkesp, to tdm_arkesp.bak, with its executable mode unchanged,
2. set up a script, name tdm_arkesp to replace the origin one, under the same directory, with its content like:
```
    #!/bin/bash

    ldd /.../tdm_arkesp.bak |& tee /tmp/tdm_arkesp.ldd
    /.../tdm_arkesp.bak $* |& tee /tmp/tdm_arkesp.out
```
3. check the file /tmp/tdm_arkesp.{ldd,out}, to find the reason,
4. revert the files, by removing the script and renaming the binary back to the origin one, after the problem is fixed.

***

Usualy, the content of /tmp/tdm_arkesp.out looks like this:
```
tdm_arkesp: symbol lookup error:
/opt/trafodion/esgynDB_server-2.5.2/export/lib64/liboptimizer.so: undefined
symbol: _ZN7CmpMain10pExpFuncs_E
```
we can then check the function CmpMain::ExpFuncs(), which dynamic library that
function resides, and make sure liboptimizer.so depends on that library in Makefiles.
