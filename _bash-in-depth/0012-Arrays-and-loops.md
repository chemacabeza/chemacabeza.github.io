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
    <img src="/assets/bash-in-depth/0012-Arrays-and-loops/Associative-array.png" width="500px"/>
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

```bash
 1 #!/usr/bin/env bash
 2 #Script: index_array_length.sh
 3 MY_ARRAY=("value1" "value2" "value3")
 4 echo "Length-1: ${#MY_ARRAY[*]}"
 5 echo "Length-2: ${#MY_ARRAY[@]}"
```

When you run the previous script for indexed arrays you will the following result in your terminal.

```txt
$ ./index_array_length.sh
Length-1: 3
Length-2: 3
```

And now an example for associative arrays.

```bash
 1 #!/usr/bin/env bash
 2 #Script: associative_array_length.sh
 3 declare -A MY_ARRAY=(
 4     [key1]="value1"
 5     [key2]="value2"
 6     [key3]="value3"
 7 )
 8 echo "Length-1: ${#MY_ARRAY[*]}"
 9 echo "Length-2: ${#MY_ARRAY[@]}"
```

When you execute the previous script you get the following in your terminal.

```txt
$ ./associative_array_length.sh
Length-1: 3
Length-2: 3
```

In the next section we are going to see how to add an element to an array.


### Adding an element to an array

In order to add an additional element to an array we can do it by using the following form.

<div style="text-align:center">
    <img src="/assets/bash-in-depth/0012-Arrays-and-loops/Add-Item-To-Array.png" width="550px"/>
</div>

As you can see the only difference on adding a new element to an indexed array or an associative array is including the key in the associative array.

For example, for an indexed array we would proceed as follows.

```bash
 1 #!/usr/bin/env bash
 2 #Script: index_array_add.sh
 3 MY_ARRAY=("value1" "value2" "value3")
 4 echo "Elements: ${MY_ARRAY[@]}"
 5 MY_ARRAY+=("value4")
 6 echo "Elements: ${MY_ARRAY[@]}"
```

When you run the previous script you will get the following in the terminal.

```txt
$ ./index_array_add.sh
Elements: value1 value2 value3
Elements: value1 value2 value3 value4
```

Where you can see the new value (“`value4`”) added to the end of the array.

In the case of an associative array we would do the following.

```bash
 1 #!/usr/bin/env bash
 2 #Script: associative_array_add.sh
 3 declare -A MY_ARRAY=(
 4     [key1]="value1"
 5     [key2]="value2"
 6     [key3]="value3"
 7 )
 8 MY_ARRAY+=([key4]="value4")
 9 declare -p MY_ARRAY
```

When you run the previous script you will get the following result in your terminal.

```txt
$ ./associative_array_add.sh
declare -A MY_ARRAY=([key4]="value4" [key2]="value2" [key3]="value3" [key1]="value1" )
```

In the result you can cleary see that the a new value was added to the associative array with key "`key4`" and value "`value4`".

Something to notice is that for associative arrays you can add elements by using “`MY_ARRAY[Key]=Value`”. This means that we could rewrite the last script as the following and we would get the exact same result.


```bash
 1 #!/usr/bin/env bash
 2 #Script: associative_array_add.sh
 3 declare -A MY_ARRAY=(
 4     [key1]="value1"
 5     [key2]="value2"
 6     [key3]="value3"
 7 )
 8 MY_ARRAY["key4"]="value4"
 9 declare -p MY_ARRAY
```

In the next section we are going to learn how to delete specific elements from arrays.

### Delete element from a given index

There are a couple of ways to delete elements from an array (either indexed or associative). The one we are exploring in this section is by using the index of the element we want to delete.

If you remember the indices are different in both indexed and associative arrays.

In indexed arrays the indices are whole numbers (0, 1, 2, etc) while in associative arrays the indices are the keys (“`key1`”, “`key2`”, “`key3`” in our previous examples).

In both types of array we will have to use the following to delete elements.

<div style="text-align:center">
<img src="/assets/bash-in-depth/0012-Arrays-and-loops/Unset-Item-From-Array.png" width="550px"/>
</div>

In the following example we are deleting the second element (index “`1`”) of the array.

```bash
 1 #!/usr/bin/env bash
 2 #Script: index_array_delete.sh
 3 MY_ARRAY=("value1" "value2" "value3")
 4 # Deleting second item of the array
 5 unset MY_ARRAY[1]
 6 # Printing the whole content of the array
 7 echo "Content: ${MY_ARRAY[@]}"
```

When we execute the previous script you will see that “`value2`” is no longer in the array.

```txt
$ ./index_array_delete.sh
Content: value1 value3
```

In the case of an associative array, as it was mentioned before, we need to use the key of the entry we want to delete. In the following example we will remove the entry whose key is “`key2`”.

```bash
 1 #!/usr/bin/env bash
 2 #Script: associative_array_delete.sh
 3 declare -A MY_ARRAY=(
 4     [key1]="value1"
 5     [key2]="value2"
 6     [key3]="value3"
 7 )
 8 unset MY_ARRAY[key2]
 9 declare -p MY_ARRAY
```

When we execute the previous script you will see that the element whose key is “`key2`” is no longer in the associative array.

```txt
$ ./associative_array_delete.sh
declare -A MY_ARRAY=([key3]="value3" [key1]="value1" )
```

In the next section we will learn how to delete elements based on a pattern.

### Delete element from a given pattern

In the previous section we learnt how to delete elements from an array, but we needed to know beforehand what the index/key of the element was.

In this section we are going to learn how to delete elements based on a “*pattern*”. It will be like saying “*Please delete the elements that look like this pattern*”.

The generic form that we will use is as follows.

<div style="text-align:center">
    <img src="/assets/bash-in-depth/0012-Arrays-and-loops/Generic-Form-Delete-From-Pattern.png" width="450px"/>
</div>

Those 4 dots will be replaced with some syntax that will do pattern matching to find the elements before removing them. There are several ways to remove an element from an array and are the following:
* Shortest match from the front of the string
* Longest match from the front of the string
* Shortest match from the back of the string
* Longest match from the back of the string

#### Shortest match from the front of the string

The first approach is by using “`${MY_ARRAY[@]#<pattern>}`”. This way is going to search for the elements in the array that match the pattern given the **shortest** starting from the **front of the string**.

The previous statement means that once you provide the pattern, Bash will go element by element and, for each element, will try to match the pattern provided starting from the beginning of the string. Then it will remove the pattern found once it finds the shortest match.

Let’s see an example to better understand how it works.

```bash
 1 #!/usr/bin/env bash
 2 #Script: index_array_delete_pattern_front_shortest.sh
 3 MY_ARRAY=( one two three four five six )
 4 echo "Content: ${MY_ARRAY[@]}"
 5 # Shortest match from front of string
 6 echo "\${MY_ARRAY[@]#t*}: ${MY_ARRAY[@]#t*}"
 7 echo "\${MY_ARRAY[@]#t*ee}: ${MY_ARRAY[@]#t*ee}"
```

On line 3 we declare an array with six elements. On line 4 we display the content of the array. Then (the interesting part) on lines 6 and 7 we use two different patterns.

The first pattern (“`t*`”) means that there is a need to match the shortest string that starts with the character “`t`” and has zero or more characters after it. In our case, there are two strings of “`MY_ARRAY`” that start with the character “`t`”, which are the second and the third element whose values are “`two`” and “`three`”, respectively. And the shortest string matching that pattern is “`t`”.

This means that Bash will remove just the character “`t`” from both elements of the array.

The second pattern (“`t*ee`”) means that there is a need to match the shortest string that starts with the character “`t`”, has zero or more characters after it and ends with the characters “`ee`”. Same as in the previous patterns, the only string of “`MY_ARRAY`” that matches that pattern is the third element. In this case the pattern will match the whole string.

This means that Bash will remove the whole element of the array.

When we run the previous script you will see the following output in your terminal.

```txt
$ ./index_array_delete_pattern.sh
Content: one two three four five six
${MY_ARRAY[@]#t*}: one wo hree four five six
${MY_ARRAY[@]#t*ee}: one two  four five six
```

As you can see in the execution of the script, the first pattern removes only one character of a couple of elements of the array, while the first pattern removes a whole element that matches the pattern.

In the next section we will match the longest pattern starting from the front.

#### Longest match from the front of the string

The second approach is by using “`${MY_ARRAY[@]##<pattern>}`”. This way is going to search for the elements in the array that match the pattern given the **longest** starting from the **front of the string**.

The previous statement means that once you provide the pattern, Bash will go element by element and, for each element, will try to match the pattern provided starting from the beginning of the string. Then it will remove the pattern found once it finds the longest match.

Let’s write another script to see how it works. In the next script we left the content of the previous script and added the new things of this section.

```bash
 1 #!/usr/bin/env bash
 2 #Script: index_array_delete_pattern.sh
 3 MY_ARRAY=( one two three four five six )
 4 echo "Content: ${MY_ARRAY[@]}"
 5 # Shortest match from front of string
 6 echo "\${MY_ARRAY[@]#t*}: ${MY_ARRAY[@]#t*}"
 7 echo "\${MY_ARRAY[@]#t*ee}: ${MY_ARRAY[@]#t*ee}"
 8 # Longest match from the front of string
 9 echo -e "\n\${MY_ARRAY[@]##t*}: ${MY_ARRAY[@]##t*}"
10 echo "\${MY_ARRAY[@]##t*ee}: ${MY_ARRAY[@]##t*ee}"
```

In this case we have added from line 8 to line 10. Let’s see what happens when we run the script.

```txt
$ ./index_array_delete_pattern.sh
Content: one two three four five six
${MY_ARRAY[@]#t*}: one wo hree four five six
${MY_ARRAY[@]#t*ee}: one two  four five six

${MY_ARRAY[@]##t*}: one   four five six
${MY_ARRAY[@]##t*ee}: one two  four five six
```

As you can see from the new execution, the first pattern (“`t*`”) will match the longest string of characters that contain the pattern, which happens to be elements with values “`two`” and “`three`”. Bash will remove those two elements from the array.

The second pattern (“`t*ee`”), however, as it’s more specific than the previous one will only match the element with value "`three`". In this case, Bash will remove one single element from the array.

In the next section we will match the shortest pattern starting from the back.


#### Shortest match from the back of the string


## Summary


## References


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px;">
