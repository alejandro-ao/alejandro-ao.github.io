---
title: "How to install PHP in your local computer"
date: 2022-04-12
draft: false
categories: 
- PHP Crash Course
tags:
- php
---

As we mentioned before, PHP is a server-side language. This means that it is made to run in a server and not in a local computer. So, in order to run it in your local computer, you need to simulate a server at home. 

This is extremely easy. There are several softwares that allow you to run PHP locally. The one that I would recommend for beginners is MAMP (you can also install LAMP if you're using Linux). All you need to do is install it in your computer and you will be able to run PHP locally by typing [http://localhost:8888](http://localhost:8888) on your browser.

Here are the steps to install PHP on your computer:

1. Download MAMP from the [official website](https://www.mamp.info/en/downloads/) and install it in your computer.
2. Open MAMP and go to "Document root". You may choose any directory you want. This will be the root of your local server. You will be able to execute all the PHP files you put in this folder. As you can see, I have set my local server root to a directory in my desktop.
3. Click on "start". This will start your local server. Now you can go to [http://localhost:8888](http://localhost:8888) 
4. Now create a php file in your chose root directory. Let's name it `test.php`.  
5. Now add the following code to that file: `echo 'Hello, world! I'm running PHP!'` and save it.
6. You should now be able to go to [http://localhost:8888](http://localhost:8888) and click on `test.php`. You will see your string displayed on your browser. 

Congratulations! Now you are running PHP locally on your computer! This will be very useful to continue learning PHP. Soon we will see how to configure a remote server to run PHP. But for the moment, this will get the job done.