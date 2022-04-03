---
title: "How to add a favicon to your Wordpress website"
date: 2022-01-15
draft: false
---

There are better and worse ways to add a favicon to your Wordpress website. In regular websites, all you would need to do to add a favicon is to add the following tag to your header:

```html
<link rel="icon" href="url-of-your-favicon.ico"/>
```

We could, of course, just plug this tag to our website’s header. But that wouldn’t be the Wordpress way of doing things.

## How to add a favicon the Wordpress way

The better way of doing this in Wordpress is by adding it to our Wordpress cycle. To do this, we create a function that returns the given tag and we add it to the `wp_head` hook. This function and it’s hook, of course, need to be added in the `functions.php` file.

Like this: 

```html
function add_favicon(){ 
		ob_start(); ?>
        <link rel="icon" type="image/png" href="https://cdn.yourwebsite.com/favicon-16x16.png" sizes="16x16">
		<link rel="icon" type="image/png" href="https://cdn.yourwebsite.com/favicon-32x32.png" sizes="32x32">
		<link rel="icon" type="image/png" href="https://cdn.yourwebsite.com/favicon-96x96.png" sizes="96x96">
        <link rel="apple-touch-icon" href="https://cdn.yourwebsite.com/favicon.ico">
        <?php return ob_get_clean();
}
add_action('wp_head','add_favicon');
```

In the example above, we created the function `add_favicon` that returns four tags: three of type “icon” for different screen sizes, and a last one for Apple devices that allow you to add a website to your home screen and make it look like an app.

We then hooked this function to the `wp_head` hook. 

This method allows you to be more organised and to centralise all your website’s settings in the `functions.php` file.