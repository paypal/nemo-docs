# Plugins

Nemo plugins add functionality to the `nemo` namespace. As an example, within PayPal, we have a RESTful service which allows developers to
create test accounts, add funding instruments to those accounts, and manage other properties of accounts. We wrote a nemo plugin which interfaces
with that RESTful API. When registered within nemo, it can automatically create accounts based on the nemo setup config, and also is available
on the `nemo` namespace for usage, ad-hoc, within test suites. There are also plugins which are more universally useful outside of PayPal

## Registering a plugin

Register plugins via a JSON object passed into the Nemo constructor:

```javascript
var Nemo = require('nemo'),
	plugins = {
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
			},
			"user": {
				"module": "nemo-jaws/plugins/user"
			}
		}
	},
	setup = {
		"view": ["login", "profile"],
		"user": {
			"simple": {
				"country": "US"
			}
		}
	};

(new Nemo(plugins)).setup(setup).then(function(_nemo) {
	nemo = _nemo;
	//now you've got the nemo namespace with plugins drivex, locatex, and view

	//nemo.view.login and nemo.view.profile access those view objects which were generated
	//from corresponding locator/profile.json and locator/login.json files

	//nemo.user.client (accesses the RESTful endpoints for creating/modifying accounts directly)
	//nemo.user.users.simple (has a JSON representation of the user created during the Nemo setup routine)

	//proceed with automation
});
```

The Nemo factory will not auto-register any plugin except one labeled as a "view". All other plugins either need to be marked as
`"register": true` or else you need to provide an identical namespace in the setup argument to enable that plugin. Note the above usage of
the user plugin, which will have been registered by the presence of the "user" namespace in the setup object.

A "view" plugin will override the default view behavior in Nemo. A good practice, as is found in nemo-view, is to expect a locator JSON file
of the same name as the registered view object to be in the locator directory.