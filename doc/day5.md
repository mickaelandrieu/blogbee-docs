Day 5: creation of layouts
==========================

In the last episodes:

 - [Day1: Setup and install BackBee](day1.md)
 - [Day2: Bootstrap of BlogBee project](day2.md)
 - [Day3: Introduction to Content model](day3.md)
 - [Day4: The BackBee Edition mode](day4.md)

Introduction
---------------

With BackBee, one of the first steps when creating a website is the design of layouts that define the overall organization of the site. BackBee Standard Edition provides "Home" and "Article" pages, which are sufficient for the realization of our website, but we will create an "Archive"  page to understand and implement the design of a layout.


What for today?
-------------------
In **Day 4**, we have discovered the toolbar and we already know that the layout creation is unavailable. At the moment, BackBee doesn't provide an "out of box" way to create his own layouts. Luckely, the community have already done a *Bundle for that*: the LayoutBuilderBundle.
Today we will install the Bundle and use it to create the "Archive" page.

What is a layout ?
==============

## the "big picture"

Let's start with a schema which represent the BackBee architecture of our *Article page*:

![the BackBee big picture](http://i.imgur.com/sLLJ19x.png "the BackBee big picture")

Each element have his object representation inside **BackBee core library**:

 - **Site** (``BackBuilder\Site\Site``) is your application.
 - **Page** (``BackBuilder\NestedNode\Page``) represent each page of your application, for example for blogbee have three pages: home page, article page and author page.
 - **Layout** (``BackBuilder\Site\Layout``) represent the layout of a page.
 - **ContentSet** (``BackBuilder\ClassContent\ContentSet``) represent the blocs/columns of your layouts, there are blocs can be editables by user.

## LayoutBuilderBundle installation

Final thoughts
============

Well, time is over!

We have seen a lot of features today, and we are now skilled enough to create any simple website you want.
We are able to integrate layouts and templates, to configure contents related to business and make them dynamic and editable by users.

The next days, we will go further on the BackBee discovery from bundle creation to events listeners and give you keys to finish your own BlogBee application!
