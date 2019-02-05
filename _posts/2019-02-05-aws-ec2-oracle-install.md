---
layout: post
title:  "AWS EC2 Oracle Install"
image: ''
date:   2019-02-05 15:00:00
tags:
- AWS
- TIL
description: ''
categories:
- AWS
---

<br/>
<br/>

## 오라클 11g XE 설치

<p>1. 사전 필수 패키지 리스트 설치</p>
- binutils
- compat-libcap1
- compat-libstdc++-33
- gcc
- gcc-c++
- glibx
- glibc-devel
- ksh
- libgcc
- libstdc++
- libstdc++-devel
- libaio
- libaio-devel
- make
- sysstat

{% highlight javascript %}
# yum install [package]
{% endhighlight %}

<p>2. 오라클 다운로드</p>
- 오라클 11g XE : <https://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/xe-prior-releases-5172097.html>

{% highlight javascript %}
# wget [파일 다운로드 경로]

# mv [현재 파일명] [변경되는 파일명.zip]
{% endhighlight %}

<p>3. 그룹 권한 설정</p>
{% highlight javascript %}
# groupadd oinstall
# groupadd dba
# useradd -g oinstall -G dba oracle
# passwd oracle
{% endhighlight %}

<p>4. 커널 및 자원 제한 설정</p>
- <p># vi /etc/sysctl.conf 입력 후, 파일 제일 아래 쪽에 입력</p>
{% highlight javascript %}
-- 주석처리
#kernel.shmmax = 68719476736
#kernel.shmall = 4294967296

-- 오라클 권장 값 입력
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
{% endhighlight %}

- 자원 제한 설정
- <p># vi /etc/security/limits.conf 입력 후, 파일 제일 아래 쪽에 입력</p>
{% highlight javascript %}
oracle          soft    nproc   2047
oracle          hard    nproc   16384
oracle          soft    nofile  1024
oracle          hard    nofile  65536
oracle          soft    stack   10240
{% endhighlight %}

- 적용을 위해 reboot
- <p># /sbin/sysctl -p 명령어로 설정 확인</p>

<p>5. 오라클 환경변수 설정</p>
- <p># cd /home/oracle 이동</p>
- <p># vi .bash_profile</p>
{% highlight javascript %}
-- 주석 처리
#PATH=$PATH:$HOME/.local/bin:$HOME/bin
#export PATH

-- 사용자 환경변수 입력
export TMP=/tmp
export TMPDIR=$TMP
export ORACLE_BASE=/u01/app/oracle
export ORACLE_SID=XE
export ORACLE_HOME=$ORACLE_BASE/product/11.2.0/xe
export ORACLE_LISTNER=$ORACLE_HOME/bin/lsnrctl
export PATH=$ORACLE_HOME/bin:$PATH
{% endhighlight %}
- <p># source .bash_profile</p> 입력

6. 오라클 설치
- <p># unzip [오라클 zip 파일명]</p>
- <p># rpm -Uvh ./Disk1/[오라클 rpm 설치 파일명]</p>
{% highlight javascript %}
-- 에러 발생시 조치방안 --
*case1 : error:open of .........rpm failed : NoSuch file or derectory
- 원인 : 해당 파일이 완전히 구성되지 않아서 발생한 문제일 확률이 높다.
- 해결 방안 : 2. 오라클 다운로드 과정을 다시 진행

*case2 : This system does noet meet the minimum requirments for swap space......
- 원인 : swap 공간 부족으로 인한 문제, oracle xe 11에서 필요한 swap공간은 2048MB, 2GB
- 해결 방안 : 2048MB의 swap 공간 정의
# mkdir /swap
# dd if=/dev/zero of=/swap/swapfile bs=1024 count=2097152
# cd /swap
# mkswap swapfile
# swapon swapfile
{% endhighlight %}
- <p># /etc/init.d/oracle-xe configure 입력</p>
- http:8080, oracle:1521, password 설정

7. 오라클 세부 환경 설정
- <p># cd /u01/app/oracle/product/11.2.0/xe/network/admin</p>
- <p># vi listener.ora</p>
{% highlight javascript %}
SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (SID_NAME = PLSExtProc)
      (ORACLE_HOME = /u01/app/oracle/product/11.2.0/xe)
      (PROGRAM = extproc) )
    (SID_DESC =
      (GLOBAL_DBNAME = XE)
      (ORACLE_NAME = /u01/app/oracle/product/11.2.0/xe)
      (SID_NAME = XE)
      (SERVICE_NAME = XE)
    )
  )

LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC_FOR_XE))
      (ADDRESS = (PROTOCOL = TCP)(HOST = ip-172-31-17-216)(PORT = 1521))
    )
  )
{% endhighlight %}

- <p># vi tnsnames.ora</p>
{% highlight javascript %}XE =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = ip-172-31-17-216)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = XE)
    )
  )

EXTPROC_CONNECTION_DATA =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC_FOR_XE))
    )
    (CONNECT_DATA =
      (SID = PLSExtProc)
      (PRESENTATION = RO)
    )
  )
  {% endhighlight %}

  8. 오라클 서비스 실행
- <p># su oracle</p>
- <p>$ source .bash_profile</p>
- <p>$ sqlplus / as sysdba</p>