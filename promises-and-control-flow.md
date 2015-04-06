# Promises and Control Flow

selenium-webdriver (the JavaScript binding for the Selenium client) uses Promises to manage statements which resolve asynchronously.
Please see https://code.google.com/p/selenium/wiki/WebDriverJs#Promises

In order to allow for a less verbose test syntax, selenium-webdriver also implements a ControlFlow class:
https://code.google.com/p/selenium/wiki/WebDriverJs#Control_Flows

The "synchronous" syntax that is allowed by this combination is both powerful, and a little confusing at first. Because
there will sometimes be statements which you need to resolve before moving on to next steps. Basically those where you
need the resolved promise value.