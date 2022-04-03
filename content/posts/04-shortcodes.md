
---
title: "How to create short-codes in Wordpress"
date: 2022-02-15
draft: false
categories: 
- Wordpress development
tags:
- wordpress
- php
---

Short-codes are one of the signatures of Wordpress. They allow you to add functionalities to your website by just adding a few characters in the following format: `[my-shortcode]`. 

Sometimes, you will be asked to add them to your website by some plugins. 

For example, WP Forms provides you with a custom short-code like this one: `[wpforms id="25”]` for each form that you build using your plugin. You will then add this anywhere you want your form to appear. 

## How do short-codes work?

What is happening behind the scenes is that when Wordpress is loading your website’s front, and it gets to your short-code, it will proceed to execute whatever action is linked it in the plugin’s code. 

To better view how that works, let’s create our own short-code:

## How to create your own short-code

Technically, all a short-code does is execute a given function. More often than not, that function returns some HTML code to include in your website. But not necessarily. 

Here is the code to create a short-code:

```php
// CREATE SHORTCODES
function create_demo_content(){
    // Whatever you want to return when you paste the shorcode
		echo 'Hello world';
}

add_shortcode( 'demo_content', 'create_demo_content' );
```

The code above creates a function called `create_demo_content`. It then creates the short-code `[demo-content]` that calls that function whenever you include in your frontend. 

As you can see, we use the WP function `[add_shortcode](https://developer.wordpress.org/reference/functions/add_shortcode/)`. It takes only two arguments:

1. the name of the short-code (this is what will be between the brackets),
2. and the function that it is supposed to call.

## Calling a short-code inside a PHP file

Short-codes are meant to be used in the frontend of your website. That is why they are extremely useful with editors or builders like Gutemberg, Elementor or Divi.

However, sometimes you will find yourself needing to include them in a PHP file when you are coding your website from scratch. To call a short-code inside a PHP file you need to use the function `do_shortcode`:

```php
<?php echo do_shortcode("[shortcode]"); ?>
```

This function returns a string, so don’t forget to `echo` it for its contents to be visible in your website.