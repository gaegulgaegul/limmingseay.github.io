---
layout: post
title:  "ES8 Async Await"
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

## async/await
- 프로미스 절을 한번 더 깔끔하게 해주는 방법

## 프로미스 문법 간략화
- promise
{% highlight javascript %}
    function findAndSaveUser(Users) {
        Users.findOne({})
            .then((user) => {
                user.name = 'zero';
                return user.save();
            })
            .then((user) => {
                return Users.findOne({gender: 'm'});
            })
            .then((user) => {
                // 생략
            })
            .catch(err => {
                console.error(err);
            });
    }
{% endhighlight %}

- async function 추가
{% highlight javascript %}
    async function findAndSaveUser(Users) {
        let user = await Users.findOne({});
        user.name = 'zero';
        user = await user.save();
        user = await Users.findOne({ gender: 'm' });
        // 생략
    }
{% endhighlight %}

- 에러 처리 추가
{% highlight javascript %}
    async function findAndSaveUser(Users) {
        try {
            let user = await Users.findOne({});
            user.name = 'zero';
            user = await user.save();
            user = await Users.findOne({ gender: 'm' });
            // 생략
        } catch(err) {
            console.error(err);
        }
    }
{% endhighlight %}

- 화살표 함수 사용
{% highlight javascript %}
    const findAndSaveUser = async (Users) => {
        try {
            let user = await Users.findOne({});
            user.name = 'zero';
            user = await user.save();
            user = await Users.findOne({ gender: 'm' });
            // 생략
        } catch(err) {
            console.error(err);
        }
    }
{% endhighlight %}

- for문과 async/await을 사용해 Promise.all 대체
    - es9문법
{% highlight javascript %}
    const promise1 = Promise.resolve('성공1');
    const promise2 = Promise.resolve('성공2');
    (async () => {
        for await (promise of [promise1, promise2]) {
            console.log(promise);
        }
    })();
{% endhighlight %}