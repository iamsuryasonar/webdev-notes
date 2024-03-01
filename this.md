- [this](#this)
- [globalThis](#globalthis)
- [this in Regular function vs Arrow function](#this-in-regular-function-vs-arrow-function)
- [call(), apply() and bind()](#call-apply-and-bind)
- [Polyfill for bind?](#polyfill-for-bind)


# this
The this represents an object that represents the current execution context.
The value of this is determined by how a function is called. In other words, the this references the object that is currently calling the function.

or we can say,
this refers to the context within which a function is executed.
```javascript
const obj = { a: "Custom" };

// Variables declared with var become properties of 'globalThis'.
var a = "Global";

function whatsThis() {
  return this.a; // 'this' depends on how the function is called
}

whatsThis(); // 'Global'; the 'this' parameter defaults to 'globalThis' in non–strict mode
obj.whatsThis = whatsThis;
obj.whatsThis(); // 'Custom'; the 'this' parameter is bound to obj
```

The behaviour of this can be influenced by the way a function is called: as a method, as a function, as a constructor, or using call or apply.

The value of this is not static and can change dynamically based on the invocation context. Here are a few common scenarios for this in JavaScript:

- Global Context:
When used in the global scope or outside of any function, this refers to the global object (window in the browser, global in Node.js).

- Function Context:
When used inside a regular function, the value of this depends on how the function is called. 
  - If the function is called as a method of an object, this refers to the object itself. 
  - If the function is called standalone, this refers to the global object (non-strict mode) or undefined (strict mode).
  - When you use the new keyword to create an instance of a function object, you use the function as a constructor. JavaScript creates a new object and sets this to the newly created object.
  - We can explicitly set this value using call(), apply() and bind() when calling a function

- Arrow Functions:
Arrow functions create a closure over the this value of its surrounding scope, which means arrow functions behave as if they are "auto-bound" — no matter how it's invoked, this is bound to what it was when the function was created.
when invoking arrow functions using call(), bind(), or apply(), the this parameter that is passed is ignored. You can still pass other arguments using these methods, though.

- Event Handlers:
When used as an event handler, this typically refers to the element that received the event.


# globalThis
ES2020 introduced the globalThis object that provides a standard way to access the global object across environments.

In the web browsers, the globalThis is the window object.
```javascript
console.log(globalThis) // Window object
```


# this in Regular function vs Arrow function

When a regular function is executed by the JavaScript engine, a new execution context is created. And a new this is bind to the object it is called upon during the execution of the regular function.

It is decided by how it is called:
- Implicit binding:  If the regular function is called as a method of an object, this refers to the object itself. 
- 
```javascript
const obj = {
  name: 'John',
  greet: function() {
    console.log(`Hello, ${this.name}!`);
  }
};

obj.greet(); // Output: Hello, John!
```

- Window binding: If the regular function is called standalone, this refers to the global object (non-strict mode) or undefined (strict mode).

```javascript
function greet() {
  console.log(`Hello, ${this.name}!`);
}

window.name = 'Alice';
greet(); // Output: Hello, Alice!
```

- new binding: When you use the new keyword to create an instance of a function object, you use the function as a constructor. JavaScript creates a new object and sets this to the newly created object.

```javascript
function Person(name) {
  this.name = name;
  
  this.greet = function() {
    console.log(`Hello, ${this.name}!`);
  };
}

const person = new Person('Jane');
person.greet(); // Output: Hello, Jane!
```

- Explicit binding: We can explicitly set this value using call(), apply() and bind() when calling a regular function.

```javascript
const obj1 = {
  name: 'Alice'
};

const obj2 = {
  name: 'Bob'
};

function greet() {
  console.log(`Hello, ${this.name}!`);
}

greet.call(obj1); // Output: Hello, Alice!
greet.apply(obj2); // Output: Hello, Bob!
const greetBound = greet.bind(obj1);
greetBound(); // Output: Hello, Alice!
```

But in case of arrow function this is set with the this context of the surrounding context.

It means the arrow function does not have it own this but inherits the this from the outer lexical environment where the arrow function is defined.

```javascript
const obj = {
  name: 'John',
  greet: function() {
    const arrowFunction = () => {
      console.log(`Hello, ${this.name}!`);
    };
    
    arrowFunction();
  }
};

obj.greet(); // Output: Hello, John!

```

# call(), apply() and bind()

The bind, call, and apply methods are used to manipulate the this context and to invoke functions with a specific context or set of arguments. These methods provide ways to control how a function is executed and which object it should use as its context.

- call:

    The call method is used to invoke a function with a specified this context(the object) and a list of arguments passed individually. It immediately executes the function with the provided context and arguments.

```javascript
someFunction.call(context, arg1, arg2, ...);
```

- apply:

    The apply method is similar to call but takes the function arguments as an array. It is used to execute a function with a specified this context(the object) and an array of arguments. It immediately executes the function with the provided context and array of arguments.

```javascript
someFunction.apply(context, [arg1, arg2, ...]);
```

- bind:

    The bind method is used to create a new function that, when called, has its this keyword set to the provided value. It does not immediately execute the function but returns a new function with the bound context.

```javascript
const boundFunction = someFunction.bind(context,arg1,arg2, ...);
```
Example for call and apply method:

```javascript
const person = {
    firstName: 'John',
    lastName: 'Doe'
}
function fullName(age) {
    console.log('My name is ' + this.firstName + ' ' + this.lastName + ' ' + 'I am ' + age + ' years old')
}

// Suppose we want to invoke function fullname with person2 context then first argument is the context and rest are function arguments we want to pass

fullName.call(person, '22');

// apply method also works same as call method but we pass function arguments in an array

fullName.apply(person, [22]);

// bind method is exactly same as call but instead of directly invoking the function it returns the function which could be called later

const fullNameFunction = fullName.bind(person, '22');
console.log(fullNameFunction());
```

```javascript
// another example:

const person = {
  name: 'John Doe',
  greet: function() {
    return `Hello, I'm ${this.name}.`;
  }
};

const anotherPerson = {
  name: 'Jane Smith'
};


// call and apply methods are used to borrow function of another object

// Using call
console.log(person.greet.call(anotherPerson)); 
// Hello, I'm Jane Smith.

// Using apply
console.log(person.greet.apply(anotherPerson)); 
// Hello, I'm Jane Smith.

// Using bind
const boundGreet = person.greet.bind(anotherPerson);
console.log(boundGreet()); 
// Hello, I'm Jane Smith.
```

# Polyfill for bind?

The polyfill captures the this value and the provided arguments, and it returns a new function that, when called, applies the original function with the saved context and arguments.

```javascript
const person = {
   firstName : 'John',
   lastName:'Doe'
}
function fullName(age){
   console.log('My name is '+this.firstName+' '+this.lastName+' '+'I am '+age+' years old')
}

const fullNameFunction = fullName.bind(person,'22');
console.log(fullNameFunction());
```

We want to create our own bind method that has the functionality exactly like the bind method.

Firstly: since the bind method is available to all functions, our custom bind method should be available to all functions, how do we do that?

```javascript
Function.prototype.mybind = function(){}
```

Secondly: we will be returning a function, since the bind method does the same.

```javascript
Function.prototype.mybind = function(){
return function(){}
}
```
Thirdly: **const** fullNameFunction= fullName.bind(person,'22');

here , we see fullName, how do we pass this method to be returned function?

```javascript
Function.prototype.mybind = function(){
// we can get the context using this
   let fn= this;
   return function(){
   }
}
```

Fourthly: we want the first argument to call, destructuring args we can get the first argument.

```javascript
Function.prototype.mybind = function(...args){
   let fn= this;
   return function(){
     fn.call(args[0])
   }
}
```

Lastly: what about the function arguments?

```javascript
Function.prototype.mybind = function(...args){
   let fn= this;
   let params = args.slice(1); 
   // since params is now an array we cannot pass that to the call method instead we will use apply method because it accepts array.
   return function(){
     fn.apply(args[0],params)
   }
}
```

Again: What if the function itself required an argument?

```javascript
const fullNameFunction = fullName.bind(person,'22');
fullNameFunction(arg1,arg2);
```

In this case the fullNameFunction.

```javascript
Function.prototype.mybind = function(...args){
   let fn = this;
   let params = args.slice(1); 
   return function(...args2){
      fn.apply(args[0],[...params,...args])
   }
}
```

In that case we will just concatenate both params and args2

