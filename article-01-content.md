# Article 01. Types, variables and functions.

## 1.1 Introduction

### 1.1.1 History

### 1.1.2 Overview

## 1.2 Code strcuture

## 1.3 Types and variables in JS

### 1.3.1 Types and `typeof` operator.

На данный момент (2019год) JavaScript представляет 7 типов данных и еще один (`BigInt`)
скоро будет добавлен в спецификацию языка. Ныншние типы это:
`undefined`, `null`, `boolean`, `number`, `string`, `symbol` и `object`.
Первые 6 часто называют примитивами или скалярами.

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

Все остальные структуры данных, такие как массивы (Array), множества (Set)
и даже функции (function) являются разновидностями объекта (Object).

Вычислить тип можно с помощью специального оператора `typeof`. Как можно предположить,
он возвращает одно из 7 возможных значений. Но в его работе есть пара ньюансов:

```javascript
console.log(typeof a); // undefined
console.log(typeof b); //! object 
console.log(typeof c); // boolean
console.log(typeof d); // number
console.log(typeof e); // string
console.log(typeof f); // symbol
console.log(typeof g); // object

var h = function (x) {
  return x;
};
console.log(typeof h); //! function
```

Было бы правильно, если бы `typeof null` возварщал `"null"`, он но возвращает `"object"`. Это баг языка,
который сохраняется в спецификации для обратной совместимости.

Кроме того `typeof` примененный к функциям возвращает `"function"`, а не `"object"`. Из-за чего может сложиться
впечатление, что функция - это один из базовых типов в JavaScript, хотя это не так.

### 1.3.2 Primitives and objects.

Каждый из примитивов хранит в себе единичное значение. Объекты являются наборами именованых значений, каждое из которых может
принадлежать любому из типов, в том числе может тоже быть объектом.

Ключевое различие между примитивами и объектами - то, что значения примитивов возвращаются и передаются как есть.
А значения объетов возвращаются и передаются в виде ссылки на хранящийся объект.

```javascript
var a = 1;
var b = a;
console.log(a); // 1
console.log(b); // 1
b += 5;
console.log(a); // 1
console.log(b); // 6
a = 4;
console.log(a); // 4
console.log(b); // 6

var objA = { name: 'Jake', age: 23 };
var objB = objA;
console.log(objA); // Object { name: "Jake", age: 23 }
console.log(objB); // Object { name: "Jake", age: 23 }
objB.age += 2;
console.log(objA); // Object { name: "Jake", age: 25 }
console.log(objB); // Object { name: "Jake", age: 25 }
objA.name = 'John';
console.log(objA); // Object { name: "John", age: 25 }
console.log(objB); // Object { name: "John", age: 25 }
```

При копировании примитива в новую переменную сохранилось значение предыдущей. И дальше они ведут себя, как независимые друг от друга значения.
Изменения одной из них никак не влияет на другую.

Что же происходит в случае с объектом? На первый взгляд может сложиться впечатление, что с помощью инструкции `var objB = objA;` мы создали
переменную `objB`, которая является копией `objA`, и дальше их поведение должно быть независимым. Но при модификации одной переменной меняется и другая.

Обратим внимение на пару ключевых отличий:
1. Во-первых, по результату выполнения иструкции `var objA = { name: 'Jake', age: 23 };` переменная `objA` хранит в себе не объект,
а только ссылку на этот объект (адресс в памяти, где он хранится). Поэтому с помощью иструкции `var objB = objA;` мы лишь производим
копировании ссылки из одной переменной в другую. И теперь обе переменные содержат одинаковую ссылку, а значит ссылаются на один и тот же объект.
И дальнейшие действия модифицируют свойства одного и того же объекта.

2. Во-вторых, мы не модифицируем переменные напрямую, а модифицируем свойства ассоциированых с ними объектов. Если производить действия над переменными
непосредственно, мы увидим независимое поведение переменных, как и в минисценарии с примитивами:
```javascript
var objC = { price: 48.75, quantity: 2 };
var objD = objC;
console.log(objC); // Object { price: 48.75, quantity: 2 }
console.log(objD); // Object { price: 48.75, quantity: 2 }
objD = 'Modified';
console.log(objC); // Object { price: 48.75, quantity: 2 }
console.log(objD); // Modified
objC = { done: false, value: 128 };
console.log(objC); // Object { done: false, value: 128 }
console.log(objD); // Modified
```

Провести полное копирование (клонирование) объекта - нетривиальная задача, учитывая то, что свойствами могут быть другие объекты, функции, а также наличие
неявных ссылок на прототип объекта.

Есть некоторая аналогия между объектами в JavaScript и механизмом ссылок в PHP
```php
$a = 1;
$b = &$a;
print($a); // 1
print($b); // 1

$b += 1;
print($b); // 2
print($a); // 2
```
С помощью объектов можно эмулировать подобное поведение в JavaScript. Для этого нужно обернуть необходимое значение в объект:
```javascript
var a = { value: 1 };
var b = a;
console.log(a); // Object { value: 1 }
console.log(b); // Object { value: 1 }
b.value += 1;
console.log(a); // Object { value: 2 }
console.log(b); // Object { value: 2 }
``` 

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
