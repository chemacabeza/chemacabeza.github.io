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

If you try to run the previous script you will get the following error.

```txt
$ ./function-0003.sh
./function-0003.sh: line 4: syntax error near unexpected token `}'
./function-0003.sh: line 4: `}'
```

The body of a function must contain any combination of commands and statements like the ones we learnt before (`if`, `case`, `for` loop, `while` loop,...). You have to write code that is clean so that you can understand it when you come back to it.

In the next sections we will dive into different details of functions. We will start by comparing declaration vs call of a function.

## Declaration vs Call

In Bash scripting you cannot call a function unless it has been previously declared. Bear in mind that declaring and calling a function are two different things. You can declare functions that call other functions/commands and it will work as long as at the moment of the execution the commands/functions are available to the script.

Let’s see what happens when you try to invoke a function that has not been declared before the call.

```bash
 1 #!/usr/bin/env bash
 2 # Script: function-0004.sh
 3 my_function # not declared before execution
 4 my_function() {
 5     echo "My function"
 6 }
```

When you run the previous script you will receive the following error.

```txt
$ ./function-0004.sh
./function-0004.sh: line 3: my_function: command not found
```

Now if you swap the order having the function declared before the call of the function, everything works.

```bash
 1 #!/usr/bin/env bash
 2 # Script: function-0005.sh
 3 my_function() {
 4     echo "My function"
 5 }
 6 my_function
```

When you execute the last script you will get a successful execution.

```txt
$ ./function-0005.sh
My function
```

In the following example you will see that there are two functions being declared which are “`my_function_1`” and “`my_function_2`”. You will also notice that “`my_function_1`” invokes “`my_function_2`”, which is declared **after** “`my_function_1`”.

```bash
 1 #!/usr/bin/env bash
 2 # Script: function-0006.sh
 3 my_function_1() {
 4     echo "Inside my_function_1"
 5     my_function_2
 6 }
 7 my_function_2() {
 8     echo "Inside my_function_2"
 9 }
10 my_function_1
```

This is OK because by the time Bash executes “`my_function_1`” all the information needed by it to execute successfully (in our case, declaration of “`my_function_2`”) is available in memory/context.

If you execute the previous script you will get the following output.

```txt
$ ./function-0006.sh
Inside my_function_1
Inside my_function_2
```

In the same way that a Bash script can have variables, a function can have as well variables that are called “*local variables*”. In the next section we will talk about local variables.

## Local variables
What is the “**scope**” of a variable? *The scope of a variable is the context in which it has meaning, in which it has a value that can be referenced*. For example, the scope of a local variable lies only within the function, block of code (`{...}`), or subshell (we will talk later in the book) within which it is defined, while the scope of a global variable is the entire script in which it appears.

A variable declared as “`local`” is one that is visible only within the block of code in which it appears. If a variable within a function is not declared as local, global scope will be by default.

Before a function is called, all variables declared within the function **are invisible outside the body of the function**, not just those explicitly declared as “`local`”.

Let's see how it works with the following example script.

```bash
 1 #!/usr/bin/env bash
 2 # Script: function-0007.sh
 3 custom1() {
 4     local localVar=324
 5     globalVar=123
 6     echo "localVar: $localVar"
 7     echo "Done with $FUNCNAME"
 8 }
 9 echo "Local variable before function: $localVar"
10 echo "Global variable before function: $globalVar"
11 custom1
12 echo "Local variable after function: $localVar"
13 echo "Global variable after function: $globalVar"
```

When you execute the previous script you will have the following output in the terminal window.

```txt
$ ./function-0007.sh
Local variable before function:
Global variable before function:
localVar: 324
Done with custom1
Local variable after function:
Global variable after function: 123
```

So, what is happening in the execution of this script? When the script reaches lines 9 and 10 it just prints the string without the content of the variables. This is because there is no information about the variables at this point.

Then, on line 11 the function “`custom1`” is executed. Inside the function there are 2 variables. The first variable is a local variable named “`localVar`” whose scope is the function itself and it will not be available outside the function. The second variable, named “`globalVar`”, is a global variable (as the “`local`” keyword was not used, global scope is the default one) that, once the function is executed, will be available to the rest of the script.

Once the execution of the function is done you see that only the global variable is present in the output.

## Overriding functions and commands

In Bash you can override the declaration of a function (or a command) by declaring a new function **with the same exact name**.

The way it works is once the functions (or commands) are available inside the script you are working on, you can add a function with the same name as the function (or command) you want to override and from that moment the overriding will work.

To summarize, **the latest declaration wins**.

Let’s see how it works with a couple of examples to show how we can override functions (or commands).

```bash
 1 #!/usr/bin/env bash
 2 # Script: function-0008.sh
 3 my_function_1() {
 4     echo "Inside my_function_1 - 1"
 5 }
 6 my_function_1() { # Will override the previous declaration
 7     echo "Inside the override of my_function_1"
 8 }
 9 my_function_1
```

As you can see in the previous script the function is declared twice, as we mentioned before the second declaration (the latest one) will be the one that will be used.

If you execute the previous script you get the following result.

```txt
$ ./function-0008.sh
Inside the override of my_function_1
```
As you can see from the execution the second declaration of the function “`my_function_1`” is the one that got executed.

Now that we know how to override functions, let’s try to do the same with commands.

In the following example we are going to override the command “`ls`”<a id="footnote-1-ref" href="#footnote-1" style="font-size:x-small">[1]</a>.

```bash
 1 #!/usr/bin/env bash
 2 #Script: function-0009.sh
 3 echo "Before overriding"
 4 echo "##########"
 5 ls  # standard command 'ls'. Will print directory content
 6 echo ""
 7 ls() { # Overriding command 'ls'
 8     echo "Nothing to see here"
 9 }
10 echo "After overriding"
11 echo "##########"
12 ls   # Will print "Nothing to see here"
13 echo ""
```

In the previous script, on line 5, the actual “`ls`” command is used to list the contents of the current folder. Later between lines 7 and 9 we do declare a function with the name “`ls`” to be able to override the command “`ls`”.

When you run the previous script you will get the following result in the terminal window.

```txt
$ ./function-0009.sh
Before overriding
##########
function-0001.sh  function-0003.sh  function-0005.sh  function-0007.sh  function-0009.sh
function-0002.sh  function-0004.sh  function-0006.sh  function-0008.sh

After overriding
##########
Nothing to see here
```

As you can see in the execution of the last script before line 7 of the script the actual “`ls`” command is used. After line 7 the function is the one used.

## Variable associated to a function


## Summary


## References


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px;">
<p id="footnote-1" style="font-size:10pt">
1. The “<code style="font-size:10pt">ls</code>” command is used to list the content of the folders that you pass as arguments, or the current folder if you do not provide any argument.<a href="#footnote-1-ref">&#8617;</a>
</p>

