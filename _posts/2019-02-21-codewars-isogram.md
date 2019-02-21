---
layout: post
title:  "Isogram"
image: ''
date:   2019-02-21 23:55:00
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
An isogram is a word that has no repeating letters, consecutive or non-consecutive. Implement a function that determines whether a string that contains only letters is an isogram. Assume the empty string is an isogram. Ignore letter case.<br/>

{% highlight javascript %}
isIsogram "Dermatoglyphics" == true
isIsogram "moose" == false
isIsogram "aba" == false
{% endhighlight %}

## My Solution
{% highlight javascript %}
public class isogram {
  public static boolean  isIsogram(String str) {
    if(str.length()==0) return true;
      
    char[] isogram = str.toLowerCase().toCharArray();
      
    for(int i=0; i<isogram.length; i++) {
      for(int j=0; j<isogram.length; j++) {
        if(i!=j && isogram[i] == isogram[j]) {
          return false;
        }
      }
    }

    return true;
  } 
}
{% endhighlight %}

## Sample Tests
{% highlight javascript %}
import org.junit.Test;
import static org.junit.Assert.assertEquals;
import org.junit.runners.JUnit4;
public class Tests {
  @Test
  public void FixedTests() {
    assertEquals(true, isogram.isIsogram("Dermatoglyphics"));
    assertEquals(true, isogram.isIsogram("isogram"));
    assertEquals(false, isogram.isIsogram("moose"));
    assertEquals(false, isogram.isIsogram("isIsogram"));
    assertEquals(false, isogram.isIsogram("aba"));
    assertEquals(false, isogram.isIsogram("moOse"));
    assertEquals(true, isogram.isIsogram("thumbscrewjapingly"));
    assertEquals(true, isogram.isIsogram("")); 
  }
}
{% endhighlight %}

## Other Solution
{% highlight javascript %}
public class isogram {
  public static boolean  isIsogram(String str) {
    return str.length() == str.toLowerCase().chars().distinct().count();
  } 
}

class isogram {
    public static boolean isIsogram(String str) {
        return str.toLowerCase()
                  .chars()
                  .distinct()
                  .count() == str.length();
    }
}

import java.util.regex.Pattern;
public class isogram {
    public static boolean  isIsogram(String str) {
        return !Pattern.compile("(?i)\\b\\w*(\\w)\\w*(?=\\1)\\w*\\b").matcher(str).matches();
    }
}
{% endhighlight %}

## 느낀점
문제가 간단해서 금방 풀 수 있었고 역시 다른 사람들의 문제 풀이는 항상 간단하게 나오는거 같다.