---
layout: chapter
title: "Chapter 32: Customizing The Prompt"
---

# Chapter 32: Customizing The Prompt


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">

## Introduction

The **prompt** in Bash is the text displayed in your terminal, signaling that the shell is ready to receive and execute commands. It is the interface between you and the Bash shell, making it a fundamental part of the user experience.

By default, the prompt in Bash provides useful information such as the current user, hostname, and working directory. However, it is highly customizable, allowing you to tailor it to your workflow, preferences, or environment. A well-designed prompt can enhance productivity by displaying relevant details, such as the current Git branch, exit status of the last command, or even system resource usage.

The main components of the prompt in Bash are:

1. **Primary Prompt** (`PS1`): This is the most commonly seen prompt, used for interactive shells to signal that Bash is ready for a command. For instance:

```bash
user@hostname:~$
```

2. **Secondary Prompt** (`PS2`): This prompt appears when more input is needed, such as when a command spans multiple lines. The default is "`>`".

3. **Error Prompt** (`PS3`): Used in the "`select`" loop when Bash requires input for choices.

4. **Debug Prompt** (`PS4`): Used when the "`-x`" debugging option is enabled, with the default value being "`+`".

## Why Customize the Prompt?

Customizing the prompt is not just about aesthetics; it can improve your workflow by:
* Displaying critical system information at a glance.
* Indicating the state of the shell or current command.
* Reducing errors by showing contextual information, like the working directory or version control status.

**Example of a Custom Prompt**

Here’s an example of a more informative prompt:

```bash
PS1="\u@\h:\w ($(git branch 2>/dev/null | grep '*' | sed "s/* //")) \$ "
```

This prompt includes:
* "`\u`": Current username.
* "`\h`": Hostname of the system.
* "`\w`": Current working directory.
* Git branch name if inside a Git repository.

The result might look like this:

<pre style="background-color:#2d2d37">
<span style="color:white;">user</span><span style="color:pink;">@hostname</span><span style="color:red;">:~/projects/myapp</span> <span style="color:white;">(main)</span> <span style="color:white;">$</span>
</pre>

## How many prompts are there?

Bash provides **six** different types of prompts, each serving a distinct purpose:
1. "`PROMPT_COMMAND`": This variable holds code that is executed just before the "`PS1`" prompt is displayed.
2. "`PS0`": Known as **P**rompt **S**tring **0**, it behaves similarly to PS1 but appears after each command and before the command’s output.
3. "`PS1`": **P**rompt **S**tring **1** is the default interactive prompt we discussed earlier. It is the primary prompt displayed when Bash is ready for user input.
4. "`PS2`": **P**rompt **S**tring **2**, the continuation prompt, appears when a command is too long and is split across multiple lines using the backslash "`\`". By default, it is set to "`> `".
5. "`PS3`": **P**rompt **S**tring **3** is used within Bash scripts by the select command for generating a menu. The default value is "`#? `".
6. "`PS4`": **P**rompt **S**tring **4** is displayed when a script is run in debug mode, prefixed with "`+`" by default.

In this chapter, we’ll primarily focus on configuring the "`PS1`" prompt. However, the principles we explore can also be applied to customize the other prompt strings.

Lastly, it’s worth noting that the prompts "`PS1`", "`PS2`", "`PS3`", and "`PS4`" were briefly introduced in the Introduction section of this chapter, providing a foundational context for their usage.


## "`PROMPT_COMMAND`"

Before diving into how to configure the Prompt Strings, let's take a closer look at the "`PROMPT_COMMAND`" variable.

This variable can hold a string (or an array of strings) that Bash interprets as a command to execute just before displaying the "`PS1`" prompt. If you're imagining that this means "`PROMPT_COMMAND`" allows you to execute code as a string, you're absolutely correct.

Why not explore a couple of examples to see it in action? Let’s get started!

### String Value

Let’s explore a simple example to understand how the "`PROMPT_COMMAND`" variable works. Imagine you want your terminal to display the current time right before showing the "`PS1`" prompt. To achieve this, you can set the "`PROMPT_COMMAND`" variable to a command that combines echo and date using command substitution.

Here’s how it looks:

```shell
$ PROMPT_COMMAND="echo -n \[\$(date +%H:%M:%S)\]\ "
                          ^ ^                 ^ ^
```

Notice the backslashes used to escape certain characters. These escapes are crucial; without them, the command will simply evaluate to a static string containing the timestamp at the moment you assign the variable. In other words, the "`date`" command will NOT execute dynamically.

If the variable is correctly set with the escape characters, your prompt will display the following:

```shell
$ PROMPT_COMMAND="echo -n \[\$(date +%H:%M:%S)\]\ "
[06:25:43] $
^^^^^^^^^^^
This is the new addition
```

The "`PROMPT_COMMAND`" variable is incredibly flexible—it’s not limited to echo commands. You can include any valid Bash commands or even scripts to customize your prompt further. Let’s consider a more advanced example.

Suppose you want your prompt to indicate whether the last executed command was successful. You can achieve this by assigning a small Bash script to "`PROMPT_COMMAND`":

```bash
 1 #!/usr/bin/env bash
 2 #Script: prompt-0001.sh
 3 PROMPT_COMMAND='
 4 result=$?
 5 if [[ "$result" != "0" ]]; then
 6 echo -n "[Last command: KO] "
 7 else
 8 echo -n "[Last command: OK] "
 9 fi'
```

This script captures the value of the "`$?`" variable (which holds the exit status of the last command). Based on this value, it prints a message indicating whether the previous command succeeded or failed, just before displaying the "`PS1`" prompt.

```shell
$ source prompt-0001.sh
[Last command: OK] $ ls /tmp &>/dev/null
[Last command: OK] $ ls non-existing-file &>/dev/null
[Last command: KO] $
```

In this example:
1. After sourcing the script, the prompt starts showing the result of the last command.
2. Running "`ls /tmp`" (with output redirected to "`/dev/null`") succeeds, so the prompt displays "`[Last command: OK]`".
3. Trying to list a non-existent file results in "`[Last command: KO]`" because the command fails, as expected.

This demonstrates the power and versatility of "`PROMPT_COMMAND`"—it can enrich your terminal experience in countless ways!


### String Array Value

As previously mentioned, the "`PROMPT_COMMAND`" variable can also hold an array of strings, where each element is a command or a small program. These commands are executed sequentially before the main prompt ("`PS1`") is displayed. Let’s look at an example:

```bash
 1 #!/usr/bin/env bash
 2 #Script: prompt-0002.sh
 3 PROMPT_COMMAND=(
 4 "echo -n \"[\$(date +%Y/%m/%d-%H:%M:%S)] \""
 5 'result=$?
 6 if [[ "$result" != "0" ]]; then
 7 echo -n "[Last command: KO] "
 8 else
 9 echo -n "[Last command: OK] "
10 fi'
11 "echo \"\""
12 )
```

In this example, the "`PROMPT_COMMAND`" variable is assigned an array containing three elements, each serving a specific purpose:
1. **Element #1**: Displays the current date and time in the format "`[YYYY/mm/dd-HH:MM:SS]`". For instance, "`[2023/02/28-06:29:00]`".
2. **Element #2**: Indicates whether the last command executed successfully ("`OK`") or failed ("`KO`"), as demonstrated in the previous section.
3. **Element #3**: Adds a new line to enhance readability and improve formatting.

Once the script is sourced into the current terminal session, the prompt behaves as follows:

```shell
$ source prompt-0002.sh
[2023/02/28-06:29:36] [Last command: OK]
$ ls /tmp &>/dev/null
[2023/02/28-06:30:01] [Last command: OK]
$ ls does-not-exist
[2023/02/28-06:30:11] [Last command: KO]
$
```

As shown, each command in the array is executed in the order it is defined, enriching the prompt with useful and dynamically updated information.

## Prompt String 1


## Summary


## References

<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">

