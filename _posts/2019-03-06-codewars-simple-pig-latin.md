---
layout: post
title:  "Simple Pig Latin"
image: ''
date:   2019-03-06 23:20:00
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
Move the first letter of each word to the end of it, then add "ay" to the end of the word. Leave punctuation marks untouched<br/>

Examples<br/>

{% highlight javascript %}
pigIt('Pig latin is cool'); // igPay atinlay siay oolcay
pigIt('Hello world !');     // elloHay orldway !
{% endhighlight %}

## My Solution
{% highlight javascript %}
public class PigLatin {
  public static String pigIt(String str) {
    String returnStr = "";
    String[] words = str.split("\\s");
    
    for(String word : words) {
      returnStr += word.substring(1) + word.substring(0,1);
      
      if(word.matches("[^\\w]")) {
        returnStr += " ";
      } else {
        returnStr += "ay ";
      }
    }
    
    return returnStr.trim();
  }
}
{% endhighlight %}

## Sample Tests
{% highlight javascript %}
import org.junit.Test;
import static org.junit.Assert.assertEquals;
import org.junit.runners.JUnit4;
public class PigLatinTests {
    @Test
    public void FixedTests() {
        assertEquals("igPay atinlay siay oolcay", PigLatin.pigIt("Pig latin is cool"));
        assertEquals("hisTay siay ymay tringsay", PigLatin.pigIt("This is my string"));
    }
}
{% endhighlight %}

## Other Solution
{% highlight javascript %}
public class PigLatin {
    public static String pigIt(String str) {
        return str.replaceAll("(\\w)(\\w*)", "$2$1ay");
    }
}

import java.util.Arrays;
import java.util.stream.Collectors;

public class PigLatin {
    public static String pigIt(String str) {
        return Arrays.stream(str.split(" ")).map(PigLatin::pigify).collect(Collectors.joining(" "));
    }

    private static String pigify(String word){
        return word.length() > 1 || Character.isLetter(word.charAt(0)) ? word.substring(1) + word.charAt(0) + "ay" : word;
               
    }
}

import java.util.regex.Pattern;

public class PigLatin {
    private static final Pattern regex = Pattern.compile("(\\w)(\\w*)");

    public static String pigIt(String str) {
        return regex.matcher(str).replaceAll("$2$1ay");
    }
}
{% endhighlight %}

## 느낀점
문자열의 순서를 바꾸면서 "ay"를 붙여서 출력하는데, 또 특수문자는 "ay"를 안 붙인다.<br/>
확실히 정규식에 대해 많은 공부가 필요하다 느끼게 되는 것 같다.<br/>
다른 개발자들의 문제풀이를 보면 replaceAll 함수를 통해 간단하게 문자열을 바꿀 수 있었다.<br/>
또 한가지를 배우게 되는 문제풀이였다.