---
layout: chapter
title: "Chapter 28: Here Documents"
---

# Chapter 28: Here Documents


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">

## Introduction

A **Here Document** (commonly referred to as "*HereDoc*") is a powerful feature in Bash that allows you to embed a block of text or commands directly within a script, treating it as though it were a separate file. This feature acts as a multiline string or file literal, making it especially useful for sending input streams to commands or programs.

HereDocs are invaluable when dealing with scenarios that require multiple commands to be redirected simultaneously. They help streamline scripts, enhancing both their readability and organization. By consolidating complex input into a single, cohesive block, HereDocs make Bash scripting neater and more intuitive.

A HereDoc begins with a delimiter specified after the "`<<` operator and continues until a line containing only the delimiter (with no additional spaces or characters) is encountered. This delimiter marks the end of the HereDoc, ensuring that its content is treated as a singular unit. If multiple HereDocs are present, they are processed sequentially, starting immediately after the preceding HereDoc ends.

## Syntax of HereDocs

The syntax for a Here Document (HereDoc) in Bash is structured as follows:

```bash
    [COMMAND] [fd]<<[-] 'DELIMITER'
      Line 1
      Line 2
      ...
    DELIMITER
```

Here’s a breakdown of each element:
* "`COMMAND`": This is optional and represents any command that can accept redirected input. If omitted, the HereDoc simply provides input for redirection.
* "`fd`": Also optional, this specifies the file descriptor for redirection. If left out, it defaults to 0 (standard input).
* "`<<`": The redirection operator, which forwards the HereDoc content to the specified command.
* "`-`": An optional parameter that suppresses leading tabs in the HereDoc content.
* "`DELIMITER`" **(First Line)**: A user-defined token that marks the start and end of the HereDoc content. Common choices include "`END`", "`EOT`", or "`EOF`", but any unique word works as long as it doesn’t appear in the body. Omitting quotes, double quotes, or backslashes in this line enables command and variable expansion within the HereDoc.
* "`DELIMITER`" **(Last Line)**: This signals the end of the HereDoc and must exactly match the token used in the first line. Ensure there are no leading spaces or extra characters.

This syntax ensures a clear and structured way to handle multiline input directly within scripts.

## Sending input to another program

HereDocs are primarily used to provide input to programs that accept data through standard input. Let’s consider an example where input is passed to Python:

```bash
 1 #!/usr/bin/env bash
 2 #Script: heredoc-0001.sh
 3 read -p "Introduce the radius of the circle: " RADIUS
 4 echo "Radius: $RADIUS"
 5 python <<EOF
 6 import math
 7 radius=$RADIUS
 8 area=math.pi*(radius**2)
 9 print("Area: ", area)
10 EOF
```

In this script, the "`read`" built-in command is first used to capture user input. Once the input is provided, it is forwarded to Python as input via a HereDoc. When the script is executed, it prompts the user to enter a value:

<pre>
$ ./heredoc-0001.sh
Introduce the radius of the circle:

</pre>

The script waits for a number, which represents the radius of a circle, to calculate its area. For example, if you enter the number "`2.5`", the script processes this input and uses Python to calculate the result.

<pre>
$ ./heredoc-0001.sh
Introduce the radius of the circle: 2.5
Radius: 2.5
Area:  19.634954084936208

$
</pre>

The output displays the calculated area of the circle based on the provided radius.

This example demonstrates how HereDocs can facilitate seamless interaction between Bash and other programs, simplifying the process of feeding dynamic input into commands.

## Using HereDocs along with the “`cat`” command to print text

One of the most common uses for HereDocs is to display text, such as the help message for a program. This is often achieved by enclosing the opening delimiter in single or double quotes to prevent variable or command substitution.

Consider the following example:

```bash
 1 #!/usr/bin/env bash
 2 #Script: heredoc-0002.sh
 3 TEST="My test"
 4 cat <<"EOF"
 5 cd: cd [-L|[-P [-e]] [-@]] [dir]
 6     Change the shell working directory.
 7     Change the current directory to DIR.  The default DIR is the value of the
 8     HOME shell variable.
 9     The variable CDPATH defines the search path for the directory containing
10     DIR.  Alternative directory names in CDPATH are separated by a colon (:).
11     A null directory name is the same as the current directory.  If DIR begins
12     with a slash (/), then CDPATH is not used.
13     ...
14     Exit Status:
15     Returns 0 if the directory is changed, and if $PWD is set successfully when
16     -P is used; non-zero otherwise.
17     Some additional example:
18     Whoami: $(whoami)
19     TEST: $TEST
20 EOF
```

In this script, a portion of the help for the "`cd`" command is printed. The opening delimiter ("`EOF`") is enclosed in double quotes ("`"EOF"`"), which disables variable and command substitution. As a result, the output remains unchanged and displays the text exactly as written:

<pre>
$ ./heredoc-0002.sh
cd: cd [-L|[-P [-e]] [-@]] [dir]
    Change the shell working directory.

    Change the current directory to DIR.  The default DIR is the value of the
    HOME shell variable.

    The variable CDPATH defines the search path for the directory containing
    DIR.  Alternative directory names in CDPATH are separated by a colon (:).
    A null directory name is the same as the current directory.  If DIR begins
    with a slash (/), then CDPATH is not used.
    ...
    Exit Status:
    Returns 0 if the directory is changed, and if $PWD is set successfully when
    -P is used; non-zero otherwise.

    Some additional example:
    Whoami: $(whoami)
    TEST: $TEST

$
</pre>

The output is a literal representation of the HereDoc content. This behavior can also be achieved with other formats, such as:
* "`cat <<"EOF"`" (as used in the example)
* "`cat <<'EOF'`"
* "`cat <<\EOF`"

However, if the quotes or backslashes are omitted from the opening delimiter, variable and command substitution become active:

```bash
 1 #!/usr/bin/env bash
 2 #Script: heredoc-0003.sh
 3 TEST="My test"
 4 cat <<EOF
 5 cd: cd [-L|[-P [-e]] [-@]] [dir]
 6     Change the shell working directory.
 7     Change the current directory to DIR.  The default DIR is the value of the
 8     HOME shell variable.
 9     The variable CDPATH defines the search path for the directory containing
10     DIR.  Alternative directory names in CDPATH are separated by a colon (:).
11     A null directory name is the same as the current directory.  If DIR begins
12     with a slash (/), then CDPATH is not used.
13     ...
14     Exit Status:
15     Returns 0 if the directory is changed, and if $PWD is set successfully when
16     -P is used; non-zero otherwise.
17     Some additional example:
18     Whoami: $(whoami)
19     TEST: $TEST
20 EOF
```

In this case, the script processes the variables and commands within the HereDoc, producing a modified output:

<pre>
$ ./heredoc-0003.sh
cd: cd [-L|[-P [-e]] [-@]] [dir]
    Change the shell working directory.

    Change the current directory to DIR.  The default DIR is the value of the
    HOME shell variable.

    The variable CDPATH defines the search path for the directory containing
    DIR.  Alternative directory names in CDPATH are separated by a colon (:).
    A null directory name is the same as the current directory.  If DIR begins
    with a slash (/), then CDPATH is not used.
    ...
    Exit Status:
    Returns 0 if the directory is changed, and if <strong>/home/username/Repositories/somerepo/_bash-in-depth/chapters/0028-Here-Documents/script</strong> is set successfully when
    -P is used; non-zero otherwise.

    Some additional example:
    Whoami: <strong>username</strong>
    TEST: <strong>My test</strong>

$
</pre>

The result reflects the evaluated variables and executed commands, demonstrating how HereDocs can be tailored for both literal and dynamic content, depending on your requirements.

## Ignoring tabs

## Summary


## References



<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">
