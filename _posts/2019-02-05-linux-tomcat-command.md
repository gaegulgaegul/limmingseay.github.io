---
layout: post
title:  "Linux Tomcat Command"
image: ''
date:   2019-02-05 15:00:00
tags:
- Linux
- Tomcat
- TIL
description: ''
categories:
- Linux
---

<br/>
<br/>

## 리눅스 웹 서비스 관련 명령어

- nohup
    - 쉘 스크립트를 데몬 형식으로 실행시키는 명령어
    - 프로세스가 죽지 않는다.
    - nohup.out 파일이 생성됨(용량을 차지하게 된다.)
{% highlight javascript %}
- 기본 실행 명령어
# nohup [실행파일]

- nohup.out 파일 생성 안되게 실행
# nohup [실행파일] 1>/dev/null 2>&1 &
{% endhighlight %}

- &
    - 백그라운드 프로세스로 실행시키는 명령어
{% highlight javascript %}
# nohup [실행파일] &
{% endhighlight %}

- tomcat 실행 스크립트

{% highlight javascript %}
#!/bin/bash
# Startup script for [프로젝트명]
# chkconfig: 345 50 50
# description: [프로젝트명] is a Web application server.
# processname: java
# directory: CATALINA_HOME=/was/[프로젝트명]/bin

export CATALINA_HOME=/was/[프로젝트명]
case "$1" in
start)
cd $CATALINA_HOME/bin
nohup ./catalina.sh start 1>/dev/null 2>&1 &
;;
stop)
cd $CATALINA_HOME/bin
./catalina.sh stop
;;
restart)
cd $CATALINA_HOME/bin
./catalina.sh stop
cd $CATALINA_HOME/bin
nohup ./catalina.sh start 1>/dev/null 2>&1 &
;;
*)
echo "Usage: service [shell script file name] {start|stop|restart}"
exit 1
esac
exit 0
{% endhighlight %}