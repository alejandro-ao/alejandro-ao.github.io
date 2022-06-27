
---
title: "How to automate custom PHP actions in Wordpress using WP Webhooks"
date: 2022-06-15
draft: false
categories: 
- Wordpress development
tags:
- wordpress
- php
---

Here is all you need to automate custom PHP actions in Wordpress. Do you need to add a new user when they sign up to your MailChimp audience? How about changing the user type when Zappier sends you a webhook notification? 

This is the solution.

## Why not use the default Wordpress REST API ?

I was required, for a recent project, to update some Wordpress users when they were added to an audience in MailChimp. “This will be easy, I thought, I’ll just create an endpoint in the Wordpress REST API and update the database when I get a request!”. You can do this using the function: [register_rest_route()](https://developer.wordpress.org/rest-api/extending-the-rest-api/adding-custom-endpoints/).

Totally wrong. 

Wordpress REST API is very useful if you want your back-end to interact with a front-end application. You can call your posts, your pages, your users, etc. **But it is not made to interact with other back-end applications,** and thus not made for automations. 

**But Wordpress only allows cookie-based authentication in their REST API**. This is perfect when your client is a front-end application running in a browser. But it’s terrible if you are building an automation with a third-party CRM, for example.

## When is this is useful to enable API key authentication in a Wordpress endpoint?

The method I show you here is especially useful if you are building an integration with another application or with a third-party CRM. 

**This is useful, for example, if you want to update your database from POST requests.** 

Say that you are running a membership site with Wordpress. You want to offer a free course to anyone who signs up to your newsletter. 

It would be terrible to manually open access to the course every time someone signs up to your newsletter. 

But there is a better way.

Here is where the automation is extremely useful. You need to configure a **webhook in Mailchimp that sends a POST request to your Wordpress endpoint** when someone is added to your audience. 

**This is impossible using the default Wordpress REST API.** Your requests will always be rejected because, of course, there is no cookie in MailChimp.

## How to open a webhook protected by an API key in Wordpress using WP Webhooks

To open a key-protected endpoint in Wordpress you will need to create a custom endpoint outside the Wordpress REST API. 

You can do it manually, but I recommend you using [WP Webhooks](https://wp-webhooks.com/) to create the endpoint, then filter what it does with the data in `functions.php`.

### 1. Install the [WP Webhooks](https://wp-webhooks.com/) plugin

The free version of the plugin will allow you to create a webhook protected by an API key. Just click on “Create Webhook URL” and collect the API key.

![WP-Webhooks](/images/wp-webhooks.png)

### 2. Configure your custom PHP action in WP Webhooks plugin

This plugin has many actions available for your automation. You can create new users, alter them, add posts, etc. You can even integrate it with other plugins like BuddyBoss (e.g. edit profile type on request), MemberPress (e.g. change membership type on request), etc.

Most of these options are available only in the premium version. If you have the budget [go for it](https://wp-webhooks.com/)!

But if you don’t want to pay for it and have the technical skills to code your own functions, **you can code a function that will fire whenever this endpoint is triggered**. The custom PHP action is available in the free version of the plugin.

To do this, go to the “Webhook actions” tab and search for Custom PHP action. The documentation is inside the plugin:

![WP-Webhooks](/images/wp-webhooks2.png)

### 3. Create the function that will be triggered

Now it’s time to create your custom function that will fire whenever your endpoint is triggered. Here is an example of a function that filters this endpoint. 

Don’t forget to add your function to this filter: 
```
wpwhpro/run/actions/custom_action/return_args
```
and to return whatever the request will return to the one making the request. 

This example takes the data from the request (`user_id` and `user_type`) and updates the user type in a BuddyPress website.

```php
add_filter('wpwhpro/run/actions/custom_action/return_args', 'update_user_profile_type', 10, 3);
/**
 * Gets the parameters user_id and user_type from the POST request made to the custom endpoint and 
 * updates the member type of the user with the value of user_type.
 */
function update_user_profile_type($return_args, $identifier, $response_body)
{
    // If the identifier doesn't match, do nothing
    if ($identifier !== 'update_user') {
        return $return_args;
    }

    // This is how you can validate the incoming value. This field will return the value for the keys user_id and user_type
    $user_to_update = WPWHPRO()->helpers->validate_request_value($response_body['content'], 'user_id');
    $new_user_type = WPWHPRO()->helpers->validate_request_value($response_body['content'], 'user_type');

    // Update the user's profile type
    $updated_user = bp_set_member_type(intval($user_to_update), $new_user_type);

    $return_args = [
        'user to edit' => intval($user_to_update),
        'new member type' => $new_user_type,
        'updated user' => $updated_user
    ];

    // This is what the webhook returns back to the caller of this action (response)
    // By default, we return an array with success => true and msg -> Some Text
    return $return_args;
}
```

The three params are:

`$return_args` 

Whatever the webhook will return to the caller. It defaults to this:

```php
{
    "success": true,
    "msg": "Custom action was successfully fired."
}
```

`$identifier` 

The name of the custom action to be triggered. It’s the value of the param `wpwh_identifier` that is sent in the request. You can use it to test whether or not a request should trigger your filter.

`$response_body`

Contains the data from the request.

### 4. Structure your request

In the documentation available inside the plugin, we see that we need a couple of queries so that your endpoint can filter it and run your custom function. 

You can also send other information as a query. This information will be available for you in your custom function. In this example, I am sending the data `user_id` and `user_type` to update my user with the new user type:

![WP Webhooks Request](/images/wp-webhooks-request.png)

Don’t forget to include your content-type header:

![WP Webhooks Request](/images/wp-webhooks-request2.png)

You can use Postman to test your requests. I like to use the Thunder Client Extension for VS Code. It’s just like Postman but inside your IDE.

## Conclusion

That was how to create a custom endpoint to automate your Wordpress actions. You can use this to automate pretty much any action in your website.

As long as you can code it your function, you can automate it. I hope you found this tutorial useful.