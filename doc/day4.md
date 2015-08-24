Day 4: the BackBee Edition mode
===========================

In the last episodes:

 - [Day1: Setup and install BackBee](day1.md)
 - [Day2: Bootstrap of BlogBee project](day2.md)
 - [Day3: Introduction to Content model](day3.md)

Introduction
---------------

The Content model is complex to understand without practical examples. As a user manipulates contents through the toolbar, we will do the link between our files and configurations and the toolbar features activated and displayed. This way, you will understand how to provide features to your final users.


What's for today?
-------------------
In **Day 3**, we added an Article content and this is great but you still can't edit it.
Today, we will also discover the different "modes" of the toolbar and then we will complete our configuration files in order to allow our final users to create and edit articles.

BackBee Edition mode
==================

## Discover the "toolbar"

The first time you connect into **Edition mode** you may be a bit lost due to multiple buttons and tabs.

![BackBee toolbar](http://i.imgur.com/Z8LtkyD.png "BackBee toolbar")


 1. the **User settings selector**: to access settings and disconnect from Edition mode.
 2. the **State manager selector**: to save, apply or cancel the modifications done to your contents.
 3. the **Mode selector**:  this allows to switch between modes of the BackBee editor.
 4. *only in Edit mode*:  the **Panel selector** to choose which kind of content to edit.


## the **User management** mode

Access the User management panel to access users and groups:

![User management](http://i.imgur.com/ICQAsM0.png "User management")

You can search for an user with the search form, manage an user, create new ones, or add them to a group.

### What is an user group ?

In BackBee the authorisations can be very precises and specifics.
Out of box, we provide three groups with the following rights on contents:

* **Administrators**: users of this group have all rights on the website and its features;
* **Validators**: users of this group can create, edit and publish;
* **Contributors**: users of this group can only create and edit but not publish. 

## the **Edit** mode

When you login to BackBee Edition mode, you start in **Edit mode**:

![Editor mode](http://i.imgur.com/7d2pD1U.png "Editor mode")

This mode is contextual and strongly depends on what content is selected.

### the *Page* panel

the **Page** panel is obviously used to manage pages, from hierarchy to SEO properties.

![http://i.imgur.com/GcgOA5k.png](http://i.imgur.com/Huk9XoU.png "Page panel")

You can change the status of a page: if set to offline, non logged users can't access it. You can also schedule it to publish and archive it.

Also, if you click on the "Sitemap" link you access the page tree we have introduced on **Day 3**, and can easily reorganize page order and hierarchy.

For example, let's create a page.
Click on "New page" and fill the form with "article" as title and "Article" as Template. *You can either do this by creating it right-clicking on Home page or using the Tools drop-down menu.*
Then, click on Save button and take a look at the "Sitemap widget".

![Sitemap widget](http://i.imgur.com/uCNilmS.png "Sitemap widget")

The article page is offline, has a red hatched logo, an online page (like the *root* Home page) is white. You can manage your pages through the widget and/or in the *Page* panel.


### the *Boxes* panel

You will probably only use this panel when you build your website.
Double-click on "article" in the "Sitemap widget" to access the new page created. Then switch to the *Boxes* panel:

![Boxes panel](http://i.imgur.com/Cf1OHVY.png "Boxes panel")

And you retrieve the blocks created in the ``repository\ClassContent\article.yml`` file.

For example, click on the "Upload picture":

![image block selected](http://i.imgur.com/8ev1lNT.png "image block selected")

In the *breadcrumb*, you can see ``ContentSet > article > Media image``.
If you take a look at your yaml file, you can see that an image has an image element, which is a ``BackBee\ClassContent\Media\image`` class: good.

You can see also that Boxes are organized by **category**. If you click on *Article* category you will find all the contents we created on **Day 4** with the category "Article": this is how BackBee orders and allows content to be used.

Each content must have a category, but sometimes you may need to deactivate a content because the final user shouldn't manipulate it.

You can use the "!" operator to filter contents, for instance we don't want the final user to add other article bodies to an article.

```yml
# /repository/ClassContent/Article/Body.yml
body:
  extends: \BackBee\ClassContent\ContentSet
  properties:
    category: [!Article]
```

And now, *body* is not part of available contents.

### the *Content* panel

This is the most used panel of BackBee, able to manipulable all editable blocks. You may wonder how to make a block editable. Usually, this is a problem solved by Yaml configuration of the block.

Let's assume you want to let users edit the title of an article:

    article:
        properties:
            name: Article
            description: "An article contains title, abstract, main image and a customizable body"
            category: [article]
        elements:
            title:
                type: BackBee\ClassContent\Element\Text
            body:
                type: BackBee\ClassContent\Article\Body
        parameters:
            accept: [BackBee\ClassContent\Text\Paragraph]

As you can see, we have added a *parameter* to the article title configuration. Each parameter has a type and a value.

* **accept**:  "lite" is a configuration tree key in ``/repository/Config/rteconfig.yml, we will talk about aloha configuration later
*  **editable**: this is explicit, we are now allowed to edit the title

![content panel mode](http://i.imgur.com/VOVGrAu.png "content panel mode")

## the **Plugins** mode
In BackBee, *Bundles* are mostly considered "plugins". All the available bundles are displayed in this view.
Notice that ``BackBee Standard edition`` comes with a DemoBundle, to help you build your own bundles.

![BackBee bundle mode](http://i.imgur.com/carbMKp.png "BackBee bundle mode")

For example, ``DemoBundle`` provides a ``ClassContent`` called *block demo* that you will find inside *Demo* category in ``Boxes`` panel. This bundle also provide a sample on how to make bundle manageable from the toolbar.

It is the *block demo* from *Demo* category in "Boxes" panel.

We will learn more about bundles in the next days.

Article model improvements
=======================

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

        abstract:
            type: BackBee\ClassContent\Text\Paragraph
            label: Abstract
            default:
                value: Your abstract here...
            maxentry: 1
        body:
            type: BackBee\ClassContent\Article\Body
        image:
            type: BackBee\ClassContent\Media\Image

```

The complete configuration of contents we have defined in **Day 3** are stored in the [application github repository](https://github.com/backbee/blogbee/tree/day3).

## Bonus: manage your urls

BackBee has a lot of features and today we will speak about urls. This is a very common need, that is, to have a total control on the urls generated by pages, mainly for search engine optimization (SEO).

Urls are managed by Content and by configuration.

Add a new file in ``repository/Config/`` folder and name it ```rewriting.yml```.

```yaml
scheme:
  _content_:
    article: $ancestor[1]/$title
```

Now make an update on your article (update the status of your article for instance),
then "Save" your modifications and BackBee will ask you to reload the browser page.

And the url will be updated: *notice you need to update each existing article in order to apply the new url pattern*.


Final thoughts
============

Well, time is over!

We have seen a lot of features today, and we are now skilled enough to create any simple website you want.
We are able to integrate layouts and templates, to configure contents related to business and make them dynamic and editable by users.

The next days, we will go further on the BackBee discovery from bundle creation to events listeners and give you keys to finish your own BlogBee application!
