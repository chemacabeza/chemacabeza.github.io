---
layout: chapter
title: "Chapter 33: Programmable Completion"
---

# Chapter 33: Programmable Completion


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">

## Introduction

Programmable completion, or autocompletion, is a powerful feature that enhances command-line efficiency by suggesting or completing commands, arguments, filenames, and other elements as you type. It simplifies interactions with the terminal, helping users enter commands quickly and accurately while minimizing manual effort.

This feature offers several key benefits. It saves time by reducing the need to type long commands or filenames in full; a few characters followed by pressing the Tab key allow Bash to complete the input. Autocompletion also helps prevent errors by suggesting valid options based on the current context, ensuring accuracy in command entry. Furthermore, it enhances discoverability by displaying a list of available commands, options, or files, making it easier to explore and utilize system resources effectively.

Autocompletion is particularly useful for working with command parameters, as it can display available flags, options, and arguments for specific commands. This aids users in constructing precise and meaningful commands without relying on memory alone. It’s also invaluable when developing shell scripts, suggesting valid command names, system variables, or file paths to ensure accuracy and streamline the scripting process. For complex commands involving multiple arguments or filenames, autocompletion reduces both the effort required and the likelihood of mistakes.

In essence, Bash autocompletion boosts productivity and precision on the command line. By providing an intuitive and efficient way to interact with the shell, it simplifies command entry, aids discovery, and makes working in the terminal more seamless.

In this chapter, we will explore the environment variables and built-in commands that enable programmable completion for various commands of our choosing. To apply what we learn, we will create a dummy script designed to display the arguments provided to it, and then use that script as the basis for building a programmable completion script. Let’s begin with an overview of the relevant environment variables.


## Programmable completion environment variables

The following sections provide detailed descriptions of the key variables used in scripts to enable programmable completion in Bash.

### "`COMP_CWORD`"
The "`COMP_CWORD`" variable serves as an index within the "`COMP_WORDS`" array, identifying the word currently being autocompleted. This variable functions as an input variable and is accessible only within shell functions invoked by Bash’s programmable completion framework.

### "`COMP_KEY`"

The "`COMP_KEY`" variable holds the ASCII code of the key pressed to trigger the current completion function. For most cases, this value will be **9**, which represents the Tab key. Like other input variables, it is limited to use within shell functions called by Bash’s completion mechanisms.

### "`COMP_LINE`"

The "`COMP_LINE`" variable contains the entire command line that is being autocompleted. It provides the full context for the autocomplete process and is also an input variable available exclusively within Bash’s completion-related shell functions.

### "`COMP_POINT`"

The "`COMP_POINT`" variable represents the cursor’s position within the command line. If the cursor is at the end of the command, its value will equal the length of "`COMP_LINE`". This variable is crucial for determining where completion should occur and, like others, is an input variable used only in Bash’s completion facilities.

### "`COMP_TYPE`"

The "`COMP_TYPE`" variable indicates the type of completion that triggered the function call, represented by an integer ASCII code. The possible values include:
* **9 (Tab)**: Normal completion
* **33** (“`!`”): Lists alternatives for partially completed words
* **37** (“`%`”): Indicates menu completion
* **63** (“`?`”): Lists completions after multiple Tab presses
* **64** (“`@`”): Lists completions for unmodified words
This variable is an input value, confined to completion-specific shell functions.

### "`COMP_WORDBREAKS`"

The "`COMP_WORDBREAKS`" variable defines the characters considered as word separators by the "`readline`" library during completion. Modifying this variable removes its special properties, **so it is recommended not to alter it**.

### "`COMPREPLY`"

The "`COMPREPLY`" variable is an array used to store suggestions for autocompletion. These suggestions are generated within a shell function and read by Bash to present possible completions to the user. Unlike the input variables above, "`COMPREPLY`" serves as an output variable.

### "`COMP_WORDS`"

The "`COMP_WORDS`" variable is an array containing the individual words of the current command line. It acts as an input variable, providing the components of the command for processing. Like other input variables, it is only accessible within Bash’s completion-related functions.

With an understanding of these variables, the next step is to explore the built-in commands that work in conjunction with them to create and manage programmable completion functionality effectively.

## Summary


## References

<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">
