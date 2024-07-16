# TypeScript Interview Questions and Answers

## Basic Level

### What is TypeScript and how does it differ from JavaScript ?
TypeScript is a typed superset of JavaScript that compiles to plain JavaScript.It adds optional static typing, classes, and interfaces to JavaScript, allowing for better tooling and error checking.

### What are the benefits of using TypeScript?

Benefits include:
Early error detection through type checking
Enhanced IDE support with autocompletion and refactoring tools
Improved code maintainability and readability
Better support for modern JavaScript features

### How do you install TypeScript ?
TypeScript can be installed via npm with the command: `npm install - g typescript`.

### What are the basic data types in TypeScript ?
Basic data types include number, string, boolean, array, tuple, enum, any, void, null, and undefined.

### How do you define a variable in TypeScript ?
Variables can be defined using let, const, or var, followed by the variable name, a colon, and the type:

```typescript
let isDone: boolean = false;
```
### What is the any type in TypeScript?
The any type is a dynamic type that can hold any value, allowing for the bypassing of type checking.

### How do you declare arrays in TypeScript?
Arrays can be declared in two ways:

```typescript
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```
### What are tuples in TypeScript and how do you use them ?
    Tuples are arrays with a fixed number of elements of specific types:

```typescript
let tuple: [string, number];
tuple = ["hello", 10];
```

### How do you define a function in TypeScript?
Functions can be defined with parameter and return types:

```typescript
function add(x: number, y: number): number {
    return x + y;
}
```
### What are interfaces in TypeScript and how do you use them ?
Interfaces define the shape of an object:

```typescript
interface Person {
    firstName: string;
    lastName: string;
}

function greet(person: Person) {
    console.log(`Hello, ${ person.firstName } ${ person.lastName } `);
}
```

## Intermediate Level
### What is type inference in TypeScript?
TypeScript automatically infers the type of a variable based on its value at initialization.

### What are enums in TypeScript?
Enums are a way to define a set of named constants:

```typescript
enum Color { Red, Green, Blue }
let c: Color = Color.Green;
```

### How do you use classes in TypeScript ?
Classes are used to create objects with properties and methods:

```typescript
class Animal {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    move(distance: number = 0) {
        console.log(`${ this.name } moved ${ distance } meters.`);
    }
}
```
### What is the difference between interface and type in TypeScript?
interface is used to define the shape of an object, while type can define a type alias, which can be a primitive, union, or intersection type:

```typescript
interface IPoint {
    x: number;
    y: number;
}

type Point = {
    x: number;
    y: number;
};

type ID = number | string;
```

### How do you implement inheritance in TypeScript ?
Inheritance is implemented using the extends keyword:

```typescript
class Dog extends Animal {
    bark() {
        console.log("Woof! Woof!");
    }
}
```
### What are generics in TypeScript?
Generics allow for creating reusable components with types specified at the time of use:

```typescript
function identity<T>(arg: T): T {
    return arg;
}
```
### What is the never type in TypeScript and when would you use it ?
The never type represents values that never occur, such as in a function that always throws an error or never returns:

```typescript
function error(message: string): never {
    throw new Error(message);
}
```
### How do you use decorators in TypeScript?
Decorators are special declarations used to modify classes and class members:

```typescript
function sealed(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}

@sealed
class BugReport {
    type = "report";
    title: string;

    constructor(t: string) {
        this.title = t;
    }
}
```
### What are modules in TypeScript and how do you export and import them?
Modules are files containing related code.They use export to expose code and import to use it:

```typescript
// math.ts
export function add(x: number, y: number): number {
    return x + y;
}

// app.ts
import { add } from './math';
console.log(add(2, 3));
```
### What is a namespace in TypeScript?
Namespaces are a way to group related code, making it easier to manage and avoid name collisions:

```typescript
namespace Validation {
    export interface StringValidator {
        isAcceptable(s: string): boolean;
    }
}
```

## Advanced Level
### How do you configure the TypeScript compiler ?
The TypeScript compiler is configured using a tsconfig.json file:

```json
{
    "compilerOptions": {
        "target": "es6",
            "module": "commonjs",
                "strict": true
    }
}
```

### What is the unknown type in TypeScript and how is it different from any ?
The unknown type is a type - safe counterpart to any.You must perform type checking before using it:

```typescript
let value: unknown;
value = true; // OK
value = 42;   // OK
value = "Hello"; // OK

let value1: unknown = value; // Error, type of 'value' is unknown
```

### Explain type guards in TypeScript.
Type guards are used to narrow down the type of a variable within a conditional block:

```typescript
function isString(x: any): x is string {
    return typeof x === "string";
}

function print(x: string or number) {
    if (isString(x)) {
        console.log(x.toUpperCase());
    } else {
        console.log(x.toFixed(2));
    }
}
```
### What is declaration merging in TypeScript ?
Declaration merging occurs when TypeScript merges two or more declarations into a single entity:

```typescript
interface Box {
    height: number;
    width: number;
}

interface Box {
    scale: number;
}

let box: Box = { height: 5, width: 6, scale: 10 };
```

### How does TypeScript handle mixins?
Mixins are a way to include properties and methods from one class into another:

```typescript
function applyMixins(derivedCtor: any, baseCtors: any[]) {
    baseCtors.forEach(baseCtor => {
        Object.getOwnPropertyNames(baseCtor.prototype).forEach(name => {
            derivedCtor.prototype[name] = baseCtor.prototype[name];
        });
    });
}

class CanEat {
    eat() {
        console.log("Eating");
    }
}

class CanWalk {
    walk() {
        console.log("Walking");
    }
}

class Person implements CanEat, CanWalk {
    eat: () => void;
    walk: () => void;
}

applyMixins(Person, [CanEat, CanWalk]);
```

### Explain the concept of type assertion in TypeScript.
Type assertion allows you to override the inferred type of a value:

```typescript
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

### What are mapped types in TypeScript?
Mapped types generate new types by transforming properties of an existing type:

```typescript
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

interface Person {
    name: string;
    age: number;
}

let readonlyPerson: Readonly<Person> = {
    name: "John",
    age: 30
};
```

### How do you create and use utility types in TypeScript ?
Utility types are predefined types that simplify common type transformations:

```typescript
interface Person {
    name: string;
    age: number;
}
type Partial<T> = {
    [P in keyof

```

## TypeScript-Specific Behavioral Questions
### Describe a scenario where you would prefer TypeScript over JavaScript.
I would prefer TypeScript over JavaScript in scenarios where the project involves a large codebase with multiple developers. TypeScript’s static typing and enhanced IDE support can significantly improve code maintainability, readability, and reduce runtime errors. For example, in a large e-commerce application where various modules and components interact, using TypeScript ensures that data types and interfaces are consistent across the application, which helps in catching errors early during development.

### How does TypeScript help in large-scale application development?
TypeScript helps in large-scale application development by providing static typing, which catches type-related errors at compile time, reducing runtime errors. It also enhances code readability and maintainability through features like interfaces, classes, and modules. TypeScript’s robust tooling and support for modern JavaScript features enable developers to write cleaner and more manageable code. Additionally, TypeScript facilitates better refactoring and code navigation, making it easier to work in large teams and complex codebases.

### Can TypeScript be used with React? If yes, how?
Yes, TypeScript can be used with React. To use TypeScript with React, you can create a new React project with TypeScript support using the create-react-app tool with the --template typescript option. Alternatively, you can add TypeScript to an existing React project by installing the necessary TypeScript and type definitions packages:

```bash
npm install typescript @types/react @types/react-dom
```
Then, rename your .js or .jsx files to .ts or .tsx, and add type annotations to your React components and props. This setup enhances the development experience by providing type safety and better IntelliSense support in your code editor.

### What is your experience with TypeScript in a team setting?
In a team setting, TypeScript has proven to be incredibly beneficial. It helps maintain code quality and consistency across the team by enforcing type safety and reducing common JavaScript errors. During code reviews, TypeScript’s static typing makes it easier to understand the code and identify potential issues. I’ve also found that TypeScript facilitates better collaboration, as team members can confidently refactor code and navigate through large codebases with the assurance that the types will catch mistakes early. Overall, TypeScript improves productivity and reduces the cognitive load on developers by providing clear and explicit contracts for data structures and functions.

### How do you approach debugging TypeScript code?
Debugging TypeScript code involves a few key steps:

- Compilation Errors: First, I address any TypeScript compilation errors, as these often point out type mismatches or incorrect usage that can prevent runtime issues.
- Source Maps: I ensure that source maps are enabled in the TypeScript configuration (tsconfig.json). Source maps allow me to debug the original TypeScript code directly in the browser’s developer tools.
- Console Logging: I use console.log statements to inspect variables and application state at different points in the code.
- Debugger: I use the debugger statement in my code and the browser’s developer tools or an IDE like VSCode to set breakpoints and step through the code to understand the flow and identify issues.
- Type Annotations: I review type annotations and interfaces to ensure they are correctly defined and used, which helps in catching logical errors and incorrect assumptions.
- Unit Tests: I write and run unit tests to validate individual components and functions, ensuring they behave as expected in isolation.
