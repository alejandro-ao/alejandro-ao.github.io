
---
title: "How to create a custom plugin in Wordpress"
date: 2022-04-01
draft: false
categories: 
- Wordpress development
tags:
- wordpress
- php
---

Learning how to create Wordpress plugins from scratch is an essential part of Wordpress development. Here we see how to use PHP to code a plugin in wht Wordpress framwork.

## What is a plugin

In short, a plugin is just an extension of the functionalities of Wordpress. It can contain any functions that will run at any specified point in the Wordpress cycle.

There is a main idea behind developing plugins to adapt your website to your needs. In an ideal world, you want every additionnal functionnality as far away from the Wordpress core as possible.

You want to add a custom interactive map using Google Maps API? Create a plugin and add it to your website. 

This comes with the added advantage that you can reuse the code for other projects, since plugins are supposed to be independent of the website.

## Plugin header requirements

In order for Wordpress to recognise your plugin, you will want to create a folder with the name of your plugin in `/wp-content/plugins`. Then, inside it, in a PHP file, add the following header (only the “plugin name” line is essential, but the rest will give valuable information about your plugin):

```php
<?php
/**
 * Plugin Name:       My Basics Plugin
 * Plugin URI:        https://example.com/plugins/the-basics/
 * Description:       Handle the basics with this plugin.
 * Version:           1.10.3
 * Requires at least: 5.2
 * Requires PHP:      7.2
 * Author:            John Smith
 * Author URI:        https://author.example.com/
 * License:           GPL v2 or later
 * License URI:       https://www.gnu.org/licenses/gpl-2.0.html
 * Update URI:        https://example.com/my-plugin/
 * Text Domain:       my-basics-plugin
 * Domain Path:       /languages
 */
```

Once that is set up, you can start adding any functionalities you want to your website. You can add any piece of code that you may develop following the information in this website or in the [official documentation](https://developer.wordpress.org/). 

## Reference your plugin

You might need to use this function when enqueuing your plugin’s scripts and styles. In that case you would need to use `wp_enqueue_script()` or `wp_enqueue_style()` and pass `plugins_url()` as your parameter. 

```php
plugins_url( 'myscript.js', __FILE__ );
// example.com/wp-content/plugins/myplugin/myscript.js
```

## Other useful functions

This kind of functions also exist for themes:

```php
get_template_directory_uri()
get_stylesheet_directory_uri()
get_stylesheet_uri()
get_theme_root_uri()
get_theme_root()
get_theme_roots()
get_stylesheet_directory()
get_template_directory()
```

For your home path:

```php
home_url()
get_home_path()
```

From Wordpress
```php
admin_url()
site_url()
content_url()
includes_url()
wp_upload_dir()
```