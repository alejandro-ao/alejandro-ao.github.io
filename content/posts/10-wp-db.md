
---
title: "How to interact with the Wordpress database (WPDB)"
date: 2022-04-15
draft: false
categories: 
- Wordpress development
tags:
- wordpress
- php
---

I started working with Wordpress at a professional level only after I had already experimented with other stacks like MERN. I remember that at the beginning I used to feel quite constrained by the apparent lack of flexibility of the Wordpress framework.

Soon, I realised that actually Wordpress is way more complex and flexible than just creating simple blogs from prebuilt themes. 

One of the things that I missed the most was the ability to manipulate the database with MongoDB. So when I learnt how to use `$wpdb` a new world of possibilities was open to me. Once you learn how to use `$wpdb` you will have the possibility to interact with a MySQL database and use it to create much more complex projects.

## What is WPDB?

`$wpdb` is a global class. You just need to call it and start using its methods. You will need to know how to write SQL queries since that is the argument that you will need to pass to the methods to get the data you need. The methods that we will see are: 

1. `$wpdb->show_errors()` displays any errors in your queries.
2. `$wpdb->get_results()` returns an array with the results of the query.
3. `$wpdb->get_row()` returns an array with the row that contains the query specified.
4. `$wpdb->get_col()` returns an array with the column that contains the query specified.
5. `$wpdb->get_var()` returns the variable stored at the specified cell.
6. `$wpdb->prepare()` sanitises the content of a table before returning it.