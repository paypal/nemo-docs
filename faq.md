# FAQ

Somewhere to record some frequently asked questions that can't ideally be documented anywhere else yet

## PhantomJS issue with SSL certificate errors

*Problem:* PhantomJS won't load a page when there is a certificate trust issue

*Solution:* Using `serverCaps`, set a flag with PhantomJS to ignore certificate errors:

```js
  "driver": {
    "browser": "phantomjs",
    "serverCaps": {
      "phantomjs.cli.args": "--ignore-ssl-errors=yes"
    }
  }
```

## Passing Chrome commandline arguments

*Problem:* You want to pass one or more commandline args to Chrome

*Solution:* Using the `builders` property in the nemo configuration you can pass them as follows:

```js
"driver": {
    "builders": {
      "withCapabilities": [{
        "browserName": "chrome",
        "chromeOptions": {
          "args": ["incognito", "window-size=200,200"]
        }
      }]
    }
  },
```
