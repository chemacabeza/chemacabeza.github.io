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

In this chapter we will go a bit deeper into the commands we mentioned and will show a little bit how to use them. For a full guide of the commands please refer to the manual page<a id="footnote-2-ref" href="#footnote-2" style="font-size:x-small">[2]</a>.

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
$ echo $PWD
/home/username/Repositories/chapter16/script
$ echo $OLDPWD
/home/username/Repositories/chapter16
```

In the next section we will play with the “`cd`” command and the “`$HOME`” environment variable.

## The `cd` command and the `$HOME` environment variable

In Bash, the “`cd`” (change directory) command is used to navigate between directories. It allows you to move from your current working directory to another location in the filesystem. If you use “`cd`” without specifying a directory, it defaults to the user's home directory, which is represented by the environment variable “`$HOME`”. The “`$HOME`” variable stores the path to the home directory of the current user, and it is where personal files and settings are typically stored.

Here are some typical use cases so that you can use in your terminal window.

When you use “`cd`” without arguments will take you to your home directory, which is stored in the “`$HOME`” environment variable.

When you pass a single argument to the command “`cd`” it will get you to the directory you specified as argument. You can pass an argument that can be either an absolute path or a relative path.

**What is an absolute path?** 

An absolute path in a Linux system is a file or directory path that starts from the root directory (`/`) and provides the full address to the location, regardless of the current working directory. It specifies the complete path to a file or directory from the top of the file system hierarchy. Absolute paths always start with a forward slash (`/`), which signifies the root directory (For example: `/home/username/Documents/file.txt`).

**What is a relative path?**

A relative path in a Linux system is a file or directory path that is relative to the current working directory, rather than starting from the root (`/`). Unlike absolute paths, relative paths do not begin with a `/`. Instead, they reference locations in relation to the directory you are currently in, making it a shorter and more flexible way to navigate the file system.

For example, if you're currently in `/home/username/Documents` and you want to access `file.txt` in the `Projects` subdirectory, the relative path would be `./Projects/file.txt`. Here, `./` refers to the current directory. Similarly, `../` is used to move up one directory level. So, if you want to reference a file located in `/home/username`, the relative path would be `../file.txt`.

Another use case of the “`cd`” command is when you provide as argument two points like “`cd ..`”. This is telling Bash to move up one level in the directory hierarchy.

The last use case I can come up with is providing a dash to the “`cd`” command like “`cd -`”, using this will return you to the previous directory you were on.

Now that we played a bit with the “`cd`” command we are going to play with another command that will be one of the commands we will use the most, which is the “`ls`” command.

## The `ls` command

The “`ls`” command is used to list the contents of the current directory or to list the arguments passed to it. This command is very rich in terms of flags. For a full explanation, I do recommend consulting “`man ls`” in your terminal. 

Here, we will focus on the flags I do consider more interesting. The following table contains a few flags and description for each one of them.

| Flag | Description |
| :----: | :---- |
| `-1` | (The numeric digit one) Force output to be one entry per line. This is the default when output is not to a terminal. | 
| `-A` | List all entries except for `.` and `..`. |
| `-a` | List all directories including those directories whose names begin with a dot (`.`). |
| `-C` | Force multi-column output; this is the default when output is to a terminal. | 
| `-c` | Use time when file status was last changed for sorting (`-t`) or long printing (`-l`). |
| `-d` | Directories are listed as plain files (not searched recursively) |
| `-G` | Enable colorized output |
| `-h` | When used with the `-l` option, use unit suffixes: Byte, Kilobyte, Megabyte, Gigabyte, Terabyte and Petabyte in order to reduce the number of digits to three or less using base 2 for sizes |
| `-l` | (The lowercase letter "ell".)  List in long format. If the output is to a terminal, a total sum for all the file sizes is output on a line before the long listing |
| `-m` | Stream output format; list files across the page, separated by commas |
| `-R` | Recursively list subdirectories encountered. |
| `-r` | Reverse the order of the sort to get reverse lexicographical order or the oldest entries first or largest files last, if combined with sort by size |
| `-S` | Sort files by size |
| `-T` | When used with the -l (lowercase letter "ell") option, display complete time information for the file, including month, day, hour, minute, second, and year. |
| `-t` | Sort by time modified (most recently modified first) before sorting the operands by lexicographical order |
| `-u` | Use time of last access, instead of last modification of the file for sorting (`-t`) or long printing (`-l`) |
| `-U` | Use time of file creation, instead of last modification for sorting (`-t`) or long output (`-l`) |

When you use the "`ls`" command in Bash with the "`-l`" flag, it provides a detailed listing of files and directories in the current directory. Here's a breakdown of the information printed on the terminal window.
1. **File Type and Permissions**: The first column indicates the file type and its permissions. It consists of 10 characters.
    * The first character indicates the file type (`-` for a regular file, `d` for a directory, `l` for a symbolic link, etc.)
    * The next nine characters show the file's permissions in three groups: owner, group, and others. For example, `rwxr-xr--` means:
        * Owner has read, write, and execute permissions (`rwx`)
        * Group has read and execute permissions (`r-x`)
        * Others have read-only permissions (`r--`)
2. **Number of Links**: The second column shows the number of hard links to the file or directory. For directories, this count includes the directory itself and its subdirectories.
3. **Owner**: The third column displays the name of the user (owner) who owns the file.
4. **Group**: The fourth column shows the name of the group that has permissions for the file.
5. **File Size**: The fifth column indicates the size of the file in bytes. For large files, you may want to use the `-h` flag with `-l` to display file sizes in a human-readable format (e.g., KB, MB).
6. **Modification Date and Time**: The sixth column gives the date and time when the file was last modified. The format is typically `MMM DD HH:MM` for files modified in the current year, and `MMM DD YYYY` for older files.
7. **File Name**: The final column shows the name of the file or directory. If it is a symbolic link, it will also display the link target.

Let's take a look at the following example.

```txt
$ ls -l
-rwxr-xr--  1 user group  4096 Oct  3  12:00 myfile.txt
drwxr-xr-x  2 user group  4096 Oct  3  11:55 mydirectory
```

In the previous example:
* The first entry (`myfile.txt`) is a regular file with `rwxr-xr--` permissions, owned by `user` in the `group` group, and is 4096 bytes in size.
* The second entry (`mydirectory`) is a directory, with `rwxr-xr-x` permissions, and contains two links (itself and a subdirectory).

This detailed view is useful for checking file permissions, ownership, and modification dates at a glance.

In the next section we will play with "`pushd`", "`popd`", "`dirs`" and "`$DIRSTACK`".

## The `pop`, `popd`, `dirs` commands and the `$DIRSTACK` environment variable

The "`pushd`", "`popd`", and "`dirs`" are shell builtins which allow you manipulate the directory stack (`$DIRSTACK`). This can be used to change directories but return to the directory from which you came.

How can we visualize the stack of directories?

<div style="text-align:center">
    <img src="/assets/bash-in-depth/0016-Navigation-and-file-folder-related-commands/DIRSTACK.png" width="650px"/>
</div>

### The `pushd` command

This builtin command is in charge of **adding directories** to `DIRSTACK`. Typically, “`pushd`” is invoked with a parameter which is the path to a directory which will be set a new current working directory and the one that will be put on top of the stack.

If invoked with no arguments, it will exchange the first two items on top of the stack, and it will change as well the current working directory. Let's say that your current working directory is your home directory and that you have inside `DIRSTACK` two directories, which are your home directory (typically represented with `~`) and the `/tmp` directory.

If you invoke "`pushd`" without arguments, your current working directory will become "`/tmp`". This means that the "`/tmp`" will be put on top of the `DIRSTACK` and your home folder (`~`) will be the second in the stack.

If there are not enough items on the stack, it will throw an error.

This command comes as well with other options that modify (or not) the stack in specific ways.

| Option | Description |
| :----: | :---- |
| `-n` | Manipulates the stack but without changing the current working directory |
| `+N` | Rotates the stack so that the Nth directory (counting **from the left** of the list shown by `dirs`, starting with zero) is at the top. |
| `-N` | Rotates the stack so that the Nth directory (counting **from the right** of the list shown by `dirs`, starting with zero) is at the top. |

Let's see a graphical representation of “`pushd +N`”.

<div style="text-align:center">
    <img src="/assets/bash-in-depth/0016-Navigation-and-file-folder-related-commands/PUSHD-N.png" width="550px"/>
</div>

So... What is going on in the previous image? The `DIRSTACK` environment variable has 6 directories pushed to it being `DIR0` the first one to be pushed and being `DIR5` the last one to be pushed to the stack `DIRSTACK` (and also is the current working directory).

When the command "`pushd +3`" is executed, the folder on the index 3 ("`DIR2`" in our case) will be moved to the top of the stack along with the directories that are in the following indices. Another effect of this command is that all the directories from the top of the stack **until the index before** the index that was passed to the command "`pushd`" will be sent to the bottom of the stack in the same order that they were on the top.

In the following image you will see a graphical representation of “`pushd -N`”.

<div style="text-align:center">
    <img src="/assets/bash-in-depth/0016-Navigation-and-file-folder-related-commands/PUSHD-negative-N.png" width="550px"/>
</div>

So... What is going on in the previous image? The `DIRSTACK` environment variable has the same configuration as in the previous example.

When the command "`pushd -3`" is executed, the folder on the index 3 ("`DIR3`" in our case) will be moved to the top of the stack along with the directories that are in the previous indices ("`DIR2`", "`DIR1`" and "`DIR0`" in our case). Another effect of this command is that all the directories from the top of the stack **until the index after** the index that was passed to the command "`pushd`" will be sent to the bottom of the stack in the same order that they were on the top.

### The `popd` command

This builtin command is in charge of **removing directories** to `DIRSTACK`. Typically, “`popd`” is invoked without parameters which result in the removal of the item located at the top of the stack. This will result (similar to the "`pushd`" command) in updating the current working directory to the next element in the stack.

This command comes as well with other options that modify (or not) the stack in specific ways.

| Option | Description |
| :----: | :---- |
| `-n` | Manipulates the stack but without changing the current working directory. |
| `+N` | Removes the Nth entry counting from the left of the list shown by `dirs`, starting with zero.  For example: `popd +0` removes the first directory, `popd +1` the second. |
| `-N` | Removes the Nth entry counting from the right of the list shown by `dirs`, starting with zero.  For example: `popd -0` removes the last directory, `popd -1` the next to last. |


### The `dirs` command

This builtin command of Bash is in charge of displaying the contents of the `DIRSTACK` environment variables. This command comes with the following options.

| Option | Description |
| :-----: | :----- |
| `-c` | Clears the directory stack by deleting all of the elements. |
| `-l` | Do not print tilde-prefixed versions of directories relative to your home directory. Will print all the directories in the "`$DIRTACK`" environment variable with absolute paths. |
| `-p` | Print the directory stack with one entry per line. |
| `-v` | Print the directory stack with one entry per line prefixed with its position in the stack. |
| `+N` | Displays the Nth entry counting **from the left** of the list shown by `dirs` when invoked without options, starting with zero. |
| `-N` | Displays the Nth entry counting **from the right** of the list shown by `dirs` when invoked without options, starting with zero. |


In the following sections, we will learn a little bit of how to create files and folders, how to show the content of files, how to move folders or files around, how to delete files and folders and, most importantly how to to search information in different files/folders.

## File/Folder related commands

In the earlier sections, we explored how to navigate the filesystem and view the contents of directories. Now, in the upcoming subsections, we will focus on essential file and folder operations, including how to:
* Create new files and directories
* View the contents of a file
* Move or rename files and directories
* Delete files and directories
* Search for files or directories based on specific criteria

These tasks form the foundation of file management in Bash and are key to efficiently working within the command-line environment.

### Creation of files

There are several ways to create files, but for now, we'll focus on a straightforward command that allows us to quickly generate an empty file: the "`touch`" command.

Primarily, "`touch`" is designed to update the access and modification timestamps of an existing file. However, if you provide the name (or path) of a file that doesn’t already exist, "`touch`" will create that file for you automatically.

One key feature of "`touch`" is its ability to handle multiple filenames at once, enabling you to create several files simultaneously by simply listing their names.

The "`touch`" command makes very easy creating new files. You can also use text editors like VIM<a id="footnote-3-ref" href="#footnote-3" style="font-size:x-small">[3]</a>, Notepad or similar ones to create text files.

Let's give it a try with this command.

```txt
$ ls

$ touch file.txt

$ ls
file.txt
```

In the previous example we went to a directory that had no files nor folders in it. Then we used thecommand "`ls`" to list everything in the directory (which is nothing). Then we used the command "`touch file.txt`" to create a file with the name of "`file.txt`". Then we used (again) the command "`ls`" to list (again) the contents of the current folder. This time it shows the new file that we created.

### Creation of directories

We’ve learned how to view the contents of a folder, but how do we create one? For that, we use the command "`mkdir`", short for "make directory."

One of the great features of "`mkdir`" is its ability to accept multiple arguments, allowing you to create several directories at once. However, there's a potential complication—sometimes you might need to create a directory within another directory, or even at a deeper level. In such cases, the parent directories of your target folder must already exist.

To address this limitation, "`mkdir`" provides a handy "`-p`" option. This option ensures that all necessary intermediate directories are created automatically, allowing you to build an entire directory structure in one go.

Let's see a few examples in the terminal window.

```txt
$ ls
file.txt
$ mkdir directory1/directory2
mkdir: cannot create directory ‘directory1/directory2’: No such file or directory
$ mkdir -p directory1/directory2
$ ls
directory1  file.txt
```

As you can see in the previous example, we first used the command "`ls`" to list the contents of the current folder, which contains the file "`file.txt`" that we created previously.

Then we tried to create 2 folders being "`directory1`" the first one and being "`directory2`" the second one, inside the first directory with the command "`mkdir`" (without any additional options). As you can see it failed.

But if we add the option "`-p`" it works. 

Last we use the command "`ls`" to show the new content of the current directory.

In the next section we will learn how to use the command "`cat`".

### Show content of a file (The `cat` command)

We’ve learned how to create files and directories, but how do we view the contents of those files in Bash? This is where the command cat comes in handy. Its primary function is to read the contents of one or more files and print them to the standard output, which is usually your terminal screen. The basic syntax of cat is as follows:

```bash
    cat [-benstuv] [file ...]
```

With "`cat`", you can display the contents of a single file or multiple files in the order they are provided. If you pass "`-`" as an argument, "`cat`" will wait for input from the standard input (such as typing into the terminal), and it will display whatever you type. Invoking "`cat`" with no arguments is equivalent to using "`cat -`", meaning it will echo back whatever you type.

There are a few useful flags with "`cat`":
* "`-b`": Numbers only non-blank lines in the output, starting from 1.
* "`-n`": Numbers all lines, blank or non-blank, starting from 1.
* "`-s`": Reduces multiple consecutive blank lines in the output to a single blank line, making the output more readable if the file contains a lot of empty space.

In summary, "`cat`" is a straightforward and versatile tool for quickly viewing file contents, but with a few useful flags, it can also tidy up your output for better readability.

Let's say we want to display the content of the file we created before (the file "`file.txt`"). For that we can use the "`cat`" command as follows.

```txt
$ cat file.txt
Content of line 1
Content of line 2
Content of line 3
Content of line 4
Content of line 5
```

Now that have learnt how to show the contents of a file using the "`cat`" command we are going to take a look to the "`mv`" command which will allow us to move files and directories.

### Moving files and folders (The `mv` command)

## Summary


## References


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px;">
<p id="footnote-1" style="font-size:10pt">
1. We will learn more about the "<code style="font-size:10pt">cd</code>" later in this chapter.<a href="#footnote-1-ref">&#8617;</a>
</p>
<p id="footnote-2" style="font-size:10pt">
2. Please refer to <code style="font-size:10pt">man ls</code>, <code style="font-size:10pt">man cd</code>,...<a href="#footnote-2-ref">&#8617;</a>
</p>
<p id="footnote-3" style="font-size:10pt">
3. More about this powerful editor in its website <a href="https://www.vim.org/">https://www.vim.org/</a>.<a href="#footnote-3-ref">&#8617;</a>
</p>

