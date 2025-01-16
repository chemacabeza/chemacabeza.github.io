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
[2024/02/28-06:29:36] [Last command: OK]
$ ls /tmp &>/dev/null
[2024/02/28-06:30:01] [Last command: OK]
$ ls does-not-exist
[2024/02/28-06:30:11] [Last command: KO]
$
```

As shown, each command in the array is executed in the order it is defined, enriching the prompt with useful and dynamically updated information.

## Prompt String 1

The environment variable "`PS1`" is used to define the format of your command prompt. By assigning it a specific string, you can customize how your prompt looks.

For example, let’s create a simple prompt that displays the text "`---Introduce Command--->`" like this:

```shell
$ PS1="---Introduce Command---> "
---Introduce Command---> 
```

In the first line of the example, we assign the value "`---Introduce Command--->`" to the "`PS1`" variable. From that point on, the prompt changes to display this string instead of the default prompt ("`$`").

While this is a basic example, the possibilities with "`PS1`" are virtually endless!

The "`PS1`" variable supports special characters that can dynamically display information such as the current date, username, hostname, and more. You can even customize colors, including both the background and font. And if you want to go further, you can introduce custom functionality by defining your own functions and incorporating them into the prompt using command substitution.

In the upcoming sections, we’ll explore:
* Special characters
* Changing colors (background and font)
* Using command substitution to enhance your prompt with additional information

### Special Characters

The Bash shell is able to recognize special characters that provide information about your system. The following table provides an overview of the special characters available to you:

| Special Character | Description |
| :----: | :----- |
| `\A` | The current time with the format `HH:MM` and 24-hour. For example: `15:23` |
| `\a` | This character tells the shell to play a beep sound through the speakers. | 
| `\D{format}` | This will show the current date in the specified format given. “format” should be replaced by any format code that “strftime”<a id="footnote-1-ref" href="#footnote-1" style="font-size:x-small">[1]</a> admits. For example, “`\D{%d.%m.%Y}`” will display the day of the month followed by the month followed by the year (e.g: “`08.02.2012`”) |
| `\d` | Shows the current date with the format “Weekday Month Day-of-the-month”. For example: '`Tue Feb 21`' | 
| `\e` | An ASCII escape character. This is used to print special characters. It is equivalent to “`\033`” but in octal representation. |
| `\H` | The full hostname |
| `\h` | The hostname up to the first `.` dot. |
| `\j` | The number of jobs currently managed by the shell. |
| `\l` | The basename of the shell's terminal device. |
| `\n` | A newline character. |
| `\r` | A carriage return character. | 
| `\s` | The name of the shell, the basename of `$0`. For example: “`-bash`” (in the case of a **login shell**) or ”`bash`” (in the case of a **non-login shell**) |
| `\T` | The current time in 12-hour "`HH:MM:SS`" format. | 
| `\t` | The current time in 24-hour "`HH:MM:SS`" format. |
| `\u` | The username of the current user. |
| `\V` | The release of bash, with the patch level. For example: “`5.1.16`”. |
| `\v` | The version of Bash. For example: “`5.1`”. |
| `\W` | The current working directory name (rather than the entire path as is used for "`\w`"). For example: “`Pictures`”. |
| `\w` | The current working directory, with `$HOME` abbreviated with a “`~`” tilde symbol. For example: “`~/Pictures`”. |
| `\@` | The current time in 12-hour am/pm format. |
| `\!` | The history number of this command. |
| `\#` | The command number of this command. | 
| `\$` | The `$` dollar symbol, unless we are a super-user, in which case the `#` hash symbol is used. |
| `\nnn` | The character corresponding to the octal number “`nnn`”, used to show special characters. |
| `\\` | A "`\`" backslash character. |
| `\[` | The 'start of non-printing characters' sequence. |
| `\]` | The 'end of non-printing characters' sequence. |

Now we are going to use some of these special characters to create a custom prompt string.

Our custom format is going to contain:
* Time with hours minutes and seconds
* The username of the current user
* The full hostname
* The “`@`” character
* The path of the current working directory
* The dollar sign before the command to be run
* A space

As this is quite a lot of information to be shown in one single line along with the command to be run, we will put the dollar sign and the command in a separate line.

To accomplish this we are going to create a small script that we will source into our shell.

```bash
 1 #!/usr/bin/env bash
 2 #Script: prompt-0003.sh
 3 PS1="\n\t \u@\H \w\n\$ "
```

<pre>
$ source prompt-0003.sh

06:23:25 username@hostname ~/Repositories/bash_in_depth/_bash-in-depth/chapters/0032-Customizing-The-Prompt/script
$ 
</pre>

### How does PS1 get evaluated?

When Bash reads the value of the PS1 variable from its declaration (e.g., in the "`.bashrc`" file), it interprets the string and processes it to produce the desired result. This involves several steps of decoding and evaluation.

Let’s explore this process with a specific example. Suppose the "`PS1`" variable is set as follows:

```bash
PS1="\u@\h:\W \$(echo "hello there!")\$ "
              ^______________________^
              Note the use of escape characters
```

When Bash sources the file containing this line, it initially interprets the string and produces the following intermediate value:

```txt
\u@\h:\W $(echo "hello there!")$
```

Notice that the escape characters preceding the dollar signs are removed. This interpreted string will then be evaluated by Bash every time the prompt is displayed. Before the prompt appears, Bash performs a series of substitutions to produce the final output.

<strong>Step 1: Replace Special Characters</strong>

Bash begins by replacing the special placeholders in the string:
* "`\u`" is replaced with the current username.
* "`\h`" is replaced with the hostname, truncated at the first dot.
* "`\W`" is replaced with the name of the current working directory.

For example, this could result in:

```txt
username@hostname:directory $(echo "hello there!")$
```

<strong>Step 2: Perform Expansions and Substitutions</strong>
Next, Bash processes the remaining parts of the string:
* Parameter expansion
* Command substitution
* Arithmetic expansion
* Quote removal

In this example, "`$(echo "hello there!")`" is evaluated, resulting in:

```txt
username@hostname:directory hello there!$
```

This final output represents the customized prompt displayed in the terminal.

<strong>Summary of the Process</strong>

To summarize, here’s what happens step by step:
1. Bash sources the file containing the "`PS1`" variable.
2. It decodes the value of "`PS1`".
3. Each time the prompt is about to be displayed, Bash performs the following:
    * **Special character replacement** (e.g., "`\u`", "`\h`", "`\W`").
    * **Expansions and substitutions**, including parameter expansion, command substitution, arithmetic expansion, and quote removal.
4. Finally, the processed value of "`PS1`" is printed as the prompt.

<strong>Beyond Special Characters</strong>

In addition to special placeholders, you can further customize your prompt by adding colors, changing the background, or even including emojis. The next section will explore these advanced customizations.

## Summary


## References

<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">
<p id="footnote-1" style="font-size:10pt">
1. For more information about the available formats consult “<code style="font-size:10pt">man strftime</code>” in your command line.<a href="#footnote-1-ref">&#8617;</a>
</p>

