---
title: "How to enqueue styles and scripts in Wordpress"
date: 2021-12-15
draft: false
categories: 
- Wordpress development
tags:
- wordpress
- php
---

## How to enqueue scripts and styles in Wordpress

As you might know, when we are using Wordpress, we don’t add our scripts and styles directly in the header like this:

```html
<script src="my_script.js"></script>
```

Of course, you can do it that way, but there is another way, the right way. 

The right way to add any dependencies on Wordpress, be it scripts or stylesheets, is to enqueue them in the Wordpress cycle. The reason for this is that Wordpress itself includes different scripts and stylesheets when it loads. 

By using a function to enqueue your own scripts and stylesheets into the same cycle, you make sure that your dependencies are loaded when you need them to load and not before or after any other scritpts that Wordpress loads. This way you can make sure that all your dependencies are well organised and that they only load when you need them to.

These are the functions that you use to enqueue a script or a stylesheet:

```php
wp_enqueue_script($script_name, $location, $dependencies, $version, $load_on_footer);
wp_enqueue_style($style_name, $location, $dependencies, $version, $load_on_footer);
```

A good habit is to add all your enqueued scripts and styles inside a function that we write in the `functions.php` file. We then hook it to the Wordpress cycle with a `wp_enqueue_scripts` hook. Like this:

```php
function add_theme_scripts(){
    wp_enqueue_style('style', get_template_directory_uri() . '/assets/css/main.min.css', [], 1.0.1, false);

    wp_enqueue_script('jquery');
    wp_enqueue_script('script', get_template_directory_uri() . '/assets/js/main.min.js', ['jquery'], 2.0, true);
}
add_action('wp_enqueue_scripts', 'add_theme_scripts');
```

Note that in the example above we added `jquery` to the dependencies of our script. That means that our script will always be inserted after jquery. 

## Help, my scripts are not updating!

From time to time you will change your stylesheet or your script and find that they are not updating on save. What is happening?

The problem is that when wordpress loads a script or a stylesheet, it stores its version as well. That is the fourth argument of the `wp_enqueue_script` hook. And if you don’t update it everytime you change your file, your changes will not be added. 

To fix this, we can set the version of the script to its `filemtime`. This is its age in seconds since the Unix Epoch (January 1, 1970). That way, its version will be constantly updating.

Here is an example:

```php
// Add root CSS
wp_enqueue_style(
	'root-css',
	get_stylesheet_directory_uri() . '/style.css',
	array('bootstrap-css', 'swiper-css'),
	filemtime(get_stylesheet_directory() . '/style.css')
);
```

Note that we're using `get_stylesheet_directory_uri()` for the file location but `get_stylesheet_directory()` for the timestamp. This is because the function `filemtime()` takes the file path as argument, not its url.

## How to enqueue styles and scripts to the admin dashboard in Wordpress

If you want to edit the admin dashboard to have a customised feel for your website admin, you can add css rules to the existing classes of the dashboard elements. But enqueuing them to the `init` hook won’t work. 

You will need to enqueue them in the `admin_enqueue_scripts` hook:

```php
add_action( 'admin_enqueue_scripts', 'my_admin_style');

function my_admin_style() {
  wp_enqueue_style( 'admin-style', get_stylesheet_directory_uri() . '/admin-style.css' );
}
```