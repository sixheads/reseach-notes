# Modern JavaScript from the Beginning

## Traversy Media/Udemy

- Source: [Modern JavaScript from the Beginning](https://www.udemy.com/modern-javascript-from-the-beginning/)

---

## 43. Constructors & 'this' keyword

- if you are looking to create multiple instances of an object its best to use a constructor
- for that we need to define a function
- this means we have scoped 'this' within this function

```javascript
// Person constructor
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// instantiate a person object
const brad = new Person('Brad', 30);
const john = new Person('John', 45);

console.log(john.age); // 45
```

- Date() is also a constuctor as we call with with the new
- we can also add a method to our constructor (basically what we call functions added to an object)

```javascript
// Person constructor
function Person(name, dob) {
  this.name = name;
  this.birthday = new Date(dob);

  this.calculateAge = function() {
    const diff = Date.now() - this.birthday.getTime;
    const ageDate = new Date(diff);
    return Math.abs(ageDate.getUTCFullYear() - 1970);
  };
}

// instantiate a person object
const brad = new Person('Brad', '9-10-1981');

console.log(brad.calculateAge()); // 36
```

## 45. Prototypes Explained

- every object in Javascript has a prototype
- we can use protypes in our objects for methods to keep our objects nice and clean

```javascript
// Person constructor
function Person(firstName, lastName, dob) {
  this.firstName = firstName;
  this.lastName = lastName;
  this.birthday = new Date(dob);
}

// Calculate age prototype
Person.prototype.calculateAge = function() {
  const diff = Date.now() - this.birthday.getTime;
  const ageDate = new Date(diff);
  return Math.abs(ageDate.getUTCFullYear() - 1970);
};
// Get full name prototype
Person.prototype.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};
```

## 48. ES6 Classes

- classes add syntactic sugar
- we use this syntax to create a constructor
- prototypes are automatic when we add a method to our class

```javascript
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstname;
    this.lastName = lastname;
  }

  greeting() {
    return `Hello there ${this.firstName} ${this.lastName}`;
  }
}
```

## 49. Sub Classes

- we can add sub classes that extend another class
- you see this all the time in React when we extend the basic React Component
- so we can create a customer class that extends our person class

```javascript
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  greeting() {
    return `Hello there ${firstName} ${lastName}`;
  }
}

// extend with our Customer class
class Customer extends Person {
  constructor(firstName, lastName, phone, membership) {
    super(firstName, lastName); // include Person properties first

    // add out properties particular to customer
    this.phone = phone;
    this.membership = membership;
  }
}

const john = new Customer('John', 'Doe', '555-555-555', 'Standard');

console.log(john.greeting()); // still able to use methods from Person
```

- you can also have methods specific to our sub class
