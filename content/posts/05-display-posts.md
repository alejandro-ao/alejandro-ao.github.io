
---
title: "How to display Recent posts in Wordpress"
date: 2022-03-01
draft: false
categories: 
- Wordpress development
tags:
- wordpress
- php
---

When working within the Wordpress framework you will soon be confronted with the necessity of displaying the posts that are saved in your database. Be it for an archive page or because you want to display your latest posts in your homepage. 

Remember that when I talk about Wordpress posts, I do not necessarily mean that we are talking about a conventional blog. Wordpress has grown to be more than a blogging platform. So what we call “posts” by convention can actually be any type of content. 

There are many different ways to get the posts of a Wordpress website. Here are the three methods I know, the two first ones being the ones I use the most: `new WP_Query`, `get_posts()` and `query_post()`

## The `WP_Query` class

This is the most conventional way to get the posts from our Wordpress database. You first need to initialise an instance of your Wordpress Query, which contains the data of the posts you ask it for.

Once initialised and called, you will need to test that it does indeed contains posts with an `if` statement to prevent server errors. Then you will loop through all its elements. 

Note that you can filter which elements you want to get in your `WP_Query` with the array you pass to it as an argument. For example, you might want to get only the three latest posts or maybe you want to get only the posts from your Custom Post Type *Movie Reviews*. That array is where you pass all this information. 

Here is an example.

```html
<?php
   $the_query = new WP_Query( array(
     'category_name' => 'news',
      'posts_per_page' => 3,
      'post_type' => 'your_cpt_name'
   ));
?>

<?php if ( $the_query->have_posts() ) : ?>
  <?php while ( $the_query->have_posts() ) : $the_query->the_post(); ?>

    <?php the_title(); ?>
    <?php the_excerpt(); ?>
		<?php the_content(); ?>
	
  <?php endwhile; ?>
  <?php wp_reset_postdata(); ?>

<?php else : ?>
	
<?php endif; ?>
```

## The `get_posts()` function

This is probably a more semantically friendly way to call the posts. The array you pass as an argument contains the same parameters as that of the `WP_Query`.

```php
$args = array(
	'numberposts'	=> 20,
	'category'		=> 4
);
$my_posts = get_posts( $args );

if( ! empty( $my_posts ) ){
	foreach ( $my_posts as $post ){
		echo get_permalink( $post->ID ); 
		echo $post->post_title;
	}
}
```

## What is the difference between `query_posts` `WP Query` and `get_posts` ?

I found this extremely good explanation on [StackOverflow](https://wordpress.stackexchange.com/questions/1753/when-should-you-use-wp-query-vs-query-posts-vs-get-posts) about  the difference between the three methods for getting posts off the database:

> [`query_posts()`](http://codex.wordpress.org/Function_Reference/query_posts) is overly simplistic and a problematic way to modify the main query of a page by replacing it with new instance of the query. It is inefficient (re-runs SQL queries) and will outright fail in some circumstances (especially often when dealing with posts pagination). Any modern WP code should use more reliable methods, like making use of the [`pre_get_posts`](https://wordpress.stackexchange.com/questions/50761/when-to-use-wp-query-query-posts-and-pre-get-posts) hook, for this purpose. TL;DR **don't use query_posts() ever**.
> 

> [`get_posts()`](http://codex.wordpress.org/Template_Tags/get_posts) is very similar in usage and accepts the same arguments (with some nuances, like different defaults), but returns an array of posts, doesn't modify global variables and is safe to use anywhere.
> 

> [`WP_Query`](http://codex.wordpress.org/Function_Reference/WP_Query) is the class that powers both behind the scenes, but you can also create and work with your own instance of it. A bit more complex, fewer restrictions, also safe to use anywhere.
> 

*[See for more info.](https://wordpress.stackexchange.com/questions/1753/when-should-you-use-wp-query-vs-query-posts-vs-get-posts)*

## Manipulating posts data inside The Loop

The loop you create when displaying all the posts is called, among Wordpress-ers, *The Loop*. You might hear it referred to it that way. So now you know what it is.

A great thing about it is that you are not actually looping though simple arrays with your post data. You are actually looping through objects that contain data and methods. This means that you can call these methods to manipulate the data form the post and display it how you want. 

Some examples of the methods you can use are:

- `the_title()` : displays the title of the current post in the Loop.
- `the_content()` : displays the content of the post.
- `get_the_ID()` : retrieves the ID of the post.

Feel free to check other functions that you can use inside The Loop to enhance your control over your data. [Here is a big list](https://developer.wordpress.org/reference/files/wp-includes/post-template.php/).

For more info:

- [https://kinsta.com/blog/wordpress-get_posts/](https://kinsta.com/blog/wordpress-get_posts/)
- [https://developer.wordpress.org/reference/classes/wp_query/](https://developer.wordpress.org/reference/classes/wp_query/)