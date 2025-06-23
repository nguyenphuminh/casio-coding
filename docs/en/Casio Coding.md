# Casio Coding

*Contact: minhmok095@gmail.com*


## I. Introduction

In this document, I will be using the Casio fx580 VN X model, and you should have prior experience with programming before continuing.

In reality, some Chinese have already discovered ways to arbitrarily execute low-level operations in the fx991 and fx580 before 2018.

What I am showing next only utilizes computation based on normal features and is not arbitrary. Thus, it will not be nearly as useful as what people have already come up with, but it is definitely easier to understand. If you want the op stuff, check out these links:

* http://casiocalc.wikidot.com/
* https://community.casiocalc.org/


## II. Fundamentals

### II.1. The program

We can use equations as commands, and since the release of the Casio fx500, we can calculate multiple equations using ":" as such:

$$1 + 1:5 - 0:3 \div 4$$

Press "CALC", you will see the result is $2$ from the equation $1 + 1$; press "=", the result will be $5$ from $5 - 0$; press "=" again, it will be $\frac{3}{4}$ from $3 \div 4$; press "=" again, now it jumps back to the first equation of $1 + 1$.

Basically we just use mathematical equations as code or commands, with each command separated by a ":" punctuation. When all finishes, the program jumps back to the first command. In the next section, I will explain the reason why we have to press "CALC" in the first step.

### II.2. Variables

In Casio, we have 10 variables A, B, C, D, E, F, x, y, z, M, with the default value of each being 0. We have to press "CALC" at the start because it will pop up an input for you to initiate values of variables in your code before execution.

For instance, let's say we have a program like this:

$$A + 1: B + 3$$

When you press "CALC", the screen will let you type in the values of A and then B. I will set A as 1, B as 6. When run, the screen will show 2, and then 9.

We can also assign values for variables during execution like this:

$$A=something$$

For example:

$$A = A + 1:A$$

Press "CALC", set A to 1, when we reach the second command, we will see that the result is 2, because variable A has been updated to be A+1 (A=1 and 1+1=2, so new A is 2).

So far it is still kinda useless, but we can use variables in loops in the next section.

### II.3. Loops

An important piece of the puzzle is loops. There are no real direct or meaningful ways to implement loops across commands in the Casio, but we can see a pattern where when all commands are run, it will jump back to first command, and more importantly, the values of variables are unchanged. Accordingly, we can utilize this behavior as a loop.

#### Fibonacci sequence

To create an example, I will use a loop to find a number in the Fibonacci sequence. If you have not already known, it is a sequence where each number is the sum of two previous numbers, and two first numbers are 0 and 1.

Example: 0, 1, 1, 2, 3, 5, 8, 13,...

To solve this problem, I have an algorithm: Declare A=0, B=1, C=0, with C being the n-th Fibonacci number, A and B serving as two previous numbers. I will loop continuously, and in each loop I will assign A+B to C, then set A as B, B as C.

With the idea in mind, we have this program:

$$C = A + B:A = B:B = C$$

Press "CALC", set A=0, B=1, C=0, press "=" continuously, each time we jump back to the first equation, we will see C updated to be the next number in the Fibonacci sequence.

#### The problem with automation

In reality, if we have to press "=" continuously, it would be really tedious. Therefore, when coding in the Casio, we should try bringing the problem down to a simple equation that does not need loops, or try the next looping methods.

#### Summation and product notation

We can do summation and product like this:

Sum:

$$\sum\limits_{x=start}^{end} (\text{some equation})$$

Product:

$$\prod\limits_{x=start}^{end} (\text{some equation})$$

If you have not already known, the summation notation will loop from x=start to x=end, with an increment of 1. For each loop, we will substitute the current x into "some_equation", then we will add the equation to the sum result. The pi/product notation works the same way, but it calculates the product by continuous multiplication.

A huge bottleneck of these two is that we can not change the value of variables inside the loop, there is no way to mutate the state, and inherently they are just calculating sum and product. Though, they can serve some interesting purposes, as exemplified in the next section on conditions.

#### Tables and modulo

The Table mode allows you to type in an equation, starting point, ending point, and step. Then, it will calculate all results of that equation with x from `start` to `end`, incrementing x by `step` per run. This is essentially a loop similar to what we do with summation/product notation above, but we get to see all results and can set our desired step here, while those force the step to be 1.

Though, the most powerful functionality of Table is that you can read/edit your memory while looping by abusing the modulo operator `÷R`. Do note that we have to go through this hassle because you can not edit variables like we did above in Table.

`A ÷R B` returns the integer answer of `A ÷ B` as well as the remainder and stores the answer into variable E, the remainder into variable F. This means if you want to pass some value of the previous run (call it `A`) to the next run, you can just do `A ÷R B` with B being bigger than A, and the next run will have its F updated to be A. Because B is bigger than A, `A ÷R B` will always result in 0 so it will not affect anything you write next to it which is neat.

If this section is too complicated, no worries, you can skip this and come back later because this is only useful if combined with the next concepts.

### II.4. Round numbers using Int

In the Casio calculator, there is a really powerful function that I use in almost all basic operations: The Int function. It can round the number down (getting only the integer part), for example, Int(3.41) returning 3, and we can use it to implement more neat features.

#### Normal rounding

If the decimal part is greater than 0.5, we will round up, otherwise we round down. We can implement it like so:

$$Int(A + 0.5)$$

This works because if a decimal part is greater than or equal to 0.5 plus 0.5, it will result in a number greater than or equal to 1, round down and now we have 1; otherwise, it will result in 0.

#### Round up

To round up (ceil), we can use this equation:

$$Int(A + 1 - 10^{-10})$$

The idea is that if a number is 0 then 0 + 0.999...9 rounded down is still 0, but a number from 0.000...01 and above plus 0.999...9 will result in a number greater than or equal to 1, rounded down to 1 (note that the amount of digits of 0.999...9 and 0.000...01 are the same). Because the Casio will only accurately represent numbers with 10 characters and below, $10^{-10}$ is good enough.

#### For negatives

Though, these methods only work if the number is positive. If the number is negative, you should use:

$$Int(A - 0.5)$$

and

$$Int(A - 1 + 10^{-10})$$

### II.5. Branching and conditions

In Casio, there is no if statements, so we will use maths to solve this problem. What we need to achieve is an expression where if one condition is true, returns a value, or else returns a different value. In reality, there are multiple approaches to this, but I will list out some that I use most frequently.

#### Absolute

Absolutes of numbers can be regarded as a conditional expression where if n >= 0, return n, or else return -n.

We can apply this trick in certain problems, such as finding the maximum or minimum between 2 numbers. We know that the sum of their sum and difference divided by 2 results in the maximum, while the difference of their sum and difference divided by 2 results in the minium. But how can we get the positive difference without knowing which one is larger between 2 numbers? We use absolutes, as demonstrated below:

Get maximum:

$$\frac{A + B + \lvert A - B \rvert}{2}$$

Get minimum:

$$\frac{A + B - \lvert A - B \rvert}{2}$$

#### Multiplication with 0 (and 1)

The most general way to create a conditional structure is to consider a condition an equation, then take that "condition" equation to multiply with another "input" equation. If the condition equation is equal to 0, then the result will only be 0, otherwise it will be the product of two equations.

But this is usually not enough, in a lot of cases, to do calculation properly, you would want to keep the original value of the "input" equation. In other words, you need the condition equation to be 1.

To bring a condition down to just 0 and 1, I have several tricks up my sleeves.

#### The tanh function

The Casio already comes with the tanh function. This function returns a number between 0 and 1 if x >= 0, or a number between -1 and 0 if x <= 0, and it is continuous. Using this effect, we can check if a number is negative or positive, which we can then use for number comparison.

**Greater/Less than comparison**

Imagine we want an equation involving A and B, such that if A >= B it returns 1, if A < B it returns 0.

A is greater than or equal to B only if A-B >= 0, less than B if A-B < 0, based on that idea we have

$$tanh(A - B)$$

returning a number from -1 to 0 if A <= B, 0 to 1 if A >= B. But we want the result to be 0 or 1, so we can just add 1 then divide by 2, now we have:

$$\frac{tanh(A - B) + 1}{2}$$

Now the equation returns a number between 0 and 1, and when A-B=0, the result is 0.5, so we just have to round the number to get either 0 or 1:

$$Int(\frac{tanh(A - B) + 1}{2} + 0.5)$$

Shortened:

$$Int(\frac{tanh(A - B) + 2}{2})$$

The equation above returns 1 if A >= B, 0 if A < B, which is perfect for our case.

**Equality**

What we aim for is something that returns 1 if A = B, 0 if A /= B. With this in mind, we recognize that tanh(0) returns 0, tanh of any other number returns a number between -1 and 1 that is not 0. Furthermore, A - B is 0 if A = B, so we have an idea: 1-0 rounded down is 1, 1-n where n is a number between 0 and 1 rounded down is 0. Finally, we have this formula:

$$Int(1 - \lvert tanh(A - B) \rvert)$$

**Tips**

We can verify equality of multiple pairs (AB, CD, EF for example) using:

$$Int(1 - \lvert tanh(A - B)tanh(C - D)tanh(E - F) \rvert)$$

We can do the same with the geq comparison:

$$Int(\frac{tanh(A - B)tanh(C - D)tanh(E - F) + 2}{2})$$

Since all equations above return 1 and 0, we can get the reversed condition simply by using 1 substracted by the result. For example, here is an equation that returns 1 if A is different from B and 0 if A is equal to B:

$$1 - Int(1 - \lvert tanh(A - B) \rvert)$$

#### Applications

In this section, let's use what we have learnt to solve some problems relating to condtions.

**Prime number check**

With n being the number to verify, we loop from 2 to the square root of n, and if n is divisable by any number in the loop, n is not a prime.

A number is divisable by another number only if the quotient is an integer, so to check divisability, we just have to check if the rounded quotient is equal to the quotient:

$$1 - Int(1 - \lvert tanh(Int(\frac{A}{x}) - \frac{A}{x}) \rvert)$$

This equation will return 0 if A is divisable by x, 1 if not.

If we use the production notation, only one multiplier being 0 can make the final product 0 regardless of others. So we can plug in the equation we have come up with above in a product notation and it's complete:

$$\prod\limits_{x=2}^{Int(\sqrt{A})} (1 - Int(1 - \lvert tanh(Int(\frac{A}{x}) - \frac{A}{x}) \rvert))$$

This will return 1 if A is a prime, 0 if not.

### II.6. Modify numbers

Let's say we have number 123456, we want to slice from the third digit to the fifth digit (or 345).

First, we remove "12" to get "3456". We realize that 123456 mod 10000 equals to 3456, but there is no built-in way to calculate modulo in Casio ($\div R$ does not give us a result that can be assigned to a variable or usable in sum/product), so we have this formula:

$$123456 - 10000Int(\frac{123456}{10000})$$

Note: You can use the `÷R` operator to calculate the remainder and access it through the F variable too, but note that it will not work in sigmas or the product notation.

Then, we remove "6" to get "345". We know that 3456 / 10 = 345.6, rounded down to 345, so now we have:

$$Int(\frac{123456 - 10000Int(\frac{123456}{10000})}{10})$$

Abstract form with A being the number, B being a known length of A, C is the start, D is the end:

$$Int(\frac{A - 10^{B-C+1}Int(\frac{A}{10^{B-C+1}})}{10^{B-D}})$$

Note that you can also extract child numbers in a different base, not just decimal:

$$Int(\frac{A - E^{B-C+1}Int(\frac{A}{E^{B-C+1}})}{E^{B-D}})$$

with E being the base you want.

#### Extra example

Based on that, we also have a formula to substitute a portion with a different number E:

$$A - (A - 10^{B-C+1}Int(\frac{A}{10^{B-C+1}})) + 10^{B-D}E + (A-10^{B-D}Int(\frac{A}{10^{B-D}}))$$

Shortened to:

$$A + 10^{B-D}(10^{D-C+1}Int(\frac{A}{10^{B-C+1}}) + E - Int(\frac{A}{10^{B-D}}))$$
