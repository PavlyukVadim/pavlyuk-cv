---
path: /posts/objects-in-javascript-complete-guide
date: '2019-07-27'
title: 📦 Complete guide to Objects in JavaScript
category: data-types-in-js
metaTitle: Complete guide to Objects in Js
metaDescription: Complete guide to Objects in Js
metaKeywords: 'javascript, js, objects'
---

## Table of content:

* [How to create object](#)

### How to create object

There're two ways: literal form and constructed:

```js
// literal form
var obj1 = { foo: 'bar' }

// constructed form
var obj2 = new Object()
obj2.foo = 'bar'
```

### How to access object properties

You have to use either the ```.``` operator or the ```[ ]``` operator:

```js
var obj = { foo: 'bar' }

console.log(obj.foo) // 'bar'
console.log(obj['foo']) // 'bar'
```

```[ ]``` operator can take any UTF-8 string as the property name:

```js
var obj = {}
obj['foo bar'] = 'baz'
console.log(obj) // { foo bar: 'baz' }
```

Property name is always a string, so name you passed as prop name will be coerce to string:

```js
var obj = {}
obj[1] = 'foo'
obj[1] === obj['1'] // true
```

And more intresting example:

```js
var a = {}, b = {}, c = {}
b[a] = 1
b[c]++
console.log(b[b]) // 2
console.log(b) // { [object Object]: 2 }
```

ES6 offers compound property names:

```js
var foo = 'bar'
var obj = {
  [foo + 'Prop']: 'baz',
}
console.log(obj) // { barProp: 'baz' }
```

### How to clone objects in JS

The esiest, but slow way (that works only with [JSON-safe](/js-dictionary/#json-safe) items) - serialize/deserialize object using built-in methods:

```js
const obj = {
  string: 'string',
  number: 123,
  bool: false,
  nul: null,
  date: new Date(),  // date.toISOString()
  undef: undefined,  // lost
  inf: Infinity,  // 'null'
  re: /.*/,  // {}
}

const newObj = JSON.parse(JSON.stringify(obj))
console.log(newObj)
```

ES6 offers more elegant solutions but only for shallow clone: Object.assign(..) and spread operator.

For deep clone you have to implement your custom solution, with recurcise copy of inner properties.

### Property Descriptors

ES5 offers ```Object.getOwnPropertyDescriptor``` to get a property descriptor:

```js
var obj = { foo: 'bar' }
Object.getOwnPropertyDescriptor(obj, 'foo')
/*
{
  value: 'bar',
  configurable: true,
  enumerable: true,
  writable: true,
}
*/
```

The opposite method for setting a property is ```Object.defineProperty(..)```:

```js
Object.defineProperty(obj, 'foo', {
  value: 'bar',
  writable: true,
  configurable: true,
  enumerable: true,
})
```

Let's examine each item from property descriptor more close

#### writable

* ability to change the value of a property
* returs TypeError for any value changes in strict mode

#### configurable

* ability to change property descriptor with Object.defineProperty(..) method
* returs TypeError for any calls of Object.defineProperty(..), regardless of strict mode (the only writable can be changed from true to false without error)
* prevents the ability to use the delete operator (silently)

#### enumerable

* ability to show property during object properties enumeration (like for..in loop)

### Immutability

#### constant property

If you want to make a constant property (cannot be changed, redefined or deleted) you can combine ```writable:false``` and ```configurable:false```.

#### prevent extensions

```Object.preventExtensions(..)``` disable ability to add new properties to object and returs TypeError for any extensions in strict mode.

#### seal

Creates a "sealed" object. (```Object.preventExtensions(..)``` + ```configurable:false``` for each property).

#### freeze

Creates a "frozen" object. (```Object.seal(..)``` + ```writable:false``` for each property).
