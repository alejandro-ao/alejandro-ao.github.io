---
title: "Variables, constants and data types in PHP"
date: 2022-04-15
draft: false
categories: 
- PHP Crash Course
tags:
- php
---

Variables and constants in PHP are preceded by the dollar sign `$`.  Convention dictates that you should define your constants with uppercase and your variables with camelCase.  Here is how you declare variables and constants :

```php
// to create a variable
$myVariable = 23;  // this assigns the value 23 to your variable

// to create a constant
define("MY_CONSTANT", "This is the value of my constant");
```

With constants, there is a third parameter that takes a boolean value to set if your constant is case-sensitive or not. [Here](https://www.w3schools.com/php/php_constants.asp) is a more detailed explanation about PHP constants.

When you declare either a variable or a constants, there is no need to declare their type. There are, however, several data types in PHP: *strings, integers, floats, booleans, arrays, objects, NULL, and resource*. 

Here is a list of the available data types in PHP: 

## PHP Strings

Strings are sequences of characters. They are surrounded by simple or double quotes in PHP. You can concatenate them by using a dot `.`  between strings. 

```php
$myString = 'Hello,';
$anotherString = "World!";
echo $myString . ' ' . $anotherString;  // 
```

Note that in this example, we added a space between both strings by adding a `' '` between them. Another way to concatenate multiple variables without having to do this is to use double quotes in a string and simply plug in the variables inside it. Note that if you do not use double quotes the variables will not be parsed.

```php
$to = 'Mayor Tom';
$from = 'Ground Control';
echo "This is $form to $to";  // 'This is Ground Control to Mayor Tom'
```

## PHP Integers

These can be positive or negative and do not include a decimal point.

```php
$num1 = 2;
$num2 = 3;
echo $num1 + $num2;  // 5
```

## PHP Floats

In short, floats in PHP are any number that contains a decimal point.

```php
$float1 = 1.5;
$float2 = 4.0;
echo $float1 + $float2;  // 5.5
```

## PHP Booleans

Can be either `true` or `false`.

```php
$var1 = true;
$var2 = false;
if ($var1) {
	echo 'This is true';  // will print 'this is true'
}
```

## PHP Arrays

Arrays are a particular type of element in PHP. They are of two types: simple or nominative.

### Simple arrays

Simple arrays work like arrays in any other language and they can be declared with `array()`.

```php
$greatSingers = array('Robert Plant', 'Jim Morrison', 'Axl Rose');
```

It has been a while since PHP introduced square bracket syntax for declaring arrays. This might be more familiar to you if you haven't worked with PHP before:

```php
$greatGuitarists = ['Joe Bonamassa', 'Steve Vai', 'Joe Satriani'];
```

### Associative arrays

This kind of arrays work like dictionaries in other languages. They take value pairs and are declared like simple arrays, but the value pairs are created with `=>`. And you can call the values with bracket notation by position or by key name.

```php
$myBand = array(
	'vocals' => 'John',
	'guitar' => 'George',
	'bass' => 'Paul',
	'drums' => 'Ringo'
);

echo $myBand[0];  // 'John'
echo $myBand['drums'];  // 'Ringo'
```

And you can also create multidimensional arrays (arrays inside arrays).

```php
$myBand = [
	'vocals' => 'John',
	'guitar' => 'George',
	'bass' => 'Paul',
	'drums' => 'Ringo'
	'albums' => ['Please Please Me', 'Revolver', 'Rubber Soul']
];
```

### PHP Objects

Objects in PHP work like objects in other programming languages. You need to first create a class with the properties and the methods to implement. To create a class, you use the keyword `class`. The to create an instance of the class, you use the keyword `new`.

As shown in the example below, you can use the keyword `$this` to refer to the object itself. This way, if you want a method to modify a property of the object, you would do like we did with the method `set_name()`.

```php
class SocialNetwork {
	// Properties 
	public $name;
	public $logo;
	private $emailCEO;

	// Methods
	function set_name($name) {
		$this -> name = $name;  // this is how you access object properties
	}
	function get_name() {
		return $this -> name;
	}
	public function contactCEO($message) {
		sendEmail($message, $emailCEO);
	}
}

$insta = new SocialNetwork;
$insta->set_name('instagram');
echo $insta->get_name()  // 'instagram'
$insta->contactCEO('Hello Mr CEO!'); 
```

The `public` and `private` modifiers define functions and properties that can be accessed from the outside and those that can only be used internally. This is important for security reasons. Some methods might use other internal functions that should not be available to anyone who calls the object from the outside. In the example below, the property `$CEOAdress` is private even though it can be used by the method `$contactCEO` (supposing the `sendEmail()` function exists).

## PHP NULL value

Variables that have no assigned value are of type NULL.