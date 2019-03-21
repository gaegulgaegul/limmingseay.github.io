---
layout: post
title:  "Josephus Survivor"
image: ''
date:   2019-03-21 14:56:00
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
In this kata you have to correctly return who is the "survivor", ie: the last element of a Josephus permutation.<br/>

Basically you have to assume that n people are put into a circle and that they are eliminated in steps of k elements, like this:
{% highlight javascript %}
josephus_survivor(7,3) => means 7 people in a circle;
one every 3 is eliminated until one remains
[1,2,3,4,5,6,7] - initial sequence
[1,2,4,5,6,7] => 3 is counted out
[1,2,4,5,7] => 6 is counted out
[1,4,5,7] => 2 is counted out
[1,4,5] => 7 is counted out
[1,4] => 5 is counted out
[4] => 1 counted out, 4 is the last element - the survivor!
{% endhighlight %}
The above link about the "base" kata description will give you a more thorough insight about the origin of this kind of permutation, but basically that's all that there is to know to solve this kata.<br/>

Notes and tips: using the solution to the other kata to check your function may be helpful, but as much larger numbers will be used, using an array/list to compute the number of the survivor may be too slow; you may assume that both n and k will always be >=1.<br/>

## My Solution
{% highlight javascript %}
import java.util.List;
import java.util.ArrayList;

public class JosephusSurvivor {
  public static int josephusSurvivor(final int n, final int k) {
  
    List<Integer> list = new ArrayList<Integer>();
    int rotin = n-1;

    for(int i=0; i<n; i++) {
      list.add(i+1);
    }
    
    for (int i=0; i<n-1; i++){
      list.remove((rotin+k) % (n-i));
      rotin = (rotin+k) % (n-i)-1;
    }
    
    return list.get(0);

  }  
}
{% endhighlight %}

## Sample Tests
{% highlight javascript %}
import static org.junit.Assert.assertEquals;
import org.junit.Test;
import java.util.Random;

public class JosephusSurvivorTest {

  @Test
	public void test1() {
    josephusTest(7, 3, 4);
  }

  @Test
	public void test2() {
    josephusTest(11, 19, 10);
  }

  @Test
	public void test3() {
    josephusTest(40, 3, 28);
  }

  @Test
	public void test4() {
    josephusTest(14, 2, 13);
  }

  @Test
	public void test5() {
    josephusTest(100, 1, 100);
  }
  
  private void josephusTest(final int n, final int k, final int result) {
    String testDescription = String.format("Testing where n = %d and k = %d", n, k);
    assertEquals(testDescription, result, JosephusSurvivor.josephusSurvivor(n, k));
  }
}
{% endhighlight %}

## Other Solution
{% highlight javascript %}
import java.util.Arrays;

public class JosephusSurvivor {
  public static int josephusSurvivor(final int n, final int k) {
    if(n == 1)
      return 1;
    
    return (josephusSurvivor(n - 1, k) + k - 1) % n + 1;
  }  
}

public class JosephusSurvivor {
  public static int josephusSurvivor(final int n, final int k) {
    int remaining = 0;
    for (int i = 2; i <= n; i++) {
      remaining = (remaining + k) % i;
    }
    
    return remaining + 1;
  }  
}

public class JosephusSurvivor {
  static int f(int n,int k){return n==1?0:(f(n-1,k)+k)%n;}  
  public static int josephusSurvivor(final int n, final int k) {
    return f(n,k)+1;
  }  
}
{% endhighlight %}

## 느낀점
문제는 간단해 보였지만 알고리즘을 생각해내는데 오래 걸리는 문제였다.<br/>
다른 개발자들의 풀이를 보면 굉장히 간단하게 풀었는데 아직 많이 공부해야 될 것 같다.<br/>
거의 10일 만에 코드워즈 문제풀이를 하려니 굉장히 머리가 돌지 않는다고 느껴졌다.<br/>
앞으로 1일 1코딩이 되어야하는데 문제다.....