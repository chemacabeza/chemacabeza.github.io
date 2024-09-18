---
layout: chapter
title: "Chapter 6: Working with Numbers - Integers"
---
# Chapter 6: Working with Numbers - Integers
Bash, by default, primarily operates on integer numerical values. It can perform various arithmetic operations like addition, subtraction, multiplication, and division with integer numbers. These operations are particularly useful for tasks like counting, indexing, and basic mathematical calculations in scripting and automation. Bash's support for integer arithmetic makes it a handy tool for many system-level operations and scripting tasks.

However, Bash does not provide native support for floating-point arithmetic by default. Floating-point numbers include decimal fractions and are used to represent real numbers with high precision. These numbers are essential in scientific computing, financial calculations, and various other domains where precise numerical representations are required. Bash's omission of native support for floating-point arithmetic is largely because it is designed as a lightweight, text-based shell scripting language, and the inclusion of floating-point arithmetic would introduce complexity and potentially slow down the execution of scripts.

To perform floating-point arithmetic in Bash, one often relies on external tools like the “bc” command-line calculator or other scripting languages like Python<a id="footnote-1-ref" href="#footnote-1" style="font-size:x-small">[1]</a> or Perl<a id="footnote-1-ref" href="#footnote-1" style="font-size:x-small">[2]</a>, which provides native support for floating-point operations. These tools offer greater precision and flexibility when working with real numbers, but they come at the cost of additional complexity compared to Bash's integer arithmetic operations. Bash's focus on simplicity and efficiency in handling text and system-level operations remains a significant reason why it doesn't natively support floating-point arithmetic.

In this chapter we will focus specifically on working with integer numbers.

In Bash there are 3 different ways to work with integer numbers.
* Using the “`expr`” command line
* Using the “`let`/`declare`” builtin commands
* Using the Bash Arithmetic Expansion `((...))`

In the following sections we will explore each one of them.

## Working with the “`expr`” command line

This is the legacy way to work with arithmetic. It works with **ONLY** Integers.

First of all, it is worth mentioning that “`expr`” is not a builtin command within Bash. It’s a binary apart. This means that for every execution, a child process will be created, making the whole execution significantly slower.

This book is not intended to give you a full detailed tutorial on “`expr`”. The main idea is to explain how it works with a few examples, leaving the full content out of here.

“`expr`” utility is in charge of evaluating an expression and writing the result on standard output.

### Addition
“`expr`” supports the addition operator “`+`”. You need to call “`expr`” as follows.

```bash
expr <integer/expression_1> + <integer/expression_2> [+ ...]
```

Let's see how it works with an example.
```bash
 1 #!/usr/bin/env bash
 2 #Script: expr_addition.sh
 3 NUM1=3
 4 NUM2=4
 5 echo -n "Result of $NUM1 + $NUM2 is "
 6 expr $NUM1 + $NUM2
```

In the previous script you can see that we declared two integer variables on lines 3 and 4. The you see that in line number 6 we use the operator `expr` to perform the calculation.

When you run the script you will have the following output in the terminal.

```txt
$ ./expr_addition.sh
Result of 3 + 4 is 7
```

### Substraction
“`expr`” supports the subtraction operator “-”. You need to call “expr” as follows:

```bash
expr <integer/expression_1> - <integer/expression_2> [- ...]
```

Let’s see the following example script.

```bash
 1 #!/usr/bin/env bash
 2 #Script: expr_substraction.sh
 3 NUM1=3
 4 NUM2=4
 5 echo -n "Result of $NUM1 - $NUM2 is "
 6 expr $NUM1 - $NUM2
```

When you run the previous script you will get the following output.

```txt
$ ./subtraction.sh
Result of 3 - 4 is -1
```

### Division
“`expr`” supports the division operator “`/`”. You need to call “`expr`” as follows:

```bash
expr <integer/expression_1> / <integer/expression_2> [/ ...]
```

Let’s see how it works with the following example script.

```bash
 1 #!/usr/bin/env bash
 2 #Script: expr_division.sh
 3 NUM1=9
 4 NUM2=3
 5 echo -n "Result of $NUM1 / $NUM2 is "
 6 expr $NUM1 / $NUM2
```

When you run the previous script you will get the following output.

```txt
$ ./expr_division.sh
Result of 9 / 3 is 3
```

In the case of using operands whose result is a decimal number, **only the integer part will be given as result**. You can see the result by modifying the value of the “`NUM2`” variable in the previous script (“`expr_division.sh`”) to 2. The result will be `4.5` but the script will print only `4`.

### Multiplication
“`expr`” supports the multiplication operator “`*`”. You need to call “`expr`” as follows:

```bash
expr <integer/expression_1> \* <integer/expression_2> [\* ...]
```

Pay attention to the detail of “*escaping*” the “`*`” character. We do this to avoid bash interpreting such a character as a globbing character so that expansion does not happen. With the escaping character we are forcing a literal interpretation of the character “`*`” without expansion.

Let's see how it works with the following example script.

```bash
 1 #!/usr/bin/env bash
 2 #Script: expr_multiplication.sh
 3 NUM1=9
 4 NUM2=3
 5 echo -n "Result of $NUM1 * $NUM2 is "
 6 expr $NUM1 \* $NUM2
```

Whe you execute the previous script you will get the following as result.

```txt
$ ./expr_multiplication.sh
Result of 9 * 3 is 27
```

### Modulo
“`expr`” also supports the modulo operator “`%`”. You need to call “`expr`” as follows:

```bash
expr <integer/expression_1> % <integer/expression_2> [% ...]
```

Let’s see how it works with the following example script.

```bash
 1 #!/usr/bin/env bash
 2 #Script: expr_modulo.sh
 3 NUM1=8
 4 NUM2=3
 5 echo -n "Result of $NUM1 % $NUM2 is "
 6 expr $NUM1 % $NUM2
```

When you execute the previous script you will get the following result.

```txt
$ ./expr_modulo.sh
Result of 8 % 3 is 2
```

### Comparison

“`expr`” supports the comparison operators (“`=`”, “`>`”, “`>=`”, “`<`”, “`<=`”, “`!=`”, will call them “`<op>`”). You need to call “`expr`” as follows:

```bash
expr <expression_1> <op> <expression_2> [<op> ...]
```

The comparison operators give as result either “`1`“ (if the result of the comparison was `true`) or “`0`” (if the result of the comparison was `false`).

Same as with the multiplication character, with the character “`<`” we have to escape it to avoid Bash confusing it with a redirection character (which we will speak about later in another chapter).

Let’s see how it works with the following example script.

```bash
 1 #!/usr/bin/env bash
 2 #Script: expr_comparison.sh
 3 NUM1=8
 4 NUM2=3
 5 echo -n "Result of $NUM1 < $NUM2 is "
 6 expr $NUM1 \< $NUM2
```

When you run the previous script you will get the following result in the terminal.

```txt
$ ./comparison.sh
Result of 8 < 3 is 0
```

The result of the script is `0`, which means that the number `8` is not smaller than the number `3`.


### Boolean

“`expr`” has support for the boolean operands **AND** (“`&`” character) and **OR** (“`|`” character) that we can use to compose logical operations.

These boolean operands allow you combine the comparison expressions that we saw in the previous section.

Pay attention that the operands AND and OR, when passed to the “`expr`” command, need to be between double quotes. 

Let’s see how it works with the following script.

```bash
 1 #!/usr/bin/env bash
 2 #Script: expr_boolean.sh
 3 NUM1=8
 4 NUM2=3
 5 echo -n "Result of $NUM1 > $NUM2 & $NUM2 >= 3 is "
 6 expr  $NUM1 \> $NUM2 "&" $NUM2 \>= 3
 7 #                    ^^^
```

When you execute the previous script you will get the following result:

```txt
$ ./expr_boolean.sh
Result of 8 > 3 & 3 >= 3 is 1
```

As you can see in the execution of the script, the result is `1`. The result `1` means that the two comparisons resulted to be `true`.

If you pay attention to the line 7 of the previous script you will see that the AND operator is within double quotes.


## Working with the “`let`/`declare`” builtin commands

In this section we are going to take a look at how to work with Integers with two builtin commands of Bash:
* `declare`: We already mentioned it in a previous chapter.
* `let`: Which is new to us and we will talk about in a bit.


### "`declare`" command

As we mentioned already, “`declare`” can be used to give to the declared variables some specific behavior (specific semantics). In our case, we are going to use the “`declare`” built-in command to declare integer numbers.

In our current case, we need to use the option “`-i`” to indicate the declaration of an integer variable.

In the following script we use the “`-i`” flag to declare a new integer variable named “`myIntVar`”.

```bash
 1 #!/usr/bin/env bash
 2 #Script: declare_integer.sh
 3 declare -i myIntVar=45
 4 myIntVar=$myIntVar+1
 5 echo $myIntVar
```

When you execute the previous script you will get the following result in the terminal.

```txt
$ ./declare_integer.sh
46
```

In this case the builtin command “`declare`” is specifying that the new variable “`myIntVar`” should behave as an integer number (remember that, by default, variables in Bash behave like STRINGs).

Something to mention is that, in the same way that “`declare`” can add specific behaviors to variables (like the previous one with integer and “`-i`”), it can remove it (setting it back to the default one) by using the “*negated*” option “`+i`”. 

Let’s see it how it works with an example.

```bash
 1 #!/usr/bin/env bash
 2 #Script: declare_integer_negated.sh
 3 declare -i myIntVar=45
 4 myIntVar=$myIntVar+1
 5 echo $myIntVar # 46
 6 declare +i myIntVar
 7 myIntVar=$myIntVar+1
 8 echo $myIntVar # 46+1
```

When you run the previous script you will see the following in the terminal.

```txt
$ ./declare_integer_negated.sh
46
46+1
```

So... What is happening in the script? On line 3 we are declaring a variable named “`myIntVar`” that has a semantic of an integer (it is because of the flag `-i` in the `declare` command).

In line 4 the variable “`myIntVar`” is incremented by one. As it has "*integer semantic*" the operation works perfectly.

However, when we remove the "*integer semantic*" on line 6, the variable “`myintvar`” becomes a String. Due to this, the value printed on line 8 is “`46+1`” as now the variable has "*string semantic*".

You can also invoke the command "`declare`" without any arguments. This invocation will show the attributes and values of all the variables and functions available.

### "`let`" command

“`let`” is a builtin command of Bash that evaluates arithmetic expressions. Its syntax is as follows:

```bash
let arg1 [arg2 ...]
```

“`let`” evaluates (from left to right) each argument as a mathematical expression. 

This builtin command supports a wide variety of operators, which are as follows:

| Operator | Operation |
| :----    | :----     |
| `var++`, `var--` | Post-increment (`++`), Post-decrement (`--`). Interpret the value of integer variable var and then add or subtract one (1) to it. |
| `++var`, `--var` | Pre-increment (`++`), Pre-decrement (`--`). Add or subtract one (1) to the value of integer variable var, and then interpret the value.|
| `-expr`, `+expr` | Unary minus, Unary plus. Unary minus returns the value of the expression expr it precedes, as if it had been multiplied by negative one (-1). Unary plus returns the expression expr unchanged, as if it had been multiplied by one (1). |
| `!`, `~` | Logical negation, Bitwise negation. Logical negation returns false if its operand is true, and true if the operand is false. Bitwise negation flips the bits in the binary representation of the numeric operand. |
| `**` | Exponentiation |
| `*`, `/`, `%` | Multiplication, division, remainder (modulo) | 
| `+`, `-` | Addition, subtraction. |
| `<<`, `>>` | Bitwise shift left, bitwise shift right. |
| `<=`, `>=` ,`<`, `>` | Comparison: less than or equal to, greater than or equal to, less than, greater than. |
| `==`, `!=` | Equality, inequality. Equality returns true if its operands are equal, false otherwise. Inequality returns true if its operands are not equal, false otherwise. |
| `&` | Bitwise AND. The corresponding binary digits of both operands are multiplied to produce a result. For any given digit, the resulting digit is 1 only if the corresponding digit in both operands is also 1. |
| `^` | Bitwise XOR (eXclusive OR). A binary digit of the result is 1 if and only if the corresponding digits of the operands differ. For instance, if the first binary digit of the first operand is 1, and the second operand's first digit is 0, the result's first digit is 1. |
| `|` | Bitwise OR. If either of the corresponding digits in the operands is 1, that digit in the result is also 1. |
| `&&` | Logical AND. Returns true if both of the operands are true. |
| `||` | Logical OR. Returns true if either of the operands is true. |
| `expr1 ? expr2 : expr3` | Conditional (ternary) operator. If “`expr1`” is true, return “`expr2`”. If “`expr1`” is false, return “`expr3`”. |
| `=`, `*=`, `/=`, `%=`, `+=`, `-=`, `<<=`, `>>=`, `&=`, `^=`, `|=` | Assignment. Assign the value of the expression that follows the operator, to the variable that precedes it. If an operator prefixes the equals sign, that operation is performed before assignment. For example `let "var += 5"` is equivalent to `let "var = var + 5"`. The assignment operation itself evaluates to the value assigned. |

Let’s see an example script where we will use several of the previous operators.

```bash
 1 #!/usr/bin/env bash
 2 #Script: let.sh
 3 # Using the post-increment operator"
 4 let "myVar = 32"
 5 echo "Orignal value of myVar: $myVar"
 6 echo "Using post-increment operator"
 7 let "myVar++"
 8 echo "New value of myVar: $myVar" # 33
 9 # Using the shift left operator
10 let "myNumber = 16"
11 echo "Original value of myNumber: $myNumber"
12 let "myNumber <<= 1"
13 echo "New value of myNumber: $myNumber" # 32
```

In the previous script you see that on line 4 a variable is declared with name "`myVar`" and value 32. On line 7 you see that the post-increment operator is used, that statement increments the value of the variable by 1.

Then on line 10 you see that a variable is declared with name "`myNumber`" with a value of 16. On line 12, we are using the shifting left assignment operator, which will move all the bits of the number once place to the left, resulting in the number being multiplied by 2.

When you run the previous script you will get the following output in your terminal.

```txt
$ ./let.sh
Orignal value of myVar: 32
Using post-increment operator
New value of myVar: 33
Original value of myNumber: 16
New value of myNumber: 32
```

## Other Integer Numbers




<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px;">

<p id="footnote-1" style="font-size:10pt">
1. https://www.python.org <a href="#footnote-1-ref">&#8617;</a>
</p>
<p id="footnote-2" style="font-size:10pt">
2. https://www.perl.org <a href="#footnote-2-ref">&#8617;</a>
</p>

