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

#### <b>Shortest match from the front of the string</b>

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

#### <b>Longest match from the front of the string</b>

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


#### <b>Shortest match from the back of the string</b>

The third approach is by using “`${MY_ARRAY[@]%<pattern>}`”. This way is going to search for the elements in the array that match the pattern given the **shortest** starting from the **back of the string**.

The previous statement means that once you provide the pattern, Bash will go element by element and, for each element, will try to match the pattern provided starting from the end of the string. Then it will remove the pattern found once it finds the shortest match.

Let's write another script that shows this way of removing parts of the items and that is comparing them to the previous ways that we saw.

```bash
 1 #!/usr/bin/env bash
 2 #Script: index_array_delete_pattern_back_shortest.sh
 3 MY_ARRAY=( one two three four five six )
 4 echo "Content: ${MY_ARRAY[@]}"
 5 # Shortest match from front of string
 6 echo "\${MY_ARRAY[@]#t*}: ${MY_ARRAY[@]#t*}"
 7 echo "\${MY_ARRAY[@]#t*ee}: ${MY_ARRAY[@]#t*ee}"
 8 # Longest match from the front of string
 9 echo -e "\n\${MY_ARRAY[@]##t*}: ${MY_ARRAY[@]##t*}"
10 echo "\${MY_ARRAY[@]##t*ee}: ${MY_ARRAY[@]##t*ee}"
11 # Shortest match from the back of string
12 echo -e "\n\${MY_ARRAY[@]%*ee}: ${MY_ARRAY[@]%*ee}"
13 echo "\${MY_ARRAY[@]%t*ee}: ${MY_ARRAY[@]%t*ee}"
```

We added lines 11 to 13. When we run the script the following appears in your terminal window.

```txt
$ ./index_array_delete_pattern_back_shortest.sh
Content: one two three four five six
${MY_ARRAY[@]#t*}: one wo hree four five six
${MY_ARRAY[@]#t*ee}: one two  four five six

${MY_ARRAY[@]##t*}: one   four five six
${MY_ARRAY[@]##t*ee}: one two  four five six

${MY_ARRAY[@]%*ee}: one two thr four five six
${MY_ARRAY[@]%t*ee}: one two  four five six
```

In the execution you can see that the first pattern (“`*ee`”) matches the shortest string ending in “`ee`” which happens to be the last two letters of the third element of the array (with value “`three`”).

In the case of the second pattern (“`t*ee`”) the pattern will match the whole string of the third element. The result will be that the element in question will be removed.

In the next section we will match the longest pattern starting from the back.

#### <b>Longest match from the back of the string</b>

The third approach is by using “`${MY_ARRAY[@]%%<pattern>}`”. This way is going to search for the elements in the array that match the pattern given the **longest** starting from the **back of the string**.

The previous statement means that once you provide the pattern, Bash will go element by element and, for each element, will try to match the pattern provided starting from the end of the string. Then it will remove the pattern found once it finds the longest match.

Same as in the previous cases we will write a new script comparing this way of deleting items (or parts of items) of arrays to the previous ones.

```bash
 1 #!/usr/bin/env bash
 2 #Script: index_array_delete_pattern_back_longest.sh
 3 MY_ARRAY=( one two three four five six )
 4 echo "Content: ${MY_ARRAY[@]}"
 5 # Shortest match from front of string
 6 echo "\${MY_ARRAY[@]#t*}: ${MY_ARRAY[@]#t*}"
 7 echo "\${MY_ARRAY[@]#t*ee}: ${MY_ARRAY[@]#t*ee}"
 8 # Longest match from the front of string
 9 echo -e "\n\${MY_ARRAY[@]##t*}: ${MY_ARRAY[@]##t*}"
10 echo "\${MY_ARRAY[@]##t*ee}: ${MY_ARRAY[@]##t*ee}"
11 # Shortest match from the back of string
12 echo -e "\n\${MY_ARRAY[@]%*ee}: ${MY_ARRAY[@]%*ee}"
13 echo "\${MY_ARRAY[@]%t*ee}: ${MY_ARRAY[@]%t*ee}"
14 # Longest match from the back of string
15 echo -e "\n\${MY_ARRAY[@]%%*e}: ${MY_ARRAY[@]%%*e}"
16 echo "\${MY_ARRAY[@]%%f*e}: ${MY_ARRAY[@]%%f*e}"
```

We added lines 13 to 16. When we run the script the following appears in your terminal window.

```txt
$ ./index_array_delete_pattern_back_longest.sh
Content: one two three four five six
${MY_ARRAY[@]#t*}: one wo hree four five six
${MY_ARRAY[@]#t*ee}: one two  four five six

${MY_ARRAY[@]##t*}: one   four five six
${MY_ARRAY[@]##t*ee}: one two  four five six

${MY_ARRAY[@]%*ee}: one two thr four five six
${MY_ARRAY[@]%t*ee}: one two  four five six

${MY_ARRAY[@]%%*e}:  two  four  six
${MY_ARRAY[@]%%f*e}: one two three four  six
```

In the execution you can see that with the first pattern (“`*e`”), half of the elements in the array were removed. This is because the elements “`on`**e**”, “`thre`**e**” and “`fiv`**e**” match the pattern of having one single character “`e`” at the very end of the string.

In the case of the second pattern (“`f*e`”), there is only one element that starts with the “`f`” character, has zero or more characters after it and ends with the character “`e`”.

Although in the previous sections we have been using indexed arrays, this way to delete elements can be applied as well to the values of an associative array<a id="footnote-1-ref" href="#footnote-1" style="font-size:x-small">[1]</a>.


### Substring replacement with regular expressions

In the previous section we learnt several ways to delete elements from an array using regular expressions. We also learnt that in some cases we could remove part of the string.

In this section we are going to learn how to replace parts of the string (and even delete it) based on a regular expression.

Same as in the previous section we will be focused on indexed arrays, but the same can be applied to the values of associative arrays.

There are 4 ways to replace with regular expressions, which are:
* Replace first occurrence
* Replace all occurrences
* Replace beginning occurrences of string
* Replace ending occurrences of string

#### <b>Replace first occurrence</b>

To be able to replace the **first occurrence** of a pattern in a string element of an array we need to use the following syntax: “`${MY_ARRAY[@]/<pattern>/<replacement>}`”.

Let's give it a try with the following script.

```bash
 1 #!/usr/bin/env bash
 2 #Script: index_array_replace_pattern_first.sh
 3 MY_ARRAY=( one two three four five six )
 4 echo "Content: ${MY_ARRAY[@]}"
 5 # Replace first occurrence
 6 echo -e "\nReplace first occurrence"
 7 echo "\${MY_ARRAY[@]/e/X}: ${MY_ARRAY[@]/e/X}"
 8 echo "\${MY_ARRAY[@]/e*/X}: ${MY_ARRAY[@]/e*/X}"
```

In the previous script we are using two different patterns. When we run the script the following is printed in the terminal window.

```txt
$ ./index_array_replace_pattern_first.sh
Content: one two three four five six

Replace first occurrence
${MY_ARRAY[@]/e/X}: onX two thrXe four fivX six
${MY_ARRAY[@]/e*/X}: onX two thrX four fivX six
```

The first pattern (“`e`”), which is in line 7 of the script, will replace the first occurrence of the character “`e`” with the character “`X`” starting from the beginning of the string (from the left). This will affect one single character in several elements of the array.

The second pattern (“`e*`”), found in line 8 of the script, will replace the first occurrence of the pattern with the character “`X`” starting from the beginning of the string. In this case, as the pattern contains the character “`*`” it will match the rest of the string starting from the first character “`e`”. This will affect more than a single character in several elements of the array.

In the next section we will learn how to replace all occurrences given a pattern.


#### <b>Replace all occurrences</b>

To be able to replace **all occurrences** of a pattern in a string element of an array we need to use the following syntax: “`${MY_ARRAY[@]/<pattern>/<replacement>}`”.

Let's write another script so that we can see how this way of replacement is different from the previous one.

```bash
 1 #!/usr/bin/env bash
 2 #Script: index_array_replace_pattern_all.sh
 3 MY_ARRAY=( one two three four five six )
 4 echo "Content: ${MY_ARRAY[@]}"
 5 # Replace first occurrence
 6 echo -e "\nReplace first occurrence"
 7 echo "\${MY_ARRAY[@]/e/X}: ${MY_ARRAY[@]/e/X}"
 8 echo "\${MY_ARRAY[@]/e*/X}: ${MY_ARRAY[@]/e*/X}"
 9 # Replace all occurrences
10 echo -e "\nReplace all occurrences"
11 echo "\${MY_ARRAY[@]//e/X}: ${MY_ARRAY[@]//e/X}"
12 echo "\${MY_ARRAY[@]//e*/X}: ${MY_ARRAY[@]//e*/X}"
```

We added lines 9 to 12 to our script. We are using the same two patterns as before.

With the first pattern (“`e`”) on line 11, Bash will replace every single instance of the pattern with the character “`X`”. This means every single character “`e`” will be replaced with the character “`X`”.

With the second pattern (“`e*`”) on line 12, Bash will replace every single instance of the pattern with the character “`X`”. In this case, every sequence of characters that starts with the character “`e`” will be replaced with a single character “`X`”.

When you run the previous script you will get the following result in your terminal window.

```txt
$ ./index_array_replace_pattern_all.sh
Content: one two three four five six

Replace first occurrence
${MY_ARRAY[@]/e/X}: onX two thrXe four fivX six
${MY_ARRAY[@]/e*/X}: onX two thrX four fivX six

Replace all occurrences
${MY_ARRAY[@]//e/X}: onX two thrXX four fivX six
${MY_ARRAY[@]//e*/X}: onX two thrX four fivX six
```

A special use case of this syntax is that we can use it to remove elements from the array as well. To be able to do this we need to adjust the pattern to match the elements we want to remove and use an empty string as replacement.

Let's see how it is done with the following script.

```bash
 1 #!/usr/bin/env bash
 2 #Script: index_array_replace_pattern_all_delete.sh
 3 MY_ARRAY=( one two three four five six )
 4 echo "Content: ${MY_ARRAY[@]}"
 5 # Replace first occurrence
 6 echo -e "\nReplace first occurrence"
 7 echo "\${MY_ARRAY[@]/e/X}: ${MY_ARRAY[@]/e/X}"
 8 echo "\${MY_ARRAY[@]/e*/X}: ${MY_ARRAY[@]/e*/X}"
 9 # Replace all occurrences
10 echo -e "\nReplace all occurrences"
11 echo "\${MY_ARRAY[@]//e/X}: ${MY_ARRAY[@]//e/X}"
12 echo "\${MY_ARRAY[@]//e*/X}: ${MY_ARRAY[@]//e*/X}"
13 # Removal of elements
14 echo -e "\nRemoval of elements"
15 echo "\${MY_ARRAY[@]//e/}: ${MY_ARRAY[@]//e/}"
16 echo "\${MY_ARRAY[@]//f*/}: ${MY_ARRAY[@]//f*/}"
```

If you pay attention, this script is the same as before but we added lines from 13 to 16.

The first pattern (“`e`”) in line 15 will remove individual characters of the elements in the array, while the second pattern (“`f*`”) in line 16 will remove two elements of the array.

If you run the previous script you will see the following result in your terminal window.

```txt
$ ./index_array_replace_pattern_all_delete.sh
Content: one two three four five six

Replace first occurrence
${MY_ARRAY[@]/e/X}: onX two thrXe four fivX six
${MY_ARRAY[@]/e*/X}: onX two thrX four fivX six

Replace all occurrences
${MY_ARRAY[@]//e/X}: onX two thrXX four fivX six
${MY_ARRAY[@]//e*/X}: onX two thrX four fivX six

Removal of elements
${MY_ARRAY[@]//e/}: on two thr four fiv six
${MY_ARRAY[@]//f*/}: one two three   six
```

In the next two sections we will learn how to do replacements targeting specifically the beginning and the ending of the string.


#### <b>Replace beginning occurrences of string</b>

To be able to replace occurrences at the beginning of a string we need to use the following syntax: “`${MY_ARRAY[@]/#<pattern>/<replacement>}`”.

What will happen is that Bash will go element by element and will replace the substring that matches the pattern provided, that is **at the beginning of the string**, with the replacement provided.

Let’s see how it works with an example by modifying our previous script.

```bash
 1 #!/usr/bin/env bash
 2 #Script: index_array_replace_pattern_front_beginning.sh
 3 MY_ARRAY=( one two three four five six )
 4 echo "Content: ${MY_ARRAY[@]}"
 5 # Replace first occurrence
 6 echo -e "\nReplace first occurrence"
 7 echo "\${MY_ARRAY[@]/e/X}: ${MY_ARRAY[@]/e/X}"
 8 echo "\${MY_ARRAY[@]/e*/X}: ${MY_ARRAY[@]/e*/X}"
 9 # Replace all occurrences
10 echo -e "\nReplace all occurrences"
11 echo "\${MY_ARRAY[@]//e/X}: ${MY_ARRAY[@]//e/X}"
12 echo "\${MY_ARRAY[@]//e*/X}: ${MY_ARRAY[@]//e*/X}"
13 # Removal of elements
14 echo -e "\nRemoval of elements"
15 echo "\${MY_ARRAY[@]//e/}: ${MY_ARRAY[@]//e/}"
16 echo "\${MY_ARRAY[@]//f*/}: ${MY_ARRAY[@]//f*/}"
17 # Replace front-end occurrences of string
18 echo -e "\nReplace front-end occurrences of string"
19 echo "\${MY_ARRAY[@]/#e/XY}: ${MY_ARRAY[@]/#e/XY}"
20 echo "\${MY_ARRAY[@]/#f/XY}: ${MY_ARRAY[@]/#f/XY}"
```

In this case we have added lines 17 to 20 to our script. In line 19 we are trying to replace the character “`e`” at the beginning of the string with “`XY`”. In line 20 we are trying to replace the character “`f`” at the beginning of the string with “`XY`”.

When you run the previous script you will see the following output in the terminal window.

```txt
$ ./index_array_replace_pattern_front_beginning.sh
Content: one two three four five six

Replace first occurrence
${MY_ARRAY[@]/e/X}: onX two thrXe four fivX six
${MY_ARRAY[@]/e*/X}: onX two thrX four fivX six

Replace all occurrences
${MY_ARRAY[@]//e/X}: onX two thrXX four fivX six
${MY_ARRAY[@]//e*/X}: onX two thrX four fivX six

Removal of elements
${MY_ARRAY[@]//e/}: on two thr four fiv six
${MY_ARRAY[@]//f*/}: one two three   six

Replace front-end occurrences of string
${MY_ARRAY[@]/#e/XY}: one two three four five six
${MY_ARRAY[@]/#f/XY}: one two three XYour XYive six
```

As you can see from the execution the line 19 from our script will have no effect at all as there is not a single element in the array that begins with the character “`e`”. Line 20, however, will affect a couple of elements in the array (elements with values “`four`” and “`five`”) because they start with the character “`f`”.

In the next section we will learn how to replace occurrences of a pattern at the end of a string.

#### <b>Replace ending occurrences of string</b>

To be able to replace occurrences at the beginning of a string we need to use the following syntax: “`${MY_ARRAY[@]/%<pattern>/<replacement>}`”.

What will happen is that Bash will go element by element and will replace the substring that matches the pattern provided, that is **at the end of the string**, with the replacement provided.

Let’s see how it works with an example by modifying our previous script.

```bash
 1 #!/usr/bin/env bash
 2 #Script: index_array_replace_pattern_back_ending.sh
 3 MY_ARRAY=( one two three four five six )
 4 echo "Content: ${MY_ARRAY[@]}"
 5 # Replace first occurrence
 6 echo -e "\nReplace first occurrence"
 7 echo "\${MY_ARRAY[@]/e/X}: ${MY_ARRAY[@]/e/X}"
 8 echo "\${MY_ARRAY[@]/e*/X}: ${MY_ARRAY[@]/e*/X}"
 9 # Replace all occurrences
10 echo -e "\nReplace all occurrences"
11 echo "\${MY_ARRAY[@]//e/X}: ${MY_ARRAY[@]//e/X}"
12 echo "\${MY_ARRAY[@]//e*/X}: ${MY_ARRAY[@]//e*/X}"
13 # Removal of elements
14 echo -e "\nRemoval of elements"
15 echo "\${MY_ARRAY[@]//e/}: ${MY_ARRAY[@]//e/}"
16 echo "\${MY_ARRAY[@]//f*/}: ${MY_ARRAY[@]//f*/}"
17 # Replace front-end occurrences of string
18 echo -e "\nReplace front-end occurrences of string"
19 echo "\${MY_ARRAY[@]/#e/XY}: ${MY_ARRAY[@]/#e/XY}"
20 echo "\${MY_ARRAY[@]/#f/XY}: ${MY_ARRAY[@]/#f/XY}"
21 # Replace back-end occurrences of string
22 echo -e "\nReplace back-end occurrences of string"
23 echo "\${MY_ARRAY[@]/%e/XY}: ${MY_ARRAY[@]/%e/XY}"
24 echo "\${MY_ARRAY[@]/%our/XY}: ${MY_ARRAY[@]/%our/XY}"
25 echo "\${MY_ARRAY[@]/%y/XY}: ${MY_ARRAY[@]/%y/XY}"
```

In this case we have added lines 21 to 25 to our script. In line 23 we are trying to replace the character “`e`” at the end of the string with “`XY`”. In line 24 we are trying to replace the substring “`our`” at the end of the string with “`XY`”. In line 25 we are trying to replace the character “`y`” at the end of the string with “`XY`”.

When you run the script you will the following output in your terminal window.

```txt
$ ./index_array_replace_pattern_back_ending.sh
Content: one two three four five six

Replace first occurrence
${MY_ARRAY[@]/e/X}: onX two thrXe four fivX six
${MY_ARRAY[@]/e*/X}: onX two thrX four fivX six

Replace all occurrences
${MY_ARRAY[@]//e/X}: onX two thrXX four fivX six
${MY_ARRAY[@]//e*/X}: onX two thrX four fivX six

Removal of elements
${MY_ARRAY[@]//e/}: on two thr four fiv six
${MY_ARRAY[@]//f*/}: one two three   six

Replace front-end occurrences of string
${MY_ARRAY[@]/#e/XY}: one two three four five six
${MY_ARRAY[@]/#f/XY}: one two three XYour XYive six

Replace back-end occurrences of string
${MY_ARRAY[@]/%e/XY}: onXY two threXY four fivXY six
${MY_ARRAY[@]/%our/XY}: one two three fXY five six
${MY_ARRAY[@]/%y/XY}: one two three four five six
```

As you can see from the execution the line 23 from our script will affect several elements of the array (the ones with values “`one`”, “`three`” and “`five`” as they all end with the character “`e`”). 

The line 24 from our script will affect one single element (element with value “`four`” as is the only one that ends with the substring “`our`”).

Line 25 from our script will not impact any element (as there is no element ending with the character “`y`”), leaving the array untouched.

Once that we have seen all the possible ways to do a replacement on an array we will learn how to delete an entire array in the next section.

### Delete an entire array

In order to delete a whole array we need to use the built-in command “`unset`” and the name of the array variable without the dollar sign.

Let’s see it in action with an example.

```bash
 1 #!/usr/bin/env bash
 2 #Script: delete_array.sh
 3 MY_ARRAY=( one two three four five six )
 4 echo "Content: '${MY_ARRAY[@]}'"
 5 # Deleting the array
 6 unset MY_ARRAY
 7 echo "Content after deletion: '${MY_ARRAY[@]}'"
```

In the previous script we are declaring an array named “`MY_ARRAY`” on line 3 with 6 elements. Then, on line 6 we are using “`unset`” to remove the contents of the whole array. Last, on line 7, the script shows the content of the array after deleting it.

When you run the previous script you will get the following output in your terminal window.

```txt
$ ./delete_array.sh
Content: 'one two three four five six'
Content after deletion: ''
```

What is basically happening with the deletion is that the variable “`MY_ARRAY`” ceases to exist. 

It’s happening the same thing we saw back in Chapter 4 in the section “[How to delete declared variables?]({{ site.url }}/bash-in-depth/0004-Variables.html#how-to-delete-declared-variables)”.

In the next section we are going to learn how to consult the length of a specific element of the array.

### Consult the length of the i-th element of the array



## Summary


## References


<hr style="width:100%;text-align:center;margin-left:0;margin-bottom:10px;">

<p id="footnote-1" style="font-size:10pt">
1. Up to thre reader to give it a try.<a href="#footnote-1-ref">&#8617;</a>
</p>

