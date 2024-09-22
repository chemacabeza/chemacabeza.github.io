---
layout: chapter
title: "Chapter 11: case-esac statement"
---

# Chapter 11: `case-esac` statement

In Bash scripting, the “`case-esac`” statement is a powerful and flexible construct that facilitates the implementation of conditional branching and decision-making logic. It's particularly useful when you need to evaluate a variable or expression against multiple possible values and execute different code blocks based on the matching conditions. Think of the “`case-esac`” statement as a versatile alternative to a series of “`if-elif-else`” statements, designed to simplify and enhance the readability of your scripts.

The “`case-esac`” statement simplifies the process of handling multiple conditional cases in a Bash script. With “`case`”, you can efficiently compare a variable or expression to a list of patterns and execute a code block corresponding to the first match. This is especially handy when you have a variable that can take on various values, and you want to perform different actions based on those values.

One of the standout features of “`case-esac`” is its support for pattern matching. You can specify patterns, often using wildcards, to match a range of possible values. This means you can create complex and expressive conditions, making it a valuable tool for tasks like parsing user input, handling different file types, or managing options in a script.

The “`case-esac`” statement can significantly enhance the readability of your scripts. It's a cleaner and more concise way to manage multiple conditions compared to long sequences of “`if-elif-else`” statements. Each condition is presented in a structured and easy-to-follow format, making your code more maintainable and comprehensible.


## Syntax for `case-esac`

Once we are familiar with `IF-ELSE` clauses, we need to take a look at an alternative to this one, which is the “`case-esac`” clause. So, what is the “*shape*” of this clause? It’s as follows.

```bash
    case "$variable" in
        pattern1)
            # Code to execute if $variable matches pattern1
            ;;
        pattern2)
            # Code to execute if $variable matches pattern2
            ;;
        *)
            # Code to execute if none of the patterns match
            ;;
    esac
```

The idea is that the value of “`$variable`” will be checked against each “*pattern*” (from top to bottom) and it will execute the code of the first pattern that matches. This is done via “word pattern matching” which is via Regular Expressions (we will take a look later at this).

Each pattern in the “`case-esac`” can either be a **literal string**, **wildcards** or a **variable**. We will see examples of both in the coming sections.

You might have noticed that after every block of code that will be executed depending on the pattern there are the following characters “`;;`”. That combination of characters are telling Bash where it needs to stop executing code for the pattern detected<a id="footnote-1-ref" href="#footnote-1" style="font-size:x-small">[1]</a>.


## How does it work?

We already saw in the previous section that the syntax of the “`case-esac`” statement has different branches that will match a specific pattern.

For example, in the example script shown in the previous section you will see that there are 3 branches. The first branch will match “`pattern1`”, the second branch will match “`pattern2`” and the last branch, with the pattern “`*`”, will match everything that is not matching the other branches. The last branch is optional

The way it works is that Bash will take the value of the variable provided in the case statement and will match it against the different patterns in the different branches from top to bottom and will execute the code of the first pattern that matches the string provided. This means that the order of the different branches does matter.

In the following section we will learn about the different patterns that we can use in the branches of this “`case-esac`” statement.


## Case patterns

As we saw in the previous section the “`case-esac`” statement allows to have different “*cases*” to match the variable. These cases allow different pattern types which are described in the following paragraphs.

### Literal Matching


## Summary


## References



<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px;">
<p id="footnote-1" style="font-size:10pt">
1. In other programming languages (such as C, C++ or Java) that sequence is called “break”.<a href="#footnote-1-ref">&#8617;</a>
</p>

