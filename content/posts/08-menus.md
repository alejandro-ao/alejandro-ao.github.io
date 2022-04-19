
---
title: "How to register Wordpress menus"
date: 2022-04-05
draft: false
categories: 
- Wordpress development
tags:
- wordpress
- php
---

In this article we are going to answer the following question: how to register a menu in Wordpress?  

If you are used to web development, you will have already worked with hard-coded menus. That is, the only way to update the elements in your menu is through your code. 

Wordpress Admin, on the other hand, allows you to manage the elements of the menus that you can place in different parts of your website. That’s this section that you find on Appearance > Menus.

![Screenshot Menus](/images/menus-wp.png)

Here you can create a menu and choose which elements you want to add to it. Then, you can select its “Display location”. This is the setting that you select at the bottom of “Menu Settings”. 

For example, let’s say your theme has two available display locations: *Primary menu* (the menu you can find at the header of your website) and *Secondary Menu* (which you can find underneath the primary menu). 

This means that when you create a menu and set its position to *Primary menu*, the elements that you added to it will show in the header. And when you set another menu to *Secondary menu* it will be shown underneath. 

This way you can edit all the elements of your menu from WP Admin.

But what happens if you want to add a menu to your footer? You would need to create a *display location* and include in your footer.

You will need to register the new menu *display location* and include it in your `footer.php` file.

# How to register a menu in Wordpress with PHP

To register one menu:

```php
register_nav_menu( string $location, string $description )
```

To register multiple menus

```php
function wpb_custom_new_menu() {
  register_nav_menus(
    array(
      'my-footer-menu' => __( 'My Footer Menu' ),
      'extra-menu'     => __( 'Extra Menu' ),
      'another-menu'   => __( 'Another Menu' )
    )
  );
}
add_action( 'init', 'wpb_custom_new_menu' );
```

To display it wherever you want to insert it.

*Theme location* is the the argument used to select which meny you want to insert. For example, above we registered three menus (”My Footer Menu”, “Extra Menu” and “Another Menu”). The admin will be able to add elements to every one of these menus. To display “My Footer Menu”, you will need to add the following code to the footer of your website: