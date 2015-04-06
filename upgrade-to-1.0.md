# Upgrade to Nemo 1.0 from previous versions

There are changes to the set of required modules, Nemo constructor, and method of configuration.
Take the below steps to migrate to 1.0

## package.json changes

1. nemo and nemo-view to "^1.0.0"
2. remove nemo-drivex and nemo-locatex from package.json
   * if you are using nemo-drivex directly then refer to below steps to replace those usages
3. remove nemo-mocha-factory
4. You "can" update grunt-loop-mocha to "^1.0.0" but not required

## addition of <basedir>/config/config.json

You can copy what is in the 1.0-develop branch of the nemo-example-app:
https://github.com/paypal/nemo-example-app/blob/1.0-develop/test/functional/config/config.json

## changes to spec files

1. remove references and calls to nemo-mocha-factory
2. remove any view configuration from the spec file
2. add a local, empty "nemo" variable to your spec file: `var nemo;` or `var nemo = {}`
3. In place of your nemo-mocha-factory call, add this: https://github.com/paypal/nemo-example-app/blob/1.0-develop/test/functional/spec/generic-spec.js#L6-L11

## changes to loopmocha file

1. comment out `nemoData` block(s) as those are no longer necessary
2. add the nemoBaseDir block as: https://github.com/paypal/nemo-example-app/blob/1.0-develop/tasks/loopmocha.js#L15
3. make any other override modifications based on the new data/plugins/driver nemo configuration


## remove direct usage of nemo-drivex

TBD
