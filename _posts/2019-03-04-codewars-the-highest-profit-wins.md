---
layout: post
title:  "The highest profit wins!"
image: ''
date:   2019-03-04 00:16:00
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
Story<br/>

Ben has a very simple idea to make some profit: he buys something and sells it again. Of course, this wouldn't give him any profit at all if he was simply to buy and sell it at the same price. Instead, he's going to buy it for the lowest possible price and sell it at the highest.<br/>

Task<br/>

Write a function that returns both the minimum and maximum number of the given list/array.<br/>

Examples<br/>

{% highlight javascript %}
MinMax.minMax(new int[]{1,2,3,4,5}) == {1,5}
MinMax.minMax(new int[]{2334454,5}) == {5, 2334454}
MinMax.minMax(new int[]{1}) == {1, 1}
{% endhighlight %}

Remarks<br/>

All arrays or lists will always have at least one element, so you don't need to check the length. Also, your function will always get an array or a list, you don't have to check for null, undefined or similar.<br/>

## My Solution
{% highlight javascript %}
import java.util.Arrays;

class MinMax {
  public static int[] minMax(int[] arr) {
    Arrays.sort(arr);
    return new int[]{arr[0], arr[arr.length-1]};
  }
}
{% endhighlight %}

## Sample Tests
{% highlight javascript %}
import org.junit.Test;
import org.junit.Before;
import java.util.Random;
import static org.junit.Assert.assertArrayEquals;

public class MinMaxTest {

  Random rand;
    
  @Before
  public void initTest() {
    rand = new Random();
  }
    
  @Test
  public void testExampleCases() {
    assertArrayEquals(new int[]{1,5}, MinMax.minMax(new int[]{1,2,3,4,5}));
    assertArrayEquals(new int[]{5, 2334454}, MinMax.minMax(new int[]{2334454,5}));
    assertArrayEquals(new int[]{1, 1}, MinMax.minMax(new int[]{1}));
  }

  @Test
  public void minMaxRandomTest() {
    for(int i = 0; i < 20; i++) {
      int r = rand.nextInt();
      assertArrayEquals(new int[]{r, r}, MinMax.minMax(new int[]{r}));
    }
  }    
}
{% endhighlight %}

## Other Solution
{% highlight javascript %}
import java.util.Arrays;
import java.util.Collections;



class MinMax {
    public static int[] minMax(int[] arr) {
        // Your awesome code here
           

    int min = Integer.MAX_VALUE; 
    int max = Integer.MIN_VALUE; 
    for(int i : arr) {         
        if(i < min) min = i;      
        if(i > max) max = i;     
    }
    return new int[] {min, max};
}
}

import java.util.*;
class MinMax {
    public static int[] minMax(int[] arr) {
       return new int[]{Arrays.stream(arr).min().getAsInt(), Arrays.stream(arr).max().getAsInt()};
    }
}

class MinMax {
    public static int[] minMax(int[] arr) {
        int min = arr[0];
        int max = min;
        for (int i : arr) {
           min = Math.min(i, min);
           max = Math.max(i, max);
        }
        return new int[] { min, max };
    }
}
{% endhighlight %}

## 느낀점
이번 문제는 정렬에 대한 함수를 알지 못했으면 코드가 엄청 길어졌을 문제인 것 같다.<br/>
어려운 문제가 아니여서 다행이였다.