# Locators

Locator files are JSON formatted files which describe the elements on your web page or native application under test.

## Example: login page locator

A login page reliably has a few elements:
* user name `type="text"` input
* password `type="password"` input
* submit button

A locator file for this might look like this:

```javascript
{
	"emailTextInput": {
		"locator": "login_email",
		"type": "id"
	},
	"passwordTextInput": {
		"locator": "login_password",
		"type": "id"
	},
	"submitButton": {
		"locator": "input[type='submit'][name='submit']",
		"type": "css"
	},
	"forgotPasswordLink": {
    	"default": {
    		"locator": "Forgot password",
    		"type": "partialLinkText"
    	},
    	"FR": {
    		"locator": "Mot de passe oublié",
    		"type": "partialLinkText"
    	},
    	"DE": {
    		"locator": "Passwort vergessen",
    		"type": "partialLinkText"
    	}
	}
	"logoutLink": {
		"locator": "a[href*='/logout']",
		"type": "css"
	}
}
```

When used along with the recommended suite of locator/view plugins, you can now write a test like this:

```
/* nemo setup */

//verify forgot password link is visible
nemo.view.login.forgotPasswordLinkVisible().then(function(isIt) {
	if (isIt === false) {
		throw new Error("didn't find forgot password link");
	} else {
		return;
	}
}).then(function() {
    //go ahead and log in
    nemo.view.login.emailTextInput().sendKeys("myusername@memail.xyz");
    nemo.view.login.passwordTextInput().sendKeys("myFancyP455w0rd");
    nemo.view.login.submitButton().click();
    return nemo.view.login.logoutLinkWait(5000);
}).then(function() {
	//we are logged in successfully
}, function(err) {
	//encountered some error along the way
});
```

The above script will work with different locales, assuming that you have the property `props.locale` available on the `nemo` namespace.
 The nemo-view interface, along with nemo-locatex and nemo-drivex, handle all the details behind the scenes.
