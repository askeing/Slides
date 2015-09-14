# Mocha & CI

Askeing Yen &lt;fyen@mozilla.com&gt;

Shing Lyu &lt;slyu@mozilla.com&gt;

Notes: For NCU Academy Program

----

## Prerequisites

* Install [Node.js](https://nodejs.org/en/) and npm
    * `sudo apt-get install nodejs`
    * `sudo apt-get install npm`
    * `brew install node` (OSX)
* Install [Git](https://git-scm.com/)
    * `sudo apt-get install git`
    * `brew install git` (OSX)

----

## Getting Start

* Install Mocha
    * `npm install -g mocha`
* Create test folder
    * `mkdir test`
* Write tests
    * `$EDITOR test/first_test.js`

----

## First Test

Write test in your editor

<div style='margin: 0px auto; text-align: left; padding-left: 180px;'>
<code>
var assert = require("assert");<br/>
var foo = 'bar'<br/>
describe('First', function () {<br/>
&nbsp;&nbsp;it('Test', function () {<br/>
&nbsp;&nbsp;&nbsp;&nbsp;assert.equal(foo, 'bar')<br/>
&nbsp;&nbsp;});<br/>
});
</code>

----

## Running Test

* Run following command in Terminal
    * `mocha`

----

## Continuous Integration

* Escape from "Integration Hell"
    * TDD, unit tests
    * Build Automation
    * Quality Control

----

## Reference

* Mocha: [Link](http://mochajs.org/)
* Wikipedia [Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration)
* Travis CI: [Link](https://travis-ci.org/)
* Jenkins CI: [Link](http://jenkins-ci.org/)
