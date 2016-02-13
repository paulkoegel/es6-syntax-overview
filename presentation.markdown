class: s s-center c-title
background-image: url(images/escher-spiral-cropped.jpg)

.c-title--headline[
# An ECMAScript 6 Syntax Primer

January 2016
]

---

class: s s-center s-center_left s-narrow

.u-center[
# Aim
]
+ enable you to **read ES6** syntax &ndash; the new _lingua franca_ of the JavaScript community
+ introduce **new and upcoming features**

---

# Topics
+ Overview of ES6
+ Babel

---

class: s s-center s-center_left

#### JavaScript
+ colloquially: **"the language"**
+ strictly speaking: name of Mozilla's implementation

#### ECMAScript
+ European Computer Manufacturers Association (standards organisation)
+ the **standard** & official name of the language
+ **"versions of JavaScript"**

#### TC39
+ ECMA Technical Committee 39
+ defines the standard
+ browser vendors & experts

???
+ Terminology / Nomenclature
+ ECMA is similar to ISO

---

class: s s-center s-center_left

##### ES3 <span class='u-unbold text-smallest'>(2008)</span>
  -  most JS code today
  - Clojure- & CoffeeScript compile to ES3

.u-margin-bottom-20.u-strikethrough[
##### ES4
]

.u-margin-bottom-20[
##### ES5 <span class='u-unbold text-smallest'>(2009)</span>
]

.u-margin-bottom-20[
##### ES6 / ES2015 <span class='u-unbold text-smallest'>(June 2015)</span>
  - [www.ecma-international.org/ecma-262/6.0](http://www.ecma-international.org/ecma-262/6.0)
]

.u-margin-bottom-20.text-grey[
##### ES7 / ES2016
]

.u-margin-top-20[
.u-margin-top-20[
##### <span class='text-green'>ES Harmony</span>
  - ES6 & 7
]]

???
+ There is no **ES4** - never finalized and hence discarded.
+ ES5 is well supported (IE9+)
+ Harmony: used to be a Babel compiler flag and you might come across it

---

class: s s-center s-center_left s-narrow

.u-center[
# Motivation
]

+ JS is being used for more than it's been designed, it has become **dangerous**
+ no proper standard library &rArr; **new syntax** and features
+ **harmonization** of promises, modules, classes etc.
+ fully **backwards-compatible** &ndash; ES6 only adds features
+ six years in the making &ndash; had to be careful not to introduce new pitfalls

---

class: s s-center

## How to use it

.c-list.u-width-70[
+ **partial support** in current browsers
+ **transpilation** to ES5 ([Babel](https://babeljs.io), [Traceur](https://github.com/google/traceur-compiler))
]

???
**transpilation**: compilation from one high-level programming language to another

---

class: s s-bottom s_padding-none
background-image: url(images/es6-compatibility.png)

.s--headline-overlay[
# ES6 compatibility table
]

???
+ red fields: lots left to do
+ .grey[
[https://kangax.github.io/compat-table/es6](https://kangax.github.io/compat-table/es6)
]

---

class: s s-center s-wide

## `let`: local variables with block scope

.row[
.col.col-50.u-no-padding-horizontal[
.text-center[
### ES5
]
.text-code_medium[
```
var outer = 'Hello';

if(true) {
  ±var± inner = outer;
}

console.log(inner === outer);
// ±true±
```]
]

.col.col-50.u-no-padding-horizontal[
.text-center[
### ES6
]
.text-code_medium[
```
let outer = 'Hello';

if(true) {
  ±let± inner = outer;
}

console.log(inner === outer);
// ±ReferenceError:±
// inner is not defined
```]
]
]

???
+ `let` uses block scope

---

class: s s-center s-wide

## `let` is safer: no more Hoisting

.text-code_medium[
```javascript
±var snack± = 'Meow Mix';

function getFood(food) {
  if (food) {
    ±var snack± = 'Friskies';
    return snack;
  }
  return snack;
}

getFood(false); // ±undefined±
```
]

???
+ even if `return snack` stood in the first line of `getFood` it would be undefined - declaration is hoisted to the beginning of the function body.

---

class: s s-center s-wide

## `const`: constants with block scope

```javascript
*const PI = 3.14159;

PI = 4;
// fails silently

const PI = 4;
*// TypeError: redeclaration of const PI

```

???
+ **`PI=4`** fails silently in Chrome and Firefox, reassignment in Babel REPL and Safari :(
+ **`const PI=4`**: no error in Babel REPL since it's converted to `var PI=3.14`

---

class: s s-center s-wide

## `const`

.text-code_medium[
```javascript
const settings = {
  url: "https://example.com"
};

settings = {}; // ERROR

*settings.url = "https://evil.com"; // NO error
```
]

???
+ "Creates a read-only reference to a value. It does not mean the value it holds is immutable" (MDN)
+ The part that's constant is the reference to an object stored within the constant variable, not the object itself.

.text-grey[
+ "If a primitive value (such as a string, number, or boolean value) is assigned to a constant variable, that variable will be a true constant. Our PI constant is an example for this. There's no way to modify the value of the numeric literal 3.141592653589793 after it has been assigned."
+ use `Object.freeze` (shallow) or immutable.js/mori to make objects immutable.
+ More: [https://blog.mariusschulz.com/2015/12/31/constant-variables-in-javascript-or-when-const-isnt-constant](https://blog.mariusschulz.com/2015/12/31/constant-variables-in-javascript-or-when-const-isnt-constant)
]

---

class: s s-center

### Template Strings: Interpolation

```javascript
let firstName = 'Peter';
let lastName = 'Parker';

// ES5
firstName + ' ' + lastName;
// 'Peter Parker'

// ES6
*`${firstName} ${lastName}`
// 'Peter Parker'
```

---

class: s s-center s-wide

## Arrow Functions

.row[
.col.col-50.u-no-padding-horizontal[
.text-center[
#### ES5
]
.text-code_medium[
```javascript
var name = ±function(f, l)± {
  return f + ' ' + l;
};


±function hello()± {
  // ...
}
```]
]

.col.col-50.u-no-padding-horizontal[
.text-center[
#### ES6
]

.text-code_medium[
```javascript
let name = ±(f, l) =>± {
  return `${first} ${last}`;
}


±function hello()± {
  // ...
}
```
]]]

???
+ arrow functions are **always anonymous**

---

class: s s-center

.text-smallest[Arrow Functions]
## Shorthand

.u-width-100.text-left[
  .text-smallest[One argument, single line body, implied `return`:]]
.text-code_medium.u-width-100[
```javascript
let func = ±x => x * x±;
```
]

.u-width-100.text-left[ 
.text-smallest[Block body requires `return`:]]
.text-code_medium.u-width-100[
```javascript
let func = (x) => ±{ return± x * x; ±}±;
```
]

.u-width-100.text-left[ 
.text-smallest[No argument:]]
.text-code_medium.u-width-100[
```javascript
let func = ±()± => 55;
```
]

???
+ no parens for single argument
+ empty parens required for no argument


---

class: s s-center s-wide

.text-smallest[Arrow Functions]
## As Object Methods

.text-code_medium[
```javascript
let car = {

  ±drive()± { // ±shorthand without arrow±
    // ...
  },

  ±stop: () => {± // rarely used
    // ...
  }

};
```
]

???
+ syntax modeled after ES5 [getter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get) and [setter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set) functions

---

class: s s-center

implicit returns:
[http://stackoverflow.com/a/28889451](http://stackoverflow.com/a/28889451)

---

class: s s-center s-wide

## .text-smaller[Arrow Functions:] `this`

.row[
.col.col-50.u-no-padding-horizontal[
.text-center.u-no-margin[
#### ES5
]
.text-code_small[
```javascript
var person = {
  age: 0,

  growOld: function() {
    this; // person
    setInterval(±function()± {
      ±this±.age++; // window
    }, 1000);
  }
};

person.growOld();
// age ±doesn't change±, remains 0
```
]
]

.col.col-50.u-no-padding-horizontal[
.text-center.u-no-margin[
#### ES6
]
.text-code_small[
```
let person = {
  age: 0,

  growOld: function() {
    this; // person
    setInterval(±() =>± {
      ±this±.age++; // person
    }, 1000);
  }
};

person.growOld();
// age ±changes± every second

```
]
]
]

???
+ in ES5 müsste `this` vorher in `self` oder `that` gespeichert werden.

---

class: s s-wide s-center

## Default Parameters

.text-code_medium[
```javascript
let myFunc = (a, ±b=1±) => {
  console.log(a, b);
};

myFunc('Hello'); // => 'Hello', 1

myFunc(99, 2);   // => 99, 2
```
]

---

class: s s-center

## Variadic Functions

.text-code_medium[
```javascript
const myFunc = (first, ±...rest±) => {
  console.log(first, rest);
}

myFunc(1, 2, '3'); // => 1 [2, '3']
```
]

<br>
+ `...` is called the **spread operator**
+ arrow functions have no `arguments`

???

---

class: s s-center

.text-smaller[Summary]
## Arrow Functions

+ **shorter syntax**
+ **lexical `this`** - do not bind their own `this`

+ must type `()` when there are no arguments.
+ are strictly anonymous (no .u-strikethrough[```myFunc (x) => { ... }```])
+ have no `arguments` (use rest arguments `...rest` instead)
+ cannot be called with `new`

???
+ `()` cannot be left out.

---

class: s s-center

### `let` & Arrow Functions

+ should be used instead of `var` and `function`
+ make JavaScript code safer:
  - `let`: no more hoisting with `let`
  - arrow functions: less error-prone thanks to lexical `this`

---

# Klassen

```javascript
class Vehicle {
  constructor(name) {
    this.name = name;
    this.kind = 'vehicle';
  }
  getName() {
    return this.name;
  }
}

let myVehicle = new Vehicle('rocky');
```

---

class: s s-center s-wide

.text-smaller[Arrow Functions]
.u-no-margin-top[
### Arrow Functions in Objects -vs- in Classes
]

.row[
.col.col-50.u-no-padding-horizontal[
.text-code_medium[
```javascript
let car = {

  ±drive()± {
    // ...
  }±,± // comma

  stop() {
    // ...
  }
};
```
]]

.col.col-50.u-no-padding-horizontal[
.text-code_medium[
```javascript
class Car {

  drive() {
    // ...
  ±}± // ±NO COMMA±

  stop() {
    // ...
  }
};
```
]]
]

???
+ **no arrow**, but a **comma** (keep in mind for classes)
+ commas: https://hacks.mozilla.org/2015/07/es6-in-depth-classes/comment-page-1/#comment-18014

---


# Vererbung

```javascript
class Car extends Vehicle {
  constructor(name) {
     super(name);
     this.kind = 'car'
  }
}
```

---

# Module: `export`

```javascript
// lib/math.js
*export function sum(x, y) {
  return x + y;
}
*export var pi = 3.141593;


// app.js
import { sum, pi } from "lib/math";
```

???
+ mehrere `export`s in einer Datei
+ `export` muss explizit sagen was exportiert wird (function oder var)

---

# Module: `export default`

```javascript
// lib/do-something.js
*export default function() {
  return 1;
}


// app.js
*import doSomething from 'lib/do-something';
doSomething();
```

---

class: s s-top

## Destrukturierung von Objekten

```javascript
let user = { first: "Peter", last: "Parker"};

*let {first, last} = user; // Reihenfolge egal
first; // "Peter"


*let {first: f, last: l} = user; // mit Aliasen
f; // "Peter"
```

---

## Destrukturierung von Funktionsargumenten

```javascript
let user = {first: 'Peter', last: 'Parker'};

*let greet = ({first, last}) => {
  return `Hallo, ${first} ${last}!`;
}

greet(user); // 'Hallo, Peter Parker!'
```

---

# Destrukturierung von Arrays

.text-code_medium[
```javascript
// ES5
var first = someArray[0];
var second = someArray[1];
var third = someArray[2];

// ES6
*let [first, second, third] = someArray;

*let [first, second, ...rest] = [1, 2, 3, 4, 5];
first;     // 1
second;    // 2
rest;      // [3, 4, 5]
```
]

+ `...` heißt **Spreadoperator**

---

# Destrukturierung von Arrays

```javascript
*let [x, y] = ['a']
x; // 'a'
y; // undefined
```

### Mit Standardwerten

```javascript
*let [x, y='b'] = ['a']
x // 'a'
y // 'b'
```

---

# Standardwerte bei Funktionen

```javascript
*function doSomething(a, b = 0) {
  return([a, b]);
}

doSomething(2);
// [2, 0]
```

---

# Objektliterale in Kurzschreibweise

```javascript
let name =  'Jim Smith';
let email = 'jim@smith.com';
*let user = { name, email };

user // {name: 'Jim Smith', email: 'jim@smith.com'}
```

Kurzschreibweise für:
```javascript
let user = { name: name, email: email }
```

---

class: s s-center

# Neue Arraymethoden

```javascript

[5, 1, 10, 8].find(n => n === 10) // 10

[5, 1, 10, 8].findIndex(n => n === 10) // 2
```

???
+ man kann Pfeilfunktionen auch in einer Zeile schreiben

---

class: s s-center

# Spreadoperator

statt `apply`:
```javascript
let attributes = ['Peter Parker', 'peter@parker.com']

let buildUser = (name, email) => {
  return { name, email };
}

*buildUser(...attributes);
// { name: "Peter Parker", email: "peter@parker.com" }
```

---

class: s s-center

# Zugewinne durch ES6

.c-list.u-width-70[
+ Syntaxverbesserungen
+ .text-background-translucent-yellow[ `this` in Pfeilfunktionen]
+ .text-background-translucent-yellow[block scope durch `let`
]
+ wichtige Vereinheitlichungen durch Einbetten in Sprache von:
  - **Klassen**
  - **Modulen**
]

---

class: s s-center

# Einige weitere ES6 Features
+ Promises
+ Generator-Funktionen
+ Symbols
+ Maps
+ Sets
+ Erweiterungen der Standardbibliothek

???
.grey[
+ Maps: An Object has a prototype, so there are default keys in the map. This could be bypassed by using map = Object.create(null) since ES5, but was seldomly done.
The keys of an Object are Strings and Symbols, where they can be any value for a Map.
[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)
]

---

class: s s-center s_background-blue

# B. Wie heute schon benutzen?

---

class: s s-top s_background-35 s_background-bottom
background-image: url(images/babel.png)

.u-margin-bottom-20[
# ES6 heute nutzen
]

.c-list.u-width-70[
+ [**Babel**](https://babeljs.io/): transpiliert ES6/7 zu ES5
+ daneben **Webpack** / Browserify als Packaging Tools, um ES6 Module zu nutzen
+ [paulkogel/simple-babel-webpack-starter-pack](https://github.com/paulkogel/simple-babel-webpack-starter-pack)
]

???
+ Link zu Jeremy Ashkenas-Vortrag "The Transpilers are coming"

---

class: s s-center s_background-blue

# ENDE. Fragen?

Danke für's Zuhören.  
Lasst mich wissen, wenn ihr was gebaut habt oder Probleme habt.

@wakkahari  
paul@railslove.com

---

# ES6 only adds features

+ ES6 adds well-signposted paths (e.g. `let`), but you can still run into hoisting problems when you follow an older path - by using `var` instead.

+ leather gloves, Waffe, safest: use a compile-to-JS language

---

ESLint
AirBnB Styleguide

https://leanpub.com/understandinges6/
http://exploringjs.com/
http://www.2ality.com/
http://javascriptweekly.com/
https://github.com/lukehoban/es6features#readme
