---
layout: post
title:  "Reverse polish notation calculator"
image: ''
date:   2019-03-09 23:59:00
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
This kata is to practice simple string output. Jamie is a programmer, and James' girlfriend. She likes diamonds, and wants a diamond string from James. Since James doesn't know how to make this happen, he needs your help.<br/>

Task:<br/>

You need to return a string that displays a diamond shape on the screen using asterisk ("*") characters. Please see provided test cases for exact output format.<br/>

The shape that will be returned from print method resembles a diamond, where the number provided as input represents the number of *’s printed on the middle line. The line above and below will be centered and will have 2 less *’s than the middle line. This reduction by 2 *’s for each line continues until a line with a single * is printed at the top and bottom of the figure.<br/>

Return null if input is even number or negative (as it is not possible to print diamond with even number or negative number).<br/>

Please see provided test case(s) for examples.<br/>

Python Note<br/>
Since print is a reserved word in Python, Python students must implement the diamond(n) method instead, and return None for invalid input.<br/>

JS Note<br/>
JS students, like Python ones, must implement the diamond(n) method, and return null for invalid input.<br/>

## My Solution
{% highlight javascript %}
class Diamond {
  public static String print(int n) {
    if(n<=0 || n%2==0) return null;
    
    String diamond = "";
    
    for(int i=0; i<n; i++) {
      for(int j=0; j<n; j++) {
        
        if(i <= n/2) { // forward
          
          if(i+j <= n/2-1) { // left forward
            diamond += " ";
          } else if(j-i >= n/2+1) { // right forward
            break;
          }else {
            diamond += "*";
          }
          
        } else if(i > n/2) { // back
        
          if(i-j >= n/2+1) { // left back
            diamond += " ";
          } else if(i+j >= (n/2)*3+1) { // right back
            break;
          }else {
            diamond += "*";
          }
          
        }
        
      } // end for j
        diamond += "\n";
    } // end for i
    System.out.println(diamond);
    return diamond;
	} // end method
} // end class
{% endhighlight %}

## Sample Tests
{% highlight javascript %}
import org.junit.Test;
import static org.junit.Assert.assertEquals;
import static org.junit.Assert.assertNull;

public class DiamondTest {
    @Test
    public void testDiamond3() {
      StringBuffer expected = new StringBuffer();
      expected.append(" *\n");
      expected.append("***\n");
      expected.append(" *\n");
      
      assertEquals(expected.toString(), Diamond.print(3));
    }
    
    @Test
    public void testDiamond5() {
      StringBuffer expected = new StringBuffer();
      expected.append("  *\n");
      expected.append(" ***\n");
      expected.append("*****\n");
      expected.append(" ***\n");
      expected.append("  *\n");
      
      assertEquals(expected.toString(), Diamond.print(5));
    }    
}
{% endhighlight %}

## Other Solution
{% highlight javascript %}
class Diamond {
  public static String print(int n) {
    if (n % 2 == 0 || n < 0) {
      return null;
    }
    StringBuilder diamond = new StringBuilder();
    for (int i = 1; i <= n; i+=2) {
      appendLine(diamond, i, n);
    }
    for (int i = n-2; i > 0; i-=2) {
      appendLine(diamond, i, n);
    }
    return diamond.toString();
  }
  
  private static void appendLine(StringBuilder diamond, int size, int totalSize) {
    int spaces = (totalSize-size)/2;
    for (int j = 0; j < spaces; j++) {
      diamond.append(" ");
    }
    for (int j = 0; j < size; j++) {
      diamond.append("*");
    }
    diamond.append("\n");
  }
}

class Diamond {
    public static String print(int n) {
        if (n < 0 || n % 2 == 0) return null;
        String shape = "";
        int midRow = n/2 + 1;
        for (int i = midRow; i <= n*2 - midRow; i++) {
            for (int j = 1; j <= n - Math.abs(n - i); j++) {
                if (j <= Math.abs(n - i))
                    shape += " ";
                else 
                    shape += "*";
            }
            shape += "\n";
        }
        return shape;
    }
}

class Diamond {

    static String row(int n, int i) {
        String s = "";
        for (int j = 0; j < (n - i) / 2; ++j) {
            s += " ";
        }
        for (int j = 0; j < i; ++j) {
            s += "*";
        }
        return s + "\n";
    }

    public static String print(int n) {
        if (n < 1 || n % 2 == 0) {
            return null;
        }
        String s = row(n, n);
        for (int i = n - 2; i >= 1; i -= 2) {
            String r = row(n, i);
            s = r + s + r;
        }
        return s;
    }

}
{% endhighlight %}

## 느낀점
다이아몬드 별을 찍는 예제로 요구사항은 금방 이해되지만 오랜만에 알고리즘 문제에 애를 먹었다.<br/>
결국 구글링을 통해 예제를 한번 확인하고 풀 수 있었다.<br/>
이제는 알고리즘 공부도 필요한 것 같다.