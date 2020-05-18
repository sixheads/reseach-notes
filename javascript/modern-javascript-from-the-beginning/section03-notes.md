# Modern JavaScript from the Beginning

## Traversy Media/Udemy

- Source: [Modern JavaScript from the Beginning](https://www.udemy.com/modern-javascript-from-the-beginning/)

---

## 23. DOM Selectors for single elements

### getElementById()

- looks for the id of an element
- will only be one of them
- see 3_3_project_files

```javascript
console.log(document.getElementById('task-title'));

// Get things from the element
console.log(document.getElementById('task-title').id);
console.log(document.getElementById('task-title').className);

// Good idea to create a varible when using this alot
const taskTitle = document.getElementById('task-title');

// Change styling
taskTitle.style.background = '#333';
taskTitle.style.color = '#fff';
taskTitle.style.padding = '5px';
taskTitle.style.display = 'none';

// Change content
taskTitle.textContent = 'Task List';
taskTitle.innerText = 'My Tasks';
taskTitle.innerHTML = '<span style="color:red">Task List</span>';
```

### document.querySelector()

- newer and much more powerfull
- allows any css selector
- will just grab the first element it finds if there is more than one

```javascript
console.log(document.querySelector('#task-title'));
console.log(document.querySelector('.card-title'));
console.log(document.querySelector('h5'));

document.querySelector('li').style.color = 'red';
document.querySelector('ul li').style.color = 'blue';

// Using CSS pseudo classes
document.querySelector('li:last-child').style.color = 'red';
document.querySelector('li:nth-child(3)').style.color = 'yellow';
document.querySelector('li:nth-child(4)').textContent = 'Hello World';
```

## 24. DOM Selectors for Multiple Elements

- see 3_4_project_files

### document.getElementsByClassName

```javascript
const items = document.getElementsByClassName('collection-item');
console.log(items); // gives us an HTML collection
console.log(items[0]); // can treat it like an array
items[0].style.color = 'red';
items[3].textContent = 'Hello';

// being more specific on an element
const listItems = document
  .querySelector('ul')
  .getElementsByClassName('collection-item');

console.log(listItems);
```

### document.getElementsByTagName

```javascript
document.getElementsByTagName;
let lis = document.getElementsByTagName('li');
console.log(lis);
console.log(lis[0]);
lis[0].style.color = 'red';
lis[3].textContent = 'Hello';

// Convert HTML Collection into array
lis = Array.from(lis);
// now we can run array methods on it
lis.reverse();

lis.forEach(function(li, index) {
  console.log(li.className);
  li.textContent = `${index}: Hello`;
});

console.log(lis);
```

### document.querySelectorAll

- returns a node list
- allows us to use array methods and for loops

```javascript
const items = document.querySelectorAll('ul.collection li.collection-item');

items.forEach(function(item, index) {
  item.textContent = `${index}: Hello`;
});

const liOdd = document.querySelectorAll('li:nth-child(odd)');
const liEven = document.querySelectorAll('li:nth-child(even)');

liOdd.forEach(function(li, index) {
  li.style.background = '#ccc';
});

for (let i = 0; i < liEven.length; i++) {
  liEven[i].style.background = '#f4f4f4';
}

console.log(items);
```

## 25. Traversing the DOM

- see 3_5_project_files

```javascript
let val;

const list = document.querySelector('ul.collection');
const listItem = document.querySelector('li.collection-item:first-child');

val = listItem;
val = list;

// Get child nodes - returns a node list
val = list.childNodes;
val = list.childNodes[0];
val = list.childNodes[0].nodeName;
val = list.childNodes[3].nodeType;

// these numbers pertain to what sort of nodeType it is
// 1 - Element
// 2 - Attribute (deprecated)
// 3 - Text node
// 8 - Comment
// 9 - Document itself
// 10 - Doctype

// Get children element nodes - returns a html collection
// more useful than childNodes
val = list.children;
val = list.children[1];
list.children[1].textContent = 'Hello';
// Children of children
list.children[3].children[0].id = 'test-link';
val = list.children[3].children[0];

// First child
val = list.firstChild;
val = list.firstElementChild;

// Last child
val = list.lastChild;
val = list.lastElementChild;

// Count child elements
val = list.childElementCount;

// Get parent node
val = listItem.parentNode;
val = listItem.parentElement;
val = listItem.parentElement.parentElement;

// Get next sibling
val = listItem.nextSibling;
val = listItem.nextElementSibling.nextElementSibling.previousElementSibling;

// Get prev sibling
val = listItem.previousSibling;
val = listItem.previousElementSibling;
console.log(val);
```

## 26. Creating Elements

- see 3_6_project_files

```javascript
// Create element
const li = document.createElement('li');

// Add class
li.className = 'collection-item';

// Add id
li.id = 'new-item';

// Add attribute
li.setAttribute('title', 'New Item');

// Create text node and append
li.appendChild(document.createTextNode('Hello World'));

// Create new link element
const link = document.createElement('a');
// Add classes
link.className = 'delete-item secondary-content';
// Add icon html
link.innerHTML = '<i class="fa fa-remove"></i>';

// Append link into li
li.appendChild(link);

// Append li as child to ul
document.querySelector('ul.collection').appendChild(li);

console.log(li);
```

## 27. Removing & Replacing Elements

- see 3_7_project_files

### Replace Element

```javascript
// Create Element
const newHeading = document.createElement('h2');
// Add id
newHeading.id = 'task-title';
// New text node
newHeading.appendChild(document.createTextNode('Task List'));

// Get the old heading
const oldHeading = document.getElementById('task-title');
//Parent
const cardAction = document.querySelector('.card-action');

// Replace
cardAction.replaceChild(newHeading, oldHeading);
```

### Remove Element

```javascript
const lis = document.querySelectorAll('li');
const list = document.querySelector('ul');

// Remove list item
lis[0].remove();

// Remove child element
list.removeChild(lis[3]);
```

### Classes & Attributes

```javascript
const firstLi = document.querySelector('li:first-child');
const link = firstLi.children[0];

let val;

// Classes
val = link.className; // string of class names
val = link.classList; // gives us a DOM token list
val = link.classList[0];
link.classList.add('test');
link.classList.remove('test');
val = link;

// Attributes
val = link.getAttribute('href');
val = link.setAttribute('href', 'http://google.com');
link.setAttribute('title', 'Google');
val = link.hasAttribute('title');
link.removeAttribute('title');
val = link;

console.log(val);
```

## 28. Event Listeners & The Event Object

- see 3_8_project_files
- we can listen to any event in the DOM
- first we select the element we want to listen on
- then we add an event listener to that.
- We listen for a particular event, a click in this instance
- then run a callback function to run some code

```javascript
// using an annonymous function
document.querySelector('.clear-tasks').addEventListener('click', function(e) {
  console.log('Hello World');

  e.preventDefault(); // stops the form submitting and refreshing the page
});

// OR with a named function
document.querySelector('.clear-tasks').addEventListener('click', onClick);

function onClick(e) {
  console.log('Clicked');
}
```

### Using the event object

```javascript
document.querySelector('.clear-tasks').addEventListener('click', onClick);

function onClick(e) {
  //console.log('Clicked');

  let val;

  val = e;

  // Event target element
  val = e.target; // represents the element the event happened on
  val = e.target.id;
  val = e.target.className;
  val = e.target.classList; // gives you a collection instead

  // Event type
  val = e.type; // returns 'click'

  // Timestamp
  val = e.timeStamp;

  // Coords event relative to the window
  val = e.clientY;
  val = e.clientX;

  // Coords event relative to the element - on the button itself
  val = e.offsetY;
  val = e.offsetX;

  console.log(val);
}
```

## 29. Mouse Events

- see 3_9_project_files

```javascript
const clearBtn = document.querySelector('.clear-tasks');
const card = document.querySelector('.card');
const heading = document.querySelector('h5');

// Click
clearBtn.addEventListener('click', runEvent);
// Doubleclick
clearBtn.addEventListener('dblclick', runEvent);
// Mousedown
clearBtn.addEventListener('mousedown', runEvent);
// Mouseup
clearBtn.addEventListener('mouseup', runEvent);
// Mouseenter (as soon as the cursor enters the element area)
card.addEventListener('mouseenter', runEvent);
// Mouseleave - only fire on the main element
card.addEventListener('mouseleave', runEvent);
// Mouseover - fire again if we hover over child elements
card.addEventListener('mouseover', runEvent);
// Mouseout - fire again if we hover over child elements
card.addEventListener('mouseout', runEvent);
// Mousemove
card.addEventListener('mousemove', runEvent);

// Event Handler
function runEvent(e) {
  console.log(`EVENT TYPE: ${e.type}`);

  // shows how we can work with mousemove and show the coordinates in our h5
  heading.textContent = `MouseX: ${e.offsetX} MouseY: ${e.offsetY}`;
  // shows how we can work with mousemove use our coordinates to change the background color
  document.body.style.backgroundColor = `rgb(${e.offsetX}, ${e.offsetY}, 40)`;
}
```

## 30. Keyboard & Input Events

- see 3_10_project_files

```javascript
const form = document.querySelector('form');
const taskInput = document.getElementById('task');
const heading = document.querySelector('h5');
const select = document.querySelector('select');

// Clear input
taskInput.value = '';

form.addEventListener('submit', runEvent);

// Keydown
taskInput.addEventListener('keydown', runEvent);
// Keydown
taskInput.addEventListener('keyup', runEvent);
// Keypress
taskInput.addEventListener('keypress', runEvent);
// Focus - when you click IN the input
taskInput.addEventListener('focus', runEvent);
// Blur - when you click OUT the input
taskInput.addEventListener('blur', runEvent);
// Cut
taskInput.addEventListener('cut', runEvent);
// Paste
taskInput.addEventListener('paste', runEvent);
// Input - fires off on any type of input event
taskInput.addEventListener('input', runEvent);
// Change - use on a select list
select.addEventListener('change', runEvent);

function runEvent(e) {
  console.log(`EVENT TYPE: ${e.type}`);

  console.log(e.target.value);

  heading.innerText = e.target.value;

  // Get input value
  console.log(taskInput.value);

  e.preventDefault();
}
```

## 31. Event Bubbling & Delegation

- see 3_11_project_files
- event bubbling is the bubbling UP of events through the DOM
- an event will bubble up to the parents of an element the event is happening on
- event delegation is when we set an event on the parent element then target a specific child element within that parent

### Event Bubbling

```javascript
// the below code shows how even though we click on the card title
// it actually bubble up through its parent, then parent then parent all the
// way up to .col
document.querySelector('.card-title').addEventListener('click', function() {
  console.log('card title');
});

document.querySelector('.card-content').addEventListener('click', function() {
  console.log('card content');
});

document.querySelector('.card').addEventListener('click', function() {
  console.log('card');
});

document.querySelector('.col').addEventListener('click', function() {
  console.log('col');
});
```

### Event Delegation

- putting the event on the parent then bubbling down
- if we set the event on the child li or a it will work for the first one but then not the other li or a elements
- also useful if you are adding items to a list afterwards

```javascript
const delItem = document.querySelector('.delete-item');

delItem.addEventListener('click', deleteItem);

document.body.addEventListener('click', deleteItem);

function deleteItem(e) {
  // className needs an exact match so you need to add all classes
  // becomes a problem if you add an extra class to one of the elements later on
  // if (e.target.parentElement.className === 'delete-item secondary-content') {
  //   console.log('delete item');
  // }

  if (e.target.parentElement.classList.contains('delete-item')) {
    console.log('delete item');
    e.target.parentElement.parentElement.remove();
  }
}
```

## 32. Local & Session Storage

- see 3_12_project_files

- local storage will let us save things to local browser storage for a time withoout the need for a database
- its a method on the window element
- it only accepts strings so you need to convert arrays and objects before you can save them - to convert them you use JSON.stringify then JSON parse to convert it back when you want to use the data
- local storage stays until you manually clear it out
- session storage stays until you close the browser and end the session

- you can check Local & Session storage in your browser via the dev tools under Application > Local Storage or Session Storage

### set local storage item

```javascript
localStorage.setItem('name', 'John');
localStorage.setItem('age', '30');

//

// set session storage item
sessionStorage.setItem('name', 'Beth');

// remove from storage
localStorage.removeItem('name');

// get from storage
const name = localStorage.getItem('name');
const age = localStorage.getItem('age');

// clear local storage
localStorage.clear();

console.log(name, age);

document.querySelector('form').addEventListener('submit', function(e) {
  const task = document.getElementById('task').value;

  let tasks;

  if (localStorage.getItem('tasks') === null) {
    tasks = [];
  } else {
    tasks = JSON.parse(localStorage.getItem('tasks'));
  }

  tasks.push(task);

  localStorage.setItem('tasks', JSON.stringify(tasks));

  alert('Task saved');

  e.preventDefault();
});

const tasks = JSON.parse(localStorage.getItem('tasks'));

tasks.forEach(function(task) {
  console.log(task);
});
```
