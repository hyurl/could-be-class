# couldBeClass

**Checks if an object could be an instantiable class.**

This module also works with those classes that are written in ES5 style.

## Example

```javascript
"use strict";

const assert = require("assert");
const couldBeClass = require("could-be-class").default;

var str = "This is a string";
assert.ok(!couldBeClass(str));

var num = 123;
assert.ok(!couldBeClass(num));

class A { }
assert.ok(couldBeClass(A));

function B() {
    this.name = this.constructor.name;
}
assert.ok(couldBeClass(B));

function C() {
    this["name"] = this.constructor.name;
}
assert.ok(couldBeClass(C));

function D() { }
D.prototype.show = function () { }
assert.ok(couldBeClass(D));

function E() { }
Object.defineProperty(E.prototype, "size", {
    get() {
        return this.length;
    }
});
assert.ok(couldBeClass(E));

function TsEs5Class() {
    var _this = _super.call(this) || this;
    _this.desc = "This class form might be generated from TypeScript to ES5";
}
assert.ok(couldBeClass(TsEs5Class));

function NotClass() {
    console.log("this is not a class");
}
assert.ok(!couldBeClass(NotClass));

function* GenFunc() {
    this.desc = "Error will be thrown if trying to instantiate a Generator function.";
}
assert.ok(!couldBeClass(GenFunc));

if (parseFloat(process.version.slice(1)) >= 7.6) {
    // this block should only appear when you NodeJS version higher than 7.6, 
    // otherwise a SyntaxError will be thrown.
    async function AsyncFunc() {
        this.desc = "Error will be thrown if trying to instantiate an async function.";
    }
    assert.ok(!couldBeClass(AsyncFunc));
}

console.log("#### OK ####"); // should outout OK
```

## Tip

Commonly, any functions (except async, generator and arrow functions) in 
JavaScript could be instantiated as a class constructor, however, in most cases, 
a class written in ES5 style will carry some special fingerprints that the
detector can test if it could be a class constructor or just a pure function.

**WARN** This module only performs about 95% percent of accuracy.