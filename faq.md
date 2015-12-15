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
