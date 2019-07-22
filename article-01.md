# Article 01. Types, variables and functions.

## 1.1 Introduction

### 1.1.1 History

### 1.1.2 Overview

## 1.2 Code strcuture

## 1.3 Types and variables in JS

### 1.3.1 Types and `typeof` operator.

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

### 1.3.2 Primitives and objects.

### 1.3.3 Dynamic types.

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

### 1.3.4 Numbers in JavaScript.

Number in JavaScript implements [double-precision 64-bit binary format IEEE 754](https://en.wikipedia.org/wiki/Floating-point_arithmetic) numbers. So it may reveal some unexpected cavities. One of most famous of them is:
```javascript
0.1 + 0.2 === 0.3; // false
```

### 1.3.5 Wrapper objects.

There are special object-wrappers for boolean, number and string values - `Boolean`, `Number`, `String`.
They contain special values and methods to deal with these types.

So `Number` holds special values like:
- `Number.MAX_VALUE` - the maximum numeric value representable in JavaScript.
- `Number.MIN_VALUE` - the minimum numeric value representable in JavaScript.
- `Number.NaN` - represents Not-A-Number (aka `NaN`). Result of incorrect calculations.
- `Number.POSITIVE_INFINITY` (aka `Infinity`) - special value to indicate calculations get out of representable range.

...

It's not necessary to call wrapper objects explicitly to use their methods and properties:
```javascript
var str = "Hi there, everybody";
var strObj = new String(str);
console.log(strObj.length); // 19
console.log(strObj.split(" ")); // [ "Hi", "there,", "everybody" ]
```

We can omit explicit initialization of wrapper objects. So it's absolutely correct to write:
```javascript
var str = "Hi there, everybody";
console.log(str.length); // 19
console.log(str.split(" ")); // [ "Hi", "there,", "everybody" ]
```

In this case wrapper objects will be created implicitly by JavaScrit engine.

### 1.3.6 Strict and non-strcit comparison.

## 1.4 Functions and scope

### 1.4.1 Functions as the first class citizens

### 1.4.2 Lexical scope

### 1.4.3 Functional scope. `var` variables

### 1.4.4 Block scope. `let` and `const` variables

### 1.4.5 Closures
