---
layout: post
title:  "CentOS TCP 优化"
date:   2019-02-11 01:44:04 +0800
category: linux
tags: tcp
---

#### TCP:
**/etc/sysctl.conf**

```conf
net.ipv4.tcp_syncookies = 1                 # 开启SYN Cookies, 当出现SYN等待队列溢出时
                                            # 启用cookies来处理，可防范少量SYN攻击, 默认为0，关闭
net.ipv4.tcp_tw_reuse = 1                   # 开启重用, 允许将TIME-WAIT sockets重新用于新的TCP连接，默认为0, 关闭
net.ipv4.tcp_tw_recycle = 1                 # 开启TCP连接中TIME-WAIT sockets的快速回收，默认为0，关闭
net.ipv4.tcp_fin_timeout= 20                # 修改系統默认TIMEOUT 时间

net.ipv4.tcp_keepalive_time=1200            # 当keepalive起用的时候，TCP发送keepalive消息的频度。缺省是2小时
net.ipv4.tcp_keepalive_probes=3             # 健康检查放弃阀值， 默认9
net.ipv4.tcp_keepalive_intvl = 30           # 健康检查时间间隔， 默认75秒

net.ipv4.ip_local_port_range=10000 65500    # 用于向外连接的端口范围，默认32768到61000
net.ipv4.tcp_max_syn_backlog=8192           # SYN队列的长度, 默认1024， 容纳待连接的网络连接数
net.ipv4.tcp_max_tw_buckets = 5000          # 系统同时保持TIME_WAIT的最大数量，默认180000
net.core.netdev_max_backlog=1024            # 流入包的最大设备队列，默认300
net.core.somaxconn=1024                     # listen()的默认参数，挂起请求的最大数量， 默认128
net.ipv4.tcp_retries2=5                     # 失败重传次数， 默认15
net.ipv4.ip_conntrack_max=200000            # 跟踪TCP的最大数量, 这个修改注意，容易造成数据被丢弃或者内核崩溃
```

`#sysctl -p`

#### 文件打开数限制
**/etc/security/limits.conf**
```sh
# 查看最大打开文件数
ulimit -n
# 查看指定端口TCP状态：
netstat -nat | grep -i "12321" | wc -l
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'

#查看进程的限制参数：
cat /proc/`pidof cbiz`/limits
```

```conf
* soft nofile 204800
* hard nofile 204800
```