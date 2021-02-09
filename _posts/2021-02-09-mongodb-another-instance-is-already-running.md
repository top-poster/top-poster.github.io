---
layout: post
title: "Shut down MongoDB instances that are already in use"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/mongodb.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/mongodb.png)

Occasionally, the following error can be returned when the MongoDB that was being started is terminated abnormally and then a command is inserted to start MongoDB again:

> Another mongod instance is already running on the 'xxx' directory,

![image](https://heropy.blog/images/screenshot/mongodb-another-instance-is-already-running/mongodb-exception-error.png)

This can be easily resolved by shutting down the port used as the MongoDB instance.
In the `kill` command (with superuser privileges), enter the PID (Process ID) to end the process (TERM signal):

```bash
$ sudo kill -TERM <PID>

```

If you do not know the PID:
With MongoDB default port number (27017) you can search with the `lsof` command:
Specify protocol (`TCP`) and port (`27017`) as `-i` option.

```bash
$ sudo lsof -i TCP:27017

```

You can find the PIDs as follows:

![image](https://heropy.blog/images/screenshot/mongodb-another-instance-is-already-running/mongodb-search-by-port-number.png)

However, if the port number cannot be retrieved (if you do not know the port number, or if you do not).
Search the list with TCP status of `LISTEN` by adding the `-s` option as follows and find the item with COMMAND as `mongod`.

```bash
$ sudo lsof -i TCP -s TCP:LISTEN

```

You can also find MongoDB`s PID as follows:

![image](https://heropy.blog/images/screenshot/mongodb-another-instance-is-already-running/mongodb-search-by-list.png)

If the PID is found, shut down the port as follows:

```bash
$ sudo kill -TERM 12020

```

# References

https://medium.com/@balasubramanim/how-to-resolve-socketexception-address-already-in-use-mongodb-75fa8ea4a2a6
http://man7.org/linux/man-pages/man8/lsof.8.html
https://www.lesstif.com/pages/viewpage.action?pageId=12943674