---
title: Homework 4 - Stack calculator
layout: default
---

IMP : Do not use the Stack container adaptor, i.e do not use the "stack" header file. You will have to write code for your own functions.

In class, I had shown you how stacks are used to evaluate expressions
in the postfix form.
Your task is to program a calculator that uses a stack. These
calculators use "postfix" notation or "Reverse Polish notation (RPN)"
for their mathematical expressions.

For example, `5 - 2` is instead written `5 2 -` and `5 - (2 + 1)`
written `5 2 1 + -`

The way this works is as follows:

  * If a number is typed, push it on the stack.

  * If an operation is typed, depending on the operation, pop off zero
    or more numbers from the stack, perform the operation, and
    possibly push the result(s) back on the stack.

It is upto you whether you want the user to enter the expression in infix form
or in postfix form. If the user enters in infix form, then you have to convert it to 
postfix form before evaluating it using the stack. 
The interface allows you to enter one element at a time, and depending on whether the
element was an operand or an operator, performs the necessary step.


*Note : I will be using just binary operators when entering an expression for evaluation.

You can test your program on these example expressions :
a) 52 7 + 12 - 8 4 * +

b) 3 103 - 

c) 8 4 / 5 6 8 3 + - * *

d) 8 2 / 7 * 9 - +

e) 9 5 3 + 89 6 - - 

