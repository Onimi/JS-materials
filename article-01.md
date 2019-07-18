# Article 01

## Introduction

### History

### Overview

## Code strcuture

## Types and variables in JS

### Types

From 2015 (ES6) JavaScript has 7 types of data:
undefined, null, boolean, number, string, symbol (added in ES6) and object.
First 6 are also known as primitives (or scalars).

```
var a = undefined;
var b = null;
var c = true;
var d = 3.14;
var e = 'Hello';
var f = Symbol('secret');
var g = {
  name: 'John',
  age: 23,
  hasDrivingLicens: true,
}
```

All other constructions like Array, Map, Set and even Function are subtypes of the Object.

In JavaScript variables don't have type. Type is related to a value stored in variable.
Each variable can at some point hold for example boolean, then number, then string, then object, etc.

```
var foo = true;

if (foo) {
  foo = 15;
  console.log(foo);
}

foo++;

if (foo > 10) {
  foo = 'Hi there';
  console.log(foo);
}

foo = {
  done: false,
  value: 128,
};
```
