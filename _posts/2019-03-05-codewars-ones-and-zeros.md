---
layout: post
title:  "Ones and Zeros"
image: ''
date:   2019-03-05 20:34:00
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
Given an array of one's and zero's convert the equivalent binary value to an integer.<br/>

Eg: [0, 0, 0, 1] is treated as 0001 which is the binary representation of 1.<br/>

Examples:<br/>

{% highlight javascript %}
Testing: [0, 0, 0, 1] ==> 1
Testing: [0, 0, 1, 0] ==> 2
Testing: [0, 1, 0, 1] ==> 5
Testing: [1, 0, 0, 1] ==> 9
Testing: [0, 0, 1, 0] ==> 2
Testing: [0, 1, 1, 0] ==> 6
Testing: [1, 1, 1, 1] ==> 15
Testing: [1, 0, 1, 1] ==> 11
{% endhighlight %}

However, the arrays can have varying lengths, not just limited to 4.<br/>

## My Solution
{% highlight javascript %}
import java.util.List;
import java.lang.Math;

public class BinaryArrayToNumber {

  public static int ConvertBinaryArrayToInt(List<Integer> binary) {
    int total = 0;
    
    for(int i=0; i<binary.size(); i++) {
      total += Math.pow(2, i) * binary.get((binary.size()-1)-i);
    }
    
    return total;
  }
}
{% endhighlight %}

## Sample Tests
{% highlight javascript %}
import org.junit.Test;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

import static org.junit.Assert.*;

public class BinaryArrayToNumberTest {
  @org.junit.Test
  public void convertBinaryArrayToInt() throws Exception {

    assertEquals(1, BinaryArrayToNumber.ConvertBinaryArrayToInt(new ArrayList<>(Arrays.asList(0,0,0,1))));
    assertEquals(15, BinaryArrayToNumber.ConvertBinaryArrayToInt(new ArrayList<>(Arrays.asList(1,1,1,1))));
    assertEquals(6, BinaryArrayToNumber.ConvertBinaryArrayToInt(new ArrayList<>(Arrays.asList(0,1,1,0))));
    assertEquals(9, BinaryArrayToNumber.ConvertBinaryArrayToInt(new ArrayList<>(Arrays.asList(1,0,0,1))));
  }
}
{% endhighlight %}

## Other Solution
{% highlight javascript %}
import java.util.List;

public class BinaryArrayToNumber {

    public static int ConvertBinaryArrayToInt(List<Integer> binary) {
        return binary.stream().reduce((x, y) -> x * 2 + y).get();
    }
}

import java.util.List;

public class BinaryArrayToNumber {

    public static int ConvertBinaryArrayToInt(List<Integer> binary) {
        
        int number = 0;
        for (int bit : binary)
            number = number << 1 | bit;
        return number;
    }
}

import java.util.List;

public class BinaryArrayToNumber {

    public static int ConvertBinaryArrayToInt(List<Integer> binary) {
        StringBuilder numS = new StringBuilder();
        
        for(int num : binary)
          numS.append(num);
        
        return Integer.parseInt((numS.toString()),2);
    }
}
{% endhighlight %}

## 느낀점
간단한 2진수를 10진수로 바꿔주는 알고리즘이였다.<br/>
역시 람다식으로 변환하면 간단하게 작성할 수 있는 것 같다.