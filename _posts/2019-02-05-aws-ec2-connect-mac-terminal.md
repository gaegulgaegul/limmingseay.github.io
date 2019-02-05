---
layout: post
title:  "AWS EC2 Connect Mac Terminal"
image: ''
date:   2019-02-05 14:23:00
tags:
- TIL
- AWS
description: 'test'
categories:
- AWS
---





## Mac Terminal에서 AWS EC2 ssh 접속
- *.pem 파일(Key Pair)을 확인하여 접근한다.

1. .pem 파일 저장소로 이동
{% highlight javascript %}
# cd ~/.ssh
{% endhighlight %}

2. config 수정
{% highlight javascript %}
# vi ./config

Host [접근 호스트명]
    HostName [퍼블릭 DNS(IPv4)]
    User oracle
    IdentityFile [Key Pair Location]
{% endhighlight %}

3. config 권한 설정
{% highlight javascript %}
# chmod 440 ./config
{% endhighlight %}

4. ssh 접속
{% highlight javascript %}
# ssh [접근 호스트명]
{% endhighlight %}


### AWS 접근 기본 계정 설정
- root 계정에서 설정

1. 현재 기본 계정에서 'authorized_keys' 설정할 계정으로 복사
- <code># cp ~/.ssh/authorized_keys [설정할 계정 home]/.ssh/authorized_keys</code>

2. 'authorized_keys' 소유자 및 그룹 권한 설정
- <code># cd /home/[설정할 계정 home]/.ssh</code>
- <code># chown [설정할 계정명] authorized_keys</code>
- <code># chgrp [설정할 그룹명] authorized_keys</code>

3. Mac에서 ssh 접속
- <code># ssh [접근 호스트명]</code>



