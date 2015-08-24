Day 5: Creating layouts
==========================

In the last episodes:

 - [Day1: Setup and install BackBee](day1.md)
 - [Day2: Bootstrap of BlogBee project](day2.md)
 - [Day3: Introduction to Content model](day3.md)
 - [Day4: The BackBee Edition mode](day4.md)

Introduction
---------------

In BackBee, one of the first steps when creating a website is designing layouts that defines the overall organization of the site. BackBee Standard Edition provides "Home" and "Article" layouts. This is enough to create our website, but we will create an "Archive"  page to understand and implement the layout design.


What's for today?
-------------------
In **Day 4**, we discovered the toolbar and we saw that layout creation was unavailable. Currently, BackBee doesn't provide an "out of box" way to create your own layouts. Fortunately, the community shares a *Bundle for that* called the LayoutBuilderBundle.
So let's install this Bundle and use it to create the "Archive" layout.

What is a layout?
==============

## the "big picture"

Let's start with a schema which describes the architecture of our *Article page*:

![the BackBee big picture](http://i.imgur.com/sLLJ19x.png "the BackBee big picture")

Each element is represented as a PHP class in **BackBee core library**:

 - **Site** (``BackBee\Site\Site``) is your application.
 - **Page** (``BackBee\NestedNode\Page``) represents each page of your application, for example in BlogBee have three pages: a home page, an article page and an author page.
 - **Layout** (``BackBee\Site\Layout``) represents the page layout.
 - **ContentSet** (``BackBee\ClassContent\ContentSet``) represents the blocks/columns of your layouts, some blocks can be edited by users.

## LayoutBuilderBundle installation

> A complete day will be dedicated to bundle system in BackBee

Add the official LayoutBuilder bundle using Composer.
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

> The command needs to be executed in the folder where the `composer.json` is located.

Finaly, register the bundle in BackBee application: add this line at the end of the file `repository/Config/bundles.yml`.

```yaml
# bundles configuration
debug: BackBee\Bundle\DebugBundle\Debug
demo: BackBee\Bundle\DemoBundle\Demo
toolbar: BackBee\Bundle\ToolbarBundle\Toolbar
layoutbuilder: BackBee\Bundle\LayoutBuilderBundle\LayoutBuilder
```

If the application is running in *production mode*, you may need to empty the application's cache:

```bash
$ ./backbee cache:clear
```

## Archive layout creation

### Configuration file

To create an archive layout, you will require a YAML configuration file in a new folder ``repository\Layouts\Definitions`` and a template.

The archive layout will be composed of one main column and a side column, both accept all availables blocks.

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

This configuration will require three sections: `template`, `label` and `columns`.

* `template`: template relative path. By default, the Renderer will look into the ``repository/Layouts`` folder;
* `label`: template name, will be displayed in the "Template" select list when we create a new page;
* `columns`: a list of columns that acts like Main content sets (cf schema above)

For each named column, the following properties are available:

* `mainZone`: if *true*, link all contents from this column to the page;
* `accept`: a collection of ClassContent that the column can accept, all by default;
* `maxentry`: limits the number of class contents in this column, *infinity* by default;
* `defaultClassContent`: if used, by default this ClassContent will be added to the column;
* `inherited`: if *true*, the content of this column is inherited from parent pages

### Template file

Now we need to create a simple template. To render a column, use the `container()` to get it in your templates.:

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

This tutorial is now over!

We reviewed many features and you should now be able to create a simple website by yourself.
We know how to integrate layouts and templates, to configure contents and make them dynamic and editable by users.

In next days, we will dive into the BackBee discovery by reviewing everything from bundle creation to events listeners and provide you with keys tips on how to improve your own BlogBee application!
