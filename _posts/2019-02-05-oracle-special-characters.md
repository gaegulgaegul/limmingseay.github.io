---
layout: post
title:  "Oracle Special Characters"
image: ''
date:   2019-02-05 15:00:00
tags:
- Oracle
- TIL
description: ''
categories:
- Oracle
---

<br/>
<br/>

## 오라클 특수문자

## ASCII, CHR
{% highlight javascript %}
    SELECT ASCII('&') FROM DUAL;

    ASCII('&')
-------------------
        38

    SELECT CHR(38) FROM DUAL;
    
    CHR(38)
-------------------
        &
{% endhighlight %}

## 문자 변환 번호
- <code>&</code> <--> 38
- <code>'</code> <--> 39
- <code>"</code> <--> 34
- <code>\\</code> <--> 92
- <code>%</code> <--> 37
- <code>$</code> <--> 36
- <code>#</code> <--> 35
- <code>@</code> <--> 64
- <code>^</code> <--> 94
- <code>*</code> <--> 42
- <code>-</code> <--> 45
- <code>_</code> <--> 95
- <code>+</code> <--> 43
- <code>/</code> <--> 47
- <code>,</code> <--> 44
- <code>.</code> <--> 46
- <code>;</code> <--> 59
- <code>:</code> <--> 58