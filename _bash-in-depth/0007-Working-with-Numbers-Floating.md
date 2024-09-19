---
layout: chapter
title: "Chapter 7: Working with Numbers - Floating-point numbers"
---

# Chapter 7: Working with Numbers - Floating-point numbers

In Bash, working with floating-point numbers can be a bit tricky because Bash’s built-in arithmetic only supports **integer operations**. This means that directly performing calculations involving decimals or fractions is not possible using the default arithmetic capabilities of Bash (`$((...))` or `let`), which are limited to whole numbers.

To handle floating-point numbers, you need to use external tools like `bc` (an arbitrary precision calculator) or `printf` (a format string utility). These tools extend Bash's capability to handle real numbers and precision-based arithmetic.

In the next sections we will learn about the commands `bc` and `printf`.


## `printf` command
An unconventional way, although practical, to work with floating point numbers with native bash is by using together with Scientific Notation.

What is the “*Scientific Notation*”? It’s a way to express numbers which has the following format.

```bash
    <base>e<exponent>
```

which would be equivalent to `<base> * 10^{<exponent>}`<a id="footnote-1-ref" href="#footnote-1" style="font-size:x-small">[1]</a>

This way of expressing numbers will tell the “`printf`” command where to put the decimal comma.

Let's see how it works with the following script.

```bash
 1 #!/usr/bin/env bash
 2 ##Script: printf.sh
 3 printf "Number 1: %.3f\n" 12345e3
 4 printf "Number 2: %.3f\n" 12345e-3
 5 printf "Number 3: %.3f\n" 1234.5e3
 6 printf "Number 4: %.3f\n" 1234.5e-3
```

When you run the previous script you will get the following output in your terminal.

```txt
$ ./printf.sh
Number 1: 12345000.000
Number 2: 12.345
Number 3: 1234500.000
Number 4: 1.235
```

From this script and its output we can understand that the scientific notation is a way of representing numbers that tells how many places should the decimal comma be moved and in what direction.

In lines 1 and 3 of the output, the decimal comma will be moved to the right, while the lines 2 and 4 will move the decimal comma to the left.

Let’s summarize the basics of using this approach:
* Make sure the operation you want to do is expressed using integer numbers only (otherwise Bash will fail)
* Decide in advance how many decimals you want to have in your result.
* When you use the Bash arithmetic expansion make sure you multiply the operation by “`10**<#decimals>`”. Like that you will calculate a number that is “#decimals” times bigger.
* Use scientific notation “`e-<#decimals>`” to be able to move the comma to the left to be able to have the number of decimals you want.

We could summarize it even more with the following line.

```bash
    printf %.<precision>f "$((10**<multiplier>*<operation>))e-<multiplier>”
```

Now let’s use the previous points in a more complex operation. We are going to create a script that will calculate the volume of a sphere.

```bash
 1 #!/usr/bin/env bash
 2 #Script: sphere.sh
 3 #!/usr/bin/env bash
 4 PI=314
 5 NUM_DECIMALS=2
 6 RADIUS=2
 7 printf "The volume of a sphere with radius $RADIUS is %.3f\n" "$((10**$NUM_DECIMALS * $PI * $RADIUS**3 * 4 / 3))e-$(($NUM_DECIMALS*2))"
```

This script will calculate the volume of a sphere with radius of 2 units. When you run the previous script you will see this in your terminal.

```txt
$ ./sphere.sh
The volume of a sphere with radius 2 is 33.493
```

Now that we know how to use the `printf` command to operate with floating-point numbers, we are going to take a look to the command `bc`.

## `bc` command

The “`bc`” command is a tool that will allow us to develop more complex calculations. It can be used in both interactive and non-interactive ways. It comes with its own language (with similarities to the C programming language) that allows you to develop scripts making mathematical calculations.

To have a quick introduction to the language behind the “`bc`” command, we are going to present a few tables with the basics so that we can move forward quickly.

<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px;">

<p id="footnote-1" style="font-size:10pt">
1. Will fix the mathematical representation at some point.<a href="#footnote-1-ref">&#8617;</a>
</p>


