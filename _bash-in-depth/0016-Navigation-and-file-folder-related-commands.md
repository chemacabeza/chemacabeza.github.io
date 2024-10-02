---
layout: chapter
title: "Chapter 16: Navigation and file/folder related commands"
---

# Chapter 16: Navigation and file/folder related commands


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px;">


In Bash, navigating and managing your Linux distribution involves a variety of commands that enable you to explore directories, manipulate files, and search for specific content. At the core of navigating the file system is the `pwd` command, which prints the current working directory, and `cd`, which allows you to move between directories. The `ls` command lists the contents of a directory, helping you view files and subdirectories at your current location. Together, these commands form the basic tools for navigating your Linux system.

Managing directories involves creating and deleting them with commands like `mkdir` (make a directory) and `rmdir` (remove an empty directory). For file creation and manipulation, `touch` creates new empty files, while `mv` moves or renames them, and `rm` removes files. If you want to view the contents of a file, the `cat` command can print it to the terminal. These commands are crucial for file and directory management, helping you control the structure and organization of your Linux environment.

Bash also provides advanced directory navigation through `dirs`, `pushd`, and `popd`. These commands let you manage a stack of directories, allowing you to switch back and forth between locations more efficiently. With `pushd`, you add a directory to the stack and move to it, while `popd` returns you to the previous directory. This is especially useful when working on different parts of a system without losing track of where you've been.

To search for files or content within files, `find` helps you locate files based on various criteria like name or modification time, while `grep` lets you search within files for specific patterns or text. These search and filtering commands are invaluable when working in larger systems or when tracking down specific information across many files. Understanding these core Bash commands equips you to efficiently navigate and manipulate your Linux environment.

In this chapter we will go a bit deeper into the commands we mentioned and will show a little bit how to use them.

Let's begin!

## The `pwd` command and the variables `$PWD` and `$OLDPWD`

The `pwd` command allows you to know what is your current **working directory**, and will print to the terminal window the absolute path to the directory you are in.

```txt
$ pwd
/home/username/Repositories/chapter16
```

The current working directory is not only avaiable via the “`pwd`” command, it's also available through the environment variable “`$PWD`”.

There is another environment variable named “`$OLDPWD`” which contains the path the previous working directory you were at. When you start a session in a terminal, the initial value of this variable will be empty. At the moment of moving to another directory, the value of “`$OLDPWD`” will be the previous directory you were at. 

Let's see with a very simple example<a id="footnote-1-ref" href="#footnote-1" style="font-size:x-small">[1]</a>.

```txt
$ pwd
/home/username/Repositories/chapter16
$ cd script
$ pwd
/home/username/Repositories/chapter16/script
$ echo $OLDPWD
/home/username/Repositories/chapter16
```

In the next section we will play with the “`cd`” command and the “`$HOME`” environment variable.

## `cd` and `$HOME`

In Bash, the “`cd`” (change directory) command is used to navigate between directories. It allows you to move from your current working directory to another location in the filesystem. If you use “`cd`” without specifying a directory, it defaults to the user's home directory, which is represented by the environment variable “`$HOME`”. The “`$HOME`” variable stores the path to the home directory of the current user, and it is where personal files and settings are typically stored.

Here are some typical use cases so that you can use in your terminal window.

When you use “`cd`” without arguments will take you to your home directory, which is stored in the “`$HOME`” environment variable.

When you pass a single argument to the command “`cd`” it will get you to the directory you specified as argument. You can pass an argument that can be either an absolute path or a relative path.

**What is an absolute path?** 

An absolute path in a Linux system is a file or directory path that starts from the root directory (`/`) and provides the full address to the location, regardless of the current working directory. It specifies the complete path to a file or directory from the top of the file system hierarchy. Absolute paths always start with a forward slash (`/`), which signifies the root directory (For example: `/home/username/Documents/file.txt`).

**What is a relative path?**

A relative path in a Linux system is a file or directory path that is relative to the current working directory, rather than starting from the root (`/`). Unlike absolute paths, relative paths do not begin with a `/`. Instead, they reference locations in relation to the directory you are currently in, making it a shorter and more flexible way to navigate the file system.

For example, if you're currently in `/home/username/Documents` and you want to access `file.txt` in the `Projects` subdirectory, the relative path would be `./Projects/file.txt`. Here, `./` refers to the current directory. Similarly, `../` is used to move up one directory level. So, if you want to reference a file located in `/home/username`, the relative path would be `../file.txt`.

## Summary


## References


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px;">
<p id="footnote-1" style="font-size:10pt">
1. We will learn more about the "<code style="font-size:10pt">cd</code>" later in this chapter.<a href="#footnote-1-ref">&#8617;</a>
</p>
