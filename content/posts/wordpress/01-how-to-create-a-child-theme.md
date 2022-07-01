---
title: "How to create a child theme in Wordpress using PHP"
date: 2021-11-15
draft: false
categories: 
- Wordpress development
tags:
- wordpress
- php
---

Do you want to learn how to create a child theme in Wordpress without a plugin? That is, creating a WordPress child theme using only PHP.

Creating a child-theme in Wordpress is extremely useful. It allows you to modify your theme while still getting updates from the parent theme. But first, let’s what it is exactly.

## What is a child-theme in Wordpress?
Imagine you just bought a very nice theme, but you would like to add a certain feature that does not come with it by default.

An intuitive approach could be to go to your theme files and add the feature yourself. But if you did that, any update to your theme would erase your custom feature.

That’s why we use child-themes. They allow us to create a theme that calls all the code from its parent and adds features on top of it. This way, the parent can change and be updated, and your child-theme will always build on top of it.

## How to create a child-theme without a plugin
The easiest way of creating a child theme is probably by using a plugin that takes care of all of this by default. But if you want your code to be as clean as possible and as little dependent as possible of foreign plugins, you can add a child-theme with code.

Here are three steps to follow:

1. Create a new folder with the name of your child theme

2. Add the following code to a style.css file inside it, customise it:
```css
/**
Theme Name:   Your child theme name
Theme URI:    Your theme's website
Description:  Your description 
Author:       Your name
Author URI:   Your website
Template:     Folder name of the parent theme
Version:      1.0.0
Text Domain:  mythemechild
*/
```

3. Create a functions.php file and add this:

```php
<?php
add_action( 'wp_enqueue_scripts', 'enqueue_parent_styles' );
function enqueue_parent_styles() {
   wp_enqueue_style( 'parent-style', get_template_directory_uri().'/style.css' );
}
```


That’s it, you have created a child theme that includes all the styles of your parent theme and which communicates with Wordpress effectively to let it know that that it is indeed a child theme.

Do note that, even though it is just a comment, step 2 is very important. It tells Wordpress that the current theme is effectively a child-theme and it documents its parent theme.