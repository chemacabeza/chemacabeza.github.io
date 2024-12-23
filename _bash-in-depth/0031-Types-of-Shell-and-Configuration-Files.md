---
layout: chapter
title: "Chapter 31: Types of Shell & Configuration Files"
---

# Chapter 31: Types of Shell & Configuration Files


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">

## Introduction

Bash includes a collection of configuration files that are executed before the shell becomes available for use. However, the specific configuration files that are applied depend on the type of shell you are using, whether it’s a login shell, interactive shell, or a non-interactive shell. Each type follows a different set of rules for loading configuration files.

In the following sections, we’ll dive into the available Bash configuration files, their purposes, and how they relate to different shell types. By the end, you’ll have a clear understanding of how these files work together to customize your Bash environment. Let’s get started!

## Available configuration files

Much like many other programs, Bash uses configuration files (or “config files”) to allow users to set parameters and define initial settings. For instance, imagine you’ve written a custom function to perform a specific task. While you can define and use the function directly in your current terminal session, it will be lost once the session ends. Even if the session remains open, a new terminal window won’t have access to the function unless you redefine it.

To make the function persist across sessions and be accessible in all terminal windows, you need to include it in a configuration file. These files are automatically loaded when a terminal starts, ensuring that your customizations, like functions, aliases, and environment variables, are always ready to use.

### "`/etc/profile`"

The "`/etc/profile`" file contains system-wide environment settings and startup scripts for Linux. Its primary purpose is to configure global settings that apply to all users on the system. Typically, this script handles tasks such as:
* Configuring the default command-line prompt
* Defining the "`PATH`" environment variable
* Setting user limits

This script is executed only for login shells and does not run when a non-login shell or script is executed. A key part of "`/etc/profile`" involves checking for the existence of the "`/etc/profile.d`" directory, which allows for additional customization.

System administrators can modify "`/etc/profile`" to tailor the system's behavior. Best practices suggest:
* Using /etc/profile for small, straightforward changes
* Creating separate scripts in the "`/etc/profile.d`" directory for larger or application-specific configurations

The scripts in "`/etc/profile.d`" are "sourced"<a id="footnote-1-ref" href="#footnote-1" style="font-size:x-small">[1]</a> by "`/etc/profile`". This means that "`/etc/profile`" contains logic to identify and execute these additional scripts in a login shell. For instance, a typical snippet in "`/etc/profile`" might look like this:

```bash
 1 # /etc/profile: system-wide .profile file for the Bourne shell (sh(1))
 2 # and Bourne compatible shells (bash(1), ksh(1), ash(1), ...).
 3 if [ "${PS1-}" ]; then
 4   if [ "${BASH-}" ] && [ "$BASH" != "/bin/sh" ]; then
 5     # The file bash.bashrc already sets the default PS1.
 6     # PS1='\h:\w\$ '
 7     if [ -f /etc/bash.bashrc ]; then
 8       . /etc/bash.bashrc
 9     fi
10   else
11     if [ "$(id -u)" -eq 0 ]; then
12       PS1='# '
13     else
14       PS1='$ '
15     fi
16   fi
17 fi
18 if [ -d /etc/profile.d ]; then
19   for i in /etc/profile.d/*.sh; do
20     if [ -r $i ]; then
21       . $i
22     fi
23   done
24   unset i
25 fi
```

Here’s how this code works:
* **Line 18**: Verifies if the "`/etc/profile.d`" directory exists.
* **Line 19**: Iterates over each script in the directory.
* **Line 20**: Checks if the script is readable.
* **Line 21**: Sources the script to execute its content.

This structure ensures modularity and makes it easier to manage system-wide configurations by allowing distinct scripts for specific tasks or applications.

### "`/etc/bashrc`" or "`/etc/bash.bashrc`"

The "`/etc/bashrc`" file is a system-wide configuration file that defines functions, aliases, and other settings that apply to all users on the system. It provides a centralized way to manage configurations for Bash shells.

You may not encounter frequent references to the "`/etc/bash.bashrc`" file, as its origins lie in Debian<a id="footnote-2-ref" href="#footnote-2" style="font-size:x-small">[2]</a>. This special feature was later adopted by other systems, such as Ubuntu<a id="footnote-3-ref" href="#footnote-3" style="font-size:x-small">[3]</a>. Its use, while specific to certain distributions, serves a similar purpose: offering a global configuration for Bash environments.

In one of Debian’s README files<a id="footnote-4-ref" href="#footnote-4" style="font-size:x-small">[4]</a>, you might find a description similar to the following:

> `5.` What is /etc/bash.bashrc? It doesn't seem to be documented.
>
>   The Debian version of bash is compiled with a special option
>   (-DSYS_BASHRC) that makes bash read /etc/bash.bashrc before ~/.bashrc
>   for interactive non-login shells. So, on Debian systems,
>   /etc/bash.bashrc is to ~/.bashrc as /etc/profile is to
>   ~/.bash_profile.

## Summary


## References


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">
<p id="footnote-1" style="font-size:10pt">
1. To “source” a script into the current script or shell you need to use the dot character (‘.’) followed by the name of the script.<a href="#footnote-1-ref">&#8617;</a>
</p>
<p id="footnote-2" style="font-size:10pt">
2. <a href="https://www.debian.org/">https://www.debian.org/</a><a href="#footnote-2-ref">&#8617;</a>
</p>
<p id="footnote-3" style="font-size:10pt">
3. <a href="https://ubuntu.com/">https://ubuntu.com/</a><a href="#footnote-3-ref">&#8617;</a>
</p>
<p id="footnote-4" style="font-size:10pt">
4. <a href="https://sources.debian.org/src/bash/4.3-11/debian/README/">https://sources.debian.org/src/bash/4.3-11/debian/README/</a><a href="#footnote-4-ref">&#8617;</a>
</p>

