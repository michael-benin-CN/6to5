# Modules

## Usage

### CLI

    $ 6to5 --modules common script.js

### Node

```javascript
var to5 = require("6to5");
to5.transform('import "foo";', { modules: "common" });
```

## Formats

### Common (Default)

**In**

```javascript
import "foo";

import foo from "foo";
import * as foo from "foo";

import {bar} from "foo";
import {foo as bar} from "foo";

export {test};
export var test = 5;

export default test;
```

**Out**

```javascript
require("foo");

var foo = require("foo").default;
var foo = require("foo");

var bar = require("foo").bar;
var bar = require("foo").foo;

exports.test = test;
var test = 5; exports.test = test;

exports.default = test;
```

### AMD

**In**

```javascript
import foo from "foo";

export function bar() {
  return foo("foobar");
}
```

**Out**

```javascript
define(["exports", "foo"], function (exports, _foo) {
  exports.bar = bar;

  var foo = _foo.default;

  function bar() {
    return foo("foobar");
  }
});
```

### UMD

**In**

```javascript
import foo from "foo";

export function bar() {
  return foo("foobar");
}
```

**Out**

```javascript
(function (factory) {
  if (typeof define === "function" && define.amd) {
    define(["exports", "foo"], factory);
  } else if (typeof exports !== "undefined") {
    factory(exports, require("foo"));
  }
})(function (exports) {
  exports.bar = bar;

  var foo = _foo.default;

  function bar() {
    return foo("foobar");
  }
});
```
