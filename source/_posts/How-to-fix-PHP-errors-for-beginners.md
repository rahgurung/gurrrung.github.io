---
title: How to fix PHP errors for beginners?
description: A quick guide on reading the PHP error logs.
date: 2020-06-20 17:45:55
thumbnail: /How-to-fix-PHP-errors-for-beginners/header.png
readtime: 2
tags: PHP
---

PHP errors are easy to be debugged as PHP has a well-defined error management system that gives you details of the error. This article is designed for very beginners who are new to the world of the web.

> A typical PHP error looks like this

{% asset_img error.png %}

I made a diagram to make it easy to break it down as:

{% asset_img profile.jpeg %}

1. **Type of error**: This part tells the type of the error. You can see a brief about them here. Using this part you can make an idea about what the error can be like wrong syntax, undefined index etc.
2. **Suspected code**: This part tells the part in your code that is infected. To fix this error most probably you need to make a change in this part of your code.
3. **File Location**: This is the handiest and useful feature which tells you the file name in which an error has occurred. In case you have multiple files in your PHP or you are using a separate file to make HTTP requests, this can be helpful.
4. **Line no**: As it sounds, this part of the error tells where the error actually is in the specified file so that you can review only specific code and make the fixes.