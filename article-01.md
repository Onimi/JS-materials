# Article 01

## Introduction

### History

### Overview

## Code strcuture

## Types and variables in JS

### Types and `typeof` operator

From 2015 (ES6) JavaScript has 7 types of data:
undefined, null, boolean, number, string, symbol (added in ES6) and object.
First 6 are also known as primitives (or scalars).

```javascript
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

There is a special `typeof` operator to get type of expression. It returns 7 different values.
But there are some trics:

```javascript
console.log(typeof a); // undefined
console.log(typeof b); //! object 
console.log(typeof c); // boolean
console.log(typeof d); // number
console.log(typeof e); // string
console.log(typeof f); // symbol
console.log(typeof g); // object

var h = function (x) { return x; };
console.log(typeof h); //! function
```

`typeof null` returns `"string"`. This is a bug that stays in specifiaction for backward compatibility.
And also `typeof` of functions returns `"function"`. Which could imply that function is one of main types
but it is not.

### Dinamic types

In JavaScript variables don't have type. Type is related to a value stored in variable.
Each variable can at some point hold for example boolean, then number, then string, then object, etc.

```javascript
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
console.log(foo);
```

### Wrapper objects

There are special object-wrappers for boolean, number and string values - `Boolean`, `Number`, `String`.
They contain special values and methods to deal with these types.

So `Number` holds special values like:
- `Number.MAX_VALUE` - the maximum numeric value representable in JavaScript.
- `Number.MIN_VALUE` - the minimum numeric value representable in JavaScript.
- `Number.NaN` - represents Not-A-Number (aka `NaN`). Result of incorrect calculations.
- `Number.POSITIVE_INFINITY` (aka `Infinity`) - special value to indicate calculations get out of representable range.
