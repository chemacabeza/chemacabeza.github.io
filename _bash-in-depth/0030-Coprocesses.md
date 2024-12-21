---
layout: chapter
title: "Chapter 30: Coprocesses"
---

# Chapter 30: Coprocesses


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">

## Introduction

Coprocesses in Bash are a powerful mechanism that allows you to execute commands **asynchronously** while maintaining a direct communication channel with them. Introduced in Bash version 4, coprocesses enable a script to run a background command or pipeline while interacting with it through dedicated input and output streams. This feature is particularly useful when you need to exchange data dynamically with a running command without blocking your script.

A coprocess is essentially a subprocess that operates in parallel to the main Bash script. When a coprocess is launched, Bash creates two file descriptors: one for writing to the coprocess's standard input and another for reading from its standard output. This setup allows seamless data exchange between the script and the coprocess, making it an efficient tool for managing complex workflows or integrating external utilities.

## What is a Coprocess?

A coprocess is an asynchronous process created using the "`coproc`" built-in command in Bash.

When a command is executed as a coprocess, it spawns a new process that runs in parallel with the main script or shell. This process is linked to the invoking shell or script through file descriptors, enabling seamless interaction between the two. Specifically, these file descriptors provide access to the coprocess’s standard input and standard output, allowing the main shell or script to send and receive data dynamically.

Curious about how to create a coprocess? That’s exactly what the next section will cover!

## How to create a Coprocess?

To create a coprocess in Bash, you use the "`coproc` command. Its syntax is straightforward:

```bash
    coproc [NAME] command [redirections]
```

Executing this command has two significant outcomes. First, the specified command runs asynchronously in a child process, and its standard input and standard output are linked to an array named "`NAME`". If no name is specified, the default name "`COPROC`"<a id="footnote-1-ref" href="#footnote-1" style="font-size:x-small">[1]</a> is used. This array consists of two positions:
* "`NAME[0]`" or "`COPROC[0]`:: Contains the file descriptor for reading the standard output of the coprocess.
* "`NAME[1]`" or "`COPROC[1]`": Contains the file descriptor for writing to the standard input of the coprocess.

The second outcome is the creation of an environment variable to store the PID of the coprocess. If a name is provided, the variable will be "`NAME_PID`"; otherwise, it defaults to "`COPROC_PID`".

Visually, a coprocess links the main script (or calling shell) with the child process via file descriptors. These descriptors enable the script to read data from the coprocess’s output and send data to its input seamlessly. If you use a custom name with "`coproc`", the array and PID variable are adjusted accordingly to reflect the specified name.

<div style="text-align:center">
    <img src="/assets/bash-in-depth/0030-Coprocesses/Coprocess-Default.png"/>
</div>

or 

<div style="text-align:center">
    <img src="/assets/bash-in-depth/0030-Coprocesses/Coprocess-With-Name.png"/>
</div>

in the case of specifying the name for the file descriptor array.

For a practical example, imagine we create a folder with some files and write a Bash script that sets up a coprocess to interact with the Bash shell. Using this coprocess, the script sends commands for execution and processes the results dynamically. Preparing the folder and running the script demonstrates the power and simplicity of coprocesses in real-world scenarios.

<pre>
$ mkdir coprocess

$ cd coprocess

$ touch{1..10}.txt

$ ls
file10.txt  file1.txt  file2.txt  file3.txt  file4.txt  file5.txt  file6.txt  file7.txt  file8.txt  file9.txt

$
</pre>

Now let's write a Bash script to practice our Coprocesses skill:

```bash
 1 #!/usr/bin/env bash
 2 #Script: coprocess-0001.sh
 3 # Creating the coprocess
 4 coproc MY_BASH { bash; }
 5 echo "ls -l coprocess; echo \"-END-\"" >&"${MY_BASH[1]}"
 6 is_done=false
 7 while [[ "$is_done" != "true" ]]; do
 8     read var <&"${MY_BASH[0]}"
 9     if [[ "$var" == "-END-" ]]; then
10         is_done="true"
11     else
12         echo $var
13     fi
14 done
15 echo "DONE"
```

Which produces the following output in your terminal window:

<pre>
$ ./coprocess-0001.sh
total 0
-rw-rw-r-- 1 username username 0 Dec 21 07:16 file10.txt
-rw-rw-r-- 1 username username 0 Dec 21 07:16 file1.txt
-rw-rw-r-- 1 username username 0 Dec 21 07:16 file2.txt
-rw-rw-r-- 1 username username 0 Dec 21 07:16 file3.txt
-rw-rw-r-- 1 username username 0 Dec 21 07:16 file4.txt
-rw-rw-r-- 1 username username 0 Dec 21 07:16 file5.txt
-rw-rw-r-- 1 username username 0 Dec 21 07:16 file6.txt
-rw-rw-r-- 1 username username 0 Dec 21 07:16 file7.txt
-rw-rw-r-- 1 username username 0 Dec 21 07:16 file8.txt
-rw-rw-r-- 1 username username 0 Dec 21 07:16 file9.txt
DONE

$ 
</pre>

Let's go line by line analyzing the script we just wrote.

In line 4, the "`coproc`" command is used to create a coprocess running a new Bash shell. This setup enables the script to send commands directly to the coprocess for execution.

In line 5, two commands are sent to the Bash coprocess. The first command, "`ls -l coprocess`", lists the files in the "coprocess" directory. The second command, "`echo \"-END-\"`", serves as a delimiter. Why is this delimiter necessary? It provides a clear marker indicating when the script should stop reading output from the coprocess. Without such a convention, the script wouldn't know when to stop waiting for data, potentially causing it to hang indefinitely while trying to read more output.

Between lines 7 and 14, a "`while`" loop handles the interaction with the coprocess. In line 8, it reads the coprocess's standard output. Line 9 checks whether the delimiter ("`-END-`") has been encountered, signaling that the script should stop reading further output. If the delimiter hasn’t been reached, line 12 ensures that the output is printed back to the script’s standard output.

This example demonstrates a simple but effective way to leverage coprocesses in Bash, showcasing how they can facilitate asynchronous interactions and process control.

Let’s see another example where we will interact with a “`python`” coprocess. In the following example we will create a coprocess for python and we will send commands to it:

```bash
 1 #!/usr/bin/env bash
 2 #Script: coprocess-0002.sh
 3 # Creating the coprocess
 4 coproc MY_PYTHON { python -i; } 2>/dev/null
 5 # Priting the PID of the coprocess
 6 echo "Coprocess PID: $MY_PYTHON_PID"
 7 # Defining functions
 8 echo $'def my_function():\n    print("Hello from a function")\n' >&"${MY_PYTHON[1]}"
 9 # Calling the function defined
10 echo $'my_function()\n' >&"${MY_PYTHON[1]}"
11 echo $'my_function()\n' >&"${MY_PYTHON[1]}"
12 echo $'print("-END-")\n' >&"${MY_PYTHON[1]}"
13 # Loop that processes the output of the coprocess
14 is_done=false
15 while [[ "$is_done" != "true" ]]; do
16   read var <&"${MY_PYTHON[0]}"
17   if [[ $var == "-END-" ]]; then
18      is_done="true"
19   else
20      echo $var
21   fi
22 done
```

As in previous examples, line 4 demonstrates the creation of a named coprocess. Notably, the "`-i`"<a id="footnote-2-ref" href="#footnote-2" style="font-size:x-small">[2]</a> flag is used here to enable Python's interactive mode, allowing the coprocess to accept and execute commands interactively.

Additionally, you’ll observe that standard error is redirected to "`/dev/null`". This is done to suppress Python's startup messages, such as version details and compiler information, which are irrelevant in this context. By redirecting this output, we ensure a cleaner and more focused result.

In line 6, the script prints the PID of the coprocess using the environment variable "`MY_PYTHON_PID`", which is automatically generated by the "`coproc`" command.

In line 8, a Python function is sent to the coprocess. Later, in lines 10 and 11, this function is invoked, demonstrating how the coprocess can execute predefined logic.

Finally, in line 12, a delimiter is sent to signal the end of the output from the coprocess. Lines 14 through 22 then use the same approach as earlier examples to read and process the coprocess's output, ensuring proper handling of the data generated.

Now if we run the "`coprocess-0002.sh`" script we will see something like the following in our terminal window:

<pre>
$ ./coprocess-0002.sh
Coprocess PID: 2021770
Hello from a function
Hello from a function

$ 
</pre>

If we had not added the redirection “`2>/dev/null`” to the “`coproc`” command our output would have been something like the following:

<pre>
$ ./coprocess-0002.sh
Coprocess PID: 2022737
Python 3.11.0 (main, Jun 22 2024, 21:15:14) [GCC 13.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> ... ... >>> >>> >>> >>> >>> >>> >>> Hello from a function
Hello from a function

$ 
</pre>

As you can see this is more difficult to read than the previous execution with the standard error redirection.

## Why is Asynchronous Tricky?



## Summary


## References


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">
<p id="footnote-1" style="font-size:10pt">
1. The advantage of using a “<code style="font-size:9pt">NAME</code>” for the coprocess is that you can create several coprocesses with different names. If you do not specify a name, you can only create a single coprocess which will be associated with the array named “<code style="font-size:9pt">COPROC</code>”.<a href="#footnote-1-ref">&#8617;</a>
</p>
<p id="footnote-2" style="font-size:10pt">
2. More about this Python flag in the following link <a href="https://www.python-engineer.com/posts/python-interactive-mode/">https://www.python-engineer.com/posts/python-interactive-mode/</a>. <a href="#footnote-2-ref">&#8617;</a>
</p>





