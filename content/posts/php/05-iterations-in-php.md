---
title: "The foreach loop in PHP"
date: 2022-04-19
draft: false
categories: 
- PHP Crash Course
tags:
- php
---

Arrays are probably one of the structures that you will work the most with. They exist for one main reason: to be able to iterate through their elements and perform operations with them. The easiest way to iterate through arrays in PHP is using the `foreach` loop. 

## The syntax

`foreach` loops look like similar to functions in their declaration method. Here is the syntax:

```php
$states = ['Alabama', 'Texas', 'California'];
foreach ($states as $state) {
	echo $state;
}
```

Foreach loops take every single element in your array and perform the same operation on them. The function takes two parameters separated by the keyword `as`. The first parameter is the name of the array and second one is the variable that you will use to perform the operations. Here is another example that is self-explanatory:

```php
$numbers = [2, 3, 4];
foreach ($numbers as $number) {
	return $number * 2;
}

/* 
will return:
>> 4
>> 6
>> 8
*/ 
```

## Where to use a foreach loop

You will certainly use `foreach` loops in your web development carrer almost every day. Let's say you have a list of images in your server that you want to display as a gallery. You will call the array with the location of your images and then you would print them in the block of your gallery. 

Another example is when you want to print out a list of comments in your website. You would take the comments from the database, store them in an array and then iterate through the array to display the elements.

This kind of loops are part of what is called *imperative methods*. They are, however, not always the only option when you want to apply a function to every element of the array. Let's see how to use mapping, filtering and reducing.
