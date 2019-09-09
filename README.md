# JavaScript Interview Questions

### How does the "this" keyword work?

- First step is to find the place where the function was called.
- Second step is to check questions from top to the bottom and stop if the rule is applied.

1. Is a function called with `new`?

   **Answer**: `this` is a newly constructed object

   ```javascript
   function Person(name) {
     this.name = name;
   }

   const person = new Person("John");
   console.log(person.name); // John
   ```

2. Is `call`, `apply` or `bind` was used to call the function?

   **Answer**: `this` is a specified `thisArg` parameter

   ```javascript
   function getName() {
     return this.name;
   }

   const person = {
     name: "John"
   };

   console.log(getName.call(person)); // John
   console.log(getName.apply(person)); // John

   const boundGetName = getName.bind(person);

   console.log(boundGetName()); // John
   ```

3. Is a function called with context object.

   **Answer**: `this` is a context object

   ```javascript
   function getName() {
     return this.name;
   }

   const person = {
     name: "John",
     getName
   };

   // "this" = person (context object) for the `getName` function
   console.log(person.getName()); // John
   ```

4. Default case:

   **Answer in `strict mode`**: `this` is an `undefined`

   ```javascript
   var name = "globalName";

   function getName() {
     "use strict";
     return this.name;
   }

   console.log(getName()); // TypeError: Cannot read property 'name' of undefined
   ```

   **Answer in non-`strict mode`**: `this` is an global object

   ```javascript
   var name = "globalName";

   function getName() {
     return this.name;
   }

   console.log(getName()); // globalName
   ```

**Note**:

Arrow function doesn't have own `this`. It takes it from the outside. More about arrow functions [link to the question about an arrow function]

### What is an arrow function?

**Syntax**:

```javascript
// arrow function
const sum = (a, b) => a + b;

// regular function
function sum(a, b) {
  return a + b;
}
```

- No `this`

Takes `this` from the outside.

```javascript
const company = {
  name: "Awesome",
  employees: ["John", "Mike", "Joe"],

  printEmployees() {
    this.employees.forEach(employee => {
      // "this" = company, is taken from the outside
      console.log(`${this.name}: ${employee}`);
    });
  }
};

company.printEmployees();

// Output:
// Awesome: John
// Awesome: Mike
// Awesome: Joe
```

- No `arguments`

Takes from the enclosing scope.

```javascript
function sum() {
  // "arguments" of the enclosing scope
  const helper = () => arguments[0] + arguments[1];
  return helper();
}

console.log(sum(2, 3)); // 5
```

- No `super`

Takes `super` from the outer function.

```javascript
class Vehicle {
  start() {
    console.log("Vehicle: start");
  }
}

class Car extends Vehicle {
  start() {
    console.log("Insert and turn the key");

    const helper = () => {
      // takes "super" from the outer function
      super.start();
      console.log("Car: started");
    };

    helper();
  }
}

const car = new Car();
car.start();

// Output:
// Insert and turn the key
// Vehicle: start
// Car: started
```

- Can't be used with `new`

```javascript
const Foo = () => {};
const foo = new Foo(); // TypeError: Foo is not a constructor
```

### What is a "hoisting"?

Variable and function declarations are processed before any part your code is executed.

That statement has actually two parts:

```javascript
var name = "John";
```

- `var name` - declaration, processed during the compilation phase
- `a = "John"` - assignment, processed during execution phase

**Example**:

```javascript
// hoisting: function could be called before declaration
getName();

function getName() {
  // hoisting: could be used before variable declaration
  console.log(name); // undefined

  var name = "John";
}
```

**Note**:

- hoisting works only per-scope
- function expressions are not hoisted
- functions are hoisted before variables
