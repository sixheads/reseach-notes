# Beginner JavaScript - Wes Bos

## Variables & Statements

- "use strict" at the top of your JS enforces some best practice
- var is function scoped
- let and const are block scoped
- variables should not start with a capital letter - unless they are a Class
- can start with letter, \$ and \_
- camel case is best or snake case (i_like_variables) but not hyphens
- Wes' eslint setup - [checkout his repo](https://github.com/wesbos/eslint-config-wesbos)
- [eslint](https://eslint.org/)
- [prettier](https://prettier.io/)
- you can edit the rules in your .eslintrc to remove rules you don't like
- /_ eslint-disable _/ will disable eslint

## Module 1: Types

- seven types

  1. String
  2. Number
  3. Object - everything in JS is an object
  4. Boolean
  5. Null
  6. Undefined
  7. Symbol

### Strings

- escape a character in strings with \
- concatentation is when you combine two strings
- interpolation is when you add a variable into the string
- backticks makes things easier:
  const hello = `hello my name is ${name}. Nice to meet you`;
- great for HTML
- typeof variableName to find out the type of variable
- is loaded because with a string it will concatenate rather than do maths

### Numbers

- modulo returns the number left over after 2 numbers are divided
  10 % 3 // 1
- dealing with floats can be tricky - dealing with money its best to go with just cents
- you can also hit a limit with what a computer can calculate
- NaN - not a number

### Objects

- good for grouping things together
- object basically contains sub variables
- dot notation is the easiest way to access object properties

### Null & Undefined

- undefined is a variable that has been created with no value
- a variable created by not defined
- Null is a value of nothing
  const nothing = null;

### Boolean & Equality

- true or false
- great for conditional statements
- == is generally a bad idea
- best to use === triple equals
- triple equals checks the value and type of the variable

## Module 2: Functions

- functions let us group together sets of statements
- functions can take in data - arguments
- they perform some work - statement
- often they return a value/some data - not all of them return something (returns undefined)
- [MDN is our best reference for all things JS](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)
- lots of built in functions in JS:
  - Math.max() Math.floor()
  - parseFloat('20.33234'); // returns a number
  - parseInt('20.33234'); // returns 20
  - Date.now(); // number of milliseconds since Jan 1 1970 (epoch.now.sh)
  - document.querySelector('p'); // DOM functions
  - navigator.vibrate(200); // effects mobiles
  - window.scrollTo(0,200); // scrolls to 200px down the page
  - scrollTo({ top: 600, left: 0, behavior: 'smooth'}); // you can pass an object to a function as well

### Custom Functions

- functions can do anything
- they take in values and return something
- functions are defined and called
- the curly braces form the function block
- we need to return something from a function to access it outside the function

```js
// Function definition
function calculateBill() {
  // this is the function body
  console.log("running calculateBill");
  const total = 100 * 1.13;
  // temporary variable (only exists in this function - gone once run)
  return total;
  // need to return total to access it outside of the function
}

// function call or run
const myTotal = calculateBill();
// need to assign this to a variable to capture the result
console.log(myTotal); // now we can access it

// OR

console.log(`Your total is $${calculateBill()}`);
// can run the function directly inside of our backticks
```

- we need to assign our returned value to a variable to capture the result
- variables within a function are only temporary as the function runs

### Functions - Parameters & Arguments

![functions](./note-images/function.png)

- not always great to reach outside of your function for data
- better to pass into our function the data we need
- when you define a function you can add parameters - these are like placeholders in the function
- then when you run it you add the arguments for these parameters

```javascript
// Function using parameters
function calculateBill(billAmount, taxRate) {
  const total = billAmount * (1 + taxRate);
  return total;
}

const myTotal = calculateBill(100, 0.13);
console.log(`Your total is $${myTotal}`);

// You can also add expressions for the arguments

const myTotal2 = calculateBill(20 + 40 + 30 + 50, 0.13);
```

- the parameters are temporary variables
- you can assign your arguments with other variables but the function just sees the values

```javascript
function sayHiTo(firstName) {
  return `Hello ${firstName}`;
}
const greeting = sayHiTo("John"); // returns John
const greeting2 = sayHiTo(); // returns undefined
```

- if you define a firstName variable outside of the function then the function will using that if no argument is added
- you can reuse the same parameter name in two different functions and they don't clash as they are scoped to their own functions

```javascript
function doctorize(name) {
  return `Dr. ${name}`;
}

function yell(name) {
  return `HEY ${name.toUpperCase()}`;
}

// you can pass another function into a function
yell(doctorize("john")); // returns HEY DR. JOHN
```

- its a good idea to add a **default** to your parameters just in case the argument isn't added

```javascript
function calculateBill(billAmount, taxRate = 0.13) {}

// OR just use an empty string so the function doesn't error out
function yell(name = "") {
  return `HEY ${name.toUpperCase()}`;
}
```

- if you don't want to use the default you need to add undefined as the argument

### Different Ways to Declare Functions

- functions are first class citizens because they are values
- this means they can be passed into other functions, stored in variables, handled and moved around
- not true for other languages
- lots of ways to write our functions

```javascript
// Function Declaration
function doctorize(firstName) {
  return `Dr. ${firstName}`;
}

// anonymous function - good for callbacks
function(firstName) {
  return `Dr. ${firstName}`;
}

// Function Expression
const doctorize = function(firstName) {
  return `Dr. ${firstName}`;
};
```

- Arrow functions can also be written in a number of ways
- nice and concise syntax
- don't have their own scope in relation to 'this' keyword
- arrow functions are always anonymous functions

```javascript
// 1. start with normal function
function inchToCM(inches) {
  const cm = inches * 2.54;
  return cm;
}

// 2. shorten this to
function inchToCM(inches) {
  return inches * 2.54;
}

// 3. convert to function expression in prep for arrow function
const inchToCM = function(inches) {
  return inches * 2.54;
};

// 4. convert to arrow function - remove function keyword and
// add fat arrow after parameter brackets
const inchToCM = (inches) => {
  return inches * 2.54;
};

// 5. implicit return - quick little one-liner
// remove curly brackets and return keyword
const inchToCM = (inches) => inches * 2.54;

// 6. with one parameter you can remove the brackets
const inchToCM = (inches) => inches * 2.54;
```

```javascript
function add(a, b = 3) {
  const total = a + b;
  return total;
}

// above as arrow function
const add = (a, b = 3) => a + b;
```

- need to be careful of curly brackets when turning an object
- in this example we add brackets around the curly brackets

```javascript
// regular function
function makeABaby(first, last) {
  const baby = {
    name: `${first} ${last}`,
    age: 0,
  };
  return baby;
}

// as arrow function
const makeABaby = (first, last) => ({ name: `${first} ${last}`, age: 0 });
```

- most likely the regular function was better and more readable in this case

### IIFE

- immediately invoked function expression

```javascript
(function() {
  console.log("Running the anon function");
  return "You are cool!";
})();
```

- wrapping the function inside of brackets followed by () runs it immediately
- good for function scoping out code

### Methods

- a method is a function that lives inside of an object
- for example console is an object & log is a method on that object

```javascript
const wes = {
  name: "Wes Bos",
  // Method
  sayHi: function() {
    console.log("Hey Wes");
    return "Hey Wes";
  },
  // Shorthand method
  yellHi() {
    console.log("HEY WESSSS!");
  },
  // arrow function
  whisperHi: () => {
    console.log("hii mess im a mouse");
  },
};
```

- here is our sayHi method inside of our wes object
- we can also write our method using the above shorthand way removing the function keyword and colon

### Callback function

- callback functions let us run another function inside our function
- called by the browser after we run our initial function

```javascript
const button = document.querySelector('.clickMe');

function yellHi() {
  console.log('HEY JOHN');
  return `HEY JOHN`;
}

button.addEventListener('click', yellHi);

// Or call a function inside our function
button.addEventListener('click', function() {
  console.log('Nice job');
}
```

### Timer call

```javascript
setTimeout(yellHi, 1000); // runs after 1 sec

// or with an anonymous function
setTimeout(function() {
  console.log("Done");
}, 1000);

// or with an arrow function
setTimeout(() => {
  console.log("Done");
}, 1000);
```

### Debugging Tools

#### Console Methods

- we have a number of console methods we can use:
  1. console.log
  2. console.error - styles return differently
  3. console.warn - styles return differently
  4. console.table - formats return in a table
  5. console.count shows how many times your function is running
  6. console.group & console.groupEnd lets you group a number of console methods together

#### Callstack ( or stack trace )

- the callstack shows the trace of how an error happened
- it allows you to follow it back to its source

![callstack](./note-images/callstack.png)

#### Grabbing elements

- when inspecting an element in the browser you can swtich to the console once something is inspected then type \$0 to expect that element and any of its methods
- \$('p') will do a document.querySelectorAll in the browser from the console
- useful of inspecting elements

#### Breakpoints

- adding breakpoints in our code pauses the code at that point and lets us inspect what it going on at that moment in time within our function

```javascript
people.forEach((person, index) => {
  debugger; // just add this at the point we want it to stop
  console.log(person.name);
});
```

- then in the Sources tab in our developer tools we can step through, see our call stack and lots of other bits of info
- you can step it forward, switch to view the console then switch back and step it forward again

  ![breakpoints](./note-images/breakpoints.png)

- you can also add a debugger breakpoint in the Sources tab by clicking on the line in your script where you want to add one

#### Network Requests

- Network tab is useful for seeing all the different elements being loaded onto a site
- you can click on each one and see timing, cookies, header etc
- you can see external calls and how long they might be taking

#### Break On Attribute

- this can let you add a break on and element, say a button for example, then when you click the button it will take you to the part in the script that it firing on button click
- inspect the element then click the dots next to it in the Elements tab and choose Break On and select Attribute modification

  ![break-on](./note-images/break-on.png)

- in the Sources tab you can also add breakpoints at Event Listener (whenever someone clicks a button) or EHR/Fetch (fetches some external data) breakpoints as well

## Module 3: The Tricky Bits

### Scope

- scope is about where my variable and functions are available to me
- a global variable is available anywhere in the browser, in any script file on the page
- global variables can cause issues
- a global scope like setInterval is attached to the window object
- with **function scope** variables declared inside of a function are only available within that function
- **scope lookup** is when a function runs and looks for a variable, if it can't find it within the function it will move up a level
- so a variable declared outside of a function is available to that function
- you can use the same name for a variable declared inside of a function that's the same as a variable outside of the function but its generally not a good idea

#### Block scope

- const and let declared inside of a block {} is not available outside of the block. A var is available outside of the block
- var variables are still function scoped though

- try not to create global variables
- a function inside of another function is scoped to that function

### Hoisting

- hoisting in JS allows you to access functions and variables before they have been created
- function declarations and variable declarations are hoisted
- the compiler moves all these functions and variable to the top so you can call them anywhere

```javascript
// you can call the function here above and it will work
sayHi();

function sayHi() {
  console.log("hey!");
}

// this one won't work
const add = (a, b) => a + b;
```

- with variable hoisting it pulls the variable declaration to the top but not necessarily the value - can cause issues

## Closures

- a closure gives you access to an outer function’s scope from an inner function
- being able to access a parent level scope from a child scope even after the parent has been terminated

```javascript
// example of a closure
function outer() {
  const outerVar = "Hey I am the outer var";
  function inner() {
    const innerVar = "Hey i am the inner var";
    console.log(innerVar);
    console.log(outerVar); // can do a scope lookup to get this variable
  }
  return inner;
}

const innerFn = outer();
// the outer function has already run
innerFn();
// but here we are still getting access to it from the inner function
```

- the trick here is that the inner function still has access to the outerVar despite the fact that it has already run so technically that variable should have been cleaned up and removed
- so the child maintains access to that outerVar variable this is whats called a closure
- as long as the child function has references the variable from the parent that parent variable continues to exist as part of this closure

```javascript
function init() {
  var name = "Mozilla"; // name is a local variable created by init
  function displayName() {
    // displayName() is the inner function, a closure
    alert(name); // use variable declared in the parent function
  }
  displayName();
}
init();
```

## Module 4: The DOM - Working with HTML & CSS

### The DOM

- the DOM is our Document Object Mode - a tree like structure of all our elements on our page
- with JavaScript we can interact wit the DOM
- our **window** object is our global scope
- so in our console on any webpage we can do **window.location** and open up an object with a whole lot of info about the page
- you can do things like **innerWidth** or **innerHeight**
- the window is everything about the currently open browser window
- inside the window we have the document object - this is everything between the opening and closing html tags
- the **navigator** is a level up above the window and gives you device info

### Element Properties and Methods

- we load our js at the bottom of the page so all the other elements have been loaded and parsed by the browser
- first thing we need to do is select our element before we can do something with it
- two main ways we select elements is:

```javascript
const p = document.querySelector("p");
// selects the first p element it finds
const div = document.querySelectorAll("div");
// returns all the div's it finds as a NodeList

const img = document.querySelectorAll(".item img");
// specify the image

const item2 = document.querySelector(".item2");
// class name
const item2Img = item2.querySelector("img");
// then grab the img on item2 - search inside
// that selector - limits the scope

// still options like, but easier to just use querySelector an querySelectorAll:
document.getElementById("idname");
```

- a NodeList is a list of elements but its not an array - sort of looks like one though

### Element Properties and Methods

- [mdn element reference](https://developer.mozilla.org/en-US/docs/Web/API/Element)
- if we do a console.dir on an element we get the object properties, methods
- this shows lots of helpful properties you can run on that object
  - parentElement
  - textContent
- you can use these as getters and setters

```javascript
const heading = document.querySelector("h2");

heading.textContent = "John is cool"; // setter
console.dir(heading.textContent); // getter

console.log(heading.innerHTML);
// logs any html tags within the selected element
console.log(heading.outerHTML);
// logs any html tags outside the selected element too

pizzaList.insertAdjacentText("beforebegin", "more text");
// adds text onto an element at different points - faster than textContent
```

### Working with Classes

- we can use classList on an element to give us info on classes on that element and what else we can do with classList
- looking in the prototype shows us some methods we can use: add, remove, toggle, contains
- we often use something like this to animate an element by changing the class

```javascript
const pic = document.querySelector(".nice");

function toggleRound() {
  pic.classList.toggle("round");
}

pic.addEventListener("click", toggleRound);
```

### Build in and Custom Data

- we can access and alter all the attributes on an element
- a lot of these are getters and setters

```javascript
pic.alt = "clouds";
pic.width = 200;

pic.setAttribute("alt", "cute pup"); // similar to .alt
console.log(pic.getAttribute("alt"));
```

- setAttribute actually lets you add no standard attributes to an element, but this is bad practice. Its better to use a data- property on the element instead

```javascript
// <img class="nice cool" data-name="hero" src="https://picsum.photos/500" />

const custom = document.querySelector(".nice");
console.log(custom.dataset); // returns all the data- attributes
```

- good for adding click events to specific elements
- nice example of adding a larger version of the image path in a data attribute to switch between

### Creating HTML

- common way to add content to a page is with createElement method and appendChild.

```javascript
const myPara = document.createElement("p");
myPara.textContent = "I am a P";
myPara.classList.add("special");

const myImage = document.createElement("img");
myImage.src = "https://picsum.photos/500";
myImage.alt = "Nice photo";

const myDiv = document.createElement("div");
myDiv.classList.add("wrapper");
```

- when you've created a number of elements you can add them to the DOM with appendChild

```javascript
myDiv.appendChild(myPara);
myDiv.appendChild(myImage);

document.body.appendChild(myDiv);
```

- its best to create the elements and build them up before adding them to the DOM to save on repaints
- append is a newer option but best to check browser support
- insertAdjacentElement is another option with fine control for inserting an element

```javascript
// this is the syntax
targetElement.insertAdjacentElement(position, element);
```

- Positions available are:
  - 'beforebegin': Before the targetElement itself.
  - 'afterbegin': Just inside the targetElement, before its first child.
  - 'beforeend': Just inside the targetElement, after its last child.
  - 'afterend': After the targetElement itself.

### HTML from Strings and XSS

- you can create HTML with innerHTML which adds it as a string.

```javascript
const item = document.querySelector(".item");

const width = 500;
const src = `https://picsum.photos/${width}`;
const desc = `Cute Pup <img onload="alert('HACKED')" src="https://picsum.photos/50"/>`;
const myHTML = `
  <div class="wrapper">
    <h2>Cute ${desc}</h2>
    <img src="${src}" alt="${desc}"/>
  </div>
`;
```

- you can also interpolate values as well
- but because its a string rather than actual DOM elements there are limits to what we can do with it
- so we need to run two methods on our HTML createRange() & createContextualFragment()

```javascript
// turn a string into a DOM element
const myFragment = document.createRange().createContextualFragment(myHTML);

document.body.appendChild(myFragment);
```

- need to be careful using innerHTML as it can open you up to XXS - there are options for securing that in a later video

### Traversing & Removing DOM elements

- traversing is about moving around the DOM
- all the methods revolve around node or element

```javascript
<p class="wes">
  My name is Wes I <em>love</em> to bbq
</p>;

const wes = document.querySelector(".wes");

console.log(wes.children); // returns a collection of elements with em

// this sees everything
console.log(wes.childNodes); // returns a node list with text/em/text
```

```javascript
// for elements:
console.log(wes.children);
console.log(wes.firstElementChild);
console.log(wes.lastElementChild);
console.log(wes.previousElementSibling); // element just before the current element
console.log(wes.nextElementSibling); // element just after the current element
console.log(wes.parentElement);

// For node elements - these will include the text nodes
el.childNodes;
el.firstChild;
el.lastChild;
el.previousSibling;
el.nextSibling;
el.parentNode;

// to get rid of an element
el.remove();
```

### DOM Cardio

- **see DOM-Cardio.js file**

## Module 5: Events

### Event Listener

- DOM elements on the page emit events when clicked, hovered, dragged etc for example
- you can attached events to any element, even the document
- first thing to do is select your elements
- then attached an .addEventsListener() method
- this generally takes 2 arguments:
  1. the type of event - eg. click
  2. a callback function (this is a normal function that gets called)
- you can add the callback as an anonymous function or a named function:

```javascript
const butts = document.querySelector(".butts");

// Anonymous callback function
butts.addEventListener("click", function() {
  console.log("it got clicked");
});

// or using a named function
function handleClick() {
  // naming it handleSOMETHING is a common practice
  console.log("it got clicked");
}
butts.addEventListener("click", handleClick);

// we can also an arrow function
const hooray = () => console.log("hooray!");
coolButton.addEventListener("click", hooray);
```

- using a named function means you can use it elsewhere with another event, keeping your code DRY
- also makes it easier to remove the event on an element as you need to be able to reference the function called as well
- arrow functions work here too as they are named via the variable name

```javascript
butts.removeEventListener("click", handleClick);
```

- **binding** is a term we use here: we bind a function to this event and also unbind it

- listening for events on multiple items requires that you loop over them
- you can't just use querySelectorAll() as it does have the method we need on that nodeList unlike say a button element
- we can see the available methods on an element by inspecting it and looking at the prototype
- so with our nodeList we need to use a forEach loop

```javascript
// Listen on multiple items
const buyButtons = document.querySelectorAll("button.buy");

function buyItem() {
  console.log("buy item");
}

buyButtons.forEach((buyButton) => {
  // console.log(buyButton);
  buyButton.addEventListener("click", buyItem);
});
```

- in our forEach the parameter we pass in can be called anything

### Events - targets, bubbling, propagation and capture

- the event object provides lots of information on our event element, methods etc
- we just pass our callback function a parameter - e or event

```javascript
function handleBuyButtonClick(event) {
  console.log("you are buying it");
  console.log(event); // gives us the entire event object
  console.log(event.target); // gives element clicked on
  console.log(event.target.dataset.price); // gives use the data-price
  console.log(parseFloat(event.target.dataset.price)); // converts it to an integer

  // could also add it to a variable
  const button = event.target;
  // then
  console.log(button.textContent);
}
```

- event.target is the element that got clicked on
- event.currentTarget is the element that fired the event
- in our example if you clicked the number wrapped in a strong tag that's what event.target returns while event.currentTarget returns the whole button
- generally you want to use **currentTarget**

- you can place event listeners on the Window as well

```javascript
window.addEventListener("click", function(e) {
  console.log("you clicked the window");
  // console.log(e);
});
```

- now we can see **propagation** in action, our event **bubbles up** from say the 10 in the button up to the button, body, window and so on - below is what is returned

```javascript
you are buying it
<button data-price=​"1000" class=​"buy">​…​</button>​
<strong>​10​</strong>​
you clicked the window
```

- basically the event is bubbling up and what ever element is listening for it will hear it
- a method of event.stopPropagation() on an element will stop that bubbling up
- you can reverse that and use the capture down instead by adding an extra option onto the event listener
- now in runs down:

```javascript
window.addEventListener(
  "click",
  function(e) {
    console.log("you clicked the window");
    // console.log(e);
  },
  { capture: true }
);

// returns below when you click the 10 element

you clicked the window
you are buying it
<button data-price=​"1000" class=​"buy">​…​</button>​
<strong>​10​</strong>​
```

- this capture option is very rarely used
- you can also use **this** in your function

```javascript
// both of these return the same thing
photoEl.addEventListener("mouseenter", function(e) {
  console.log(e.currentTarget);
  console.log(this);
});
```

- **this** can have issues if your callback function is an arrow function
- better to used e.target() or e.currentTarget()

### Events - Prevent Default and Form Events

- lots of your work with events will have to do with forms and inputs
- a lot of elements have default actions like the link on <a> - we can grab these and work with them with JS by preventing the default action - event.preventDefault();
- when selecting a form its better to use a name attribute rather than a class

```javascript
const signUpForm = document.querySelector('[name="signup"]');

signUpForm.addEventListener("submit", function(event) {
  event.preventDefault();
  console.dir(event.currentTarget); // will list all the elements and methods on our form

  console.log(event.currentTarget.name.value); // returns value in name input
  console.log(event.currentTarget.email.value); // returns value in email input
  console.log(event.currentTarget.agree.checked); // returns boolean in agree checkbox
});
```

- we can also listen for other events like:
  - keyup
  - keydown
  - focus
  - blur

### Events - Accessibility Gotchas and Keyboard Codes

- a link should be used for going to another page
- a button should be used when we want to trigger some sort of action in our page
- we can listen for 'click' but if we also need to listen for the enter key if someone is using a keyboard rather than a mouse then we listen for the 'keyup' event
- Wes has a site to help with keycodes - [keycodes](http://keycode.info/)

```javascript
const photo = document.querySelector(".photo");

function handleClick(event) {
  if (event.type === "click" || event.key === "Enter") {
    console.log("You clicked the photo");
  }
}

photo.addEventListener("click", handleClick);
photo.addEventListener("keyup", handleClick);
```

## Module 6: Serious Practice Exercises

### Etch-a-Sketch

- once we've selected the canvas we can set the context between 2d or 3d

```javascript
const canvas = document.querySelector("#etch-a-sketch");
const ctx = canvas.getContext("2d"); // set canvas context (could be 3d too)

// To start or drawing 200 in and 200 down:
ctx.beginPath(); // start the drawing
ctx.moveTo(200, 200);
ctx.lineTo(200, 200);
ctx.stroke();
```

- see etch-a-sketch.js for comments

### Click Outside Modal

- useful to use pointer-events: none; to deal with how the modal is on top of content when hidden so that the card buttons no longer work.

```javascript
const cardButtons = document.querySelectorAll(".card button");
const modalInner = document.querySelector(".modal-inner");
const modalOuter = document.querySelector(".modal-outer");

function handleCardButtonClick(event) {
  const button = event.currentTarget;
  // finds the closest parent with a class of .card
  const card = button.closest(".card");
  // now grab the image in that closest card
  const imgSrc = card.querySelector("img").src;
  // and the data-description from that card
  const desc = card.dataset.description;
  const name = card.querySelector("h2").textContent;

  // target the modal inner and add a larger version of the image
  modalInner.innerHTML = `
    <img src="${imgSrc.replace("200", "600")}" alt="${name}" />
    <p>${desc}</p>
  `;

  // then add the class of open so it shows up
  modalOuter.classList.add("open");
}

cardButtons.forEach((button) =>
  button.addEventListener("click", handleCardButtonClick)
);

// function to close the modal
function closeModal() {
  modalOuter.classList.remove("open");
}

modalOuter.addEventListener("click", function(event) {
  // takes the event & creates a boolean
  // clicking inside the modal-inner returns false, outside returns true
  const isOutside = !event.target.closest(".modal-inner");
  // console.log(isOutside);

  // so if true then run the close modal function
  if (isOutside) {
    closeModal();
  }
});

// add a listener for the esc key also
window.addEventListener("keydown", (event) => {
  if (event.key === "Escape") {
    closeModal();
  }
});
```

### Scroll Events and Intersection Observer

- a good practice to follow if wrapping our scripts in a function
- then testing if the element we want to target is on the particular page
- if it isn't then return and exit the function
- if it is continue on with your script

```javascript
// wrap our script in a function to isolate it
function scrollToAccept() {
  // select our terms
  const terms = document.querySelector(".terms-and-conditions");
  // if terms doesn't exist on the page then return nothing
  if (!terms) {
    return;
  }

  // but if it is on the page then continue our script
  terms.addEventListener("scroll", function(e) {
    console.log(e);
  });
}

scrollToAccept();
```

- in the past you would have had to find the scrollTop and also the scrollHeight so you'd know when the distance from the scrollTop was close to the scrollHeight
- we can use intersection observer to find out if an element is visible on the page

```javascript
const watch = document.querySelector(".watch");

// IntersectionObserver needs a callback
function obCallback(payload) {
  console.log(payload);
}
// for intersection observer we create a new IntersectionObserver
const ob = new IntersectionObserver(obCallback);

// now we can use that IntersectionObserver to observer something
ob.observe(watch);
```

### Tabs

- looked at using aria roles to target with our JS
- how to set aria role with setAttribute
- how to use the .find() method

## Module 7: Logic and Flow Control

### BEDMAS

- B-rackets
- E-xponents
- D-ivision
- M-ultiplication
- A-ddition
- S-ubtraction

### Flow Control - If Statements, Function Returns, Truthy, Falsy

- if statements expect a value of true or false
- if statements take a condition which either returns true or false
- you can chain multiple conditions with 'if else' - JS splits the words
- as soon as an if statement hits true all the other if else conditions are ignored
- you can finish up with a simple else

```javascript
if (condition) {
  // do something
} else if (condition2) {
  // do something else
} else {
  // a last resort
}
```

- you can use if statements in a function

```javascript
function slugify(sentence, lowercase) {
  if (lowercase) {
    return sentence.replace(/\s/g, "-").toLowerCase();
  }
  // else
  return sentence.replace(/\s/g, "-");
}
```

- we could have wrapped the second statement in an else but the above still works
- if the first statement is true it returns and ignores the next return. But if the first statement is false it just skips down to the next return

```javascript
// a nicer way to write the above function
// good if the regex is complicated as you only write it once
function slugify(sentence, lowercase) {
  let slug = sentence.replace(/\s/g, "-");
  if (lowercase) {
    return slug.toLowerCase();
  }
  return slug;
}
```

- = a single equal assigns a value
- == a double equals compares two values but it it doesn't check the type
- so 10 == '10' would return true
- === a triple equals is best practice as that will compare 2 values and also take into account the type
- !== not equal

- < > greater than and less than
- <= less than or equal
- > = greater than or equal

- && AND
- || OR
- often a good idea with multiple && or || statements to break them out into a variable

#### Truthy & Falsy

**Truthy Values**

- 1
- -10
- full string
- a string of "0"

**Falsy values**

- 0
- undefined variable
- variable set to null
- a variable set to `"hello" - 10` NaN
- empty string

### Coercion, Ternaries and Conditional Abuse

- coercion is when we take a different type and turn it in to a boolean
- adding a ! infront turns it into a boolean returning false
- adding !! infront returns true

```javascript
if (!isCool) {
  // return opposite
}
```

- a **ternary** is a shorthand if statement - good when you want to see if something is true or false
- a ternary is 3 things:
  1. a condition
  2. what to return if true
  3. what to return if false
- you must always have a final case.
-

```javascript
const count = 1;
const word = count === 1 ? "item" : "items";

// can also run a function in it
function showAdminBar() {
  // something
}

const isAdmin = true;
isAdmin ? showAdminBar() : null;

// AND AND Trick
isAdmin && showAdminBar(); // this will run the function because isAdmin is true, if false it never moves to the next one
// often see this in react
```

### Intervals and Timers

- if you want to run something after 5 sec you run a timeout
- if you want to run something every 5 sec you run a interval
- setTimeout() if a method we have access too
- setTimeout(callback, sec) - takes a callback and a number of sec

```javascript
setTimeout(function() {
  console.log("Done!");
}, 500);
```

- runs an anonymous function 500 milliseconds after the function starts

```javascript
function buzzer() {
  console.log("ENNNNG!");
}
setTimeout(buzzer, 500);
```

- or running it with a named function

```javascript
function buzzer() {
  console.log("ENNNNG!");
}
console.log("starting");
setTimeout(buzzer, 500);
console.log("finishing");

// the above would return
starting
finishing
ENNNNG!
```

- this is because JS is asynchronous - it will do the first log, then start the setTimeout, then immediately move to the last log with the callback happening after the timer

```javascript
function buzzer() {
  console.log("ENNNNG!");
}
setInterval(buzzer, 500);
```

- with setInterval we're running our buzzer function every 500 milliseconds
- a setInterval will not run immediately, but affer the timer set
- if you need to stop your timer you need to give it a variable name then use clearTimeout(variable);

## Module 8: Data Types

### Objects

- good for grouping together properties and values
- store related data or functionality
- objects are good when the order of the properties don't matter

```javascript
// this is an object literal
const person = {
  age: 100,
  name: "john",
  clothing: {
    // sub properties
    shirts: "black",
    pants: "jeans",
  },
  // our method
  sayHello: function(greeting = `Hey`) {
    return `${greeting} ${this.name}`;
  },
  // method shorthand
  sayHello(greeting = `Hey`) {
    return `${greeting} ${this.name}`;
  },
};

// add another property
person.job = "web developer";

// Overwrite property
person.age = 50;

// Access a value with dot notation
person.name;
person.clothing.shirts;

// Access a value with bracket notation
person["age"];
// useful if the property you are accessing won't work with dot notation

// if a property doesn't exist you might need to check for it
person.job ? person.job.side : `Jobs doesn't exist`;

// Remove a property
delete person.age;
```

- property on the left value on the right
- the value can be anything, a string, function etc
- good idea to get into the habit of using a trailing comma
- you can re-assign the const variable, but the properties inside it can change

```javascript
// checking if a value exists
const nameInput = document.querySelector('[name="first"]');

const name = nameInput ? nameInput.value : "";
```

- a **method** is a function that lives inside of an object
- in our method the **this** keyword references the object - eg. this.name = person.name
- this is very important when we get into prototypes

### Object Reference vs Value

- setting an object variable to equal another object doesn't make a copy, but just references the original object

```javascript
const person1 = {
  first: "john",
  last: "fry",
};

const person2 = person1;

// so then changing the first value in person2
person2.first = "dave";

console.log(person1.first); // returns dave
```

- one way to copy an object is to use a spread
- a spread takes the contents of the other object and copies it into a new object

```javascript
const person2 = { ...person1 };

// old way to do it, not used any more
const person2 = Object.assign({}, person1);
```

- this only does a **shallow copy**, meaning it won't grab deeper levels of properties, like nested properties
- libraries like **lodash** do offer an option for a **deep clone** - https://lodash.com/
- you can also use the spread operator to merge two objects

```javascript
const mergedObject = { ...object1, ...object2 };
```

- order matters with this merge, duplicate values can get overwritten
- so a duplicate in object1 would be overwritten by the one in object2

- with a function if you pass in an argument that changes a value in an object, this updates that object, whereas with a string the variable isn't effected

```javascript
const name1 = 'wes';

function doStuff(data) {
  data = 'something else';
  console.log(data); // returns something else
}

doStuff(name1);
console.log(name1); // still returns wes

// the argument is just passing in a reference to the variable not the variable itself so nothing is changed permanently
// this is the case for booleans, numbers and strings

// Whereas
const inventory = {
  lettuce = 4,
  pizza = 10,
  tomatoes = 4,
}

function doStuff2(data) {
  data.tomatoes = 1000;
  console.log(data); // returns something else
}

doStuff2(inventory);

console.log(inventory.tomatoes); // returns 1000
// So it actually reached out and changed the value in the object
// this is the case for objects and arrays

```

- the above can cause bugs, better to pass it in as a copy

### Maps

- Maps work in a similar way to objects but they have additional methods on it that we can use
- they also let you store properties that don't meet the variable name requirements that an object needs
- when using set() we create entries within our Map which can be useful for storing additional information without property
- order is guaranteed in a Map
- you can use .size to get the length of your Map
- can't put a method inside of a Map like you can on an object
- JSON can't handle Maps so you can stringify it

```javascript
const myMap = new Map();

// .set()
myMap.set("name", "wes");
myMap.set(100, "is a number"); // this property name wouldn't work with an object

// .has()
// .delete()
```

### Arrays

- arrays are good for collecting data where the order matters
- each thing inside an array is an item
- the position of that item is called its index - no keys in array, just the index
- an array has a typeof object
- Arrays are 0 based

```javascript
// this is an array literal
const names = ["wes", "kait", "snickers"];

typeof names; // returns object

// to test if it is an array
Array.isArray(names); // true

console.log(names[0]); // returns wes
console.log(names[1]); // returns kait

// number if items in array
console.log(names.length); // 3

// check last item
console.log(names[names.length - 1]);
```

- if you log the array and check out the prototypes you'll see all the methods available

**Mutable & Immutable**

- mutable methods cause mutations - changes the original
- immutable methods return a new array & don't change the original

```javascript
// Mutation Methods
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];
numbers.reverse();

// Immutable Method
// talks a copy at index 2 and stop before 4
const pizzaSlice = numbers.slice(2, 4); // [3,4]
```

- if you need to use a mutation method then you need to take a copy of the original array first up

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];
const numbersReversed = [...numbers];
numbersReversed.reverse();
// or combine the two
const numbersReversed = [...numbers].reverse();
```

#### other array methods

```javascript
names.push("lux"); // adds to the end of the array - this does mutate the array
const names2 = [...names, "lux"]; // make a new array, copy the old one and add to it

names.unshift("poppy"); // adds to the front of the array
const names3 = ["poppy", ...names];

names.splice(3, 2); // starts at 3 index and removes 2
// this is mutable - returns [1,2,3,6,7,8,9]

// add an item to the middle of an array
const bikes = ["bianchi", "miele", "panasonic", "miyata"];
const newBikes = [...bikes.slice(0, 2), "benotto", ...bikes.slice(2)];
// by not adding a second value in our slice it just goes to the end of the array

// to remove an item (panasonic) from the array
const newBikes2 = [...newBikes.slice(0, 3), ...newBikes.slice(4)];
// simply skipped taking a copy of panasonic which was 3 item in the array
```

- we can use findIndex() method to search for an item in our array
- this runs a callback that loops through the array and returns either true or false if it finds the value

```javascript
const names = ["poppy", "wes", "kait", "snickers", "lux"];

const kaitIndex = names.findIndex((name) => {
  if (name === "kait") {
    return true;
  } else {
    return false;
  }
});

// // could be shorted to this:
// const kaitIndex = names.findIndex((name) => {
//   if (name === "kait") {
//     return true;
//   }
//   return false;
// });

// // even shorter:
// const kaitIndex = names.findIndex((name) => {
//   return name === "kait"; // this returns true or false
// });

// // or with an implicit return:
// const kaitIndex = names.findIndex((name) => name === "kait");

console.log(kaitIndex); // returns 2
```

### Array Cardio - Static Methods

- [check out MDN for all the array methods available](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

- Array.from(), Array.isArray() & Array.of() are all static methods
- all the Array.prototype. ones are Instance methods
- when we create an array we generally have access to all the prototype methods
- whereas static methods are utilities for working with arrays and they are called differently to the instance methods
- Array.of() no overly useful
- Array.from() useful for creating a range by specifying a length
- Array.isArray() useful for checking if something is an array or not as typeof will just return 'object' on an array
- Object.entries(), Object.keys, Object.values() can all be run on a array and will return both entries, the keys or the values as arrays

### Array Cardio - Instance Methods

- .join() will take an array and turn it into a string separated with commas or whatever you define as the argument
- .split() will take a string and split it up into an array based on the argument

```javascript
const foodString = "hot dogs,hamburgers,sausages,corn";
console.log(foodString.split(","));
// ["hot dogs", "hamburgers", "sausages", "corn"]
```

- you can also leave off the argument and it will split between each character
- we also have .pop(), .push(), .shift(), .unshift()

```javascript
// take the last item off toppings with pop()
const lastItem = toppings.pop();
// add it back with push()
toppings.push(lastItem);
// take the first item off toppings with shift()
const firstItem = toppings.shift();
// add it back in with unshift()
toppings.unshift(firstItem);

// Do the last four,but immutable (with spreads and new variables)
let newToppings = toppings.slice(0, toppings.length - 1);
newToppings = [...newToppings, toppings[toppings.length - 1]];
console.log(newToppings);

// Make a copy of the toppings array with slice()
const toppingsCopy = toppings.slice(0);
// Make a copy of the toppings array with a spread
const toppingsCopy2 = [...toppings];
// take out items 3 to 5 of your new toppings array with splice()
newToppings.splice(3, 5);
console.log(newToppings);
// find the index of Avocado with indexOf() / lastIndexOf()
const avoIndex = toppings.indexOf("Avocado");
console.log(avoIndex);
// Check if hot sauce is in the toppings with includes()
const isInToppings = toppings.includes("Hot Sauce");
console.log(isInToppings);
// add it if it's not
if (!isInToppings) {
  toppings.push("Hot Sauce");
}
console.log(toppings);
// flip those toppings around with reverse()
toppings.reverse(); // mutable

const toppingsReversed = [...toppings].reverse(); // immutable
```

### Array Cardio - Callback Methods and Function Generation

- find the first rating that talks about a burger with find()

```javascript
function findBurgRating(singleFeedback) {
  // if (singleFeedback.comment.includes("burg")) {
  //   return true;
  // }
  // return false;

  // simplify the above as includes returns true or false
  return singleFeedback.comment.includes("burg");
}
// .find() takes a function
const burgRating = feedback.find(findBurgRating);
console.log(burgRating);
// returns {comment: "Love the burgs", rating: 4}
```

- the above function is very much tied to a particular term so you could instead create a **higher order function** that takes in a value and returns another function

```javascript
function findByWord(word) {
  return function(singleFeedback) {
    return singleFeedback.comment.includes(word);
  };
}

const burgRating = feedback.find(findByWord("burg"));
console.log(burgRating);
```

- find all ratings that are above 2 with filter()
- filter() returns a new array after filtering out values that return false

```javascript
const goodReviews = feedback.filter((singleFeedback) => {
  if (singleFeedback.rating > 2) {
    return true;
  }
  return false;
});

// or shorten that as its a condition that automatically returns either true or false
const goodReviews = feedback.filter(
  (singleFeedback) => singleFeedback.rating > 2
);

console.table(goodReviews);
```

- this could also be turned into a higher order function

```javascript
function filterByMinRating(minRating) {
  return function(singleFeedback) {
    return singleFeedback.rating > minRating;
  };
}

const goodReviews = feedback.filter(filterByMinRating(2));
console.table(goodReviews);
```

- find all ratings that talk about a burger with filter()

```javascript
const burgRatings = feedback.filter(findByWord("burg"));
console.table(burgRatings);
```

- here we've been able to reuse our findByWord function
- shows how these higher order functions can be really elegant in how they can be reused if created well

- Remove the one star rating however you like:

```javascript
const lowRatings = feedback.filter((singleFeedback) => {
  if (singleFeedback.rating === 1) {
    return false;
  }
  return true;
});

// could also write the above as
const lowRatings = feedback.filter(
  (singleFeedback) => singleFeedback.rating !== 1
);

console.table(lowRatings);
```

- check if there is at least 5 of one type of meat with some()

```javascript
const isThereEnoughOfAtLeastOneMeat = Object.values(meats).some(
  (meatValue) => meatValue >= 5
);
console.log(isThereEnoughOfAtLeastOneMeat);
```

- make sure we have at least 3 of every meat with every()

```javascript
const isThereEnoughOfEveryMeat = Object.values(meats).every(
  (meatValue) => meatValue >= 3
);
console.log(isThereEnoughOfEveryMeat);
```

- .sort() will convert items to strings and compare alphabetically so we need to do some work to do it with numbers
- .sort() will loop over the array and compare numbers in order, the callback function will compare pairs of numbers

```javascript
const numbers = [1, 2, 100, 3, 200, 400, 155];
const numbersSorted = numbers.sort(function(firstItem, secondItem) {
  return firstItem - secondItem;
});
console.log(numbersSorted);
```

- sort the toppings alphabetically with sort()

```javascript
console.log(toppings.sort());
```

- sort the order totals from most expensive to least with .sort()

```javascript
function numberSort(a, b) {
  return a - b;
}
console.log(orderTotals.sort(numberSort));
```

- Sort the prices with sort()

```javascript
const productSortedByPrice = Object.entries(prices).sort(function(a, b) {
  const aPrice = a[1];
  const bPrice = b[1];
  return aPrice - bPrice;
});
console.table(productSortedByPrice);
```

## Module 9: Gettin' Loopy

### Looping and Iterating - Array .forEach

- often looping over an array - method runs a callback each time it loops over an array item

```javascript
function logTopping(topping) {
  console.log(topping);
}
toppings.forEach(logTopping);

// OR
toppings.forEach((topping) => {
  console.log(topping);
});

// You also can get the index and array itself
toppings.forEach((topping, index, array) => {
  console.log(topping, index, array);
});
```

- our **topping** parameter here can be anything
- forEach doesn't really return anything it just loops over our array and we want to do something with each item

### Looping and Iterating - Mapping

- Map takes in data, performs an operation on the data and returns something new on the other side

```javascript
function frownify(name) {
  return `${name} Frown`;
}

const fullName = ["john", "bec", "issy", "auds"].map(frownify);
console.log(fullName);
```

- you can also chain maps - here we also capitalised the first name

```javascript
function frownify(name) {
  return `${name} Frown`;
}
function capitalise(word) {
  return `${word[0].toUpperCase()}${word.slice(1)}`;
}

const fullName = ["john", "bec", "issy", "auds"].map(capitalise).map(frownify);
console.log(fullName);
```

- often you will have an array of objects you need to map over from an API

```javascript
const cleanPeople = people.map(function(person) {
  // Get their birthday (timestamp)
  const birthday = new Date(person.birthday).getTime();
  // now timestamp
  const now = Date.now();

  // Work out their age (big number is milliseconds in a year)
  const age = Math.floor((now - birthday) / 31536000000);

  return {
    age,
    name: `${person.names.first} ${person.names.last}`,
  };
});

console.table(cleanPeople);
```

- when working with dates you can use this tool to get a timestamp:
  epoch.now.sh
- setting const now = new Date(); with no arguments give you the current date
- now.getTime() will give a timestamp of that date in milliseconds from Jan 1, 1970
- the shortcut for that is Date.now()

### Looping and Iterating - Filter, Find and Higher Order Functions

- filter loops over our times and returns either true or false based on a specific condition

```javascript
const over40 = cleanPeople.filter((person) => person.age > 40);
```

- .find just returns the single item (an object) you are looking for while filter returns all the items that match (as an array)

```javascript
const specialStudent = students.find((student) => student.id === "565a");
console.log(specialStudent);
```

- this could also be written as a higher order function

```javascript
function findById(id) {
  return function isStudent(student) {
    return student.id === id;
  };
}

const specialStudent = students.find(findById("565a"));
console.log(specialStudent);
```

- to make a function even more useful we can set it up to search by any property on the object

```javascript
function findByProp(prop, propValue) {
  return function isStudent(student) {
    return student[prop] === propValue;
  };
}

const student = students.find(findByProp("id", "565a"));
console.log(student);
const student2 = students.find(findByProp("first_name", "Micki"));
console.log(student2);
```

### Looping and Iterating - Reduce

- reduce() takes in an array of data and returns to you a result or single value

```javascript
// takes in the tally from the array and also a current total to work with
function tallyNumbers(tally, currentTotal) {
  console.log(`The current tally is ${tally}`);
  console.log(`The current total is ${currentTotal}`);
  console.log("--------------");
  // return the current tally plus the amount of this order
  return tally + currentTotal;
}

// the 0 sets the currentTotal initially
// this tally's up all the orderTotals
const allOrders = orderTotals.reduce(tallyNumbers, 0);
console.log(allOrders); // tallys the total amount
```

```javascript
const inventory = [
  { type: "shirt", price: 4000 },
  { type: "pants", price: 4532 },
  { type: "socks", price: 234 },
  { type: "shirt", price: 2343 },
  { type: "pants", price: 2343 },
  { type: "socks", price: 542 },
  { type: "pants", price: 123 },
];

// this function counts the number of different types
function inventoryReducer(totals, item) {
  console.log(`Looping over ${item.type}`);
  // increment the type by 1
  // uses a conditional for each initial item type
  totals[item.type] = totals[item.type] + 1 || 1;
  // return the totals, so the next loop can use it
  return totals;
}
// starts with an empty object when counting
const inventoryCounts = inventory.reduce(inventoryReducer, {});
console.log(inventoryCounts);

// this loops over inventory saves the acc (accumulated total)
// then adds the item price for a grand total
// starts the count at 0
const totalInventoryPrice = inventory.reduce(
  (acc, item) => acc + item.price,
  0
);
console.log(totalInventoryPrice);
```
