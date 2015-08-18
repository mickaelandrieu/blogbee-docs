Day 5: creation of layouts
==========================

In the last episodes:

 - [Day1: Setup and install BackBee](day1.md)
 - [Day2: Bootstrap of BlogBee project](day2.md)
 - [Day3: Introduction to Content model](day3.md)
 - [Day4: The BackBee Edition mode](day4.md)

Introduction
---------------

In BackBee, one of the first steps when creating a website is the design of layouts that define the overall organization of the site. BackBee Standard Edition provides "Home" and "Article" layouts which are enough to create our website, but we will create an "Archive"  page to understand and implement the design of a layout.


What for today?
-------------------
In **Day 4**, we discovered the toolbar and we saw the layout creation is unavailable. Currently, BackBee doesn't provide an "out of box" way to create his own layouts. Fortunately, the community shares a *Bundle for that* called the LayoutBuilderBundle.
So let's install this Bundle and use it to create the "Archive" layout.

What is a layout ?
==============

## the "big picture"

Let's start with a schema which represent the BackBee architecture of our *Article page*:

![the BackBee big picture](http://i.imgur.com/sLLJ19x.png "the BackBee big picture")

Each element is represented as a PHP class in **BackBee core library**:

 - **Site** (``BackBee\Site\Site``) is your application.
 - **Page** (``BackBee\NestedNode\Page``) represent each page of your application, for example for blogbee have three pages: home page, article page and author page.
 - **Layout** (``BackBee\Site\Layout``) represent the layout of a page.
 - **ContentSet** (``BackBee\ClassContent\ContentSet``) represent the blocks/columns of your layouts, there are blocks can be editables by user.

## LayoutBuilderBundle installation

> A complete day will be dedicated to bundle system in BackBee

Add the official LayoutBuilder bundle dependency using Composer.
For instance, you can add the following line to your `composer.json` file:

```javascript
{
    // ...
    "require": {
        // ...
        "backbee/layout-builder-bundle": "1.0.*@dev"
    },
}
```

And then install the dependency in `vendor`:

```bash
$ composer update
```

> The command need to be executed in the folder where is located the `composer.json` file.

Finaly, register the bundle in BackBee application: add this line at the end of the file `repository/Config/bundles.yml`.

```yaml
# bundles configuration
debug: BackBee\Bundle\DebugBundle\Debug
demo: BackBee\Bundle\DemoBundle\Demo
toolbar: BackBee\Bundle\ToolbarBundle\Toolbar
layoutbuilder: BackBee\Bundle\LayoutBuilderBundle\LayoutBuilder
```

If the application is running in *production mode*, you may need to empty the application cache:

```bash
$ ./backbee cache:clear
```

## Archive layout creation

### Configuration file

You need a YAML configuration file in a new folder ``repository\Layouts\Definitions`` and a template.

The archive layout will be composed by one main column and an aside column, both can accept all availables blocks.

```yaml
# Archive.yml
template: Archive.twig
label: Archive Page Layout
columns:
  MainColumn:
    mainZone: true
    accept: ~
    maxentry: ~
    defaultClassContent: ~
    inherited: false
  SidePane:
    mainZone: false
    accept: ~
    maxentry: ~
    defaultClassContent: ~
    inherited: true
```

This configuration have three required sections: `template`, `label` and `columns`.

* `template`: the template relative path. By default, the Renderer will look into the ``repository/Layouts`` folder;
* `label`: the template name, will be displayed in the "Template" select list when we create a new page;
* `columns`: a list of columns that act like Main content sets (cf schema above)

For each named columns, we have some properties availables:

* `mainZone`: if *true*, link all contents from this column to the page;
* `accept`: a collection of ClassContent the column can accept, all by default;
* `maxentry`: limit the number of class contents in this column, *infinity* by default;
* `defaultClassContent`: if used, by default this ClassContent will be put into the column;
* `inherited`: if *true*, the content of this column is inherited from parent pages

### Template file

Now we need to create a simple template. To render a column, use the `container()` to get them in your templates.:

```jinja
{% extends "Main.twig" %}

{% block content %}
<!-- BEGIn cols -->
<div class="contentcol row">
    <div class="col-sm-8 content-area" id="content">
        {{ this.container().first()|raw }}
    </div>
    <aside class="col-sm-4 aside">
        {{ this.container().next()|raw }}
    </aside>
</div>
<!-- END cols -->
{% endblock %}
```


Finally, use the command line interface provided by the Bundle:

```bash
$ ./backbee layout:create --layout=Archive --site=blogbee.dev
```

> The ``site`` argument can accept URI or Site Label (available in ``sites.yml``).


Final thoughts
============

This tutorial is over!

We saw many features and you must be able to create a simple website by yourself.
We are able to integrate layouts and templates, to configure contents related to business and make them dynamic and editable by users.

The next days, we will go further on the BackBee discovery from bundle creation to events listeners and give you keys to improve your own BlogBee application!
