
Day 4: the Content Model
=====================

In the last episods:

 - [Day1: Setup and install BackBee](https://github.com/backbee/blogbee/blob/day1/doc/day1.md)
 - [Day2: Bootstrap of BlogBee project](https://github.com/backbee/blogbee/blob/day2/doc/day2.md)
 - [Day3: Explanation and layouts creation](https://github.com/backbee/blogbee/blob/day3/doc/day3.md)

Introduction
---------------

Content model of BackBee is one of the most difficult part to understand.
Fortunately, the Content management system is highly inspired from standards with the aim to be fully compatible with *PHP Content Repository* API.

What for today?
-------------------
As detailed in **Day 2** in AP stories, any User should be able to create an article, today we will
add the minimal files and configurations to allow any user to set an article.


BackBee Content model
-----------------------------

### What is PHP Content Repository?

The PHP Content Repository is an adaption of the Java Content Repository ([JCR](http://en.wikipedia.org/wiki/Content_repository_API_for_Java "Java Content Repository")) standard, an open API specification defined in [JSR-283](https://jcp.org/en/jsr/detail?id=283 "Java Specification Request - 283").
The API defines how to handle hierarchical semi-structured data in a consistent way.

The typical use case is content management systems. PHPCR combines the best out of document-oriented databases (weak structured data) and of XML databases (hierarchical trees). On top of that, it adds useful features like searching, versioning, access control and locking on top of it.

**BackBee Content Management System** follow most of the *PHP Content Repository* recommandations.

If you want to learn more about PHPCR, you can read the [online documentation](http://phpcr.github.io/documentation/ "PHPCR documentation") or take a look at this [speak from one of the PHPCR contributor](http://davidbu.ch/slides/20130404-sflive_phpcr.html "PHP CR by David Buchmann") which provides a good understanding of the whole system.

### BackBee Content Management System

BackBee Content Management System provides the following features:

 - Access hierarchical tree
 - Access by UUID
 - Node search
 - Versioning
 - XML import & export
 - Secured Access

#### Site & Pages

A website can have multiple pages, and page can have a parent page and a collection of "children" pages, ordered by "level" in the tree.

A page can have a parent (if not, the page is considered as "root page"):

![page tree](http://i.imgur.com/VWdneRE.png "page tree")

#### Layout

We have seen on **Day 3** the layouts, each page has a layout and all the contents are linked to the layout.
The reality is a little bit complex, because a page has a ContentSet (this is a special Content type: for now let's say this is an orderer collection of Content) which contains all the contents of the page.

When we need to render a web page, the ``Renderer``:

 - Check is the Page ContentSet is not ``null``
 - Get all the zones of the Page layout (1 column, 2 columns like the layouts defined on *Day 3*)
 - Loop on the contents of the ContentSet and **put the content** on the correct colum (or "zone")

To conclude, ``Layout`` is used by the ``Renderer`` to put the correct content on the zones defined in the layout.

#### Content Element

A content element is a very simple yaml file, which have *properties* and *elements*.
For instance, this is the actual representation an **text** element:

```yml
# BackBuilder\ClassContent\Element\text.yml
text:
  properties:
    name: Text
    description: A simple text input
  elements:
    value: !!scalar
```

A content element is not selectable in BackBee out of box, in order to use it
you need to embed it into a Content. Notice a content element can have parameters too,
but it's not mandatory.

List of native content elements in BackBee:
 * **text**
   * **date**
   * **select**
 * **file**
   * **attachment**
   * **image**
 * **keyword**
 * **link**

#### Content

The contents are also stored in an hierarchical tree, we can describe them
as a list of content elements (``BackBuilder\ClassContent\Element``).

Contents of your application are stored inside ``repository\ClassContent`` folder.

##### How to build a Content ?

For instance, this is the configuration for *Paragraph* content we want to allow user to use
in article.

```yml
# /repository/ClassContent/Article/Paragraph.yml
paragraph:
    properties:
        name: Paragraph
        description: Paragraph
        category: [Article]
    elements:
        body:
            type: BackBuilder\ClassContent\Element\text
```

A **paragraph** is an extend of the **text** element we have seen before, the interesting parts
of this configuration are:

* *category*: allow the content to be displayed on "Edition mode"
* *type*: the related class, can be "scalar", "array" or the fully qualified class name (FQCN) which is an instance of ``ClassContent``?

Finally, this is the final render of this Content inside BackBee after you will have drag & dropped the paragraph
inside the "Article" block.

![the Paragraph Content](http://i.imgur.com/sZ9ZnJi.png "the Paragraph Content")

##### ContentSet

A ContentSet is a content which accept an ordered collection of contents.


Article implementation
----------------------------

All the contents created by the developper are located in the ``/repository/ClassContent/`` repository.
Today, we will focus on the *article* creation.

Reminder, an article consists of:

 - A title
 - A picture
 - An abstract
 - A body which is a rich content

The ``article`` content configuration will be setted to multiple YAML files to allow re-use.

### Yaml files

```
repository/
    ClassContent/
        Article.yml
        Article/
            Body.yml
            Paragraph.yml
            Picture.yml
            ColumnDivider.yml
```

```yml
# /repository/ClassContent/Article/Paragraph.yml
paragraph:
    properties:
        name: Paragraph
        description: Paragraph
        category: [Article]
    elements:
        body:
            type: BackBuilder\ClassContent\Element\text
```

```yml
# /repository/ClassContent/Article/Picture.yml
picture:
    properties:
        name: Article picture
        description: A media image
        category: [Article]
    elements:
        title:
            type: BackBuilder\ClassContent\Element\text
        image:
            type: BackBuilder\ClassContent\Element\image
```

```yml
# /repository/ClassContent/Article/ColumnDivider.yml
column_divider:
    properties:
        name: "Column Divider"
        description: ""
        category: [Article]
    elements:
        left:
            type: \BackBuilder\ClassContent\container\column
        right:
            type: \BackBuilder\ClassContent\container\column
```

```yml
# /repository/ClassContent/Article.yml
article:
    properties:
        name: Article
        description: "An article contains title, abstract, main image and a customizable body"
        category: [article]
    elements:
        title:
            type: BackBuilder\ClassContent\Element\text
        abstract:
            type: BackBuilder\ClassContent\article\paragraph
        body:
            type: BackBuilder\ClassContent\article\body
        image:
            type: BackBuilder\ClassContent\Media\image
```

After adding this configuration file, re-connect to Edition Mode and create a new page with "Article" layout.

Access to this new page and look at the result:

![article page](http://i.imgur.com/a8ppU97.png "article page")

Final thoughts
============

Well, time is over!

We now have a better understanding on how BackBee manage his contents, tomorrow we will explore more the Editor mode
and we will go further on the "Content" model of BackBee.

