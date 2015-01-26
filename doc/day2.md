Day 2: Bootstrap of BlogBee project
===================================

In the last episod:

 - [Day 1: Setup and install BackBee](https://github.com/backbee/blogbee/blob/day1-doc/doc/day1.md)

Introduction
------------

Today is about the project specifications.


What for today?
---------------

We will list the users stories to complete in order to realize the application.
We will also make the fonctionnal mockups and make nice templates with Bootstrap 3 frontend framework.


What is BlogBee?
----------------

BlogBee is Open-Source blog-publishing platform software that only does one thing, but does it well. It is easy to use, customize, extend, and embed into your website. It supports user and content management out of the box, and of course uses the latest Web technologies to enhance user experience.

BlogBee is inspired by the famous [Medium website](https://medium.com/).

User stories
------------

We will have 3 types of users:

 - Admins: they can access to the admin panel and manage users and articles
 - Users: logged users can write an article, and manage his own account
 - Guests: they can comment article and/or share it through social networks

### Homepage

**Story HP1: On the homepage, any user can see the latest and top articles**

On the BlogBee homepage a user should see a list of 3 recent active articles.
He also see the 3 first top articles based on the number of readings.

**Story HP2: On the homepage, any user can see the top contributors list**

On the BlogBee homepage, a user should see a list of actual top contributors based on the number of articles they have published on the platform.

**Story HP3: On the homepage, any user can connect to his account and/or register**

On the BlogBee homepage, a guest can connect or register to the platform. If connected he can access his account management page.

**Story HP4: On the homepage, any user can filter the lists of article by category**

On the BlogBee homepage, a user can filter the articles displayed by category ("keyword").

And the following mockups:

*Home page*

![Homepage mockup](http://i.imgur.com/kQwdziY.png "Homepage mockup")

*Login feature on Home page*

![Login access on Homepage mockup](http://i.imgur.com/2VyKuK2.png "Login access on Homepage mockup")


### Article page

**Story AP1: On the article page, any user can see the article and the author profile**

On the BlogBee aticle page, a user can see the full content article, with a picture header if available.
On the right side, a user can see the author profile with picture if available and fullname.

**Story AP2: On the article page,  any user can comment an article**

On the BlogBee article page, any guest can comment the article or share it on social networks. We can use an external Comment system like [Disqus](http://disqus.com/).

And the following mockup:

![Article page mockup](http://i.imgur.com/Yq6EVlJ.png "Article page mockup")

### Author page

**Story AUP1: On the author page, any user should access author informations**

On the BlogBee author page, any visitor should access:

 - Author picture and description if available
 - Author fullname
 - Latest articles from this author

And the following mockup:

![Author page mockup](http://i.imgur.com/9KfFc8z.png "Author page mockup")

### Account management page

**Story AM1: On the account page, any logged user can update his informations**

On the BlogBee account management page, any logged user can update his informations:

 - fullname
 - description
 - picture

And the following (last) mockup:

![User account page mockup](http://i.imgur.com/wFi98Hu.png "User account page mockup")

What should it look like?
-------------------------

Learning Bootstrap 3 front integration is out of scope of this tutorial, but to ease the following of this tutorial we will provide you a complete design available [here](http://backbee.github.io/blogbee/integration/).

![Bootstrap static website](http://i.imgur.com/pTAkfDt.png "Bootstrap static website")

Archive can be downloaded here in the [github repository](https://github.com/backbee/blogbee/archive/day1.zip).


Final thoughts
==============

Well, time is over! Even if we have not yet started talking about BackBee, we have defined the features of our application and designed it using Bootstrap 3.

Tomorrow, we will start coding to integrate our static pages into BackBee and let the magic and interactivity happens.

