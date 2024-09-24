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

### How do you access items of an indexed array?

The way you select an item from an indexed array is as follows.

```bash
    echo "My item: ${MY_ARRAY[$index]}"
```

Where "`$index`" is a whole number (for example `0`, `1`, `2`, ...).

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

If you try to declare the previous associative array without “`declare -A`” you will be surprised. Let’s see it with an example.

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

### How do you access items of an associative array?

The way you select an item from an associative array is as follows.

```bash
    echo "My item: ${MY_MAP[$key]}"
```

Where "`$key`" is one of the keys stores in the associative array (something like "`Madrid`", "`London`" or "`Paris`" from the previous example).

## Associative Arrays as Sets

In the previous sections we learnt that an associative array is used to map a string of characters to some value. This is known in software engineering as a “*Map*”, “*Dictionary*” or (as is known in Bash) “*Associative arrays*”.

In software engineering there is another data structure called “*Set*”. A set is a data structure that contains unique values without a particular order.

Bash, technically, does not provide the data structure “Set” but we can use an associative array to simulate the same behavior.

The idea is to create an associative array that has as keys the unique information you want to store and “`1`” as value. That’s it!

Let's see how it works with an example script.

```bash
 1 #!/usr/bin/env bash
 2 #Script: array_as_set.sh
 3 # Declaring set with 3 cities
 4 declare -A CITIES=(
 5  [London]=1
 6  [Paris]=1
 7  [Madrid]=1
 8 )
 9 # Checking for a city inside the Set
10 if [[ -n "${CITIES[London]}" ]]; then
11     echo "London is in the set" # This will be printed
12 else
13     echo "London is NOT in the set"
14 fi
15 # Checking for a city that is not in the Set
16 if [[ -n "${CITIES[Seville]}" ]]; then
17     echo "Seville is in the set"
18 else
19     echo "Seville is NOT in the set" # This will be printed
20 fi
```

In this simple case we are using the option “`-n`” of test which will return true if the string is not empty. When we run the previous script we get the following result.

```txt
$ ./array_as_set.sh
London is in the set
Seville is NOT in the set
```

Now that we have learnt how to declare indexed and associative arrays we are going to dive on the operations we can do on them in the next section.

## Operations with arrays

In this section we are going to assume that we have declared an array called “`MY_ARRAY`”.

Once the array is declared, what can we do with it? In the next subsections we will have a short view of what we can do with some examples.

### First element of the array

In the case of an indexed array referring to the variable “`$MY_ARRAY`” will show the first element of the array itself. 

```bash
 1 #!/usr/bin/env bash
 2 #Script: array_index_first.sh
 3 MY_ARRAY=("value1" "value2" "value3")
 4 echo "First item: $MY_ARRAY"
```

When we execute the previous script, you will get the following in your terminal.

```txt
$ ./array_index_first.sh
First item: value1
```

In the case of an associative array referring to the variable “`$MY_ARRAY`” will not show anything as associative arrays are not sorted.

```bash
 1 #!/usr/bin/env bash
 2 #Script: array_associative_first.sh
 3 declare -A MY_ARRAY=(
 4     [key1]="value1"
 5     [key2]="value2"
 6     [key3]="value3"
 7 )
 8 echo "First item: $MY_ARRAY"
```

As you will see in the following execution of the script, “`$MY_ARRAY`” returns empty.

```txt
$ ./array_associative_first.sh
First item:
```

It returns empty **because Bash does not directly support printing an entire associative array by just referencing its name** like you would with a regular variable. Associative arrays in Bash require you to access individual elements or loop through the keys.

In the next section we will see how to get the whole content of the array.

### Get the whole content of the array

There are two ways to get the whole content of an indexed array or the values of an associative array. Which are “`${MY_ARRAY[*]}`” and “`${MY_ARRAY[@]}`”.

The difference is that “`${MY_ARRAY[*]}`” will display the elements of the indexed array (or the values of the associative array) as **a single string**, while “`${MY_ARRAY[@]}`” will display the same ones quoted separately.

```bash
 1 #!/usr/bin/env bash
 2 #Script: index_array_content.sh
 3 MY_ARRAY=("value1" "value2" "value3")
 4 echo "Content 1: ${MY_ARRAY[*]}"
 5 echo "Content 2: ${MY_ARRAY[@]}"
```

When we execute the previous script we get the following result in the terminal.

```txt
$ ./index_array_content.sh
Content 1: value1 value2 value3
Content 2: value1 value2 value3
```
The execution does not show us the difference between “`${MY_ARRAY[*]}`” and “`${MY_ARRAY[@]}`” but don’t worry we will see it more clearly when we get into the different kinds of loops, just keep this in mind.

Something similar happens when we try to use “`${MY_ARRAY[*]}`” and “`${MY_ARRAY[@]}`” with associative arrays.

```bash
 1 #!/usr/bin/env bash
 2 #Script: associative_array_content.sh
 3 declare -A MY_ARRAY=(
 4     [key1]="value1"
 5     [key2]="value2"
 6     [key3]="value3"
 7 )
 8 echo "Content-1: ${MY_ARRAY[*]}"
 9 echo "Content-2: ${MY_ARRAY[@]}"
```

When you run the previous script you will get the following result in your terminal.

```txt
$ ./associative_array_content.sh
Content-1: value2 value3 value1
Content-2: value2 value3 value1
```

Same as with indexed arrays, we will come back to this when we will talk about loops.

In the next section we will see how to get the list of indices for both indexed and associative arrays.

### Get the list of indices

The way to get the indices of both indexed and associative arrays is by using “`${!MY_ARRAY[@]}`”.

If you use it with an indexed array it will return the indices that contain a value. 

```bash
 1 #!/usr/bin/env bash
 2 #Script: index_array_indices.sh
 3 MY_ARRAY=("value1" "value2" "value3")
 4 echo "Indices: ${!MY_ARRAY[@]}"
```

When you execute the previous script you will get the following result in your terminal.

```txt
$ ./index_array_indices.sh
Indices: 0 1 2
```

If you use it with an associative array it will return the keys.

```bash
 1 #!/usr/bin/env bash
 2 #Script: associative_array_indices.sh
 3 declare -A MY_ARRAY=(
 4     [key1]="value1"
 5     [key2]="value2"
 6     [key3]="value3"
 7 )
 8 echo "Indices: ${!MY_ARRAY[@]}"
```

When you execute the previous script you will get the list of keys unsorted.

```txt
$ ./associative_array_indices.sh
Indices: key2 key3 key1
```

In the next section we will learn how to get the length of the array.

### Get the length of the array

To be able get the length of an array (both indexed and associative) you can use either “`${#MY_ARRAY[*]}`” or “`${#MY_ARRAY[@]}`”, **both expressions are equivalent**.

Let’s see it with a couple of examples.



## Summary


## References


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px;">
