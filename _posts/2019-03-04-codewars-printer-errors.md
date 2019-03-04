---
layout: post
title:  "Printer Errors"
image: ''
date:   2019-03-04 23:11:00
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
In a factory a printer prints labels for boxes. For one kind of boxes the printer has to use colors which, for the sake of simplicity, are named with letters from <code>a to m.</code><br/>

The colors used by the printer are recorded in a control string. For example a "good" control string would be <code>aaabbbbhaijjjm</code> meaning that the printer used three times color a, four times color b, one time color h then one time color a...<br/>

Sometimes there are problems: lack of colors, technical malfunction and a "bad" control string is produced e.g. <code>aaaxbbbbyyhwawiwjjjwwm</code> with letters not from <code>a to m.</code><br/>

You have to write a function printer_error which given a string will output the error rate of the printer as a string representing a rational whose numerator is the number of errors and the denominator the length of the control string. Don't reduce this fraction to a simpler expression.<br/>

The string has a length greater or equal to one and contains only letters from <code>a</code> to <code>z</code>.<br/>

Examples:

{% highlight javascript %}
s="aaabbbbhaijjjm"
error_printer(s) => "0/14"

s="aaaxbbbbyyhwawiwjjjwwm"
error_printer(s) => "8/22"
{% endhighlight %}

## My Solution
{% highlight javascript %}
public class Printer {
    
  public static String printerError(String s) {
    return s.toLowerCase().replaceAll("[a-m]", "").length() +
            "/" +
            s.length();
  }
}
{% endhighlight %}

## Sample Tests
{% highlight javascript %}
import static org.junit.Assert.*;

import org.junit.Test;

public class PrinterTest {
    @Test
    public void test() {
        System.out.println("printerError Fixed Tests");
        String s="aaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbmmmmmmmmmmmmmmmmmmmxyz";
        assertEquals("3/56", Printer.printerError(s));
    }
}
{% endhighlight %}

## Other Solution
{% highlight javascript %}
public class Printer {
    
    public static String printerError(String s) {
        long errs = s.chars().filter( ch -> ch > 'm').count();
        return errs+"/"+s.length();
    }
}

public class Printer {
    
    public static String printerError(String s) {
        int c=0;
        for(char item:s.toCharArray())
          if(item>'m'||item<'a')c++;
        return c+"/"+s.length();
    }
}

public class Printer {
    
    public static String printerError(String s) {
        if(s == null || s.length() < 1) {
          return "";
        }
        
        int length = s.length();
        int errors = 0;
        for(char c : s.toCharArray()) {
          if(c < 'a' || c > 'm') {
            errors += 1;
          }
        }
        
        return errors + "/" + length;
    }
}
{% endhighlight %}

## 느낀점
정규식을 통해 간단하게 풀 수 있는 문제였던 것 같다.