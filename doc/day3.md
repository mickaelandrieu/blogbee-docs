Day 3: Explanation and layouts creation
=======================================

In the last episods:

 - [Day1: Setup and install BackBee](day1.md)
 - [Day2: Bootstrap of BlogBee project](day2.md)

Introduction
---------------

BackBee is a complete and flexible content management system. If architecture is a bit complex to understand, you will see that it is subsequently very easy to quickly build any type of website.


What for today?
-------------------

Today we will learn how BackBee manages content. This will be a somewhat difficult day and we recommend you to reread this tutorial to understand the system. By late afternoon, you will understand how to integrate the templates that we built in the second day and how to create the layouts for our pages.

The "big picture"
--------------------

Let's start with a schema which represent the BackBee architecture of our *Article page*:

![the BackBee big picture](http://i.imgur.com/sLLJ19x.png "the BackBee big picture")

Each element have its object representation inside **BackBee core library**:

 - **Site** (``BackBuilder\Site\Site``) is your application.
 - **Page** (``BackBuilder\NestedNode\Page``) represents each page of your application, for example for blogbee we have three pages: home page, article page and author page.
 - **Layout** (``BackBuilder\Site\Layout``) represents the layout of a page.
 - **ContentSet** (``BackBuilder\ClassContent\ContentSet``) represents the blocs/columns of your layouts, there are blocks that can be editable by the user.

We will create the complete structure of our pages and go further on **Day 4**, for now let's create our layouts.

Layout creation process
----------------------------

It's time to work!

In our website we will have static and dynamic content as you can see in this detailed mockup:

![Layout decomposition](http://i.imgur.com/2KiVJxd.png "Layout decomposition")

We need to create two layouts:

 - A "one-column" layout for author related pages
 - A "two-columns" layout for home and article pages

### Creation of layouts

Connect to BackBee (Remember, ``CTRL + ALT + B`` then fill your credentials) and select **Templates** tab on top of the editor.

BackBee embeds a lot of common layouts and the layouts we need are already provided by the CMS.

 1. Click on "Other templates", and select "Default template" which is only composed of one single column.

 ![Author layout](http://i.imgur.com/LUQTsiv.png "Author layout")

 2. Edit the Template name and set it to *OneColumn* and do the same for a "two columns" layout that we will name "TwoLayouts".

You will see two files generated inside ``repository/Layouts`` :  ``OneColumn.twig`` and ``TwoColumns.twig``, where we will insert our previous Bootstrap 3 templates from **Day 2**, and the calls to BackBee.

We have used [Twig](http://twig.sensiolabs.org/) as templating engine, in order to extend via [template inheritance](http://twig.sensiolabs.org/doc/tags/extends.html "Twig extends tag"), but BackBee also supports old simple *phtml* templating engine.

Then, add a ``Main.twig`` file in the ``repository\Layout`` folder which will act as a parent of each layout.

### Integration of our layouts

``Main.twig`` is the parent template with all static parts of the website and blocks we will extend in ``Article.twig`` and ``Author.twig`` templates

```twig
<!DOCTYPE html>

<html>
    <head lang="en">
        <meta charset="utf-8"></meta>
        <title>BlogBee - Home</title>
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.1/css/bootstrap.min.css">
        <link rel="stylesheet" href="{{ this.getUri("css/main.css") }}">
    </head>

    <body>
        <!-- Helper to get the toolbar -->
        {{ this.bb5toolbar()|raw }}
        <!-- #bb5-site-wrapper is very important as required by BackBee Editor -->
        <div class="container-fluid" id="bb5-site-wrapper">
            <div class="header-section">

                <header class="row header page-header">
                    <!-- include the header static file -->
                    {{ this.partial("partials/header.twig")|raw }}
                </header>

                <nav class="navbar navbar-default">
                    <div class="container-fluid">
                        <div class="collapse navbar-collapse row">

                            <section class="hidden-xs category-list-section col-sm-9">
	                            <!-- include the static navigation file -->
                                {{ this.partial("partials/navigation.twig")|raw }}
                            </section>

                            <section class="col-xs-12 col-sm-3 search-section">
                                <!-- include the search static file -->
                                {{ this.partial("partials/search.twig")|raw }}
                            </section>

                        </div>
                    </div>
                </nav>
            </div>

            <div class="row">
                {% block content %}{% endblock %}
            </div>

            <footer class="footer page-footer">
                <!-- include the footer static file -->
                {{ this.partial("partials/footer.twig")|raw }}
            </footer>

        </div>
        <script type="text/javascript" src="https://code.jquery.com/jquery-2.1.3.min.js"></script>
        <script type="text/javascript" src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.1/js/bootstrap.min.js"></script>
    </body>
</html>
```

This is the "parent" layout with common blocks to both layouts.

Now, take a look to the ``OneColumn.twig`` layout:

```twig
{% extends "Main.twig" %}

{% block content %}
<div class="col-xs-12">
    {{ this.container().first()|raw }}
</div>
{% endblock %}
```
This need little explanation here. Like in PHP inheritance, we have extended the main layout and only "overrode" the behavior of *content* block. This means that in the case that this layout is called by BackBee, the HTML rendered by the CMS will be the content of the *main* layout **plus** the replacement of the content of *twig content block* by *OneColumn's* content.

This is pretty easy, let's take a look at the ``TwoColumns.twig`` layout:

```twig
{% extends "Main.twig" %}

{% block content %}
<div class="col-sm-8 col-xs-12">
    {{ this.container().first()|raw }}
</div>
<div class="col-sm-4 col-md-3 col-md-offset-1 hidden-xs">
    <aside>
        {{ this.container().next()|raw }}
    </aside>
</div>
{% endblock %}
```

##### Helpers

You can access to BackBee Renderer through the **this**. We said before that a ContentSet is a container.
In the first layout, we have only one "container" so when we call ```this.container().first()|raw`` we ask BackBee to render the content of the first (and the only) ContentSet.

In the second template, where we have two ContentSet, you can call ```this.container().next()``` because *container helper* implements [ArrayIterator interface](http://php.net/manual/en/class.arrayiterator.php "Array Iterator").

BackBee provides a lot of helpers which are available in layouts, they are located in ``BackBuilder/Renderer/Helper`` folder, you can also create your own.

##### Partials

Partials are statics files which are commonly used in pages of the website.

```twig
# /repository/Templates/scripts/partials/header.twig
<div class="col-xs-8">
    <h1 class="site-title h1"><a class="link-unstyled" href="index.html">BlogBee <small>community blog</small></a></h1>
</div>
<section class="login-section col-xs-4 text-righ">
    <button class="btn btn-success pull-right" href="/login" data-toggle="modal" data-target="#authentication-modal">login / create account</button>
</section>
```

```twig
# /repository/Templates/scripts/partials/navigation.twig
<ul class="nav navbar-nav list-unstyled list-inline row">
    <li class="col-sm-2 text-nowrap"><a href="category.html#1">category 1</a></li>
    <li class="col-sm-2 text-nowrap"><a href="category.html#2">category 2</a></li>
    <li class="col-sm-2 text-nowrap"><a href="category.html#3">category 3</a></li>
    <li class="col-sm-2 text-nowrap"><a href="category.html#4">category 4</a></li>
    <li class="col-sm-2 text-nowrap"><a href="category.html#5">category 5</a></li>
    <li class="col-sm-2 text-nowrap"><a href="category.html#6">category 6</a></li>
</ul>
```

```twig
# /repository/Templates/scripts/partials/search.twig
<form action="/search" method="post" class="form-inline">
    <div class="form-group">
        <div class="input-group">
            <span class="input-group-addon btn-search-form btn-reset"><button type="reset" class="btn btn-warning glyphicon glyphicon-remove-sign"></button></span>
            <input type="text" class="input form-control" placeholder="search">
            <span class="input-group-addon btn-search-form btn-submit"><button type="submit" class="btn btn-primary glyphicon glyphicon-search"></button></span>
        </div>
    </div>
</form>
```

```twig
# /repository/Templates/scripts/partials/footer.twig
<ul class="row list-unstyled">
    <li class="col-xs-3 text-left">
        <a href="http://www.backbee.com/">Build with BackBee</a>
    </li>
    <li class="col-xs-3 text-center">
        <a href="https://backbee.github.com/blogbee">BlogBee tuorial</a>
    </li>
    <li class="col-xs-3 text-center">
        <a href="http://github.com/backbee/blogbee">github repository</a>
    </li>
    <li class="col-xs-3 text-right">
        <a href="http://www.lp-digital.fr/en/">lp digital system &copy;</a>
    </li>
</ul>
```

And we have added all we need to integrate the layouts.

Final thoughts
============

Well, time is over!

We have integrated our layouts into BackBee and tomorrow we will go further and talk about our “content blocks”.
