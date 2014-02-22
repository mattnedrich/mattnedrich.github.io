---
layout: post
title: "Good ol' C Casting"
date: 2013-03-25 21:28
comments: true
categories: [Type Conversion, C, Floating Point]
---

Understanding data representation is important. Consider the following C code.

{% coderay lang:c %}
float foo = 5;
int bar = foo;
printf("%d", bar);
{% endcoderay %}

This little C snippet outputs the number 5, as is expected. Now, let's make a slight change. Instead of assigning foo to bar, lets cast it to an int by casting the address that foo is stored to an int pointer and then dereferencing it.

{% coderay lang:c %}
float foo = 5;
int bar = *(int*)&foo;
printf("%d", bar);
{% endcoderay %}

This time around we get the value 1084227584. That's certainly not 5 (as we observed the first time around), so what's going on here. As you may already know, integer and floating point numbers are stored differently.

### Floating Point Numbers
Let's start with how the float value is stored in memory. Wikipedia gives us this handy IEE 754 single precision binary floating point representation diagram:

<img width="500px" src="{{ root_url }}/images/c_casting/500px-Float_example.svg.png"/>

The most significant bit denotes the sign of the number. The next 8 bits store what's called the exponent. Finally the last 23 bits store the fraction, also known as the mantissa. When determining the value stored in such a floating point number, the following approach is used.

$$(1.0+fraction)\cdot 2^{(exponent-127)}$$

In our example we were storing the value $$5$$. This can be computed as $$1.25\cdot 2^2$$. As a 32-bit floating point number this is stored as 

$${01000000101000000000000000000000}_2$$. 

We can partition this binary number into the three different segments defined in the above diagram:

$$\color{blue}0\color{green}{10000001}\color{red}{01000000000000000000000}_2$$. 

Here, the sign bit $$\color{blue}{0}_2$$ tells us the number is positive. The exponent $$\color{green}{100000001}_2$$ is equal to $$129-127=2$$ in base ten. Finally the fraction $$\color{red}{01000000000000000000000}_2$$ gives us $$2^{-2}$$, also known as $$0.25$$. If we put these three pieces together we see that they give us $$1.25\cdot 2^2 = 5$$.

### Integer Numbers
Integers are stored in standard [two's complement](http://en.wikipedia.org/wiki/Two%27s_complement) format. The number five, would be stored as $${101}_2$$. In two's complement, this is $$1\cdot 2^2 + 0\cdot 2^1 + 1\cdot 2^0$$. We can easily see that this two's complement representation of $$5$$ is very different than the above floating point representation.

### Conclusion
In the first snippet when we assigned foo to bar an implicit floating point to two's complement conversion happened. The result of this conversion was then stored in bar's memory location. In the second snippet we cast foo to an integer and the bits that make up foo were copied verbatim and stored as bar. Because no conversion occurred we ended up with a different result because we treated the bits from the floating point representation of 5 as if they were two's complement.