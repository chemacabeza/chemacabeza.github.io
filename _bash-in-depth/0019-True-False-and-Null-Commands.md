---
layout: chapter
title: "Chapter 19: True, False and Null Commands"
---

# Chapter 19: True, False and Null Commands

<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">

In Bash, the "`true`", "`false`", and null commands play essential roles in scripting by providing fundamental control over conditional logic and flow.

The "`true`" command is a built-in Bash command that does nothing but return an exit status of 0, which represents success. It's often used in scripts or loops where an infinite loop is needed, or as a placeholder in conditions that require a positive (true) result. For example, in a while loop, "`while true; do ... done`" creates a loop that continues indefinitely until it's explicitly stopped.

The "`false`" command is the opposite of "`true`". It does nothing except return an exit status of "`1`", representing failure. It's useful for testing error conditions or to force a script to behave as though a condition has failed. For example, if you want a loop or condition to exit early, you might use "`false`" to simulate an error state or stop a loop.

The **null command**, written as a colon ("`:`"), is another built-in Bash command that returns a successful exit status ("`0`"). It's often used as a placeholder for commands that you plan to add later or when a statement requires a command but no operation is needed. Like "`true`", it ensures that the script or condition passes, but it performs no action. For example, "`if :; then ... fi`" will always execute the commands inside the if block since "`:`" always returns success.

Each of these commands is lightweight and serves a specific purpose in structuring scripts and handling conditions. Whether used to control loops, handle conditions, or serve as placeholders, they add flexibility and clarity to your Bash scripts.

## The `true` command

The purpose of the “`true`” command is to return a successful exit status, **always**. It’s also referred to as “*Do nothing, successfully*”.

This command has no output and will always return zero (success).

The syntax of the command is as follows.

```bash
    true [ignored command line arguments]
```

One of the use cases for the "`true`" command is **Infinite Loops**. In a "`while`" loop, the condition has to be evaluated for each iteration. If you want the loop to run indefinitely (until explicitly broken), you can use "`true`" as the condition, ensuring it always returns success.

The following example script shows an infinite loop,

```bash
 1 #!/usr/bin/env bash
 2 #Script: true-0001.sh
 3 while true; do
 4 		echo "This loop will run forever until it's interrupted manually with Control+C"
 5 done
```

If you run the previous script the you will see the following in your terminal window.

```txt
$ ./true-0001.sh
This loop will run forever until it's interrupted manually with Control+C
This loop will run forever until it's interrupted manually with Control+C
This loop will run forever until it's interrupted manually with Control+C
This loop will run forever until it's interrupted manually with Control+C
This loop will run forever until it's interrupted manually with Control+C
This loop will run forever until it's interrupted manually with Control+C
This loop will run forever until it's interrupted manually with Control+C
This loop will run forever until it's interrupted manually with Control+C
This loop will run forever until it's interrupted manually with Control+C
This loop will run forever until it's interrupted manually with Control+C
This loop will run forever until it's interrupted manually with Control+C
This loop will run forever until it's interrupted manually with Control+C
This loop will run forever until it's interrupted manually with Control+C
This loop will run forever unt^C
```

In the previous execution you will see that the "`while`" loop is running forever until I stop the execution with "Control+C" (which is the "`^C`" that appears at the end).

Another use case for the "`true`" command is being a **Placeholder in Conditions**, when writing a script, sometimes you need to add conditional logic that will be developed later. You can use "`true`" as a placeholder in "`if`" statements or loops to ensure the script remains valid without breaking.

Let's see with use case with the following example script.

```bash
 1 #!/usr/bin/env bash
 2 #Script: true-0002.sh
 3 if true; then
 4   # Placeholder for future logic
 5   echo "This IF statement will always execute"
 6 fi
```

When you execute the previous script you will the following result in your terminal window.

```txt
$ ./true-0002.sh
This IF statement will always execute
```



## Summary


## References

<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px">

