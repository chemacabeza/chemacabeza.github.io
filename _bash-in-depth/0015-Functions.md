---
layout: chapter
title: "Chapter 15: Functions"
---

# Chapter 15: Functions


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px;">

Now that we have learnt the basic blocks of Bash we are going to add another layer of abstraction that are… the functions.

In Bash scripting, functions serve as essential building blocks that allow for the modularization and organization of code. A function in Bash is a self-contained block of code that performs a specific task, and it can be invoked or called from anywhere within the script. This modular approach not only enhances code readability but also promotes code reuse, making scripts more efficient and maintainable.

To declare a function in Bash, the “`function`” keyword or a shorthand syntax with parentheses is used, followed by the function name and the curly braces that encapsulate the function body. Functions may or may not receive arguments, allowing for flexibility in handling input parameters. Additionally, they can return values to the calling code, contributing to the versatility of Bash scripts.

Functions in Bash enable the creation of structured and organized scripts by encapsulating specific functionalities. As we go deeper into Bash scripting, we will explore the syntax, parameters, return values, and best practices for utilizing functions. Understanding how to leverage functions enhances the efficiency and readability of Bash scripts, contributing to the development of robust and maintainable automation solutions.

## Declaration

In Bash scripting, declaring a function involves specifying the function's name, defining its behavior or code block, and optionally providing parameters that the function can accept. The syntax for declaring a function is straightforward and can be done using either the “`function`” keyword or a concise shorthand notation.

<div style="text-align:center">
    <img src="/assets/bash-in-depth/0015-Functions/Function-Declaration.png" width="450px"/>
</div>

Let’s see next a couple of examples of how to declare a function.

The first example is by using the “`function`” keyword.

```bash
 1 #!/usr/bin/env bash
 2 # Script: function-0001.sh
 3 function my_function() {
 4     echo "Hello from inside the function"
 5 }
 6 my_function
```

When you run the previous script you get the following output.

```txt
$ ./function-0001.sh
Hello from inside the function
```

And the second example is by declaring the function without the “`function`” keyword, like the following script.

```bash
 1 #!/usr/bin/env bash
 2 # Script: function-0002.sh
 3 my_function() {
 4     echo "Hello from inside the function"
 5 }
 6 my_function
```

If you run the last script you will see that it produces the same output as the "`function-0002.sh`" script.

Although both approaches are equivalent, the second one (without the “`function`” keyword) is more portable and because of portability reasons it will be the approach that we will use in this book.

Something to notice is that a function **can NEVER have an empty body**. For example, if we try to execute the following script.

```bash
 1 #!/usr/bin/env bash
 2 # Script: function-0003.sh
 3 error_fn_1() {
 4 }
 5 error_fn_2() {
 6     echo "Some commands"
 7 }
```

If you try to run the previous script you will the following error.

```txt
$ ./function-0003.sh
./function-0003.sh: line 4: syntax error near unexpected token `}'
./function-0003.sh: line 4: `}'
```

The body of a function must contain any combination of commands and statements like the ones we learnt before (`if`, `case`, `for` loop, `while` loop,...). You have to write code that is clean so that you can understand it when you come back to it.

In the next sections we will dive into different details of functions. We will start by comparing declaration vs call of a function.

## Summary


## References


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px;">

