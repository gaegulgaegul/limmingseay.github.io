---
layout: post
title:  "Tribonacci Sequence"
image: ''
date:   2019-03-10 11:55:00
tags:
- codewars
description: ''
categories:
- codewars
---

<br/>
<br/>

{% highlight javascript %}
{% endhighlight %}

## 문제 설명
Well met with Fibonacci bigger brother, AKA Tribonacci.<br/>

As the name may already reveal, it works basically like a Fibonacci, but summing the last 3 (instead of 2) numbers of the sequence to generate the next. And, worse part of it, regrettably I won't get to hear non-native Italian speakers trying to pronounce it :(<br/>

So, if we are to start our Tribonacci sequence with [1, 1, 1] as a starting input (AKA signature), we have this sequence:
{% highlight javascript %}
[1, 1 ,1, 3, 5, 9, 17, 31, ...]
{% endhighlight %}

But what if we started with [0, 0, 1] as a signature? As starting with [0, 1] instead of [1, 1] basically shifts the common Fibonacci sequence by once place, you may be tempted to think that we would get the same sequence shifted by 2 places, but that is not the case and we would get:
{% highlight javascript %}
[0, 0, 1, 1, 2, 4, 7, 13, 24, ...]
{% endhighlight %}
Well, you may have guessed it by now, but to be clear: you need to create a fibonacci function that given a signature array/list, returns the first n elements - signature included of the so seeded sequence.<br/>

Signature will always contain 3 numbers; n will always be a non-negative number; if n == 0, then return an empty array (except in C return NULL) and be ready for anything else which is not clearly specified ;)<br/>

If you enjoyed this kata more advanced and generalized version of it can be found in the Xbonacci kata<br/>

*[Personal thanks to Professor Jim Fowler on Coursera for his awesome classes that I really recommend to any math enthusiast and for showing me this mathematical curiosity too with his usual contagious passion :)]*<br/>

## My Solution
{% highlight javascript %}
public class Xbonacci {

  public double[] tribonacci(double[] s, int n) {
    double[] result = new double[n];
    
    if(n <= 3) {
    
      for(int a=0; a<n; a++) {
        result[a] = s[a];
      }
       
    } else {
    
      for(int i=0; i<s.length; i++) {
        result[i] = s[i];
      }
      
      for(int j=s.length; j<n; j++) {
        result[j] = result[j-3] + result[j-2] + result[j-1];
      }
       
    }
     
    return result;
  }
}
{% endhighlight %}

## Sample Tests
{% highlight javascript %}
import org.junit.Test;
import org.junit.Before;
import org.junit.After;
import static org.junit.Assert.*;

public class XbonacciTest {
  private Xbonacci variabonacci;
  
  @Before
  public void setUp() throws Exception {
    variabonacci = new Xbonacci();
  }

  @After
  public void tearDown() throws Exception {
    variabonacci = null;
  }
  
  private double precision = 1e-10;
  
  @Test
  public void basicTests() {
    assertArrayEquals(new double []{1,1,1,3,5,9,17,31,57,105}, variabonacci.tribonacci(new double []{1,1,1},10), precision);
    assertArrayEquals(new double []{0,0,1,1,2,4,7,13,24,44}, variabonacci.tribonacci(new double []{0,0,1},10), precision);
    assertArrayEquals(new double []{0,1,1,2,4,7,13,24,44,81}, variabonacci.tribonacci(new double []{0,1,1},10), precision);
  }
}
{% endhighlight %}

## Other Solution
{% highlight javascript %}
import java.util.Arrays;

public class Xbonacci {
  public double[] tribonacci(double[] s, int n) {

      double[] tritab=Arrays.copyOf(s, n);
      for(int i=3;i<n;i++){
        tritab[i]=tritab[i-1]+tritab[i-2]+tritab[i-3];
      }
      return tritab;

    }
}

public class Xbonacci {

  public double[] tribonacci(double[] s, int n) {
      double[] r = new double[n];
      for(int i = 0; i < n; i++){
        r[i] = (i<3)?s[i]:r[i-3]+r[i-2]+r[i-1];
      }
      return r;
  }
}

public class Xbonacci {

  public double[] tribonacci(double[] s, int n) {
      double[] r = new double[n];
      for(int i = 0; i < n && i < 3; i++) r[i] = s[i];
      for(int i = 3; i < n; i++)
        r[i] = r[i - 1] + r[i - 2] + r[i - 3];
      return r;
  }
}

import java.util.stream.Stream;

public class Xbonacci {

  public static double[] tribonacci(double[] s, int n) {
     return Stream.iterate(s, p -> new double[]{p[1], p[2], p[0] + p[1] + p[2]}).limit(n).mapToDouble(p -> p[0]).toArray();
  }

}
{% endhighlight %}

## 느낀점
문제는 간단하지만 3이하의 수를 처리하는 부분에서 애를 많이 먹었다<br/>
그래도 금방 문제를 푼 것 같고 알고리즘에 대한 공부가 필요할 것 같다.