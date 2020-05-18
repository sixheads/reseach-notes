# Better Javascript - LevelUp Tuts

## 1. Functions

### Function Declaration

- this is hoisted so its automatically pulled to the top of the doc

```javascript
function sayHi() {
  return console.log('Say Hi');
}

sayHi(); // running the function
```

### Function Xxpression

- this can be used as a callback
- the function itself is annonymous
- not hoisted

```javascript
const sayHi = function() {
  return console.log('Say hi');
};
```

### Arrow function

```javascript
const sayHi = () => {
  return console.log('Say hi');
};

// short version omitting the return
// works for single line functions like this
const sayHi = () => console.log('Say hi');
```

---

## 2. Destructuring

- allows you to pull out itens from an array or object and assign them to a variable of the same name
- keeps your code a bit more readable
- less need to make up lots of variable names

```javascript
const newArray = ['hi', 'john', 'item3'];

// old way to pull out items and assign to variables
const a = newArray[0];
const b = newArray[1];

// new destructuring way
const [a, b, c] = newArray;
```

- we can use destructuring to simply our objects

```javascript
const person = {
  name: 'Scott',
  age: 32,
  job: 'Web Dev'
};

// old way to set variables
const makePerson = (name, age, job) => {
  return {
    name: name,
    age: age,
    job: job
  };
};

// new way that simplfies the return statement because the names are all the same
const makePerson = (name, age, job) => {
  return {
    name,
    age,
    job
  };
};

const dev = makePerson('scott', 32, 'web dev');
// old way to pull out the varibles
const name = dev.name;

// pulling out a property from the object and assigning a new variable
const { name, age } = dev;

// can also use the spread operator to pull out the rest
// 'rest' could have been named anything & returns the other to items in the object
const { name, ...rest } = dev;

// react example
// this.props.names
// const { names } = this.props;
// ...this.props
```

---

## 3. Names Parameters

- makes more robust use if parameters
- means that refactoring can be less error prone

```javascript
// old way
const makePerson = (name, age, job) => {
  return {
    name,
    age,
    job
  };
};

const dev = makePerson('scott', 32, 'web dev');

// New way using named parameters
// just add parentheses around the parameters and arguments
const makePerson = ({ firstName, age, lastName, job }) => {
  return {
    name: firstName + ' ' + lastName,
    age,
    job
  };
};

// then used the named parametes here
const dev = makePerson({
  firstName: 'Scott',
  lastName: 'Tolinski',
  age: 32,
  job: 'Web Dev'
});
```

---

## 4. Naming things

- Consistancy Is King
- Clear, searchable & obvious
- Var names that make sense

```javascript
// Constant variables uppercase syntax common
// no need for comment to explain
const BASE_SALARY = 16000;
const SALARY_MULTIPLIER = 4;

const makePerson = ({ firstName, age, lastName, job }) => {
  return {
    name: firstName + ' ' + lastName,
    age,
    job,
    salary: BASE_SALARY * SALARY_MULTIPLIER
  };
};
```

---

## 5. Immutable & Pure Functions

These are concepts from functional programming:

- Immutable vs Mutable
- can't be changed vs can be changed
- isn't changed vs changed

- ideally we want to use immutable variables

```javascript
const name = 'John';
// bad option for adding the last name
name = name + ' Fry';

// better to create a new variable that makes sense
const fullName = name + ' Fry';
```

### Pure Functions

- Always return the same thing, with the same input

```javascript
// Pure - always returns the same result
const addTwo = x => x + 2;
console.log(addTwo(2));
console.log(addTwo(2));
console.log(addTwo(2));

// NOT PURE!!
let multi = 2; // modifting External State
const addThree = x => x + multi;
console.log(addThree(2));
multi = 4;
console.log(addThree(2));
multi = 6;
console.log(addThree(2));
```

---

## 6. Benefits of Smaller Functions

- great example of refactoring a function into smaller simpler functions
- look at creating pure functions pulled out of the original function
- original function was doing too many things
- makes for much more testable code, you can test the pure functions separately
- means you can also re-use some of the smaller functions elsewhere

---

## 7. JS & THE DOM + MDN DOCS

- MDN provides an amazing resource for coding in general, but especially for manipulating the DOM
- check out the Web API reference, specifically the DOM references
  https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model

- more specifically to look at the top level element is document
  https://developer.mozilla.org/en-US/docs/Web/API/Document

- document properties will just return something
- document methods are functions you can run
  https://developer.mozilla.org/en-US/docs/Web/API/Document#Methods

- can also look at Nodes for example the body is a node of the document

```javascript
// create a new element
const heading = document.createElement('h1');
// Add some HTML into that new element
heading.innerHTML = '<span>Hello</span>' + document.URL;
// Append to the body (page)
document.body.appendChild(heading);
```

---

## 8.INTERACTING WITH THE DOM

- can also look at element section in MDN
  https://developer.mozilla.org/en-US/docs/Web/API/Element

- another good reference for not needing jQuery
  https://github.com/nefe/You-Dont-Need-Jquery

---

## 9.JS & THE DOM PART 3

- nice little random background colour script

```javascript
// 1.Generate random number between 0 - 255
const generateColorValue = () => Math.floor(Math.random() * 256);

// 2.Create random RGB color
const createColor = () => {
  const red = generateColorValue();
  const green = generateColorValue();
  const blue = generateColorValue();
  return `rgb(${red}, ${green}, ${blue})`;
};
// 3.Apply to dom
const applyColorToBody = color => {
  return (document.body.style.backgroundColor = color);
};
// 4.Run all the functions
const addRandomColorToBg = () => {
  const color = createColor();
  return applyColorToBody(color);
};

addRandomColorToBg();
```
