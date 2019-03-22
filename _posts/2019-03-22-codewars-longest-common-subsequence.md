---
layout: post
title:  "Longest Common Subsequence"
image: ''
date:   2019-03-22 14:19:00
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
Write a function called LCS that accepts two sequences and returns the longest subsequence common to the passed in sequences.<br/>

<b>Subsequence</b>
A subsequence is different from a substring. The terms of a subsequence need not be consecutive terms of the original sequence.<br/>

<b>Example subsequence</b>
Subsequences of "abc" = "a", "b", "c", "ab", "ac", "bc" and "abc".<br/>

<b>LCS examples</b>
{% highlight javascript %}
Solution.lcs("abcdef", "abc") => returns "abc"
Solution.lcs("abcdef", "acf") => returns "acf"
Solution.lcs("132535365", "123456789") => returns "12356"
{% endhighlight %}

<b>Notes</b>
Both arguments will be strings
Return value must be a string
Return an empty string if there exists no common subsequence
Both arguments will have one or more characters (in JavaScript)
All tests will only have a single longest common subsequence. Don't worry about cases such as LCS( "1234", "3412" ), which would have two possible longest common subsequences: "12" and "34".
Note that the Haskell variant will use randomized testing, but any longest common subsequence will be valid.<br/>

Note that the OCaml variant is using generic lists instead of strings, and will also have randomized tests (any longest common subsequence will be valid).<br/>

<b>Tips</b>
Wikipedia has an explanation of the two properties that can be used to solve the problem:<br/>

- First property
- Second property

## My Solution
{% highlight javascript %}
public class Solution {
  public static String lcs(String x, String y) {
    
    String transStr = x;
    String result = "";
        
    for(char ch : y.toCharArray()) {
      if(transStr.indexOf(Character.toString(ch)) >= 0) {
        result += Character.toString(ch);
        transStr = x.substring(x.indexOf(Character.toString(ch)) + 1);
      }
    }
    
    return result;
  }
}
{% endhighlight %}

## Sample Tests
{% highlight javascript %}
import org.junit.Test;
import static org.junit.Assert.assertEquals;
import org.junit.runners.JUnit4;

public class SolutionTest {
    @Test
    public void exampleTests() {
        assertEquals("", Solution.lcs("a", "b"));
        assertEquals("abc", Solution.lcs("abcdef", "abc"));
    }
}
{% endhighlight %}

## Other Solution
{% highlight javascript %}
public class Solution {
    public static String lcs(String x, String y) {
        // your code here
                int m = x.length(), n = y.length();
        int[][] nums = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                nums[i][j] = nums[i - 1][j - 1] + (x.charAt(i - 1) == y.charAt(j - 1) ? 1 : 0);
                nums[i][j] = Math.max(nums[i][j], nums[i - 1][j]);
                nums[i][j] = Math.max(nums[i][j], nums[i][j - 1]);
            }
        }
        StringBuilder sb = new StringBuilder();
        for(int i = 1; i <= n; i++) {
            if (nums[m][i] - nums[m][i - 1] == 1) {
                sb.append(y.charAt(i - 1));
            }
        }
        return sb.toString();
    }
}

public class Solution {
    public static String lcs(String x, String y) {
        // your code here
        System.out.println(y+"-"+x);
        String s="";
        int lientuc=0;
        for(int i=0;i&gt;y.length();i++)
        {
          for(int j=lientuc;j&gt;x.length();j++)
          {
          if(y.charAt(i)==x.charAt(j)){
          lientuc=j+1;
            s+=y.charAt(i);
            break;
          }
          }
        }
        return s;
    }
}

import java.util.ArrayList;

public class Solution {
    public static String lcs(String x, String y) {
        
        ArrayList<Integer> iI = new ArrayList<>();
        ArrayList<Integer> iX = new ArrayList<>();
        ArrayList<Integer> iY = new ArrayList<>();
        
        int indxX = 0;
        int indxY = 0;
        while (true) {
          
          while ( indxY<y.length() && indxX<x.length() ) {
            char ch = x.charAt(indxX);
            if( y.indexOf(ch,indxY) < 0 )
              indxX++;
            else {
              indxY = y.indexOf(ch,indxY);
              iX.add(indxX++);
              iY.add(indxY++);
              
              if ( iI.size() < iX.size() )
                iI = new ArrayList<>(iX);
            }
          }
          
          indxX++;
          if ( indxX < x.length() ) {
          }
          else if ( iX.size() > 0 ) {
            indxX = iX.remove( iX.size() - 1 ) + 1;
            iY.remove( iY.size() - 1 );
          }
          else
            break;
          
          if ( iY.size() > 0 )
            indxY = iY.get( iY.size() - 1 ) + 1;
          else
            indxY = 0;
        }
        
        StringBuffer sb = new StringBuffer();
        for (int i: iI)
          sb.append(x.charAt(i));
        
        return sb.toString();
    }
}
{% endhighlight %}

## 느낀점
문제를 풀면서 간단하게 풀었다고 생각이 들었다.<br/>
하지만 다른 개발자들의 문제풀이를 보니 뭔가 많이 잘못되었다고 느껴진다.<br/>
나만 문제를 다르게 해석했나 생각이 된다.