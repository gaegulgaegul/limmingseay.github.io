---
layout: post
title:  "Jaden Casing Strings"
image: ''
date:   2019-02-10 14:11:00
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
Jaden Smith, the son of Will Smith, is the star of films such as The Karate Kid (2010) and After Earth (2013). Jaden is also known for some of his philosophy that he delivers via Twitter. When writing on Twitter, he is known for almost always capitalizing every word.<br/>

Your task is to convert strings to how they would be written by Jaden Smith. The strings are actual quotes from Jaden Smith, but they are not capitalized in the same way he originally typed them.<br/>

Example:
{% highlight javascript %}
Not Jaden-Cased: "How can mirrors be real if our eyes aren't real"
Jaden-Cased:     "How Can Mirrors Be Real If Our Eyes Aren't Real"
{% endhighlight %}
Note that the Java version expects a return value of null for an empty string or null.<br/>

문제가 영어로만 되어있어 당황했지만 예제를 보고 바로 알 수 있었다.

## My Solution
{% highlight javascript %}
public class JadenCase {

	public String toJadenCase(String phrase) {
    if("".equals(phrase) || null == phrase) {
      return null;
    }
  
		String[] phraseArray = phrase.split(" ");
    String returnStr = "";
    
    for(String str : phraseArray) {
      returnStr += str.substring(0,1).toUpperCase() + str.substring(1) + " ";
    }
		
		return returnStr.trim();
	}

}
  }

}
{% endhighlight %}

## Sample Tests
{% highlight javascript %}
import static org.junit.Assert.assertEquals;
import static org.junit.Assert.assertNotNull;
import static org.junit.Assert.assertNull;

import org.junit.Test;

public class JadenCaseTest {

	
	JadenCase jadenCase = new JadenCase();
	
	@Test
	public void test() {
		assertEquals("toJadenCase doesn't return a valide JadenCase String! try again please :)", "Most Trees Are Blue", jadenCase.toJadenCase("most trees are blue"));
	}
	
	@Test
	public void testNullArg() {
		assertNull("Must return null when the arg is null", jadenCase.toJadenCase(null));
	}
	
	@Test
	public void testEmptyArg() {
		assertNull("Must return null when the arg is empty string", jadenCase.toJadenCase(""));
	}

}
{% endhighlight %}

## Other Solution
{% highlight javascript %}
// collection을 이용한 방식1
import java.util.Arrays;
import java.util.stream.Collectors;

public class JadenCase {

  public String toJadenCase(String phrase) {
      if (null == phrase || phrase.length() == 0) {
          return null;
      }

      return Arrays.stream(phrase.split(" "))
                   .map(i -> i.substring(0, 1).toUpperCase() + i.substring(1, i.length()))
                   .collect(Collectors.joining(" "));
  }

}

// Collection을 이용한 방식2
import java.util.Arrays;
import java.util.stream.Collectors;

public class JadenCase {

  public String toJadenCase(String phrase) {
    if(phrase == null || phrase.isEmpty()) return null;
    return Arrays.stream(phrase.split("\\s+")).map(str -> Character.toUpperCase(str.charAt(0)) + str.substring(1))
        .collect(Collectors.joining(" "));
  }

}

// 정규식을 이용한 방식
import java.util.regex.*;

public class JadenCase {

  public String toJadenCase(String phrase) {
    Matcher matcher = Pattern.compile("(?:^|\\s)(\\w)").matcher(phrase);
    StringBuilder builder = new StringBuilder(phrase);

    while (matcher.find()) {
      builder.setCharAt(matcher.end() - 1, matcher.group(1).toUpperCase().charAt(0));
    }
    
    return phrase == null || phrase.length() == 0 ? null : builder.toString();
  }
}
{% endhighlight %}

## 느낀점
약간 하드코딩의 느낌이 있지만 그래도 얼마 안걸리게 한거 같은 느낌이다.<br/>
다른 사람들의 문제풀이를 보니 람다식과 정규식을 많이 이용하고 무엇보다 collection을 이용해 문제를 풀고 있었다.<br/>
그리고 isEmpty() 함수의 기능을 생각하지 못했고 회사에서 쓰는 방식 그대로 사용하고 있어 다시 생각해보는 계기가 되었다.<br/>
정규식의 공부가 필요하다는 것을 뼈져리게 느끼게 되는 것 같다.<br/>