---
layout: post
title:  "vowel count"
image: ''
date:   2019-02-07 20:37:00
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
Return the number (count) of vowels in the given string.<br/>
문자열에서 모음의 개수를 반환하세요.<br/>
We will consider a, e, i, o, and u as vowels for this Kata.<br/>
a, e, i, o, u는 모음입니다.<br/>
The input string will only consist of lower case letters and/or spaces.<br/>
입력 문자열은 소문자 및 공백으로 구성됩니다.

## My Solution
{% highlight javascript %}
public class Vowels {

  public static int getCount(String str) {
    int vowelsCount = 0;
    char[] vowels = {'a','e','i','o','u'};
    char[] strToArray = str.toCharArray();
    
    for(char vowel : vowels) {
      for(char ch : strToArray) {
        if(vowel == ch) {
          vowelsCount++;
        }
      }
    }
    
    return vowelsCount;
  }

}
{% endhighlight %}

## Sample Tests
{% highlight javascript %}
import org.junit.Test;
import static org.junit.Assert.assertEquals;

public class VowelsTest {
    @Test
    public void testCase1() {
      assertEquals("Nope!", 5, Vowels.getCount("abracadabra"));
    }
    
}
{% endhighlight %}

## Other Solution
{% highlight javascript %}
// 정규식 사용
public class Vowels {

    public static int getCount(String str) {
        return str.replaceAll("(?i)[^aeiou]", "").length();
    }

}

// for문 하나로 사용
public class Vowels {

  public static int getCount(String str) {
    int vowelsCount = 0;
    
    for(char c : str.toCharArray())
      vowelsCount += (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') ? 1 : 0;
    
    return vowelsCount;
  }

}

// contains를 사용하는 방법
public class Vowels {

  public static int getCount(String str) {
    int vowelsCount = 0;
   for(int i=0; i< str.length(); i++)
        {
            if("AEIOUaeiou".contains(str.subSequence(i,i+1)))
            {
                vowelsCount++;
            }
        }

    return vowelsCount;
  }

}
{% endhighlight %}

## 느낀점
지금 이중 for문을 사용해서 작성하였지만 역시 다른 방법으로 하는 것을 많이 봐야겠다.<br/>
처음하는 Codewars 였는데 나름 재밋다고 생각이 든다.<br/>
하루에 하나씩이라도 하도록 해봐야겠다.