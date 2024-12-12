---
layout: chapter
title: "Chapter 27: Aliases"
---

# Chapter 27: Aliases


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">

## Introduction

Aliases in Bash are shortcuts that allow you to define custom command substitutions or abbreviations for frequently used commands. They simplify and speed up your workflow by reducing the need to type lengthy or complex commands repeatedly. An alias essentially maps a user-defined name to a specific command or sequence of commands, making it an invaluable tool for enhancing efficiency and productivity in the command line.

One of the most common use cases for aliases is to replace frequently used commands with shorter alternatives. For example, instead of typing "`ls -la`" every time, you can create an alias like ll to achieve the same result with minimal effort. Aliases can also include options and arguments, allowing you to tailor commands to your specific needs or preferences. This customization empowers users to create a more streamlined and personalized shell environment.

It’s important to note that aliases are typically session-specific, meaning they exist only for the duration of the current terminal session. However, they can be made permanent by adding them to your shell’s configuration files, such as "`~/.bashrc`" or "`~/.bash_profile`". By leveraging aliases effectively, you can significantly reduce repetitive typing, minimize errors, and make your Bash experience more efficient and enjoyable.

## What is an alias?

An alias in Bash acts as a convenient shortcut for commands, allowing you to assign a short, memorable name to a potentially lengthy sequence of commands, with or without arguments. Aliases are designed to streamline your workflow, making it faster and easier to provide instructions to the shell.

Imagine you frequently need to list the contents of the current directory, sorted by modification time, with human-readable file sizes and color-coded output. While you can achieve this with the following command: "`ls -l -t -h --color`", typing it repeatedly can become tedious over time.

Instead of manually re-entering the command every time, you can create an alias to simplify the process and improve your efficiency. With a single alias, you can transform this verbose command into a quick, easy-to-remember shortcut. Let’s explore how to set one up!

## How to create an alias?

In order to create an alias you need to use the “`alias`” built-in command with the following syntax:

<div style="text-align:center">
    <img src="/assets/bash-in-depth/0027-Aliases/Alias-syntax.png"/>
</div>

Once you execute this command you can start using the “`shortcut`” command as if it was the “`long command to execute`”.

Let’s practice what we’ve just learned with an example!

To start, we’ll add the following line to the "`.bash_aliases`" file located in your home directory:

```bash
# Alias for a complex ls command
alias llc="ls -l -t -h --color"
```

Next, let’s create a script that attempts to use this alias:

```bash
 1 #!/usr/bin/env bash
 2 #Script: aliases-0001.sh
 3 echo -e "Calling new alias\n"
 4 llc
 5 echo -e "\nDone calling new alias"
```

When we run the script, we encounter the following output:

```txt
$ ./aliases-0001.sh
Calling new alias

./aliases-0001.sh: line 4: llc: command not found

Done calling new alias

$
```

The script fails to recognize the alias. Why? By default, Bash aliases are **not expanded in scripts**. But why is this the case?

This behavior stems from how the "`.bashrc`" configuration file works. Within "`.bashrc`", there’s typically a block of code like this:

```bash
...
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
...
```

This code checks whether the "`.bash_aliases`" file exists in your home directory. If it does, the aliases within it are loaded into your **interactive shell session**. However, this setup is only applied to interactive Bash instances, such as when you open a terminal, because "`.bashrc`" checks if the environment variable "`$-`" contains the interactive flag. For non-interactive sessions, like running scripts, aliases defined in "`.bash_aliases`" are not automatically loaded.

To make aliases usable in scripts, you need to explicitly enable their expansion. One way to achieve this is by sourcing the "`.bash_aliases`" file within the script and enabling a special shell option. Here’s how that can be done:

```bash
 1 #!/usr/bin/env bash
 2 #Script: aliases-0002.sh
 3 # Enable alias expansion
 4 shopt -s expand_aliases
 5 # Source the aliases
 6 source ~/.bash_aliases
 7 echo -e "Calling new alias\n"
 8 llc
 9 echo -e "\nDone calling new alias"
```

When you run the previous script you will have the following (or a similar one) output in your terminal window.

```txt
$ ./aliases-0002.sh
Calling new alias

total 8.0K
-rwxrwxr-x 1 username username 207 Dec 12 05:38 <strong style="color: green">aliases-0002.sh</strong>
-rwxrwxr-x 1 username username 114 Dec 12 05:31 <strong style="color: green">aliases-0001.sh</strong>

Done calling new alias
$ 
```

## Summary


## References


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">

