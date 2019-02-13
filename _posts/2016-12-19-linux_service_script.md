---
layout: post
title:  "Linux 服务脚本编写"
date:   2016-12-19 22:09:21 +0800
category: linux
tags: centos
---

### 常规脚本
```sh
#! /bin/sh

#chkconfig:2345 10 90
#description:msvr-cluster node msgworkerd daemon

PATH=/usr/local/bin:/sbin:/usr/bin:/bin
EXEC=/data/service/msgworkerd
PIDFILE=/var/run/msgworkerd.pid

case "$1" in
start)
	if [ -f $PIDFILE ]
	then
		echo "$PIDFILE exists, process is already running or crashed."
	else
		echo "Starting msgworker ..."
		nohup $EXEC & > /dev/null
	fi
	if [ "$?"="0" ]
	then
		echo "msgworker is running..."
	fi
;;
stop)
	if [ ! -f $PIDFILE ]

. /etc/init.d/functions
#service name
SNAME=msgworkerd
PROG=/data/service/$SNAME
LOCK=/var/lock/subsys/$SNAME

start(){
    if [ -f $LOCK ]; then
        warning "$SNAME is already started!"
    else
        action "Starting $SNAME ..." $PROG 
        [ $? -eq 0 ] && touch $LOCK
    fi
    exit 0
}

stop(){
    echo "Stopping $SNAME ..."
    killproc $SNAME
    rm -rf $LOCK
}

case "$1" in 
"start")
    start
    ;;
"stop")
    stop
    ;;
"status")
    status $SNAME
    ;;
*)
    echo "usage: $0 [start|stop]"
    ;;
esac
```

### Systemctl脚本
路径: `/usr/lib/systemd/system/xxx.service`

样例:
```service
[Unit]
Description=description for service
After=network.target

[Service]
Type=simple
LimitNOFILE=204800
ExecStart=full path to command
ExecStop=/bin/kill -TERM $MAINPID
ExecReload=/bin/kill -HUP $MAINPID
TimeoutStopSec=0

[Install]
WantedBy=multi-user.target
```