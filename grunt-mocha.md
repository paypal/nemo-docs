# Mocha, Grunt, and Nemo

Mocha, grunt and Nemo work great together. Follow the steps below to see how.

Note: This document is aimed at getting you started quickly. For more detailed information on the individual modules, please see the documentation for each.

## Get started
Do all of the following within a node.js web application. We are going to modify the package.json and also add some files to it.

### Install Grunt globally

```bash
$ sudo npm install -g grunt-cli
```

### Add grunt and nemo dependencies to your project

In package.json

```javascript
  "grunt": "^0.4.4",
  "grunt-loop-mocha": "^0.3.0",
  "nconf": "~0.6.7",
  "mocha": "^1.18.0",
  "nemo": "^0.2.0",
  "nemo-drivex": "^0.1.0",
  "nemo-view": "^0.2.0",
  "nemo-locatex": "^0.1.0",
  "nemo-mocha-factory": "^0.2.0"
```

```bash
$ npm install
```

* __grunt-loop-mocha__: This grunt task will run mocha one or more times, and feed name/value pairs to nemo via the 'nemoData' stringified JSON environment variable
* __nemo* modules__: nemo is the core module, and the additional modules add functionality as plugins, or reduce boilerplate in your tests

### Create your directory structure and initial files

```
<project root>
  |-test/
    |-functional/
      |-config/
          nemo-plugins.json
      |-locator/
      |-page/
      |-report/
      |-spec/
          first-spec.js
  Gruntfile.js
```
### Files

nemo-plugins.json
```javascript
{
	"plugins": {
		"drivex": {
			"module": "nemo-drivex",
			"register": true
		},
		"locatex": {
			"module": "nemo-locatex",
			"register": true
		},
		"view": {
			"module": "nemo-view"
		}
	}
}
```

first-spec.js
```javascript
/*global nemo:true, describe:true, it:true */
var plugins = require("../config/nemo-plugins"),
	nemoFactory = require("nemo-mocha-factory"),
	homePage = require("../page/homePage"),
	setup = {};
describe('this is a @nemoSuite@', function() {
	nemoFactory({"plugins": plugins, "setup": setup});
    it('should @openAnUrl@', function(done) {
        nemo.driver.get(nemo.props.targetBaseUrl).then(function() {
			done()
		}, function(err) {
			done(err);
		});
    });
});
```

Gruntfile.js (just the grunt-loop-mocha config):
```javascript
"loopmocha": {
  "src": ["<%=loopmocha.basedir%>/spec/*.js"],
	"basedir": process.cwd() + "/" + "test/functional",
	"options": {
		"mocha": {
			"reportLocation": grunt.option("reportLocation") || "<%=loopmocha.basedir%>/report",
			"timeout": grunt.option("timeout") || 120000,
			"grep": grunt.option("grep") || 0,
			"debug": grunt.option("debug") || 0,
			"reporter": grunt.option("reporter") || "spec"
		},
		"nemoData": {
			"autoBaseDir": "<%=loopmocha.basedir%>",
			"targetBrowser": nconf.get("TARGET_BROWSER") || "firefox",
			"targetServer": nconf.get("TARGET_SERVER") || "localhost",
			"targetBaseUrl": "http://warm-river-3624.herokuapp.com/",
			"seleniumJar": nconf.get("SELENIUM_JAR") || "/usr/local/bin/selenium-standalone.jar",
			"serverProps": {"port": 4444}
		},
		"iterations": [
			{
				"description": "default"
			}
		]
	},
	"local": {
		"src": "<%=loopmocha.src%>"
	}
}
```

Gruntfile.js (where you import/configure your tasks)
```javascript
grunt.loadNpmTasks('grunt-loop-mocha');
grunt.registerTask('automation', ['loopmocha:local']);
```

### Decide on a webdriver

Defining your local webdriver setup is a critical step. [Please see this document for some guidance on the matter](driver-setup.md).

### Now run a test!

```bash
$ grunt automation --grep @nemoSuite@
Running "loopmocha:local" (loopmocha) task
[grunt-loop-mocha] setting ENV var  nemoData with value { autoBaseDir: '/Users/medelman/src/_nemo/nemo-example-app/test/functional',
  targetBrowser: 'firefox',
  targetServer: 'localhost',
  targetBaseUrl: 'http://warm-river-3624.herokuapp.com/',
  seleniumJar: '/usr/local/bin/selenium-standalone.jar',
  serverProps: { port: 4444 } }
[grunt-loop-mocha] iteration:  1397151350714-default
[grunt-loop-mocha] mocha argv:  --timeout,120000,--grep,@nemoSuite@,--reporter,spec,/Users/medelman/src/_nemo/nemo-example-app/test/functional/spec/first.spec.js,/Users/medelman/src/_nemo/nemo-example-app/test/functional/spec/foo-test.js

  this is a @nemoSuite@

    ✓ should @openAnUrl@ (20359ms)

  1 passing (20s)

Done, without errors.
$
```

If you didn't see a Firefox browser open on your desktop and run the test, [please see this document for some guidance on the matter](driver-setup.md).

## Organizing your tests for a team, continuous integration environment

The key to success in a distributed team environment is to atomize your tests and categorize them properly.

### Atomization

Make sure each mocha test (i.e. each call to "it") can run ENTIRELY INDEPENDENTLY of any other test. This means it does not rely upon any prior test to have run in advance of it. You can shrewdly plan your suites (i.e. "describe" calls) to do this, with the right usage of "before", "after", "beforeEach", "afterEach". To use mocha well, you need to firmly understand the lifecycle of all of these working together.

Here is a scenario where a test author has made tests dependent upon one another. It is an anti-pattern.

```javascript
describe("Let's @verifyProfilePage@", function () {
	before(function (done) {
		//nemo setup, any other account setup

		//log in
		nemo.view.login.login();
	});
	after(function (done) {
		//log out
		nemo.view.login.logout();

		//kill nemo
		done();
	});
	it("should @verifyAccountProfile@", function (done) {
		//verify account profile
	});
	it("should @displayCreditCards@", function (done) {
		//click a link to unhide credit card feature
		//verify list of credit cards
	});
	it("should @addCreditCard@", function (done) {
		//add a credit card using button revealed in previous test
	});
	it("should @verifyNewCreditCard@", function (done) {
		//verify a credit card
	});
});
```

While this works great when all the tests run in a row, if I only want to add and verify a credit card, I cannot simply run the two tests which add/verify. First, they require a previous test (@displayCreditCards@) and second, the verify test relies upon the add test.

If I am a developer working on the add card feature, I now have to run this entire suite file which tests other features I'm not touching. There are more than one way to resolve this. Here is a better approach:

```javascript
describe("Let's @verifyEntireProfilePage@", function () {
	before(function (done) {
		//nemo setup, any other account setup
	});
	after(function (done) {
		//kill nemo
		done();
	});
	beforeEach(function(done) {
		//log in
		nemo.view.login.login();
	});
	afterEach(function(done) {
		//log out
		nemo.view.login.logout();
	});

	it("should @verifyAccountProfile@", function (done) {
		//verify account profile
	});

	it("should @displayCreditCards@", function (done) {
		//click a link to unhide credit card feature
		//verify list of credit cards
	});
	it("should @addAndVerifyCreditCard@", function (done) {
		//click a link to unhide credit card feature
		//add a credit card using button revealed in previous test
		//verify the credit card
	});
});
```

While the change is subtle, you'll note that now each test can run completely independently. So, as a developer, I can run just one test when I'm focused narrowly.

Also, in a continuous integration environment, it is easy to re-run specific test failures.

### Categorization

The entire team should be in on the categorization process. You will use these categories to isolate sets of tests for different purposes. Building on the above example, if I wanted to run just the verify credit card test, I can use the mocha "grep" feature to do so:

`grunt automation --grep @addAndVerifyCreditCard@`

The before/after/beforeEach/afterEach methods are structured such that this test will run.

The categories you choose to list out in your describe blocks will be important for test segregation as illustrated above. Also though, if you want to run tests in parallel tracks (in CI for example, to lower execution time), then you can add additional categories for this purpose.
