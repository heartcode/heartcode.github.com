---
layout: post
title: "Doing LESS right using compilers"
date: 2012-03-27 14:33
comments: true
categories: JavaScript CSS Compiler Tools Mac
---

<a href="/blog/2012/03/27/doing-less-right/">{% img /images/posts/doing-less-right/doing-less-right.jpg 789 443 'Screenshot of the Less app for Mac' 'Screenshot of the Less app for Mac' %}</a>

Developers hate repeating themselves over and over again. It is especially true when we're talking about writing CSS3 behaviours and we are forced to use the elusive vendor prefixes to tell every single browser what we want them to do. Don't get me wrong, I don't think <a href="http://www.netmagazine.com/features/big-question-are-vendor-prefixes-hurting-web" target="_blank">vendor prefixes are necessarily evil</a>, but they are very unconvenient for frequent use.
<!--more-->

Delevopers like to come up with brilliant solutions to problems. <a href="http://lesscss.org/" target="_blank">Less</a> is a good solution for people who want to write less CSS and don't want to keep repeating themselves. You write some pretty Less code, which then will be compiled on the client side using JavaScript and will be turned into CSS. It works, that's for sure, but there are some problems, whitch bother me about this solution:

1.	You use client-side JavaScript to generate your CSS
2.	You load an external JavaScript file to process your Less files (one extra http request)
3.	There is always going to be a short "psychological" delay between the content loading, the JavaScript loading and running, and the CSS displaying

I am not saying that these problems are very concerning, but still there must be a neeter way of doing Less, right?

##Right!

As I was trying to get my head around Less today, I've found a very good <a href="http://fadeyev.net/2012/02/16/less-update/" target="_blank">article about Less compilers</a>. Of course, what a brilliant idea! Use Less as you usually do, but instead of using the cient-side JavaScript solution, you use the compiler, that helps you to generate the output CSS whilst you are developing your content. The article mentions some cool Less compilers for <a href="http://wearekiss.com/simpless" target="_blank">Windows</a>, <a href="http://incident57.com/less/" target="_blank">Mac</a> and <a href="http://wearekiss.com/simpless" target="_blank">Linux</a>. 

Now I am going to quickly go through of setting up the Less.app on Mac and will show you how to use it.

##Using the Less.app (on Mac OS X Lion)

I tried the Less.app for Mac only, as I refuse to use Windows anymore :) The app is really lightweight (under 3Mb) and absolutely free. You can download it <a href="http://incident57.com/less/" target="_blank">here</a>.

After downloading the app, the user needs to unzip and run it.

The Less.app has a very nice, simple and easy to use UI with two tabs in the header for "Less Files" and "Compiler".

###Less Files
In the "Less Files" view you can drag-and-drop a web project on the side bar, and the app automatically searches for Less files in the structure.

{% img /images/posts/doing-less-right/doing-less-right-drag-drop.jpg 789 443 'Adding a project to the Less.app on Mac' 'Adding a project to the Less.app on Mac' %}

Once the search is done, you can see your Less files in the "File & Output" panel. According to the Less.app tutorial when you drop your project on the side-bar, the app should automatically offer the output location for the compiled CSS files, but for some reason it didn't happen to me, so I manually had to set the output folder. To set your CSS output destination folder simpy right-click on the Less file in the list and choose "Set CSS Output Path". There is an option for minifying your output CSS file, which comes very handy.

You can add multiple projects to the side-bar and then you can manage them separately, which is clever.

Before you compile your files you need to go to the "Compiler" view.

###Compiler View
{% img /images/posts/doing-less-right/doing-less-right-compile.jpg 789 443 'Compiler view of Less.app on Mac' 'Compiler view of Less.app on Mac' %}

In the "Compiler View" you can see a list of your already done and pending processes. In the footer there is a checkbox for automatic compilation on save - this is a very smart feature: If you tick it, the app will automatically compile your CSS file every time you change and save your Less file. On the right hand side of the footer there is a button for clearing the list, and another one for compiling all your Less files.

## Conclusion
After the very easy and straightforward setup process the Less.app is ready to go. All you need to do is to check if it's running when you're working on your project. When you save your less files, the app compiles silently in the background and generates your CSS files.

Whith Less compilers developers can get rid of using JavaScript for generating the CSS on the client-side, whilst they still can use Less code and save precious time for creative coding (and debugging) :)

I'd love to hear your voice! Do you use any Less compilers? What is your experience with them and why do you think using Less compilers is a good/bad idea?