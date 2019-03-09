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
Your job is to create a calculator which evaluates expressions in Reverse Polish notation.<br/>

For example expression 5 1 2 + 4 * + 3 - (which is equivalent to 5 + ((1 + 2) * 4) - 3 in normal notation) should evaluate to 14.<br/>

For your convenience, the input is formatted such that a space is provided between every token.<br/>

Empty expression should evaluate to 0.<br/>

Valid operations are +, -, *, /.<br/>

You may assume that there won't be exceptional situations (like stack underflow or division by zero).<br/>

## My Solution
{% highlight javascript %}
import java.util.List;
import java.util.ArrayList;

public class Calc {

  public double evaluate(String expr) {
    if(expr.length() == 0) return 0;
    if(expr.indexOf("+") <= 0 &&
       expr.indexOf("-") <= 0 &&
       expr.indexOf("*") <= 0 &&
       expr.indexOf("/") <= 0)
       return Double.parseDouble(expr);
    
    String[] arr = expr.split("\\s");
    double num1, num2;
    List<Double> list = new ArrayList<>();
    
    for(int i=0; i<arr.length; i++) {
      if("+".equals(arr[i]) || "-".equals(arr[i]) || "*".equals(arr[i]) || "/".equals(arr[i])) {
        num1 = list.get(list.size()-1);
        list.remove(list.size()-1);
        
        num2 = list.get(list.size()-1);
        list.remove(list.size()-1);
        
        switch(arr[i]) {
          case "+": list.add(num2 + num1); break;
          case "-": list.add(num2 - num1); break;
          case "*": list.add(num2 * num1); break;
          case "/": list.add(num2 / num1); break;
        }
      } else {
        list.add(Double.parseDouble(arr[i]));
      }
    }
    
    return list.get(list.size()-1);
  }
}
{% endhighlight %}

## Sample Tests
{% highlight javascript %}
import org.junit.Test;
import static org.junit.Assert.assertEquals;

public class CalcTest {

  private Calc calc = new Calc();

  @Test
  public void shouldWorkWithEmptyString() {
      assertEquals("Should work with empty string", 0, calc.evaluate(""), 0);
  }
  
  @Test
  public void shouldParseNumbers() {
      assertEquals("Should parse numbers", 3, calc.evaluate("3"), 0);
  }

  @Test
  public void shouldParseFloatNumbers() {
      assertEquals("Should parse float numbers", 3.5, calc.evaluate("3.5"), 0);
  }

  @Test
  public void shouldSupportAddition() {
      assertEquals("Should support addition", 4, calc.evaluate("1 3 +"), 0);
  }

  @Test
  public void shouldSupportMultiplication() {
      assertEquals("Should support multiplication", 3, calc.evaluate("1 3 *"), 0);
  }

  @Test
  public void shouldSupportSubstraction() {
      assertEquals("Should support substraction", -2, calc.evaluate("1 3 -"), 0);
  }

  @Test
  public void shouldSupportDivision() {
      assertEquals("Should support division", 2, calc.evaluate("4 2 /"), 0);
  }
}
{% endhighlight %}

## Other Solution
{% highlight javascript %}
import java.util.Stack;

public class Calc {

  public double evaluate(String expr) {
    if (expr.equals("")) {
      return 0;
    }
  
    Stack<Double> stack = new Stack<Double>();
    String[] atoms = expr.split(" ");
    
    for (String atom: atoms) {
      Double a, b;
      switch (atom) {
        case "+": stack.push(stack.pop() + stack.pop()); break;
        case "-": b = stack.pop(); a = stack.pop(); stack.push(a - b); break;
        case "*": stack.push(stack.pop() * stack.pop()); break;
        case "/": b = stack.pop(); a = stack.pop(); stack.push(a / b); break;
        default:
          stack.push(Double.parseDouble(atom));
      }
    }
    
    return stack.pop();
  }
}

import java.util.ArrayDeque;
import java.util.Collections;
import java.util.Deque;
import java.util.HashMap;
import java.util.Map;
import java.util.function.DoubleBinaryOperator;

public class Calc {
  private static final String DELIMITER = " ";
  private static final String OPERATOR_PLUS = "+";
  private static final String OPERATOR_MINUS = "-";
  private static final String OPERATOR_MULTIPLY = "*";
  private static final String OPERATOR_DIVIDE = "/";
  
  private static final Map<String, DoubleBinaryOperator> OPERATORS;
  
  static {
    final Map<String, DoubleBinaryOperator> operators = new HashMap<>(4, 1);
    operators.put(OPERATOR_PLUS, (summand2, summand1) -> summand1 + summand2);
    operators.put(OPERATOR_MINUS, (subtrahend, minuend) -> minuend - subtrahend);
    operators.put(OPERATOR_MULTIPLY, (factor2, factor1) -> factor1 * factor2);
    operators.put(OPERATOR_DIVIDE, (divisor, dividend) -> dividend / divisor);
    OPERATORS = Collections.unmodifiableMap(operators);
  }

  public double evaluate(final String expr) {
    if (expr.isEmpty()) {
      return 0;
    }
    
    final String[] parts = expr.split(DELIMITER);
    final Deque<Double> operands = new ArrayDeque<>(parts.length / 2 + 1);
    for (final String part : parts) {
      final DoubleBinaryOperator operator = OPERATORS.get(part);
      operands.push(operator == null ? Double.parseDouble(part) : operator.applyAsDouble(operands.pop(), operands.pop()));
    }
    return operands.peek();
  }
}

import java.util.Stack;

public class Calc {

  public double evaluate(String expr) {
    if (expr.isEmpty())
        return 0.0;
    Stack<Double> operands = new Stack<Double>();
    for (String token : expr.split(" ")) {
        switch (token) {
            case "+":
                operands.push(operands.pop() + operands.pop());
                break;
            case "-":
                operands.push(-operands.pop() + operands.pop());
                break;
            case "*":
                operands.push(operands.pop() * operands.pop());
                break;
            case "/":
                operands.push(1.0 / operands.pop() * operands.pop());
                break;
            default:
                operands.push(Double.parseDouble(token));
        }
    }
    return operands.pop();
  }
}
{% endhighlight %}

## 느낀점
후위표기법으로 계산하는 로직을 작성하는 문제였다.<br/>
구글링을 하니 stack을 이용해서 문제를 풀었지만 list를 통해 문제를 풀 수 있었다.<br/>
다른 개발자들이 한 것을 보니 stack을 많이 이용해서 코드가 간략해졌다.<br/>
또한 쓸데없는 if문이 없는 것으로 보아 아직 공부가 필요한 것 같다.