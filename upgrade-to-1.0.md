## package.json changes

1. nemo and nemo-view to "^1.0.0"
2. remove nemo-drivex/nemo-locatex
   * if you are using nemo-drivex directly then refer to below steps to replace those usages
3. remove nemo-mocha-factory
4. You "can" update grunt-loop-mocha to "^1.0.0" but not required

## addition of <basedir>/config/config.json

You can copy what is in the 1.0-develop branch of the nemo-example-app:
https://github.com/paypal/nemo-example-app/blob/1.0-develop/test/functional/config/config.json

## changes to spec files

1. remove references and calls to nemo-mocha-factory
2. add a local, empty "nemo" variable to your spec file: `var nemo;` or `var nemo = {}`
3. In place of your nemo-mocha-factory call, add this: https://github.com/paypal/nemo-example-app/blob/1.0-develop/test/functional/spec/generic-spec.js#L6-L11