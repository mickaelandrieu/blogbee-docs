
Day 5: the BackBee Edition mode
===========================

In the last episods:

 - [Day1: Setup and install BackBee](day1.md)
 - [Day2: Bootstrap of BlogBee project](day2.md)
 - [Day3: Explanation and layouts creation](day3.md)
 - [Day4: Introduction to Content model](day4.md)

Introduction
---------------

The Content model is complex to understand without practical examples. As a user manipulate contents through the toolbar, we will do the link between our files and configurations and the toolbar features activated and displayed. This way, you will understand how to provide features to your final users.


What for today?
-------------------
In **Day 4**, we added an Article content and this is great but you still can't edit it.
Today, we will also discover the differents "modes" of the toolbar and then we will complete our configuration files in order to allow our final users to create and edit articles.

BackBee Edition mode
==================

## the "big picture"

The first time you connect into **Edition mode** you may be a bit lost due to multiples buttons and tabs.

![BackBee toolbar](http://i.imgur.com/MTAPVc7.png "BackBee toolbar")


 1. the **Language Selector**: the Edition mode is available in english and in french.
 2. the **StateManager**: to save, apply or cancel the modifications done on your content.
 3. the **Mode Selector**:  this allow to switch between modes of BackBee editor.
 4. *only in Edit mode*:  the **Panel Selector** to choose which kind of content edit.


## the **Templates** mode

We already have used the **Template editor** before to create our layouts.

![Template editor](http://i.imgur.com/7VSBncD.png "Template editor")

All the templates stored in ``repository/Layouts`` folder appears in the left side.
You can select and edit them with the available options, all templates are based on a **grid system** like in the front-end framework [Bootstrap](http://getbootstrap.com/css/#grid).

Each time you create a template, a file ``<templateName>.twig`` is created in ``repository/Layouts`` folder.
In previous versions of BackBee CMS, any user can drag the column size but this feature is now deprecated and be removed in **1.0 release**.

## the **Edit** mode

When you login to BackBee Edition mode, you start in **Edit mode**:

![Editor mode](http://i.imgur.com/c2B0sbs.png "Editor mode")

This mode is contextual and strongly depends on what content is selected.

### the *Page* panel

the **Page** panel is obviously used to manage pages, from hierarchy to SEO properties.

![http://i.imgur.com/GcgOA5k.png](http://i.imgur.com/GcgOA5k.png "Page panel")

You can change the status of a page: if offline, non logged users can't access it, and schedule to publish and archive it.

Also, if you click on "Sitemap" link you access to the page tree we have introduced on **Day 3**, and easily re-organize page order and hierarchy.

For example, let's create a page.
Click on "New page" and fill the form with "article" as title and "Article" as Template. *You can either by create it by right-clicking on Home page or using Tools drop-down menu.*
Then, click on Save button and take a look to the "Sitemap widget".

![Sitemap widget](http://i.imgur.com/TU72yQl.png "Sitemap widget")

The article page is offline, his red hatched logo, an online page (like the *root* Home page) is white. You can manage your pages through the widget and/or in the *Page* panel.


### the *Boxes* panel

You will probably only use this panel when you build your website.
Double-click on "article" in the "Sitemap widget" to access the new page created. Then switch to the *Boxes* panel:

![Boxes panel](http://i.imgur.com/fVqJVPP.png "Boxes panel")

And you retrieve the blocks created in the ``repository\ClassContent\article.yml`` file.

For example, click on the "Upload picture":

![image block selected](http://i.imgur.com/biiNiLk.png "image block selected")

In the *breadcrumb*, you can see ``ContentSet > article > Media\image``.
If you take a look on your yaml file, you can see that an image has an image element, which is a ``BackBuilder\ClassContent\Media\image`` class: good.

You can see also that Boxes are organized by **category**. If you click on *Article* category you will find all contents we have created on **Day 4** with the category "Article": this is how BackBee order and allow contents to be used.

Each content must have a category, but sometimes you may need to deactivate a content because final user should'nt manipulate it.

You can use the "!" operator to filter contents, for instance we don't want final user add another article bodies to an article.

```yml
# /repository/ClassContent/Article/Body.yml
body:
  extends: \BackBuilder\ClassContent\ContentSet
  properties:
    category: [!Article]
```

And now, *body* is not part of available contents.

### the *Content* panel

This is the more used panel of BackBee, able to manipulable all editable blocks.

![content panel mode](http://i.imgur.com/gJmD8wL.png "content panel mode")

You may wonder how to do to make a block editable. As usualy, this is a problem solved
by configuration in yaml.

Let's assume you want to let users edit the title of an article:

    article:
        properties:
            name: Article
            description: "An article contains title, abstract, main image and a customizable body"
            category: [article]
        elements:
            title:
                type: BackBuilder\ClassContent\Element\text
                parameters:
                    aloha: !!scalar lite
                    editable: !!boolean true

As you can see, we have added some *parameters* to the article title configuration. Each parameter has a type and a value.

* **aloha**:  "lite" is a configuration tree key in ``/repository/Config/rteconfig.yml, we will talk about aloha configuration later
*  **editable**: this is explicit, we are now allowed to edit the title

Refresh your browser page and click on your article title, which is now editable.

![title now editable](http://i.imgur.com/XhNPR5v.png "title now editable")

## the **Bundles** mode
In BackBee, *Bundles* are mostly considered are "plugins". All the availables bundles are displayed in this view.
Notice that ``BackBee Standard edition`` come with a DemoBundle, to help you build your own bundles.

![BackBee bundle mode](http://i.imgur.com/kLg6UP6.png "BackBee bundle mode")

For example, ``DemoBundle`` provide a ``ClassContent`` called *block demo* that you will find inside *Demo* category in ``Boxes`` panel.

This is the first blue block you discover when you install the CMS is the *block demo* from Demo bundle.

We will learn how to do bundles in the next days.

Article model improvements
=======================

## Aloha editor

Aloha is an highly customisable WYSIWYG ("What you see is what you get") frontend editor.

Let's take a look to ``/repository/Config/rteconfig.yml`` to understand how to complete or restrict options displayed:

```yaml
config:
# in 0.1* versions, only aloha is available
  adapter: aloha
  mainNodeClass: .bb5-content
  inlineContentClass: .contentAloha
  editContentClass: ""
  fieldPrefix: data-aloha

aloha:
  plugins: []
  settings: []

  # this is where we define configurations of plugins for our website
  customconf:
    all:
      plugins: []
      pluginsconf:
        format:
          config: ['b', 'i', 'p', 'sub', 'sup', 'del', 'title', 'h1', 'h2', 'h3', 'h4', 'h5', 'h6', 'pre', 'removeFormat', 'table']
    # for article title we allow a format plugin
    lite:
      plugins: []
      pluginsconf:
        format:
          config: ['p','title', 'h1', 'h2', 'h3', 'h4']

    paragraph:
      # list of plugins is available in aloha documentation
      plugins: ['common/format', 'extra/formatlesspaste']
      pluginsconf:
        format:
          config: ['b', 'i', 'sup', 'sub', 'p','title', 'h3']

```

All the configuration of Aloha can be exposed in ``rteconfig.yml``, you only need [Aloha documentation](http://aloha-editor.org/guides/) to customise it.

Notice the configuration set in ``pluginsconf`` is merged into Aloha global configuration, so you can override a global behavior by set your own.


## Complete our Article model

This is the configuration we will use for our Article model:

```yaml
article:
    properties:
        name: Article
        description: "An article contains title, abstract, main image and a customizable body"
        category: [article]
    elements:
        title:
            type: BackBuilder\ClassContent\Element\text
            label: Title
            # default value
            default:
                value: Your title here...
            # set the max of content you can set
            # an article has only one title
            maxentry: 1
            parameters:
                aloha: !!scalar lite
                editable: !!boolean true
        abstract:
            type: BackBuilder\ClassContent\Article\Paragraph
            label: Abstract
            default:
                value: Your abstract here...
            maxentry: 1
            parameters:
                aloha: !!scalar lite
                editable: !!boolean true
        body:
            type: BackBuilder\ClassContent\Article\Body
        image:
            type: BackBuilder\ClassContent\Media\image

```

The complete configuration of contents we have defined in **Day 4** are stored in the [application github repository](https://github.com/backbee/blogbee/tree/day4).

## Bonus: manage our urls

BackBee has a lot of features and today we will speak about urls. This is a common need to have a total control on the urls generated by pages,  mainly for search engine optimization (SEO).

Urls are managed by Content and by configuration.

Add a new file in ``repository/Config/`` folder and name it ```rewriting.yml```.

```yaml
scheme:
  _content_:
    article: $ancestor[1]/$title
```

Now make an update on your article (update the status of your article for instance),
then "Save" your modifications and BackBee will ask you  to reload the browser page.

And the url will be updated: *notice you need to update each existing article in order to apply the new url pattern*.


Final thoughts
============

Well, time is over!

We have seen a lot of features today, and we are now enough skilled to create every simple websites you want.
We are able to integrate layouts and templates, to configure contents related to business and make them dynamics and editables by users.

The next days, we will go further on the BackBee discovery from bundle creation to events listeners and give you keys to finish your own BlogBee application!
