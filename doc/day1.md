Day 1 : Project start
================

Introduction
---------------

[BackBee](http://www.backbee.com) is an open source Content Management System (CMS) project built on top of Symfony Components and Doctrine 2.

This tutorial describe the creation of real web application, we will say more on day 2 about the application and features we expect to be implemented.
This kind of site is very popular and very easy to build with BackBee CMS, for instance you should take a look at [Medium](http://www.medium.com).

This goal is to demonstrate that BackBee can be used to make websites with style and little efforts.

Each day of this tutorial is meant to last between one and two hours, and will be the occasion to learn BackBee by coding a real website, from start to finish. Every day, new features will be added to the application, and we'll take advantage of this development to introduce you to new BackBee functionalities as well as good practices in web development.

This tutorial is also inspired from **so famous Jobeet**, the book to learn [symfony 1](http://symfony.com/legacy) which describe how to build a business jobs application.

What for today?
-------------------

The objective of this day is to setup the environment and install the CMS. At the end of the day we expect to see the "Hello world" from BackBee.

We assume you have basic knowledges in PHP5, Apache or Nginx and MySQL.

Prerequisites
----------------

Before installing BackBee, you need to check has everything installed and configured correctly. You should realy care about this first day and follow all the steps required to check your configuration, this may your day further down the road.

### Third party Software

First of all, you need to check that your computer has a friendly working environment for web development. At a minimum, you need a web server (Apache, for instance), a database engine (MySQL, PostgreSQL, SQLite, or any PDO-compatible database engine), and **PHP 5.4** or later.

Also, you will need to install [Composer](https://getcomposer.org/) the dependency manager for PHP.

Follow this link to install it if your are on [Linux/Unix based OS](https://getcomposer.org/doc/00-intro.md#globally) (like ubuntu or Mac OS X), Composer is also available on [Windows system](https://getcomposer.org/doc/00-intro.md#installation-windows).

In a command line interface, check if ``composer`` is correctly installed:

![composer on cmder](http://i.imgur.com/xDZi6Sc.png "composer")

Also, check your PHP version which should be 5.4 or least.

![PHP on cmder](http://i.imgur.com/DkgQJz2.png "PHP")


Eventually, you need to set a domain name in your operating system.
On Unix/Linux based OS, you can add it into ``/etc/hosts`` file:

    127.0.0.1   blogbee.dev

#### PHP extensions and configuration

You need to activate ``mb_string``, ``pdo_mysql`` and ``pdo_sqlite`` extensions.
Then you also need to set a correct date.timezone in your ``php.ini`` configuration file:

    [Date]
    date.timezone=Europe/Paris

Ok ! We can now launch the installation process.

Installation
--------------

Open a command line interface and get BackBee:

    $ composer create-project "backbee/backbee-standard" /path/to/your/folder v0.11.1

Then on the ``public`` directory, launch the builtin server of PHP:

    $ cd /path/to/your/folder/public && php -S blogbee.dev:8000/config.php

And you should see the first installation step of BackBee:

![BackBee Installer - first step](http://i.imgur.com/saok4nc.png "BackBee Installer - first step")

You need to create ``cache`` and ``log`` folders with the correct rights, then you can refresh the page and access to the **second step** of the installer.

![BackBee Installer - step 2](http://i.imgur.com/YBUecHz.png "BackBee Installer - step 2")

If required, check the ``repository/Config`` folder rights and then access to the **third** step.

You need to set your database settings. If you use different database engine than MySQL or MariaDB, set the correct driver else you have nothing to change, only fill the fields **database name**, **username** and **password**.

![BackBee Installer - step 3](http://i.imgur.com/ylWQZPm.png "BackBee Installer - step 3")

BackBee Installer creates and populates the database, fill the last informations to complete the installation process: the **site_name** and the **domain**.

![BackBee Installer - step 4](http://i.imgur.com/OMhfsrI.png "BackBee Installer - step 4")

You are done: BackBee gives you the ``Apache`` or ``Nginx`` configuration you have to set to be able to use BackBee on the choosen domain.

![BackBee Installer - step 5](http://i.imgur.com/hvdhOjN.png "BackBee Installer - step 5")

When you have set your web server, access to **[http://blogbee.dev](http://blogbee.dev)**.

If everything is correctly installed, you should see the BackBee welcome message:

![BackBee welcome message](http://i.imgur.com/rWY8IQk.png "BackBee welcome message")

You can even connect to the "Edition mode", press  ``CTRL + ALT + B`` to show the login dialog. Note that we created a super admin user for you with login (=admin) and password (=admin).

![BackBee Installed - Edition mode](http://i.imgur.com/3LBfnBL.png "BackBee Installed - Edition mode")

Final thoughts
============

Well, time is over! Even if we have not yet started talking about BackBee, we have setup a solid development environment and we are ready to start coding.

Tomorrow, we will unveil what the application will do and talk about the requirements we need to implement for **BlogBee**.

