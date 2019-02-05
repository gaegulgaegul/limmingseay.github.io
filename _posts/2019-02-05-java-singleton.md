---
layout: post
title:  "Java Singleton"
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

## 싱글턴 패턴
- 인스턴스를 하나만 만들어 사용하기 위한 패턴
- 커넥션 풀, 스레드 풀, 디바이스 설정 객체 등의 경우, 인스턴스를 여러개 만들게 되면 자원을 낭비라게 되거나 버그를 발생시킬 수
  있으므로 오직 하나만 생성하고 그 인스턴스를 사용하도록 한다.

## 싱글턴 패턴 구현

- version1
    - 게으른 인스턴스 생성(lazy instantiation)
    - 멀티스레드 상황에서 인스턴스가 두개 이상 만들어질 가능성이 있다.
{% highlight javascript %}
    public class Singleton {

        private static Singleton uniqueInstance;

        private Singleton() {}

        public static Singleton getInstance() {

            if (uniqueInstance == null) {
                uniqueInstance = new Singleton();
            }

            return uniqueInstance;

        }

    }
{% endhighlight %}

- version2
    - getInstance()를 동기화 하면 멀티스레딩 관련 문제가 해결된다.
    - getInstance()의 속도가 느려진다.(메소드를 동기화하면 성능이 100배 정도 저하)
{% highlight javascript %}
    public class Singleton {

        private static Singleton uniqueInstance;

        private Singleton() {}

        public static synchronized Singleton getInstance() {

            if (uniqueInstance == null) {
                uniqueInstance = new Singleton();
            }

            return uniqueInstance;

        }

    }
{% endhighlight %}

- version3
    - 인스턴스를 처음부터 만든다.
{% highlight javascript %}
    public class Singleton {

        private static Singleton uniqueInstance = new Singleton();

        private Singleton() {}

        public static synchronized Singleton getInstance() {

            return uniqueInstance;

        }

    }
{% endhighlight %}

- version4
    - DCL(Double-Checking Locking)을 사용한다.
    - 멀티코어 환경에서 동작할 때, 하나의 cpu를 제외하고는 다른 cpu가 lock이 걸리게 된다.
{% highlight javascript %}
    public class Singleton {

        private volatile static Singleton uniqueInstance;

        private Singleton() {}

        public static Singleton getInstance() {

            if (uniqueInstance == null) {

                synchronized (Singleton.class) {

                    if(uniqueInstance == null) {
                        uniqueInstance = new Singleton();
                    }

                }

            }

            return uniqueInstance;

        }

    }
{% endhighlight %}

- version5
    - Eager initialization
    - 프로그램이 실행되고 나서 처음부터 끝까지 객체가 메모리에 있게 된다.
{% highlight javascript %}
    public class Singleton {

        private volatile static Singleton uniqueInstance = new Singleton();

        private Singleton() {}

        public static Singleton getInstance() {

            return uniqueInstance;

        }

    }
{% endhighlight %}

- version6
    - 중첩 클래스를 이용한 Holder를 사용하는 방법
{% highlight javascript %}
    public class Singleton {

        private Singleton() {}

        private static class SingletonHolder {

            public static final Singleton INSTANCE = new Singleton();

        }

        public static Singleton getInstance() {

            return SingletonHolder.INSTANCE;

        }

    }
{% endhighlight %}

## volatile
- 멀티 스레딩 환경에서 동기화를 해주는 키워드, 컴파일러가 특정 변수에 대해 옵티마이져가 캐싱을 적용하지 못하도록 하는 키워드
- 멀티 스레드에서 for/while 문이 옵티마이져에 의해 캐싱을 사용하는데, 이 때 동기화 문제가 발생할 수 있다. 한 스레드에서 다른
  스레드의 작업이 마치기를 기다린다고 가정했을 때, 최신의 변수를 읽어오지 못한다면 무한 루프에 빠질 수 있다. 이러한 문제 발생
  상황을 volatile 키워드를 사용하면 모든 스레드에 대해 항상 최신의 값을 읽을 수 있게 해준다.
- syncronized는 작업 자체를 원자화해버리고 volatile은 특정 변수에 대해 최신 값을 제공한다.

## 참고
<http://asfirstalways.tistory.com/335>, <http://jusungpark.tistory.com/16>