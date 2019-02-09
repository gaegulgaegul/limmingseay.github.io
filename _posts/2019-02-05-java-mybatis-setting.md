---
layout: post
title:  "Java Mybatis Setting"
image: ''
date:   2019-02-05 15:00:00
tags:
- Java
- TIL
description: ''
categories:
- Java
---

<br/>
<br/>

## Map으로 받은 결과값의 순서가 유지가 되지 않은 경우
- resultType을 Map -> LinkedHashMap으로 변경한다.
{% highlight javascript %}
    &lt;select id="selectList" resultType="java.util.Map"&gt;

    &lt;select id="selectList" resultType="java.util.LinkedHashMap"&gt;
{% endhighlight %}

## Map으로 받은 결과값이 value가 null일 때, key가 누락되는 경우
- 리턴 value 값이 Null 이라도 칼럼 값이 누락이 안되는 세팅값을 제공
- mybatis-config.xml 설정
{% highlight javascript %}
    &lt;settings&gt;
        &lt;setting name="callSettersOnNulls" value="true"/&gt;
    &lt;/settings&gt;
{% endhighlight %}