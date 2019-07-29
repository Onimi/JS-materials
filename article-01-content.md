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

Значения сохраненные в объекте назвываются свойствами (полями) объекта, имена свойств - ключами объекта. В общем, объект в JavaScript
можно представить себе, как набор пар "ключ-значение", что делает его похожим на ассоциативные массивы из PHP.

Обычно объекты создаются с помощью литерала объекта (`Object literal`):
```javascript
var product = {
  name: "Chocolate",
  weight: 100,
  price: 21.5,
  available: true,
};
```
Доступ к свойствам объекта можно получить с помощью оператора точки (точечной нотации) или с помощью оператора квадратных скобок (скобочной нотации):
```javascript
console.log(product.name); // "Chocolate"
console.log(product.weight); // 100
console.log(product.price); // 21.5
console.log(product.available); // true

console.log(product["name"]); // "Chocolate"
console.log(product["weight"]); // 100
console.log(product["price"]); // 21.5
console.log(product["available"]); // true
```
Точечная нотация более краткая и предпочтительная. Но ее невозможно использовать если ключ не является стандартным JavaScript-идентификатором,
т.е, если он начинается с цифры или содержит пробелы и спецсимволы. Кроме того скобочная нотация - едиственный выход, если мы заранее не знаем
значения ключа, а получили его из переменной или как результат работы функции.
```javascript
var key = "weight";
console.log(product[key]); // 100
```

Ключевое различие между примитивами и объектами - то, что значения примитивов возвращаются и передаются так, как есть.
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
неявных ссылок на прототип объекта или циклических ссылок.

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

В JavaScript у переменных нет типа. Тип относится к значанию, сохраненному в переменной. И сама переменная не накладывает
каких-либо ограничений на сохраненные в ней значения, причем типы этих значений могу меняться в процессе выполнения кода.
Любая переменная в какой-то момент может хранить логическое значение, затем число, затем строку, затем объект, и т.д.

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

Числа в JavaScript реализуют формат [чисел с плавающей точкой](https://en.wikipedia.org/wiki/Floating-point_arithmetic).
Что дает ограничения на точность вычислений. А также неожиданное поведение в некоторых ситуациях.
Наиболее известный из таких примеров:
```javascript
0.1 + 0.2 === 0.3; // false
```
Но абсодтно такая же ситуация есть и в других языках, которые используют числа с плавающей точкой:
```php
$test = (0.1 + 0.2 === 0.3);
var_dump($test); // bool(false)
```

Также к числам в JavaScript относятся 4 спецзначения:
- `NaN` - (Not-A-Number) результат некорректной арифметической операции (деление/умножение на строку)
- `Infinity` - результат деления неотрицательного числа на нуль.
- `-Infinity` - результат деления отрицательного числа на нуль.
- `-0` - результат деления ннотрицательно числа на `-Infinity`.

Для описания чисел можно использовать стандартную десятичную запись, а также бинарную, восьмиричную, шестанадцатиричную и экспоненциальную:
```javascript
var decimal = 125;
var binar = 0b1111101;
var octal = 0o175;
var hexadecimal = 0x7d;
var exponental = 1.25e2;

var fractal = 125.788;
```

### 1.3.5 Wrapper objects.

Для логических значений, чисел, строк и символов есть вспомогательные объекты-обертки - `Boolean`, `Number`, `String`, `Symbol`.
Они содержат дополнительную функциональность (специальные занчения и методы) для работы с этими типами.

Так, например, `Number` содержит:

Спецзначения:
- `Number.MAX_VALUE` - наибольшее числовое значение представимое в JavaScript.
- `Number.MIN_VALUE` - наименьшее числовое значение представимое в JavaScript.
- `Number.NaN` - (Not-A-Number) результат некорректной арифметической операции.
- `Number.POSITIVE_INFINITY` (тоже самое, что и `Infinity`) - спецзначение указывающее, что результат вычислений вышел за представимые границы.

Статические методы:
- `Number.isInteger()` - проверка на целочисленность
- `Number.parseFloat()` - преобразование строки в дробное число
- `Number.parseInt()` - преобразование строки в целое число

Методы экземпляра (методы прототипа):
- `toFixed()` - округление до определенного количества знаков после запятой
- `toString()` - представление в виде строки

Объект `String` предоставляет возможность определить длину строки с помощью свойства `length`.
А также такие методы для работы с экземпляром строки, как `charAt()`, `includes()`, `match()`, `replace()`, `slice()`, `split()` и другие.
```javascript
var text = new String("Hello, my name is Bob.");
console.log(text.length); // 22
console.log(text.includes("name")); // true
console.log(text.replace("Bob", "James")); // "Hello, my name is James."
console.log(text.slice(0, 5)); // "Hello"
```

Важным моментом является то, что для получения доступа к вспомогательной фунциональности необязательно явно инициализировать объекты-обертки:
```javascript
var str = "Hi there, everybody";
var strObj = new String(str);
console.log(strObj.length); // 19
console.log(strObj.split(" ")); // [ "Hi", "there,", "everybody" ]
```

На самом деле эта инициализация может быть опущена. И следующий код будет рабочим:
```javascript
var str = "Hi there, everybody";
console.log(str.length); // 19
console.log(str.split(" ")); // [ "Hi", "there,", "everybody" ]
```

В этом случае объекты-обертки будут неявно вызваны движков JavaScrit. И такой стиль считается более предпочтительным.

### 1.3.6 Strict and non-strcit comparison.

JavaScript предоставляет 3 явных метода сравнения значений (и один неявный).
Это "строгое сравнение", "нестрогое сравнение" и "сравнение на одинаковое значение".
Производятся они за счет операторов `===` и `==`, а также метода `Object.is()`.

#### 1.3.6.1 Strict equality using `===`

Строгое сравнение (`left === right`) в процессе своей работы выполняет примерно следующие шаги:
1. Если `left` и `right` - значения разных типов, то сразу возвращается `false`
2. Если `left` и `right` - одного типа и не являются числами, тогда:
   1. в случае совпадения значений возвращается `true`,
   2. в случае различных значений возвращается `false`.
3. Если `left` и `right` - являются числами, тогда:
   1. Если они оба не являются `NaN` и их значения совпадают, то возвращается `true`,
   2. Если одно из них `0`, а другое `-0`, то возвращается `true`,
   3. Во всех остальных случаях возвращается `false` (в том числе если оба значения являются `NaN`).

На примерах:
```javascript
var num = 0;
var obj = new String('0');
var str = '0';

console.log(num === 0); // true
console.log(num === -0); // true
console.log(obj === obj); // true
console.log(str === '0'); // true

console.log(Number.NaN === Number.NaN); // false
console.log(num === obj); // false
console.log(num === str); // false
console.log(obj === str); // false

console.log(null === undefined); // false
console.log(obj === null); // false
console.log(obj === undefined); // false
```

`left !== right` дает тот же результат, что `!(left === right)`.

#### 1.3.6.2 Loose equality using `==`

#### 1.3.6.3 Same-value equality using `Object.is()`

### 1.3.7 Array as subtype of Object.

## 1.4 Functions and scope

### 1.4.1 Functions as the first class citizens

### 1.4.2 Lexical scope

### 1.4.3 Functional scope. `var` variables

### 1.4.4 Block scope. `let` and `const` variables

### 1.4.5 Closures
