---
title: "What is a server-side language?"
date: 2022-04-08
draft: false
categories: 
- PHP Crash Course
tags:
- php
---

To understand what it means when you say that a language is "server-side", we first need to understand that there are two places where your code can be compiled in web development: client-side and server-side.

## Client-side languages

The first type of programming languages for the web are client-side programming languages. This means that they run on the browser of the people visiting our website (a.k.a. the client). In other words, let's say you put your website code in your server and someone wants to visit your website. 

They open their web browser and then type the URL of your website. Your server then sends the HTML, CSS and JavaScript code to their computer and it is their browser that runs this code before their eyes to create the website. All of that happens in your visitor's computer when they visit your website! 

So it is their computer that is running the code. All your server does in this case is store the code to then send it to your client.

## Server-side languages

The other type of language is the server-side programming language. As you might have guessed, this kind of languages run on the server and not on your client's computer. But what is the difference exactly?

So let's say that you have a list of 100 projects that you want to list in your portfolio website. Let's say you want to display them in an ordered HTML list like this:

```html
<ol>
	<li>Project 1</li>
	<li>Project 2</li>
	<li>Project 3</li>
	.
	.
	.
	<li>Project 100</li>
</ol>
```

You could indeed just code 100 elements like that. Let's call that version 1 of your list.

But that's a lot of typing! 102 lines of repetitive code to be precise! And we are programmers, we do not like repetitive tasks. So what we do is we create a PHP script that will loop through an array of projects and print a `li` element for every element in the array. So instead of a 100 lines of code, we can use a "for each" loop. Let's call this version 2 of your list:

```php
<ol>
<?php 
// This is your array of projects:
$myProjects = ['Project 1', 'Project 2', 'Project 3', ... , 'Project 100'];

// This is the loop of projects
foreach($myProjects as $projectName) {
	echo '<li>' . $projectName . '</li>';
}
?>
</ol>
```

And here is the important part: since we did this on PHP, this operation will be made in the server before event the client receives it! This means that the client will have no idea of what happened behind the curtains. In other words, to your client's browser, both versions of the list are exactly identical!

## But why not use Javascript for that?

Javascript is extremely useful to run operations on your client's browser. It allows you to create interactive elements in your website, among many other things. And it's indeed true that you can use Javascript to perform the above operation! You can, for example, do something like this:

```html
<ol id="my-list">
</ol>

<script src="createList.js"></script>
```

```jsx
// createList.js

const myProjets = ['Project 1', 'Project 2', ..., 'Project 100'];
const myList = document.querySelector("#my-list");

myProjects.forEach(project => {
	let listElement = document.createElement('li');
	listElement.innerHTML = project;
	myList.appendChild(listElement);
});
```

This code will display exactly the same result as the previous code in PHP. But the difference is that your client's browser will not receive the HTML code ready to display. It will receive the initial empty list and the Javascript code on the side. It will first have to run the Javascript to generate the HTML and only then will it be able to display it. 

More often than not, whenever you have the opportunity of using PHP instead of Javascript, you should probably let your server do the heavy loading than your client's browser. That way the operation is done only once in your server, rather than on your client's browser every time someone visits your website.

For example, imagine that 10,000 people visit your website, the list will be generated individually 10,000 times in 10,000 computers. 

On the other hand, with PHP, you could loop through the array once, create the list and then display it to every visitor.

This is why server-side languages are important. They allow you to run complex operations on the server and then feed the already built product to your client. So it doesn't matter if your client has a terrible computer from 2001. Your website will run like butter because all the heavy-loading will be done by your server (which is supposed to be a very powerful computer anyways).