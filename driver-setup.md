# Setting up a driver

Running selenium automation tests locally requires a browser (safari, firefox, chrome, internet explorer, phantomjs,
etc) and usually some webdriver. You will need to complete one or more of the below installations depending on which
browser/s you want to automate.

## Using phantomjs and ghostdriver

By far the quickest way to get a test to run locally is by installing phantomjs.

Mac folks will find that "brew install phantomjs" works quite well to install phantomjs. Just make sure the installation
is somewhere on your $PATH environment variable.

## Setting up a local selenium webdriver

For browsers, besides Chrome and PhantomJS, you will need the selenium standalone server. Download the latest here:
http://selenium-release.storage.googleapis.com/index.html

As of this edit, the latest is: selenium-server-standalone-2.44.0.jar

Make sure that the jar is exposed via your PATH variable. Otherwise Selenium will not be able to locate it.

There are other "drivers", like Appium or ios-driver, which can be used to automate iOS/Android simulators and devices.
That setup is covered elsewhere.

## Getting chromedriver
For Chrome, you will need the chromedriver binary, which is maintained by the Chromium project. Find the latest here:
http://chromedriver.storage.googleapis.com/index.html

Make sure the chromedriver binary is exposed via your PATH variable. Otherwise Selenium will not be able to locate it.

For OS X, if you're using [Homebrew](http://brew.sh/), there's a [Homebrew formula](http://brewformulas.org/Chromedriver),
which makes installation much easier.  To install, it's simply `brew install chromedriver`.
