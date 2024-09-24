---
layout: chapter
title: "Chapter 12: Arrays and loops"
---

# Chapter 12: Arrays and loops

Bash scripting introduces arrays and loops as essential constructs, facilitating the manipulation and processing of data within scripts. Arrays, the cornerstone of structured data storage in Bash, come in two main types: indexed arrays and associative arrays. Indexed arrays employ numerical indices to represent elements, starting from zero, while associative arrays use user-defined keys for a more versatile and human-readable approach to data organization. This diversity enables scriptwriters to choose the array type that best fits the nature of their data, making Bash scripts adaptable to various scenarios.

In tandem with arrays, Bash incorporates different types of loops, providing the means to iterate over arrays and perform repetitive tasks efficiently. The "`for`" loop, a fundamental iteration construct, facilitates the sequential processing of elements within an array or a predefined range of values. Meanwhile, the "`while`" and "`until`" loops offer conditional iteration, allowing scripts to execute code repeatedly based on specified conditions. This mixture of arrays and loops empowers Bash scripts with the ability to automate complex tasks, process large datasets, and implement dynamic solutions.

As we delve into the intricacies of Bash scripting, we will explore the syntax, usage, and best practices for working with arrays and loops. From basic array declarations to advanced loop structures, understanding these core concepts unlocks the full potential of Bash scripting, enabling scriptwriters to create robust and versatile solutions for various computing tasks and automation scenarios.

Let’s dive in!


## Indexed Arrays

An **Indexed Array** is a data structure that associates values to indices (0, 1, 2, etc) and allows you to access those values via indices.

Let’s take a look at a visualization for this kind of array.

<div style="text-align:center">
    <img src="/assets/bash-in-depth/0012-Arrays-and-loops/Indexed-array.png" width="500px"/>
</div>

How do we declare this kind of array? Well there are 2 ways to declare an indexed array.

The first way is by using initialization declaration, which is assigning a value directly to a variable, like the following.

```bash
    MY_ARRAY=("value1" "value2" "value3")
```

The second one is by using the “`declare`” builtin command that we saw previously with the option “`-a`”.

```bash
    declare -a MY_ARRAY=("value1" "value2" "value3")
```

In the next section we are going to learn about associative arrays.

## Associative Arrays (a.k.a. Hashes/Maps)

An **Associative Array** is a data structure that associates values to “*keys*”. A key can be any random string or number.

<div style="text-align:center">
    <img src="/assets/bash-in-depth/0012-Arrays-and-loops/Associative-array.png"/>
</div>

So, how do we declare an associative array? We use the option “`-A`” of the “`declare`” builtin command.

```bash
    declare -A MY_MAP=( [Madrid]="Spanish" [London]="English" [Paris]="French")
```

This is **the only valid way** to declare an associative array.

If you try to declare the previous associative array without “declare -A” you will be surprised. Let’s see it with an example.

```bash
 1 #!/usr/bin/env bash
 2 #Script: bad_associative_array.sh
 3 MY_MAP=( [Madrid]="Spanish" [London]="English" [Paris]="French")
 4 declare -p MY_MAP
```

When you run the previous script you will the following result in your terminal.

```txt
$ ./bad_associative_array.sh
declare -a MY_MAP=([0]="French")
```

As you can see without using “`declare -A`” the variable “`MY_MAP`” is treated like an indexed array. This is because the keys “`Madrid`”, “`London`” and “`Paris`” are treated as variables and as they do not exist, Bash assumes the value “`0`” for all of them. 

This means that the values “`Spanish`”, “`English`” and “`French`” are associated to the same key/index (“`0`” in our case) which will cause that the different values will be overriding the previous value, leaving the value “`French`” as the last value overriding the content of the key/index “`0`”.

Now, if we declare one of the keys as a variable and assign a whole number (0, 1, 2, etc) the result will change. Let’s see it with an example.

```bash
 1 #!/usr/bin/env bash
 2 #Script: bad_associative_array_2.sh
 3 Madrid=20
 4 MY_MAP=( [Madrid]="Spanish" [London]="English" [Paris]="French")
 5 declare -p MY_MAP
```

When you run the previous script you will have now the following result.

```txt
$ ./bad_associative_array_2.sh
declare -a MY_MAP=([0]="French" [20]="Spanish")
```

Now, rather than having one single element in the array there are **two**. One associated with index “`0`” and another one associated with index “`20`”.


## Associative Arrays as Sets

In the previous sections we learnt that an associative array is used to map a string of characters to some value. This is known in software engineering as a “*Map*”, “*Dictionary*” or (as is known in Bash) “*Associative arrays*”.

In software engineering there is another data structure called “*Set*”. A set is a data structure that contains unique values without a particular order.

Bash, technically, does not provide the data structure “Set” but we can use an associative array to simulate the same behavior.

The idea is to create an associative array that has as keys the unique information you want to store and “`1`” as value. That’s it!


## Summary


## References


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px;">
