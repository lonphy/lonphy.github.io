---
layout: post
title:  "CentOS Process command"
date:   2018-03-23 22:47:47 +0800
category: linux
tags: centos
---

```sh
# 获取指定名称进程ID
pidof name

# 获取进程IO信息
pidstat -d -p pid f

#获取进程CPU信息
pidstat -u -p pid f

#获取进程内存信息
pidstat -r -p pid f

#查看进程打开文件数
lsof -p PID | wc -l
```