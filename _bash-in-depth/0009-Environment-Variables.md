---
layout: chapter
title: "Chapter 9: Environment Variables"
---

# Chapter 9: Environment Variables
**Environment Variables** are a fundamental concept in operating systems and software development. They are dynamic values that provide essential information about the system environment or influence how processes behave. In Bash, environment variables are used to store configuration settings, system information, and user-defined values. These variables can be accessed by processes and programs to retrieve information or modify behavior.

In this chapter we are going to learn about the commands “`env`”, “`export`” and will revisit the “`declare`” command.

## Command “`env`”

The “`env`” command in Bash is used to display a list of all environment variables. When you run “`env`” in the terminal, it will show a list of key-value pairs representing the current environment variables. For example, you might see variables like “`PATH`”, which defines where the system should look for executable files, and “`HOME`”, which points to the current user's home directory.

The purpose of this command is either:
* Print environment information
* Set/modify environment and execute a command

To print the environment of your current Bash session just invoke it without any arguments.
You will see that it will produce a result similar to the following.

```txt
$ env
SHELL=/bin/bash
LSCOLORS=Gxfxcxdxbxegedabagacad
SESSION_MANAGER=local/prometeus:@/tmp/.ICE-unix/3579,unix/prometeus:/tmp/.ICE-unix/3579
QT_ACCESSIBILITY=1
COLORTERM=truecolor
XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg
LESS=-R
XDG_MENU_PREFIX=gnome-
TERM_PROGRAM_VERSION=3.4
GNOME_DESKTOP_SESSION_ID=this-is-deprecated
TMUX=/tmp/tmux-1000/default,9002,0
JAVA_HOME=/usr/lib/jvm/current
GNOME_SHELL_SESSION_MODE=ubuntu
SSH_AUTH_SOCK=/run/user/1000/keyring/ssh
MEMORY_PRESSURE_WRITE=c29tZSAyMDAwMDAgMjAwMDAwMAA=
XMODIFIERS=@im=ibus
DESKTOP_SESSION=ubuntu
GTK_MODULES=gail:atk-bridge
...
TERM_PROGRAM=tmux
_=/usr/bin/env
```

Which are the environment variables of your current session in Bash.

This command comes with some options that allows you to:
* Add new environment variables temporarily
* Override environment variables temporarily
* Delete environment variables temporarily
* Ignore environment variables temporarily

For all of these cases, if a command is provided it will be executed with the environment variables resulting from the behavior specified. Let’s take a look at examples for all the cases mentioned before.

But first we are going to write a script that prints a few variables (with the “`echo`” command) and to print the full list of environment variables (with the “`env`” command).



