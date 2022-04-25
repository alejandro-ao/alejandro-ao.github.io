
---
title: "Custom Post Types and Taxonomies in Wordpress"
date: 2022-03-15
draft: false
categories: 
- Wordpress development
tags:
- wordpress
- php
---

Custom Post Types (CPT) in Wordpress are just like blog posts, except that you can manipulate them to make them correspond to any type of publication you want. 

For example, let’s say that you have a website where you publish movie reviews. In this case, instead of using the default post type of “blog post” you can create a CPT with the name of movie review. 

This post type would probably have other fields, other than content and thumbnail. You could include fields like “release date”, “box office”, “main actors”, etc. And these fields would be specific to this post type.

This would also allow you to create special taxonomies (like categories or tags) for this CPT. You could categorise them by movie genre or by country, for example. And these taxonomies would be associated with this blogpost only. 

Then, when calling The Loop with all these posts, you would be able to access all this information associated with the custom post type. 

In addition to that, you can have as many CPTs as you want! This way we are able to organise the content of our website more efficiently. 

## How to create a CPT in Wordpress

To create a CPT in Wordpress, you will need to use the function `register_post_type()` and call it from your `functions.php` file. When organising my `functions.php` file, I often split it into modules to make it easier to organise and maintain. This way I can keep my custom post types declared separately in a `cpt.php` file.

Something interesting that we are also going to see here is the use of custom taxonomies. These are ways to organise (or categorise) your content. By default, Wordpress comes with two types of taxonomies (category and tag). But you can create your own and use them to better organise your content.

Here is n example of the base function I use to generate the CPTs that I need as well as their taxonomies.

```php
function register_custom_post_types()
{

    $default_cpt_args = [
        'hierarchical'          => false,  
        'public'                => true,   
        'show_ui'               => true,   
        'show_in_menu'          => true,   
        'show_in_admin_bar'     => true,   
        'show_in_nav_menus'     => true,   
        'show_in_rest'          => true,   
		'menu_position'         => 3,      
        'can_export'            => true,   
        'exclude_from_search'   => false,  
        'publicly_queryable'    => true,   
        'capability_type'       => 'page',
    ];

    $default_tax_args = [
        'hierarchical'               => true,  
        'public'                     => true,  
        'show_ui'                    => true,  
        'show_admin_column'          => true,  
        'show_in_nav_menus'          => true,  
        'show_tagcloud'              => false, 
        'show_in_rest'               => true,  
    ];

    register_post_type('my_post_type', array_merge($default_cpt_args, [
        'menu_icon'             => 'dashicons-universal-access',   
        'label'                 => 'My Post Type',                 
        'labels'                => ['post, posts'],                
        'supports'              => ['title', 'editor', 'thumbnail', 'comments'],
        'taxonomies'            => ['category'],                   
        'has_archive'           => 'posts',                        
    ]));

    register_taxonomy('genre', ['post', 'other_cpt', 'music album'], array_merge($default_tax_args, [
        // other labels
    ]));
}

add_action( 'init', 'register_custom_post_types', 0 );

````


What you see here is the following: 

1. First, we create an array of default arguments that we will include in all our CPTs. 
2. The we create another array with the default arguments of the taxonomy that we want to associate to our CPT. 
3. We call the `register_post_type($post_type, $args)` function to create the custom post type.
4. We cal the `register_taxonomy($name, $cpts)` function to do the same with our taxonomy. As an argument, we pass the CPTs that will support this taxonomy.
5. We put all that in a function and we call it with an action hook. We hook it to the `init` stage of the Wordpress cycle.
