- [Object in javascript](#object-in-javascript)
      - [Object.defineProperty()](#objectdefineproperty)
      - [delete](#delete)
      - [instanceof](#instanceof)
      - [hasOwnProperty](#hasownproperty)
      - [valueOf](#valueof)
      - [Object coercion](#object-coercion)
      - [Places that use object coercion include:](#places-that-use-object-coercion-include)
    - [Some static methods of Object class](#some-static-methods-of-object-class)
      - [Object.assign()](#objectassign)
      - [Object.entries()](#objectentries)
      - [Object.freeze()](#objectfreeze)
      - [Object.getOwnPropertyDescriptor()](#objectgetownpropertydescriptor)
      - [Object.getPrototypeOf()](#objectgetprototypeof)
      - [Object.hasOwn()](#objecthasown)
      - [Object.is()](#objectis)
      - [Object.keys()](#objectkeys)
      - [Object.values()](#objectvalues)
    - [Getters](#getters)
    - [setters](#setters)
    - [Constructor function:](#constructor-function)
    - [Prototype](#prototype)
      - [Shadowing properties](#shadowing-properties)
      - [Setting a prototype](#setting-a-prototype)
      - [Prototypes and inheritance](#prototypes-and-inheritance)




# Object in javascript
An object is a collection of key-value pairs where each key is a unique string (or Symbol) and each value can be any data type, including other objects. Objects in JavaScript are used to represent complex data structures and are a fundamental part of the language.

Objects in JavaScript can be created using object literals, constructor functions, or the Object.create() method. 

- Object literals

```javascript
const person = {
    name: 'John',
    age:23,
}
```

- Constructor function:
  
  Constructor function is a regular function used to create multiple similar objects.

```javascript
function Person(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;

    this.getFullName = function () {
        return this.firstName + " " + this.lastName;
    };
}

let person = new Person("John", "Doe");
console.log(person.getFullName()); //John Doe

console.log(person);
// Person {
//   firstName: 'John',
//   lastName: 'Doe',
//   getFullName: [Function (anonymous)]
// }

```
>**NOTE** more about it below
- Object.create():

It creates a new object, using an existing object as the prototype of the newly created object.

The second parameter of Object.create() is an optional argument that allows you to define properties and their descriptors (such as value, writable, enumerable, configurable) for the newly created object. This parameter is an object itself, where each property represents a property descriptor for the new object.


```javascript
const personPrototype = {
    greet: function() {
        console.log(`Hello, my name is ${this.name}`);
  }
};

// using single parameter
const person = Object.create(personPrototype);
person.name = 'John';
person.age = 30;
person.city = 'New York';


// using second parameter
const person = Object.create(personPrototype, {
  name: {
    value: 'John', //value of the property
    writable: true, //whether it can be modified
    enumerable: true, //whether it shows up in for...in loops and Object.keys() methods
    configurable: true //whether it can be deleted or have its property descriptor changed.
  },
  age: {
    value: 30,
    writable: true,
    enumerable: true,
    configurable: true
  },
  city: {
    value: 'New York',
    writable: true,
    enumerable: true,
    configurable: true
  }
});

person.greet(); // Output: Hello, my name is John
```

#### Object.defineProperty()
The Object.defineProperty() static method defines a new property directly on an object, or modifies an existing property on an object, and returns the object.

```
Object.defineProperty(obj, prop, descriptor)
```
- obj
The object on which to define the property.
- prop
A string or Symbol specifying the key of the property to be defined or modified.
- descriptor
The descriptor for the property being defined or modified.


```javascript
const object1 = {};

Object.defineProperty(object1, 'property1', {
  value: 42,
  writable: false,
  configurable:true,
  enumerable:true,
});

object1.property1 = 77;
// Throws an error in strict mode

console.log(object1.property1);
// Expected output: 42
```
#### delete
The delete operator removes a property from an object, if it exists.
```javascript
const person = {
  name: 'John',
  age: 30,
  city: 'New York'
};

console.log(person); // Output: { name: 'John', age: 30, city: 'New York' }

delete person.age;

console.log(person); // Output: { name: 'John', city: 'New York' }
```

#### instanceof
To check whether an object is an instance of a particular class or constructor function.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
}

const myDog = new Dog('Buddy', 'Golden Retriever');

console.log(myDog instanceof Dog); // Output: true
console.log(myDog instanceof Animal); // Output: true
console.log(myDog instanceof Object); // Output: true
console.log(myDog instanceof Array); // Output: false
```

#### hasOwnProperty

The hasOwnProperty method in JavaScript is used to check whether an object has a property with a specific name. It returns true if the object has the property, and false otherwise.

```javascript
const person = {
  name: 'John',
  age: 30,
  city: 'New York'
};

console.log(person.hasOwnProperty('name')); // Output: true
console.log(person.hasOwnProperty('gender')); // Output: false
```


#### valueOf
The valueOf method in JavaScript is a method that is called automatically by JavaScript when an object is used in a context where a primitive value is expected (for example, when an object is used in an arithmetic operation or when an object is compared to a primitive value using the == or === operators). The valueOf method returns the primitive value of the object.

```javascript
const person = {
  name: 'John',
  age: 30,
  city: 'New York'
};

console.log(person + 10); // Output: [object Object]10
```

default valueOf returned String.ie. [object Object], hence the result of concatenation [object Object]10 

#### Object coercion

- Objects are returned as-is.
- undefined and null throw a TypeError.
- Number, String, Boolean, Symbol, BigInt primitives are wrapped into their corresponding object wrappers.

Object(x) constructor also does the same thing when x is passed as parameter, but if the value is null or undefined, it creates and returns an empty object.

####  Places that use object coercion include:

- The object parameter of for...in loops:
In a for...in loop, JavaScript iterates over the enumerable properties of an object. When the object person is used in the loop, JavaScript implicitly calls the valueOf method of the person object to convert it to a primitive value (string) for the key name. This is because for...in loops expect string keys, and objects are automatically coerced to strings when used as keys.

- The this value of Array methods:
When an object is used as the this value of a method, JavaScript implicitly calls the valueOf method of the object to convert it to a primitive value (string). This is because methods expect a primitive value as the this value, and objects are automatically coerced to strings when used as this values.

- Parameters of Object methods such as Object.keys():
When an object is passed as a parameter to a method, JavaScript implicitly calls the valueOf method of the object to convert it to a primitive value (string). This is because methods expect primitive values as parameters, and objects are automatically coerced to strings when used as parameters.

- Auto-boxing when a property is accessed on a primitive value:
When a property is accessed on a primitive value, JavaScript automatically creates a wrapper object (also known as auto-boxing) and calls the valueOf method of the wrapper object to convert it to a primitive value (string). This is because properties are expected to be accessed on objects, and primitives do not have properties.

- The this value when calling a non-strict function:
When a non-strict function is called, JavaScript implicitly calls the valueOf method of the this value to convert it to a primitive value (string). This is because non-strict functions expect a primitive value as the this value, and objects are automatically coerced to strings when used as this values.

### Some static methods of Object class

#### Object.assign()

The Object.assign() static method copies all enumerable own properties from one or more source objects to a target object. It returns the modified target object.

```javascript
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);
// there can be multiple sources
//Object.assign(target, source1, source2, /* …, */ sourceN)

console.log(target);
// Expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget === target);
// Expected output: true
```

####  Object.entries()

The Object.entries() static method returns an array of a given object's own enumerable string-keyed property key-value pairs.
Each key-value pair is an array with two elements: the first element is the property key (which is always a string), and the second element is the property value.

```javascript
const object1 = {
  a: 'somestring',
  b: 42,
};

for (const [key, value] of Object.entries(object1)) {
  console.log(`${key}: ${value}`);
}

// Expected output:
// "a: somestring"
// "b: 42"
```

#### Object.freeze()
  
The Object.freeze() static method freezes an object. Freezing an object prevents extensions and makes existing properties non-writable and non-configurable. A frozen object can no longer be changed: new properties cannot be added, existing properties cannot be removed, their enumerability, configurability, writability, or value cannot be changed, and the object's prototype cannot be re-assigned. freeze() returns the same object that was passed in.
Freezing an object is equivalent to preventing extensions and then changing all existing properties' descriptors' configurable to false — and for data properties, writable to false as well.

```javascript
const obj = {
  prop: 42,
};

Object.freeze(obj);

obj.prop = 33;
// Throws an error in strict mode

console.log(obj.prop);
// Expected output: 42
```
#### Object.getOwnPropertyDescriptor()

The Object.getOwnPropertyDescriptor() static method returns an object describing the configuration of a specific property on a given object (that is, one directly present on an object and not in the object's prototype chain). The object returned is mutable but mutating it has no effect on the original property's configuration.

```javascript
const object1 = {
  property1: 42,
};

const descriptor1 = Object.getOwnPropertyDescriptor(object1, 'property1');

console.log(descriptor1.configurable);
// Expected output: true

console.log(descriptor1.value);
// Expected output: 42
```

#### Object.getPrototypeOf()

The Object.getPrototypeOf() static method returns the prototype (i.e. the value of the internal [[Prototype]] property) of the specified object.

```javascript
const prototype1 = {};
const object1 = Object.create(prototype1);

console.log(Object.getPrototypeOf(object1) === prototype1);
// Expected output: true
```

#### Object.hasOwn()

The Object.hasOwn() static method returns true if the specified object has the indicated property as its own property. If the property is inherited, or does not exist, the method returns false.

```javascript
const object1 = {
  prop: 'exists',
};

console.log(Object.hasOwn(object1, 'prop'));
// Expected output: true

console.log(Object.hasOwn(object1, 'toString'));
// Expected output: false

console.log(Object.hasOwn(object1, 'undeclaredPropertyValue'));
// Expected output: false
```

#### Object.is()

The Object.is() static method determines whether two values are the same value.
The only difference between Object.is() and === is in their treatment of signed zeros and NaN values. The === operator (and the == operator) treats the number values -0 and +0 as equal, but treats NaN as not equal to each other.

```javascript
console.log(Object.is('1', 1));
// Expected output: false

console.log(Object.is(NaN, NaN));
// Expected output: true

console.log(Object.is(-0, 0));
// Expected output: false

const obj = {};
console.log(Object.is(obj, {}));
```

Two values are the same if one of the following holds:

    - both undefined
    - both null
    - both true or both false
    - both strings of the same length with the same characters in the same order
    - both the same object (meaning both values reference the same object in memory)
    - both BigInts with the same numeric value
    - both symbols that reference the same symbol value
      - both numbers and
      - both +0
      - both -0
      - both NaN
      - or both non-zero, not NaN, and have the same value

#### Object.keys()

The Object.keys() static method returns an array of a given object's own enumerable string-keyed property names.
It returns an array of strings representing the given object's own enumerable string-keyed property keys.
```javascript
const object1 = {
  a: 'somestring',
  b: 42,
  c: false,
};

console.log(Object.keys(object1));
// Expected output: Array ["a", "b", "c"]
```

#### Object.values()

The Object.values() static method returns an array of a given object's own enumerable string-keyed property values.
Returns an array containing the given object's own enumerable string-keyed property values.
```javascript
const object1 = {
  a: 'somestring',
  b: 42,
  c: false,
};

console.log(Object.values(object1));
// Expected output: Array ["somestring", 42, false]
```

### Getters

The get syntax binds an object property to a function that will be called when that property is looked up. It can also be used in classes.

Sometimes it is desirable to allow access to a property that returns a dynamically computed value, or you may want to reflect the status of an internal variable without requiring the use of explicit method calls. In JavaScript, this can be accomplished with the use of a getter.

- Defining a getter on new objects in object initializers

This will create a pseudo-property latest for object obj, which will return the last array item in log.
```javascript
const obj = {
  log: ["example", "test"],
  get latest() {
    if (this.log.length === 0) return undefined;
    return this.log[this.log.length - 1];
  },
};
console.log(obj.latest); // "test"
```
Note that attempting to assign a value to latest will not change it.


- Using getters in classes

You can use the exact same syntax to define public instance getters that are available on class instances. In classes, you don't need the comma separator between methods.
```javascript
class ClassWithGetSet {
  #msg = "hello world";
  get msg() {
    return this.#msg;
  }
  set msg(x) {
    this.#msg = `hello ${x}`;
  }
}

const instance = new ClassWithGetSet();
console.log(instance.msg); // "hello world"

instance.msg = "cake";
console.log(instance.msg); // "hello cake"
```

Getter properties are defined on the prototype property of the class and are thus shared by all instances of the class. Unlike getter properties in object literals, getter properties in classes are not enumerable(meaning - whether a property will be included when iterating over an object's properties using a for...in loop or when using methods like Object.keys() or Object.entries()).

- Deleting a getter using the delete operator

If you want to remove the getter, you can just delete it:
```javascript
delete obj.latest;
```

- You can also use expressions for a computed property name to bind to the given function.

```javascript
const obj = {
  // Define a getter using a computed property name
  get [Math.random() > 0.5 ? 'propertyA' : 'propertyB']() {
    return 'Hello, World!';
  }
};

// Access the value of the dynamic property
console.log(obj.propertyA); // Output: 'Hello, World!'
```

In this example, the getter for the property is defined using a computed property name. The expression [Math.random() > 0.5 ? 'propertyA' : 'propertyB'] is evaluated, and depending on the result of Math.random() > 0.5, either 'propertyA' or 'propertyB' becomes the name of the property. When the property is accessed, the getter function is called, and it returns the value 'Hello, World!'.



### setters

The set syntax binds an object property to a function to be called when there is an attempt to set that property. It can also be used in classes.

A setter can be used to execute a function whenever an attempt is made to change a property's value. Setters are most often used in conjunction with getters.
```javascript
const obj = {
  set prop() {
    // setter, the code executed when setting obj.prop
  },
}
```
Properties defined using this syntax are own properties of the created object, and they are configurable and enumerable.

- Defining a setter on new objects in object initializers

The following example defines a pseudo-property current of object language. When current is assigned a value, it updates log with that value:

```javascript
const language = {
  set current(name) {
    this.log.push(name);
  },
  log: [],
};

language.current = 'EN';
language.current = 'FA';

console.log(language.log);
// Expected output: Array ["EN", "FA"]
```

>**NOTE** A setter must have exactly one parameter.

- Using setters in classes

You can use the exact same syntax to define public instance setters that are available on class instances. In classes, you don't need the comma separator between methods.

```javascript
class ClassWithGetSet {
  #msg = "hello world";
  get msg() {
    return this.#msg;
  }
  set msg(x) {
    this.#msg = `hello ${x}`;
  }
}

const instance = new ClassWithGetSet();
console.log(instance.msg); // "hello world"

instance.msg = "cake";
console.log(instance.msg); // "hello cake"
```
Setter properties are defined on the prototype property of the class and are thus shared by all instances of the class. Unlike setter properties in object literals, setter properties in classes are not enumerable.

- Removing a setter with the delete operator
```javascript
delete language.current;
```

- You can also use expressions for a computed property name to bind to the given function. Same as getters.

```javascript
const obj = {
  // Define a setter using a computed property name
  set [Math.random() > 0.5 ? 'propertyA' : 'propertyB'](value) {
    console.log('Setting dynamic property:', value);
  }
};

// Set the value of the dynamic property
obj.propertyA = 'Hello, World!';
```

In this example, the setter for the property is defined using a computed property name. The expression [Math.random() > 0.5 ? 'propertyA' : 'propertyB'] is evaluated, and depending on the result of Math.random() > 0.5, either 'propertyA' or 'propertyB' becomes the name of the property. When the property is set, the setter function is called with the value 'Hello, World!'.


### Constructor function:
  
  Constructor function is a regular function used to create multiple similar objects.

```javascript
function Person(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;

    this.getFullName = function () {
        return this.firstName + " " + this.lastName;
    };
}

let person = new Person("John", "Doe");
console.log(person.getFullName()); //John Doe

console.log(person);
// Person {
//   firstName: 'John',
//   lastName: 'Doe',
//   getFullName: [Function (anonymous)]
// }

```

The problem with the constructor function is that when you create multiple instances of the Person, the this.getFullName() is duplicated in every instance, which is not memory efficient.

To resolve this, you can use the prototype so that all instances of a custom type can share the same methods.

- Calling a constructor function without the new keyword

You can call a constructor function like a regular function without using the new keyword like this:
```javascript
let person = Person('John','Doe');
Code language: JavaScript (javascript)
```
But, the this inside the Person function doesn’t bind to the person variable but the global object.

If you attempt to access the firstName or lastName property, you’ll get an error:
```javascript
console.log(person.firstName);
// TypeError: Cannot read property 'firstName' of undefined
```

To prevent a constructor function from being invoked without the new keyword, ES6 introduced the new.target property.

If a constructor function is called with the new keyword, the new.target returns a reference of the function. Otherwise, it returns undefined.

```javascript
function Person(firstName, lastName) {
    if (!new.target) {
        throw Error("Cannot be called without the new keyword");
    }

    this.firstName = firstName;
    this.lastName = lastName;
}
```

### Prototype
Prototypes are the mechanism by which JavaScript objects inherit features from one another.

Every object in JavaScript has a built-in property, which is called its prototype. The prototype is itself an object, so the prototype will have its own prototype, making what's called a prototype chain. The chain ends when we reach a prototype that has null for its own prototype.

>**Note:** Note: The property of an object that points to its prototype is not called prototype. Its name is not standard, but in practice all browsers use \_\_proto__. The standard way to access an object's prototype is the Object.getPrototypeOf() method.

When you try to access a property of an object: if the property can't be found in the object itself, the prototype is searched for the property. If the property still can't be found, then the prototype's prototype is searched, and so on until either the property is found, or the end of the chain is reached, in which case undefined is returned.

Example:
```javascript
const myObject = {
  city: "Madrid",
  greet() {
    console.log(`Greetings from ${this.city}`);
  },
};
```
In the above example we can't see toString method
```javascript
myObject.toString(); // "[object Object]"
```

but when we call it it returns something, when we call myObject.toString(), the browser looks for toString in myObject, can't find it there, so looks in the prototype object of myObject for toString, finds it there, and calls it.
To find out what is the prototype for myObject, we can use the function Object.getPrototypeOf():

```javascript
Object.getPrototypeOf(myObject); // Object { }
```
This is an object called Object.prototype, and it is the most basic prototype, that all objects have by default. The prototype of __Object.prototype__ is null, so it's at the end of the prototype chain

#### Shadowing properties
What happens if you define a property in an object, when a property with the same name is defined in the object's prototype.
It first looks in the object itself for a property with that name, and only checks the prototype if the property is not defined in it.

This is called "shadowing" the property.

#### Setting a prototype
- Using Object.create

The Object.create() method creates a new object and allows you to specify an object that will be used as the new object's prototype.
```javascript
const personPrototype = {
    greet: function () {
        console.log(`Hello, my name is ${this.name}`);
    }
};

const person = Object.create(personPrototype);
person.name = 'John';
person.age = 30;
person.city = 'New York';

console.log(person); //{name: 'John', age: 30, city: 'New York'}
```

function greet() is set in the prototype of person object.


- Using a constructor
```javascript
const personPrototype = {
  greet() {
    console.log(`hello, my name is ${this.name}!`);
  },
};

function Person(name) {
  this.name = name;
}

Object.assign(Person.prototype, personPrototype);
// or
// Person.prototype.greet = personPrototype.greet;
```

All functions have a property named prototype, So if we set the prototype of a constructor, we can ensure that all objects created with that constructor are given that prototype.

>**Note:** It's common to see this pattern, in which methods are defined on the prototype, but data properties are defined in the constructor. That's because methods are usually the same for every object we create, while we often want each object to have its own value for its data properties.

#### Prototypes and inheritance

Prototypes are a powerful and very flexible feature of JavaScript, making it possible to reuse code and combine objects.

Inheritance is a feature of object-oriented programming languages that lets programmers express the idea that some objects in a system are more specialized versions of other objects.

Suppose that we have two objects with similar properties, we add those in another object and make it a seperate object, which could be used as prototype of this two objects. Therefore inheriting the same properties.

