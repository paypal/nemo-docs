# Upgrade to Nemo 1.0 from previous versions

There are changes to the set of required modules, Nemo constructor, and method of configuration.
Take the below steps to migrate to 1.0.

Please note that slight differences in your codebase may make it necessary to closely analyze the provided samples
to determine if you need to make some changes for those samples to work for your case.

## package.json changes

1. nemo and nemo-view to "^1.0.0"
2. remove nemo-drivex and nemo-locatex from package.json
   * if you are using nemo-drivex directly then refer to below steps to replace those usages
3. remove nemo-mocha-factory
4. You "can" update grunt-loop-mocha to "^1.0.0" but not required

## addition of <basedir>/config/config.json

You can copy config from nemo-example-app:
https://github.com/paypal/nemo-example-app/blob/master/test/functional/config/config.json

## changes to spec files

1. remove references and calls to nemo-mocha-factory
2. remove any view configuration from the spec file
2. add a local, empty "nemo" variable to your spec file: `var nemo;` or `var nemo = {}`
3. In place of your nemo-mocha-factory call, add this: https://github.com/paypal/nemo-example-app/blob/master/test/functional/spec/generic-spec.js#L6-L11

## changes to loopmocha file

1. comment out `nemoData` block(s) as those are no longer necessary
2. add the nemoBaseDir block as: https://github.com/paypal/nemo-example-app/blob/master/tasks/loopmocha.js#L15
3. if any of your loopmocha subtasks require different driver properties (e.g. loopmocha:mysubtask)
   * add the overrides to `mysubtask.json` and place in the config directory
   * in the subtask config, set `NODE_ENV: 'mysubtask'`


## remove direct usage of nemo-drivex

nemo-drivex functionality is fully replaced by the generic methods of nemo-view. See here: https://github.com/paypal/nemo-view#genericunderbar-methods
