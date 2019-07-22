---
title: How to find a list of jar files that contains a particular class in Linux
date: 2019-07-22 09:33:24
tags: [Java, Jar]
---
```
find . -type f -name '*.jar' -print0 | xargs -0 -I '{}' sh -c 'jar tf {} | grep
com/mypackage/Message.class >/dev/null && echo {}'
```

Steps to find:
1. find all Jar files,
2. then deal with each jar file by
    a. list the contents of every jar file,
    b. filter the class expected,
    c. show the name of the jar file.
