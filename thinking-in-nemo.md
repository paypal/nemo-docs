# Thinking in Nemo

Nemo is essentially a factory pattern that provides a single JavaScript namespace (we prefer to use `nemo`) with an active selenium-driver
instance (i.e. the API into a "hot" browser or native application) and any additional plugins/features you've enabled or written.
The plugins you use, and how you organize your files, is almost completely up to you. Before you run off into the wild though,
please take some time to understand the building blocks.

## Nemo namespace

You will see the phrase "`nemo` namespace" throughout these documents. The `nemo` namespace is a JavaScript object which the Nemo factory
provides via the setup method. By default, your `nemo` namespace will include:
```javascript
   {
   		"driver": selenium-webdriver API into a "hot" browser,
   		"wd": selenium-webdriver "webdriver" namespace (includes locators etc),
   		"props": a JSON object which Nemo constructs from the nemoProps environment variable,
		"view": an array of view objects,
		"locator": an array of locator objects,
		"<plugin1>": a user registered plugin,
		"<pluginN>": a user registered plugin
   }
```

You can store the returned object to any JavaScript variable name, but if you use `nemo` you will find it a lot easier to follow these
documents. Plus it's cute!

As a brief example,

```javascript
var Nemo = require('nemo'),
	plugins = require('../config/nemo-plugins'),
	setup = require('../config/setup').mySetup,
	nemo = {};

(new Nemo(plugins)).setup(setup).then(function(_nemo) {
	nemo = _nemo;
	//now you've got the nemo namespace

	//proceed with automation
});

```

## Promises

Promises are one way to blend the asynchronous nature of JavaScript with commands which will resolve in future turns of the event loop.
selenium-webdriver has an internal implementation of Promises which is used for virtually every webdriver command. Nemo uses this same promise
implementation internally for building the nemo object.

In addition, selenium-webdriver has a ControlFlow class which abstracts this promise layer, allowing for a simpler syntax in your test scripts.

However, it is very important to understand that there is the promise layer beneath, in order to successfully write scripts.

Please read more about this here: [http://code.google.com/p/selenium/wiki/WebDriverJs#Understanding_the_API](http://code.google.com/p/selenium/wiki/WebDriverJs#Understanding_the_API)

## Selenium webdriver and locator strategies

selenium-webdriver provides several strategies for locating elements in a browser document. Please see the source code for details:
[http://selenium.googlecode.com/git/docs/api/javascript/source/lib/webdriver/locators.js.src.html](http://selenium.googlecode.com/git/docs/api/javascript/source/lib/webdriver/locators.js.src.html)

It is very important to consider locator strategies DURING planning and development of a web application. If the elements you will
interact with during your tests are clearly and consistently marked up, writing tests will be a lot easier. And the tests will be a
lot more robust.

Drivers which automate non-browser things (like iOS/Android native applications) use locators as well, but you will have to consult the
documentation of the driver for your preferred target to understand locator strategies as applied there.

## Nemo plugins

Nemo Plugins are essential to effective testing. You can use Nemo alone with no plugins, but adding the plugins released along with Nemo
 will greatly enhance your productivity. [Please see here for more information on using plugins](plugins.md).

## Nemo Views and Locator files

Nemo has native support for a rudimentary view object. But it is highly recommended to provide Nemo with a view interface plugin to expand
this capability. A view interface can consume a locator file and provide you with convenience methods which will simplify your codebase and
test syntax.

Read about [locator files](locators.md) here.

Read about the recommended [nemo-view](https://github.com/paypal/nemo-view/tree/v0.3-beta) view interface here.

## Mixing view objects together

You should think of a view as a fairly small section of your user interface. It could be a form, or even a portion of a form. Ideally a
view is a set of UI elements that are repeated in different parts of your application. An overall page of your application could be
 broken into several different views. A set of interactions with your application would therefore require multiple views working together.

There is no one way to mix views together. You could do so directly in your spec files. You could also create modules which take the `nemo` namespace
as an argument, and therefore would have access to whichever views you have in your namespace. The nemo-example-app has a directory called "page"
with such a module, which mixes together multiple views to create a set of UI interactions.

## Organizing boilerplate and config

Nemo is primarily configured with a variety of JSON-formatted variables at different points. It is recommended to pack the JSON away
into external modules so as to simplify your test scripts and promote sharing of configuration data between script files. The Nemo example
application provides the recommended structure for doing this. I.e.
* create a JSON file for defining your plugin configuration
* require that JSON file into your spec files where you call the Nemo constructor
* create a JSON file (or dynamic module) for your setup configuration
* require that file into your spec files

This approach puts all your important configuration information into well known places, and reduces lines of code in your spec files

## Directory structure

Nemo has an opinion about directory structure. Generally, a certain class of file (spec, locator, etc) should each be in a single directory.
I.e. no nested directories beneath. This may conflict with how some people like to organize their files. For those people, please consider
naming your files as you would have nested them in directories. E.g. If you would have preferred to have a file `spec/profile/funding/checkAll.js`
instead create the file `spec/profile-funding-checkAll.js`

## Input and collaboration is welcome

Nemo will succeed with a thriving user community. That means you have a voice! Please use it in the form of issues filed on any of the repositories,
as well as forks and pull requests. We look forward to hearing from you.