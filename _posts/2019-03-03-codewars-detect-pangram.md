---
layout: post
title:  "Detect Pangram"
image: ''
date:   2019-03-03 14:57:00
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
A pangram is a sentence that contains every single letter of the alphabet at least once. For example, the sentence "The quick brown fox jumps over the lazy dog" is a pangram, because it uses the letters A-Z at least once (case is irrelevant).<br/>

Given a string, detect whether or not it is a pangram. Return True if it is, False if not. Ignore numbers and punctuation.<br/>

## My Solution
{% highlight javascript %}
public class PangramChecker {
  public boolean check(String sentence){
    if(sentence.length() == 0) return false;
  
    int[] cnt = new int[26];
    
    String regex = "[\\s\\W\\d]";
    String blank = "\\p{Z}";
    char[] str = sentence.replaceAll(regex, "")
                         .replaceAll(blank, "")
                         .toLowerCase()
                         .toCharArray();
    
    for(char ch : str) {
      cnt[ch-'a']++;
    }
    
    for(int check : cnt) {
      if(check == 0) return false;
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

public class PangramTest {
  @Test
  public void test1() {
    String pangram1 = "The quick brown fox jumps over the lazy dog.";
    PangramChecker pc = new PangramChecker();
    assertEquals(true, pc.check(pangram1));
  }
  @Test
  public void test2() {
    String pangram2 = "You shall not pass!";
    PangramChecker pc = new PangramChecker();
    assertEquals(false, pc.check(pangram2));
  }
}
{% endhighlight %}

## Other Solution
{% highlight javascript %}
public class PangramChecker {
  public boolean check(String sentence){
        for (char c = 'a'; c<='z'; c++)
            if (!sentence.toLowerCase().contains("" + c))
                return false;
        return true;

  }
}

class PangramChecker {
    boolean check(final String sentence) {
        return sentence.chars()
            .filter(Character::isLetter)
            .map(Character::toLowerCase)
            .distinct()
            .count() == 26;
    }
}

public class PangramChecker {
  public boolean check(String sentence){
    return sentence.chars().map(Character::toLowerCase).filter(Character::isAlphabetic).distinct().count() == 26;
  }
}
{% endhighlight %}

## 느낀점
모든 알파벳이 다 확인되어야 하고 숫자나 특수문자를 제외시켜주는 곳에서 헤멘거 같다.<br/>
확실히 람다식을 이용하면 코드가 간결하게 줄어드는 것 같다.<br/>
<code>Arrays.asList(cnt).contains(0);</code>를 이용해 값을 비교해 반환하려했지만<br/>
검색결과 String에서만 확인 가능한 것이였다.(또 하나 배웠네......)<br/>
또한, 정규식을 통해 숫자와 특수문자, 공백을 제외시켜주는 것을 직접 해본 것이 큰 소득인 것 같다.