
---
title: "How to do Ajax in Wordpress"
date: 2022-04-10
draft: false
categories: 
- Wordpress development
tags:
- wordpress
- php
---

Sometimes we refer to AJAX in somewhat underestimating terms. Since it is everywhere, sometimes beginner programmers see it as a simple technology that should not be too complicated to use. 

That is absolutely not true. AJAX can be extremely complicated for beginners since it involves both front and back-end technologies working together in a particular way. In this tutorial I will teach you how to do AJAX in Wordpress considering you are a beginner familiar with Javascript and PHP.

If you already know what AJAX is and only need the code snippets, you can go directly to implementing AJAX in Wordpress.

**TLDR:** If all you need are the JS and PHP code snippets for your request, here they are:

### With jQuery

```jsx
jQuery.ajax({
    type: "post",
    url: `${window.location.origin}/wp-admin/admin-ajax.php`,
    data: {
      action: "offres_apply_filters",
      all_filters: appliedFilters,  // any JS object
    },
    complete: function (response) {
      console.log(JSON.parse(response.responseText).data);
    },
  });
```

```php
add_action('wp_ajax_offres_apply_filters', 'offres_apply_filters');
add_action('wp_ajax_nopriv_offres_apply_filters', 'offres_apply_filters');

function offres_apply_filters() {
    $current_filters = $_POST['all_filters'];
    wp_send_json_success($current_filters);
}
```

### With another library (Axios, for example)

```jsx
const axios = require("axios").default;
const Qs = require("qs");

let data = {
    action: "offres_apply_filters",
    all_filters: appliedFilters,  // any JS object
  };
  
  axios
    .post(
      `${window.location.origin}/wp-admin/admin-ajax.php`,
      Qs.stringify(data)
    )
    .then((response) => console.log(response))
    .catch((error) => console.log(error));
```

```php
add_action('wp_ajax_offres_apply_filters', 'offres_apply_filters');
add_action('wp_ajax_nopriv_offres_apply_filters', 'offres_apply_filters');

function offres_apply_filters() {
    $current_filters = $_POST['all_filters'];
    wp_send_json_success($current_filters);
}
```

## What is AJAX?

Let’s start by saying what AJAX is not. It is not a new programming language or a framework. It is not a Javascript or PHP library. It is not even a “technology”. So you will not need to learn any new languages or frameworks to use it. 

AJAX is a technique that appeared in the late 1990s and early 2000s. The real revolutionary thing about it is that it used current technologies to do something extremely powerful and new: updating the content of your website without reloading the page. 

Until then, if you wanted to update a simple button or the elements in your inbox, you would’ve needed to refresh the entire page. But reloading an entire page for only a single element is not very ressource-effective.

What AJAX allows us to do is to update a particular part of your website with information from your server without refreshing the entire website. 

The following diagram from the Wikipedia page illustrates the difference between AJAX and full refresh of your page pretty clearly. On the left side you have a page refresh action and on the right side you have an AJAX call.

![AJAX-wikipedia](/images/ajax-wikipedia.svg)

This technique came from the realisation that we could use Javascript to update our web page even if the entire page did not refresh and make a request to our server while the page stood still. 

Here is a simple step-by-step summary of what an AJAX request does:

## The steps of regular AJAX request

1. First, we define a trigger that will create our AJAX call. It can be a button or a time interval, for example. Let’s say that we clicked on “refresh inbox”, for example.
2. That trigger makes a request to our server with Javascript. There are several methods and libraries that help you do this. Let’s say that we sent a request that asks our server the question “are there any new messages in our inbox?” 
3. The server then receives this request and executes the appropriate function to find out if there are any new messages in our inbox. Let’s say it found that there are three new messages. So the server sends the 3 new messages back to the client (our Javascript AJAX engine).
4. In our Javascript code we now need to catch the response of the server. In this case, the three new emails. We take that response and we update the content of our website with the three new emails. 

That’s all there is to it. By the end, we updated our inbox without refreshing the page. 

## Implementing AJAX in Wordpress

So that’s the theory. Let’s see how we do this in Wordpress.

### Step 1: create a trigger

The first thing that you will need to do is create an event listener that will trigger your AJAX request. 

```jsx
const myButton = document.querySelector(".my-button");

myButton.addEventListener('click', e => {
	// our request will be here

});
```

### Step 2: Create the AJAX request in Javascript

This is where you have several options. I usually like making my AJAX requests with a JS library called Axios. I like it because it is minimalistic and very straightforward. Another option is to use jQuery, which has an AJAX module that is very easy to use as well.

The downside of jQuery is that it is an extremely heavy library with lots of functionalities. Installing it in your website only to create AJAX requests would be a total overkill and would probably make your website unnecessarily heavy. 

But since we are working with Wordpress and jQuery comes out-of-the-box with it, why not take advantage of it? The following code goes inside the callback function of your event listener.

AJAX w**ith jQuery**

```jsx
jQuery.ajax({
    type: "post",
    url: `${window.location.origin}/wp-admin/admin-ajax.php`,
    data: {
      action: "my_action_name",
      my_data: myData,  // any JS object
			some_more_data: moreData
    },
		beforeSend: function(){
			// while processing

    },
    complete: function (response) {
			// when the response from the server arrives
      console.log(JSON.parse(response.responseText));

    },
  });
```

In the request above, we used the following params:

- `type`: We created a new POST request to our server.
- `url`: Where we send our request to. In wordpress, we need to send our request to `admin-ajax.php`. That’s the default file that does the AJAX configuration for us on the server side.
- `data`: The data to send in the request. It takes a Javascript object. You can send as much data as you wish. **It is very important that one of the properties be “action”.** This will tell Wordpress the name of the hook to be fired when this AJAX request is triggered on the server-side.
- `beforeSend`: A function to execute before the request is sent. I usually put my “loading animations here”.
- `complete`: A function that takes the response param. This contains the response of your server.  Since the response comes in JSON format, you will need to use JSON.parse to use it.

### Step 3: Handle the request on the server

Once you sent the data you will need to catch it on the server. 

In Wordpress, AJAX comes preconfigured. That is, when you send a POST request to `admin-ajax.php`, you will be able to retrieve the data of the post from inside your `functions.php` file.

But how does that work?

When you send a POST request to `admin-ajax.php`, Wordpress sanitises that data and makes it available through two custom hooks: one prefixed by `wp_ajax` and the other by `wp_ajax_nopriv`. The former one is fired when the user is logged in and the other one when she’s not. 

Remember the property `action` that we added to the data that we sent to the server on step 3? I told you that was going to define the name of our hook. **That name that you gave your action is the second part of your custom hooks**. In our example above, our action was `my_action_name`. So our two hooks will be:

- `wp_ajax_my_action_name`
- `wp_ajax_nopriv_my_action_name`

If you want your AJAX request to work for both, logged-in and logged-out users, you will need to add two action hooks with the same function like this:

```php
add_action('wp_ajax_my_action_name', 'my_function');
add_action('wp_ajax_nopriv_my_action_name', 'my_function');

function my_function() {
    $my_data = $_POST['my_data'];
		// do something with the data
    wp_send_json_success($my_data);
}
```

There are two main ways to send data back to your client from an AJAX request in Wordpress. The first one is to `echo` the data you want to send and to execute `wp_die();` afterwards to kill the current process. But there is a better way. 

The best way is to send the data using `wp_send_json_success`. This way you don’t even need to execute `wp_die();` afterwards.

This is the data that you will get in your Javascript function under the method `complete` of your `jQuery.ajax()` function. You can `console.log` the data like the example of Step 3 to see your response.

## Doing the same with Axios

As I mentioned above, using jQuery only for an AJAX request is a total overkill. If you already have jQuery in your Wordpress installation, then why not use it as it comes prebuilt with a simple AJAX method.

But if you are not already using jQuery in your website, please do not install it only for this. I recommend that you use this other library to send your POST request on plain Javascript. 

To use Axios, you can install it with npm, along with Qs, which will help you stringify your data so that Wordpress can read it:

```bash
npm install axios
npm install qs
```

Once those are installed, you can use the following code to send your POST request. This code does the same as the code above in Step 3 (we use interceptors to execute code while the function request is loading).

```jsx
const axios = require("axios").default;
const Qs = require("qs");

let data = {
    action: "my_action_name",
    my_data: myData,  // any JS object
		some_more_data: moreData
  };
  
  axios
    .post(
      `${window.location.origin}/wp-admin/admin-ajax.php`,
      Qs.stringify(data)
    )
    .then((response) => console.log(response))
    .catch((error) => console.log(error));
		.interceptors.request.use(config => { // always takes config argument
			console.log(`${config.method} request sent to ${config.url}`);
		return config;
	}
)
```

I hope this was useful. Feel free to contact me if you have any questions.