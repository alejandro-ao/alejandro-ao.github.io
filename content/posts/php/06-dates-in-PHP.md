---
title: "How to work with dates in PHP"
date: 2022-04-25
draft: false
categories: 
- PHP Crash Course
tags:
- php
---

## How to date in PHP
You can use the `date` function to get the current date.

```php
// Current : 
date('d'); // day
date('m'); // month
date('Y'); // year
date('l'); // day of week
date('h'); // hours
date('i'); // minutes
date('s'); // seconds
date('a'); // am or pm

echo date('Y/m/d');  // to print in a format
```

## Define timezone

Just remember to set your timezone before calling the `date()` function so that it fits with the content you want to use.

```php
 date_default_timezone_set('Europe/Paris');
```

### Timestamps

Timestamps are an easy way to 

Create a timestamp with number of seconds since *epoch time*. *Epoch time* or *Unix epoch* refers to the number of seconds that have passed since January 1st 1970. It is an arbitrary date used very often by programmers to add or subtract dates easily. 

To create a timestamp, you can use the function `mktime()` and pass it the arguments as you can see below. 

Note that if you run the `date()` function on the `mktime()` functio, you turn it into a date.

```php
$timestamp = mktime($seconds, $minutes, $hours, $day, $month, $year);
echo date("m-d-Y", $timestamp); // takes timestamp turns it into date
```

### String to timestamp

PHP is intelligent enough that you can pass it a string with a date and it will return the timestamp when you use the function `strtotime()`.

```php
echo strtotime('January 10, 2020'); // will print unix timestamp
```

This should allow you to manipulate dates and even build an interactive calendar. After all, all you need is to be able to add and subtract dates by turning them into timestamps and then turning them back to dates.