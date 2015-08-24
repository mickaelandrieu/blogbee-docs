Day 6: introduction to bundles: the QuoteBundle
==========================

In the last episodes:

 - [Day1: Setup and install BackBee](day1.md)
 - [Day2: Bootstrap of BlogBee project](day2.md)
 - [Day3: Introduction to Content model](day3.md)
 - [Day4: The BackBee Edition mode](day4.md)
 - [Day5: Creation of Layouts](day5.md)

Introduction
---------------

Today we will talk about one of the most powerful features of BackBee.
Indeed, bundles allow CMS to provide new content types and behaviors.In addition, a bundle can become a manageable and independent application, which gives the CMS web framework capabilities.


What's for today?
-------------------

We will take a little time to define what a bundle is and how to enable it in applications.
As we have said, bundles are powerful tools and we will see how they can be used to add and/or alter behavior of the CMS.
Then we will create a simple bundle that adds a feature allowing us to insert quotes.


What is a bundle ?
==============

**Think of a bundle as a plugin or an extension for BackBee.**

Bundles are loaded with the application and can share class contents,
commands, listeners, services, templates... everything you can imagine.

When BackBee application is started, it loads all the activated Bundles and registers the configuration
of each bundle and services if the Bundle shares services. Also, all commands of Bundles are autoloaded by the BackBee console.

For instance, the official [`DebugBundle`](https://github.com/backbee/DebugBundle) shares a list of useful commands to help you develop
your website.

### Architecture

Let's take a look at the architecture of our "QuoteBundle":

```
QuoteBundle
    |_ ClassContent/
        |_ Quote.yml
    |_ Config/
        |_ config.yml
    |_ Resources/
        |_ css/
            |_ quote.css
    |_ Templates/
        |_ scripts/
            |_ Quote.twig
    |_ Quote.php
```

There are no best practices for now but some common ways:
* all Commands classes MUST BE in the ``Command`` folder in order to be autoloaded;
* ``Config`` folder, ``config.yml`` and an entry point (here, ``Quote.php``) are required;
* all Class Contents MUST BE in the ``ClassContent`` folder in order to be autoloaded;

If you need more information on Bundle architecture, check the [official docs](http://docs.backbee.com/developper-documentation/components/bundle/).

### Configuration

Each bundle needs a configuration file, that contains Bundle configuration loaded by BackBee. Look at this configuration reference file:

```yaml
bundle:
    name: Foo bundle
    description: A description of the bundle, displayed in extensions menu
    author: John Doe <john.doe@backbee.com>
    version: 1
    enable: true
```

If you want more information, each property is documented in [official docs](http://docs.backbee.com/developper-documentation/components/bundle/#bundle-folder-structure-and-entry-point).

### EntryPoint and Recipes

The entry point of bundle is a simple POPO (Plain Old PHP Object) that MUST extends ``BackBee\Bundle\AbstractBundle`` class.

```php
<?php
namespace BackBee\Bundle\FooBundle;
/*
 * Copyright (c) 2011-2015 Lp digital system
 *
 * This file is part of BackBee.
 *
 * BackBee is free software: you can redistribute it and/or modify
 * it under the terms of the GNU General Public License as published by
 * the Free Software Foundation, either version 3 of the License, or
 * (at your option) any later version.
 *
 * BackBee is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with BackBee. If not, see <http://www.gnu.org/licenses/>.
 */
use BackBee\BBApplication;
use BackBee\Bundle\AbstractBundle;
use BackBee\Config\Config;

/**
 * Bundle that provide something
 *
 * @author Charles Rouillon <c.rouillon@lp-digital.fr>
 */
class Foo extends AbstractBundle
{
    /**
     * {@inheritdoc}
     */
    public function start()
    {
    }

    /**
     * {@inheritdoc}
     */
    public function stop()
    {
    }

    /**
     * Override the expected behavior when BackBee load Templating helpers
     */
    public function loadHelpers(BBApplication $application, Config $config)
    {
        // do some stuff here
    }

    /**
     * Override the expected behavior when BackBee load Templates
     */
    public function loadTemplates(BBApplication $application, Config $config)
    {
        // do some stuff here
    }
}
```

This class introduces two functions that are called when Application start and stop.
Others are called recipes and allow you to override some Bundle behaviors.

### Recipes

By default, BackBee declare some automatic behaviors in Bundles related to ressources (commands, templates, class contents ...).
But you can complete or totaly change all the behaviors with Recipes.
In the class entry point and in the configuration file, we have declared two functions that allow us to manage how templates and templates helpers
will be managed by BackBee for this Bundle (for instance, register them only in a specific application context).

All the recipes are documented in the [official docs](http://docs.backbee.com/developper-documentation/components/bundle/#how-the-bundle-loader-works), note that you can also add a ``custom`` behavior and that theses recipes are called when the application start.

### Activation

In ``repository/Config/bundles.yml`` associate the PHP fully qualified class name (FQCN) entry point to a string key that has to be unique..

```yaml
# repository/Config/bundles.yml
#...
foo: BackBee\Bundle\FooBundle\Foo
```

Keep in mind the order of bundles, because you can override configuration of your application and all the configurations are merged recursively. So if a bundle A have a service called foo
and then a bundle called B have also a service called foo,
your application will only have reference to the service declared in the bundle B.

> As always when you alter the application configuration, you may need to clear the cache.

Creating QuoteBundle
====================

Let's go: create the architecture and the Quote PHP class file.
Then we have to create the block quote. Let's keep it simple, only two fields:
* the author
* the quote

```yaml
# /bundle/QuoteBundle/ClassContent/Quote.yml
Quote:
    properties:
        name: Quote demo
        description: "Block that provide Quote feature"
        category: [Notes] # a category (at least) is required
    elements:
        quote:
            type: BackBee\ClassContent\Element\Text
            label: Quote
            default:
                value: "Put your quote here ..."
            parameters:
                rte: lite
        author:
            type: BackBee\ClassContent\Element\Text
            label: Author
            default:
                value: "Put your name here ..."
            parameters:
                rte: lite
```

And of course, the corresponding (minimal) template:

```twig
{# /bundle/QuoteBundle/Templates/scripts/Quote.twig #}
<div id="quote_thread" {{ this.bbcontent(null, {'class': 'quote-block'})|raw }}>
    <div {{ this.bbcontent(quote, {'class': 'quote-value'})|raw }}>
        {{ this.render(quote)|raw }}
    </div>
    <div {{ this.bbcontent(author, {'class': 'quote-author'})|raw }}>
        {{ this.render(author)|raw }}
    </div>
</div>
```

That's simple.

Additionaly, you can provide a logo for this new Class Content: the only required thing is to create an image called *Quote.png* in `/bundle/QuoteBundle/Resources/img/contents/Quote/`.

You can now access the new Quote Block in "Quote" in "Notes" category!

Final thoughts
============

We have seen how to create a new Bundle that has simple features. Tomorrow we will see more: persisting data into entities in a manageable bundle. Happy coding!
