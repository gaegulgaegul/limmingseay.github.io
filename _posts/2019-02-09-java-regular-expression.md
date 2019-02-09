---
layout: post
title:  "Regular Expression"
image: ''
date:   2019-02-09 16:40:00
tags:
- Java
- TIL
description: ''
categories:
- Java
---

<br/>
<br/>

## 정규표현식에 대해
Web Project를 진행하면 Client에게서 어떠한 정보가 입력될지 모르기에, 이 정보를 정규화하는 과정이 필요합니다. 이를 유효성 검사라고 하는데 유효성검사를 제대로 하지않으면, 데이터의 신뢰도가 떨어지게 되고, 예기치 않은 에러를 발생시키기 때문입니다. 이를 위해서는 각 언어마다 정규표현식을 제공해주고 있으며, 크게 다르지 않습니다.<br/>

## 문법
<code>^</code> : 문자열 시작
<code>$</code> : 문자열 끝
<code>.</code> : 임의의 한 문자(문자의 종류를 가리지 않습니다.) 단, '', \는 넣을 수 없습니다.
<code>*</code> : 앞 문자가 0개 이상의 개수가 존재할 수 있습니다.
<code>+</code> : 앞 문자가 1개 이상의 개수가 존재할 수 있습니다.
<code>?</code> : 앞 문자가 없거나 하나 있을 수 있습니다.
<code>[]</code> : 문자의 집합이나 범위를 표현합니다. <code>-</code>기호를 통해 범위를 나타낼 수 있습니다. <code>^</code>가 존재하면 Not을 나타냅니다.
<code>{}</code> : 횟수 또는 범위를 나타냅니다.
<code>()</code> : 괄호안의 문자를 하나의 단어로 인식합니다.
<code>|</code> : 패턴을 OR연산을 수행할 때 사용합니다.
<code>\s</code> : 공백 문자
<code>\S</code> : 공백 문자가 아닌 나머지 문자
<code>\w</code> : 알파벳이나 문자
<code>\W</code> : 알파벳이나 숫자를 제외한 문자
<code>\d</code> : 숫자([0-9]와 같은 의미)
<code>\D</code> : 숫자를 제외한 모든 문자
<code>(?i)</code> : 대소문자를 구분하지 않습니다.

## 예제
{% highlight javascript %}
숫자만 : ^[0-9]*$

영문자만 : ^[a-zA-Z]*$

한글만 : ^[가-힣]*$

영어 & 숫자만 : ^[a-zA-Z0-9]*$

E-Mail : ^[a-zA-Z0-9]+@[a-zA-Z0-9]+\.[a-zA-Z0-9]|(\.[a-zA-Z0-9])+$

휴대폰 : ^01(?:0|1|[6-9])-(?:\d{3}|\d{4})-\d{4}$

일반전화 : ^\d{2,3}-\d{3,4}-\d{4}$

주민등록번호 : \d{6}\-[1-4]\d{6}

IP 주소 : ([0-9]{1,3})\.([0-9]{1,3})\.([0-9]{1,3})\.([0-9]{1,3})

영문 대소문자, 언더바(_), 숫자 : [a-zA-Z_0-9]

영문 대소문자, 언더바(_), 숫자  이외 : [^\w]
{% endhighlight %}

## 참고
<a href="https://blog.outsider.ne.kr/360">알고 있어야 할 8가지 정규식 표현</a>
<a href="https://m.blog.naver.com/PostView.nhn?blogId=javaking75&logNo=220491298440&categoryNo=7&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F">자바 정규표현식 사용법과 관련 API</a>