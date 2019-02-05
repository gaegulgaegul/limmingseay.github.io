---
layout: post
title:  "ES6 Destructuring"
image: ''
date:   2019-02-05 15:00:00
tags:
- Javascript
- TIL
description: ''
categories:
- Javascript
---

<br/>
<br/>

## 비구조화 할당
- 객체와 배열로부터 속성이나 요소를 쉽게 꺼낼 수 있다.

## 객체 비구조화 할당 예제
{% highlight javascript %}
    var candyMachine = {
        status: {
            name: 'node',
            count: 5
        },
        getCandy: function() {
            this.status.count--;
            return this.status.count;
        }
    };
    var getCandy = candyMachine.getCandy();
    var count = candyMachine.status.count;

    // destructuring
    const candyMachine = {
        status: {
            name: 'node',
            count: 5
        },
        getCandy: function() {
            this.status.count--;
            return this.status.count;
        }
    };
    const { getCandy, statuc : { count } } = candyMachine;
{% endhighlight %}

## 배열 비구조화 할당 예제
{% highlight javascript %}
    var array = ['node.js', {}, 10, true];
    var node = array[0];
    var obj = array[1];
    var bool = array[array.length - 1];

    // destructuring
    const array = ['node.js', {}, 10, true];
    const [node2, obj2, bool2] = array;
{% endhighlight %}