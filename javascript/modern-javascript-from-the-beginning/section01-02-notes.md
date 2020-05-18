# Modern JavaScript from the Beginning

## Traversy Media/Udemy

- Source: [Modern JavaScript from the Beginning](https://www.udemy.com/modern-javascript-from-the-beginning/)

---

## 1. Console Log

- Search console on MDN

```javascript
console.log('Hello World');
console.error('This is an error');
console.warn('This is a warning');
console.table(); // returns in a table setup

console.time();
// do stuff
// ...
console.timeEnd(); // returns the time it took to 'do stuff'
```

## 2. Variables

- var is globally scoped
- let and const are block scoped

- variables can contain: letters, numbers, underscore, dollar
- cannot start with a number

```javascript
let age = 30;
age = 31;
console.log(age);

const age = 30;
age = 31;
console.log(age); // this would error out in the console
```

- const can't be reassigned
- with const you can change the values inside the object/array

### Init a variable

- useful for conditionals where greeting might be assigned something or something else
- you can't initialise const it needs a value from the start

```javascript
let greeting;
console.log(greeting); // returns undefined
greeting = 'Hello';
console.log(greeting); // returns Hello
```

## 7. Data Types

- two types of data types:

1.  primitive data types: stored directly in the location the varible accesses
2.  reference data types: accessed by reference

- Primitive data types: String, Numbers, Boolean, Null, undefined, Symbol

```javascript
const name = 'John'; // strings
const age = 30; // numbers
const rating = 4.5; // numbers
const isCool = true; // boolean
const x = null; // Null
const y = undefined; // Undefined
let z; // also undefined
const sym = Symbol(); // symbol

console.log(typeof isCool); // would return boolean
```

- reference types are:

  1. Arrays
  2. Object literals
  3. Functions
  4. Dates
  5. Anything else...

- JS is dynamically typed
- types are associated with values not variables
- variables can hold multiple types
- Typescript adds static typing to JS

## 8. Type Conversion

- converting numbers to string

```javascript
let val;

// Number to string
val = String(555);
//Bool to string
val = String(true);
// Date to string
val = String(new Date());

// OR
val = (5).toString();

// String to number
val = Number('5');
val = Number('true'); // 1
val = Number('false'); // 0
val = Number('null'); // 0
val = Number('hello'); // NaN
val = Number([1, 2, 3]); // NaN

val = parseInt('100.30'); // 100
val = parseFloat('100.30'); // 100.30
```

- type coersion is when JS automatically converts a type for us
- like when you add a string and a number e.g '5' + 6 returns 56 as a string

## 9. Numbers & The Math Object

- simple math operators: + - \* /
- the modulo ? returns the remainder
- Maths is an object - it has properties and methods

```javascript
val = Math.PI;
val = Math.round(2.4); // 2
val = Math.ceil(2.4); // 3
val = Math.floor(2.4); // 2
val = Math.sqrt(64); // 8 square root
val = Math.abs(-3); // 3 absolute number
val = Math.power(8, 2); // 64 power 8 power of 2
val = Math.min(2, 33, 4, 7, 8); // 2 minimum
val = Math.max(2, 33, 4, 7, 8); // 33 maximum

val = Math.random(); // returns a random decimal number between 0 - 1
// below returns a number between 1 - 20
val = Math.floor(Math.random() * 20 + 1);
```

## 10. String Methods & Concatenation

```javascript
const firstName = 'John';
const lastName = 'Fry';

let val;

val = firstName + lastName; // JohnFry

// Concatenation
val = firstName + ' ' + lastName; // John Fry

// Append
val = 'John ';
val += 'Fry'; // John Fry

// Escaping
// val = 'That\'s awesome, I can\'t wait';

// concat()
val = firstName.concat(' ', lastName); // John Fry

// String methods and properties
const s = 'Hello World';

let val;

val = s.length; // property because it doesn't have ()
val = s.toUpperCase(); // method
val = s.toLowerCase(); // method
val = s.indexOf('l'); // 2
val = s.LastIndexOf('l'); // 10

// Get last character
val = s.charAt(s.length - 1); // d

val = s.substring(0, 5); // returns Hello
val = s.substring(0, 5).toUpperCase(); // returns HELLO
val = s.slice(0, 5); // returns Hello

const s = 'techonology, computers, it, code';
val = s.split(''); // splits a string into an array returns each separate letter in the array
val = s.split(', '); // returns each word using the comma space as a separator

// replace()
val = s.replace('code', 'music'); // replaces 'code' with 'music/

// includes()
val = s.includes('computers'); // true - test if something is inside a string
```

## 11. Template Literals

- also called template strings
- introduced in ES6
- new way to concatenate strings in this instance

```javascript
const name = 'John';
const age = 30;
const job = 'Web Developer';
const city = 'Miami';
let html;

let s = `My name is ${name} and I am ${age}`;

// ES5 way (could also be one big long line)
html =
  '<ul>' +
  '<li>Name: ' +
  name +
  '</li>' +
  '<li>Age: ' +
  age +
  '</li>' +
  '<li>Job: ' +
  job +
  '</li>' +
  '<li>City: ' +
  city +
  '</li>' +
  '</ul>';

// ES6 template literals
html = `
  <ul>
    <li>name: ${name}</li>
    <li>age: ${age}</li>
    <li>job: ${job}</li>
    <li>city: ${city}</li>
  </ul>
`;
```

## 12. Arrays and Array Methods

- arrays are great for holding multiple values in a variable

```javascript
// using square brakets instead - more common
const numbers = [43, 56, 33, 23, 44, 36, 5];
// using the constructor array way
const numbers2 = new Array(1, 2, 3, 4, 5);

const fruits = [
  'apples',
  'oranges',
  'pears',
  22,
  true,
  null,
  { a: 1, b: 1 },
  new Date()
]; // can include multiple data types if you like

let val: // get array length
val = numbers.length; // 7
// check if is array
val = Array.isArray(numbers); // true
// get single value
val = numbers[3]; // 23
// Insert into an array
numbers[2] = 100; // changes 33 to 100
// find index of value
val = numbers.indexIf(36); // 5

// MUTATING ARRAYS
numbers.push(250); // adds it to the end of the array
numbers.unshift(120); // adds it to the start of the array
numbers.pop(); // removes the last item of the array
numbers.shift(); // removes the first item of the array
numbers.splice(1, 1); // removes the number in index 1
numbers.splice(1, 3); // removes the number in index 1, 2, 3
numbers.reverse(); // reverses values in the array

// concatenate arrays
val = numbers.concat(numbers2); // joins both arrays
// sort arrays
val = fruit.sort(); // puts it in alphabetical order

val = numbers.sort(); // orders by first number so not correct

// Use the "compare function" to fix numbers.sort() issue
val = numbers.sort(function(x, y) {
  return x - y;
}); // orders from lowest to highest

val = numbers.sort(function(x, y) {
  return y - x; // orders from highest to lowest
});

// Find
function over50(num) {
  return num > 50;
}

val = numbers.find(over50); // 56
```

## 13. Object Literals

```javascript
const person = {
  firstName: 'John',
  lastName: 'Doe',
  age: 30,
  hobbies: ['music', 'movies', 'sports'], // add array
  address: {
    // embedded object
    street: '50 main st',
    city: 'Boston',
    state: 'MA'
  }
};

let val;

val = person; // gives you the entire object
val = person.firstName; // gives you single value
val = person.hobbies[1]; // gives you movies
val = person.address.city; // gives you boston
val = person.address[city]; // gives you boston
```

### Destructuring

- using destructuring - pulling out variables from an object to use

```javascript
const {
  firstName,
  lastName,
  address: { city }
} = person;
console.log(firstName); // gives you firstname now
console.log(city); // gives you boston from the embedded object

person.email = 'john@gmail.com'; // adds to the object

// working with an array of objects
const todos = [
  {
    id: 1,
    text: 'Take out trash',
    isCompleted: true
  },
  {
    id: 2,
    text: 'Meeting with boss',
    isCompleted: true
  },
  {
    id: 3,
    text: 'Dentist appt',
    isCompleted: false
  }
];

console.log(todos[1].text); // returns meeting with boss

// to convert our array into JSON (good for sending to a server)
const todoJSON = JSON.stringify(todos);
console.log(todosJSON); // returns our array as a JSON string
```

## 14. Dates & Time

- JS has the Date object for working with dates and times

```javascript
let val;

const today = new Date();
val = today: // returns todays date, time etc

let birthday = new Date('9-10-1981 11:25:00'); // returns Thu Sep 10 1981 11:25
birthday = new Date('September 10 1981'); // returns Thu Sep 10 1981 00:00
birthday = new Date('9/10/1981'); // returns Thu Sep 10 1981 00:00
val = birthday:

val = today.getMonth(); // 0 based so Jan would be 0
val = today.getDate();
val = today.getDay(); // 0 based so Sun would be 0
val = today.getFullYear();
val = today.getHours();
val = today.getMinutes();
val = today.getSeconds();
val = today.getMilliseconds();
val = today.getTime(); // returns timestamp of seconds passed since Jan 1 1970

// manipulating dates
birthday.setMonth(2);
birthday.setDate(12);
birthday.setFullYear(1985);
birthday.setHours(3);
birthday.setMinutes(30);
birthday.setSeconds(30);

// the above produces Tue Mar 12 1985 03:30:25
```

## 15. If Statements and Comparison Operators

- if statements are used to evaluate some kind of condition then do something based on that condition

```
if(somehthing) {
  do something
} else {
  do something else
}
```

```javascript
// if else / else if
const x = 10;

if (x === 10) {
  console.log('x is 10');
} else if (x > 10) {
  console.log('x is greater than 10');
} else {
  console.log('x is NOT 10');
}

// can also do not equal !==

// to test if something is not defined
if (typeof x !== 'undefined') {
  console.log(`x is ${x};`);
} else {
  console.log('No x');
}

// Also have: < <= > >=

// IF ELSE
const color = 'red';

if (color === 'red') {
  console.log('color is red');
} else if (color === 'blue') {
  console.log('color is blue');
} else {
  console.log('color is NOT red or blue');
}

// Logical Operators
const x = 10;
const y = 11;

if (x > 5 || y > 10) {
  // or one needs to be true
  console.log('x is more than 5 or y is more than 10');
}

if (x > 5 && y > 10) {
  // and - both need to be true
  console.log('x is more than 5 or y is more than 10');
}

// ternary operator
const x = 10;
const color = x > 10 ? 'red' : 'blue'; // returns blue in this case
// if x is great than 10 THEN red ELSE blue
```

## 16. Switches

- good when you have lots of conditions to test

```javascript
const color = 'red';

switch (color) {
  case 'red':
    console.log('color is red');
    break;
  case 'blue':
    console.log('color is blue');
    break;
  default:
    console.log('color is NOT red or blue');
    break;
}
```

## 17 Function Declarations & Expressions

```javascript
// function declaration
function addNums(num1, num2) {
  console.log(num1 + num2);
}
addNums(5, 4); // calling the function returns 9
addNums(); // NaN - not a number

function addNums(num1 = 1, num2 = 2) {
  //setting default values to our parameters
  console.log(num1 + num2);
}
addNums(); // now returns 3

function addNums(num1, num2) {
  return num1 + num2; // you normally return a value from your function
}
console.log(addNums(5, 4)); // returns 9

// FUNCTION EXPRESSIONS
// assigns the function to a variable
// good for hoisting and closures
const square = function() {
  return x * x;
};
```

### IIFEs

```javascript
// IIFEs
// need to wrap in all in brackets
// also end with brackets () to run it immediately
(function() {
  console.log('IIFE ran...');
})();

// IIFe with parameters
(function(name) {
  console.log(`Hello ${name}`);
})('John');
```

### Property Methods

- can add functions inside of objects - called methods

```javascript
const todo = {
  add: function() {
    console.log('Add todo...');
  },
  edit: function(id) {
    console.log(`Edit todo ${id}`);
  }
};

// can also put them outside the object and still use them on the object
todo.delete = function() {
  console.log('delete todo...');
};

todo.add();
todo.edit(22);
todo.delete();
```

### Arrow Functions

```javascript
// old way
function addNums(num1, num2) {
  console.log(num1 + num2);
}
// converting above addNums function into an arrow function
const addNums = (num1, num2) => {
  return num1 + num2;
};
console.log(addNums(5, 4)); // returns 9

// because we're just returning 1 thing we can simplfy it further
const addNums = (num1, num2) => num1 + num2; // uses an implicit return

// for 1 parameter you can get rid of the parentheses
const addNums = num1 => num1 + 5;

// good for nice short forEach loops
todos.forEach(todo => console.log(todo));
```

---

## 18. General Loops

```javascript
// For
for (i = 0; i <= 10; i++) {
  console.log(`For Loop Number: ${i}`);
}

// if you don't know the length of the array
const todos = [
  {
    id: 1,
    text: 'Take out trash',
    isCompleted: true
  },
  {
    id: 2,
    text: 'Meeting with boss',
    isCompleted: true
  },
  {
    id: 3,
    text: 'Dentist appt',
    isCompleted: false
  }
];

for (i = 0; i <= todos.length; i++) {
  console.log(todos[1].text); // output text for each todo
}

// Skipping or change something for an item in a loop
for (let i = 0; i < 10; i++) {
  if (i === 2) {
    console.log('2 is my favourite number');
    continue; // uses the continue property to keep loop going after this
  }
  if (i === 5) {
    console.log('Stop the loop');
    break; // uses the break property to stop the loop
  }

  console.log(`Number ${i}`);
}

// While
// good when you don't know how long the loop needs to be
let i = 0; // assign variable outside of loop
while (i <= 10) {
  console.log(`While Loop Number: ${i}`);
  i++; // iincrement i
}

// Do While
// Runs the first time no matter what
let i = 0;
do {
  console.log(`Number ${i}`);
} while (i > 10);
```

### For In

```javascript
const user = {
  firstName: 'John',
  lastName: 'Doe',
  age: 40
};
for (let x in user) {
  console.log(`${x} : ${user[x]}`);
}
```

### Higher order array methods - forEach, map, filter

- these methods take in a function as a parameter and a assign variable as a parameter - todo in this case
- can take multiple parameters
- can use arrow functions as well

### Foreach

```javascript
const todos = [
  {
    id: 1,
    text: 'Take out trash',
    isCompleted: true
  },
  {
    id: 2,
    text: 'Meeting with boss',
    isCompleted: true
  },
  {
    id: 3,
    text: 'Dentist appt',
    isCompleted: false
  }
];

for (let todo of todos) {
  // todo could be anything but the second value need to be the array
  console.log(todo);
  // Or
  console.log(todo.text); // don't need [i] ids
}
```

### Map

- returns a new array from the array
- assign the new array as a variable first

```javascript
const todoText = todos.map(function(todo) {
  return todo.text;
});

console.log(todoText); // returns a new array with just the text
```

### Filter

- returns a new array based on a condition

```javascript
const todoCompleted = todos.filter(function(todo) {
  return todo.isCompleted === true;
});

console.log(todoCompleted); // returns a new array with just the completed items

// tak on another method like map
const todoCompleted = todos
  .filter(function(todo) {
    return todo.isCompleted === true;
  })
  .map(function(todo) {
    return todo.text;
  });

console.log(todoCompleted); // returns a new array with just the completed items but only shows the text value
```

## Object Orientated Programing

- Source: https://www.youtube.com/watch?v=hdI2bqOjy3c

```javascript
// constructor function to create objects
function Person(firstName, lastName, dob) {
  this.firstName = firstName;
  this.lastName = lastName;
  this.dob = new Date(dob); // use date constructor to turn this into a date object
  this.getBirthYear = function() {
    return this.dob.getFullYear();
  };
  this.getFullName = function() {
    return `${this.firstName} ${this.lastName}`;
  };
}

// instantiate object
const person1 = new Person('John', 'Doe', '4-3-1980');
const person2 = new Person('Mary', 'Smith', '3-6-1970');

console.log(person2);
console.log(person1.firstName);
console.log(person2.dob); // returns Fri Mar 06 1970 00:00:00 GMT-0500 (Eastern Standard Time)
console.log(person2.dob.getFullYear()); // returns 1970 - lots of methods you can do on a date object
console.log(person1.getBirthYear()); // returns '1980' from our method
console.log(person1.getFullName()); // returns 'John Doe' from our method
```

### Prototypes

- using prototypes rather than methods in our object
- results in cleaner object, just in case we don't need to call those methods

```javascript
function Person(firstName, lastName, dob) {
  this.firstName = firstName;
  this.lastName = lastName;
  this.dob = new Date(dob);
}

// to avoid these methods always being called on an object we can pull them out as a prototype
Person.prototype.getBirthYear = function() {
  return this.dob.getFullYear();
};
Person.prototype.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};

console.log(person1.getFullName()); // returns 'John Doe' from our method
console.log(person1); // returns object without the methods
```

## Classes

- ES6 introduced class syntax - same as constructor above but much cleaner to write

```javascript
class Person {
  constructor(firstName, lastName, dob) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.dob = new Date(dob);
  }
  // methods still get added to the prototype
  getBirthYear() {
    return this.dob.getFullYear();
  }

  getFullName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

console.log(person1.getFullName()); // returns 'John Doe' from our method
console.log(person1); // returns object without the methods
```

## 20. A Look At the Window Object

- window is the global scope when dealing with clientside
- if you type window into the console you get all the methods, objects, properties

```javascript
// Alert
alert('Hello World');

// Prompt
const input = prompt();
alert(input);

// Outer height and width of widown
val = window.outerHeight;
val = window.outerWidth;

// Inner height and width of widow without scroll bars
val = window.innerHeight;
val = window.innerWidth;

// Scroll points
val = window.scrollY;
val = window.scrollX;

// Location Object
val = window.location;
val = window.location.hostname;
val = window.location.port;
val = window.location.href;

// Redirect
window.location.href = 'http://google.com';
// Reload
window.location.reload();

// History Object
window.history.go(-2); // will take us to the previous URL by 2
val = window.history.length; // show how many URLs are in history

// Navigator Object - info about browser
val = window.navigator;
val = window.navigator.appName;
val = window.navigator.appVersion;
val = window.navigator.userAgent;
val = window.navigator.platform;
val = window.navigator.vendor;
val = window.navigator.language;
```
