---
layout: post
title:  "CentOS Systemd"
date:   2017-11-10 15:35:12 +0800
category: linux
tags: centos
---

## centos 7 之 systemctl
```sh
# 基本操作
systemctl [start|stop|status|restart|reload] xxxx.service

# 开机启用/禁用服务
systemctl [enable|disable] xxxx.service
systemctl load xxx.service

#重新读取服务脚本
systemctl daemon-reload

# 远程执行命令
systemctl -H user@hostname

# 防火墙(centos 7)
systemctl stop firewalld.service  # 关闭
systemctl disable firewalld.service # 禁用
```

### *.service配置文件
> Type=[simple, forking, oneshot, dbus, notify, idle]

进程启动类型
如果设为"simple"(设置了 ExecStart= 但未设置 BusName= 时的默认值)，那么表示 ExecStart= 所设定的进程就是该服务的主进程。