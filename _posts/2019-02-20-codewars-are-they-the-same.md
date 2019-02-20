---
layout: post
title:  "Are they the same?"
image: ''
date:   2019-02-20 23:27:00
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
Given two arrays a and b write a function comp(a, b) (compSame(a, b) in Clojure) that checks whether the two arrays have the "same" elements, with the same multiplicities. "Same" means, here, that the elements in b are the elements in a squared, regardless of the order.
<br/>
Examples
<br/>
Valid arrays
<br/>
{% highlight javascript %}
a = [121, 144, 19, 161, 19, 144, 19, 11]  
b = [121, 14641, 20736, 361, 25921, 361, 20736, 361]
{% endhighlight %}
<br/>
comp(a, b) returns true because in b 121 is the square of 11, 14641 is the square of 121, 20736 the square of 144, 361 the square of 19, 25921 the square of 161, and so on. It gets obvious if we write b's elements in terms of squares:
{% highlight javascript %}
a = [121, 144, 19, 161, 19, 144, 19, 11] 
b = [11*11, 121*121, 144*144, 19*19, 161*161, 19*19, 144*144, 19*19]
{% endhighlight %}
<br/>
Invalid arrays
<br/>
If we change the first number to something else, comp may not return true anymore:
{% highlight javascript %}
a = [121, 144, 19, 161, 19, 144, 19, 11]  
b = [132, 14641, 20736, 361, 25921, 361, 20736, 361]
{% endhighlight %}
comp(a,b) returns false because in b 132 is not the square of any number of a.
<br/>
{% highlight javascript %}
a = [121, 144, 19, 161, 19, 144, 19, 11]  
b = [121, 14641, 20736, 36100, 25921, 361, 20736, 361]
{% endhighlight %}
comp(a,b) returns false because in b 36100 is not the square of any number of a.
<br/>
Remarks
a or b might be [] (all languages except R, Shell). a or b might be nil or null or None or nothing (except in Haskell, Elixir, C++, Rust, R, Shell, PureScript).
<br/>
If a or b are nil (or null or None), the problem doesn't make sense so return false.
<br/>
If a or b are empty then the result is self-evident.



## My Solution
{% highlight javascript %}
import java.util.Arrays;
import java.lang.Math;

public class AreSame {
	
	public static boolean comp(int[] a, int[] b) {
    
    if(a==null || b==null) return false;
    if(a.length!=b.length) return false;
    
    for(int j=0; j<a.length; j++) {
      if(a[j]<0) {
        a[j] = Math.abs(a[j]);
      }
    }
    
    Arrays.sort(a);
    Arrays.sort(b);
    
    for(int i=0; i<a.length; i++) {
      if((a[i]*a[i]) != b[i]) return false;
    }
    
    return true;
  }
{% endhighlight %}

## Sample Tests
{% highlight javascript %}
import static org.junit.Assert.*;
import org.junit.Test;

public class AreSameTest {

	@Test
	public void test1() {
		int[] a = new int[]{121, 144, 19, 161, 19, 144, 19, 11};
		int[] b = new int[]{121, 14641, 20736, 361, 25921, 361, 20736, 361};
		assertEquals(true, AreSame.comp(a, b)); 
	}

}
{% endhighlight %}

## Other Solution
{% highlight javascript %}
import java.util.Arrays;

public class AreSame {
  public static boolean comp(final int[] a, final int[] b) {
    return a != null && b != null && a.length == b.length && Arrays.equals(Arrays.stream(a).map(i -> i * i).sorted().toArray(), Arrays.stream(b).sorted().toArray());
  }
}

import java.util.*;
import java.io.*;
import java.util.Arrays;

public class AreSame {
  
  public static boolean comp(int[] a, int[] b) {
    if ((a == null) || (b == null)){
          return false;
    }
    int[] aa = Arrays.stream(a).map(n -> n * n).toArray();
    Arrays.sort(aa);
    Arrays.sort(b);
    return (Arrays.equals(aa, b));
    
  }
}

import java.util.HashMap;
public class AreSame {
  
  public static boolean comp(int[] a, int[] b) {
    if (a == null && b == null) return true;
    if (a == null || b == null || a.length != b.length) return false;
    HashMap<Integer, Integer> counter = new HashMap<Integer, Integer>();
    for (int n2 : b) {
      Integer times = counter.get(n2);
      counter.put(n2, times == null ? 1 : times + 1);
    }
    for (int n : a) {
      int n2 = n * n;
      Integer times = counter.get(n2);
      if (times == null) return false;
      if (times == 1) counter.remove(n2);
      else counter.put(n2, times - 1);
    }
    return counter.size() == 0;
  }
}
{% endhighlight %}

## 느낀점
이번 문제는 보기에 간단해보였지만 막상 문제를 풀어보려하니 생각해줘야할 것이 많았던 것 같았고<br/>
결국 내 힘으로 풀지 못하고 구글신의 도움을 받았다............<br/>
java8에서 사용하는 람다식에 대해 자세히 볼 필요가 있다고 느껴진다.<br/>