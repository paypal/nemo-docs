# Nemo in the wild!

Put Nemo MVP out in the world, with enough docs, examples, modules to allow people to get started and provide feedback.

## Packages and versions

* [nemo](https://github.paypal.com/NodeTestTools/nemo/tree/v0.6-beta): v0.6-beta
* [nemo-view](https://github.paypal.com/NodeTestTools/nemo-view/tree/v0.3-beta): v0.3-beta
* [nemo-mocha-factory](https://github.paypal.com/NodeTestTools/nemo-mocha-factory/tree/v0.3-beta): v0.3-beta
* [nemo-drivex](https://github.paypal.com/NodeTestTools/nemo-drivex/tree/master): v0.1.x
* [nemo-locatex](https://github.paypal.com/NodeTestTools/nemo-locatex/tree/v0.3-beta): v0.3-beta

## Example application

* [nemo-example-app](https://github.paypal.com/NodeTestTools/nemo-example-app)

## Documentation

* This repo's md docs. [Start here](https://github.paypal.com/medelman/nemo-docs/blob/master/README.md)

## Tickets or other approvals

* [Domain Registration](http://lob.corp.ebay.com/sites/legal/IntellectualProperty/Lists/Domain%20Name%20Registration%20Form/AllItems1.aspx): submitted 25 Apr 2014
* [Trademark Request](http://lob.corp.ebay.com/sites/legal/IntellectualProperty/Lists/Trademark%20Candidate%20Questionnaire/My%20Trademark%20View.aspx): submitted 25 Apr 2014
* Open source project license request: email sent to Mark.Yuan@ebay.com on 25 Apr 2014

## Additional contributions

### Nemo generator

A yeoman generator for adding Nemo to node.js applications would be splendid. Such a generator would:
* create appropriate directory structure
* add appropriate dependencies to existing package.json
* ask user which plugins they would like to include
* insert a sample test suite, and locator file (for example, one that navigates around the PayPal logged out pages)

### Nemo GUI

A GUI which leads users through the process of automating a page. E.g.
* User wants to load a particular URL
* User wants to build a locator file (or files) from that URL
* User wants to add locator file to existing test suite
* User wants to build sets of actions and save as a page object
