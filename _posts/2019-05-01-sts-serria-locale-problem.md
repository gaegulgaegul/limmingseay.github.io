---
layout: post
title:  "MAC 시에라 업데이트 시 Locale 문제"
image: ''
date:   2019-05-01 11:17:00
tags:
- sts
description: ''
categories:
- sts
---

<br/>
<br/>

{% highlight javascript %}
{% endhighlight %}

Mac에서 OS를 시에라로 업데이트 후 로컬에서 개발하려고 할때 갑자기<br/>

"로케일을 인식할 수 없습니다" 라는 오라클 접속 불가 메세지를 리턴했다.<br/>

구글에 검색해보니 문제는 바로 시에라에서 지역을 인식을 하지 못 해서 그랬었다.<br/>

Java application을 구동할 때 VM Option에<br/>

-Duser.language=ko -Duser.country=KR<br/>

을 추가하면 정상 작동하는 것을 볼 수 있다.<br/>