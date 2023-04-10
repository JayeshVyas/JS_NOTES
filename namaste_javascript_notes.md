### How JavaScript Works?

> Is JavaScript:
>
> - <i><mark>**Synchronous**</mark> or Asynchronous?</i>
> - <i><mark>**Single-threaded**</mark> or Multi-threaded?</i>

- Everything in JavaScript happens inside an **Execution Context**
  - You can assume this _execution context_ to be a big box or a container in which the whole JavaScript code is executed.
  - This **big box** has two components in it:
    - **Memory:** This is the place where all variables and functions are stored as key-value pairs. This _'memory component'_ is also known as the **Variable Environment**. So, it's sort of an environment in which all these variables and functions are stored as key-value pairs.
    - **Code:** This is the place where the code is executed one line at a time. This _'code component'_ is also known as the **Thread of Execution**. So, this _thread of execution_ is just like a thread in which the whole code is executed one line at a time.
- JavaScript is a **_synchronous single-threaded_** language.
  - **Single-threaded** means that JavaScript can only execute one command at a time.
  - **Synchronous single-threaded** means that JavaScript can only execute one command at a time and in a specific order. That means that it can only go to the next line once the current line has finished executing. This is what it means by being **_synchronous single-threaded_**.

---

### How JavaScript Code is executed?

**What happens when you run JavaScript code?**

> _An <mark>**Execution Context**</mark> is created._

Let's take an example using some code.

```js
var n = 2;
function square(num) {
  var ans = num * num;
  return ans;
}
var square2 = square(n);
var square4 = square(4);
```

- When the above code is executed, an **Execution Context** is created
  - This **Execution Context** is created in 2 phases:
    - **Creation:** The **Creation Phase** is also known as the **Memory Creation Phase**. This is a very critical phase.
      - In the first phase of _memory creation_, JavaScript will allocate memory to all the variables and functions.
      - As soon as JavaScript encounters `var n = 2;`, it allocates memory to `n`.
      - On encountering the function `square(num)`, it allocates memory to this function (`square`) as well.
      - When it allocates memory to `n`, it stores a special value called `undefined`. `undefined` is treated like a special placeholder in JavaScript.
      - In case of the function `square(num)`, it stores the entire code of this function in the memory space.
      - It will also allocate memory to `square2` and `square4` and store `undefined` for both.
      - In order to complete this **Creation** phase, JavaScript goes through the code, line by line, top to bottom.
    - **Code Execution:**
      - Now JavaScript once again runs through this whole program line by line.
      - When it encounters `var n = 2;`, it actually places `2` as a value for `n` in the **Memory Component**.
      - When it encounters the function definition of `square(num)`, it has nothing to execute, so JavaScript simply passes through.
      - When it encounters `var square2 = square(n);`, we are now invoking a function.
      - Functions are the heart of JavaScript. They behave very differently in JavaScript than in any other language.
      - Whenever a function is invoked, an all together new **Execution Context** is created.
      - So, technically, a brand new **Execution Context** is created inside the **Code Component** of the overall **Execution Context**.
      - This new **inner Execution Context** also has its own **Memory Component** and the **Code Component**.
      - What happens in the **innner** one now is:
        - We have 2 variables in this case, namely `num` (which is the parameter) and `ans`.
        - So memory will be allocated to `num` and `ans`.
        - In phase 1, alike the overall Execution Context, `undefined` will be assigned to `num` and `ans`.
        - Now coming to phase 2 (code execution phase), the value of the argument is assigned to the parameter. So, in the statement `var square2 = square(n);` where we are invoking a function, we're passing the argument `n` to the function `square(num)` and the value of this argument replaces the placeholder `undefined` in the **Memory Component** of the **inner Execution Context**.
        - After calculation of `num * num`, the value is stored in `ans`.
        - On encountering `return ans;`, the value stored in `ans` is returned to the invoked location and this **inner Execution Context** ends. When it ends, it actually gets deleted.
      - Now, the same process is followed when the line `var square4 = square(4);` is encountered.
      - After successful execution of the last line, the **overall Execution Context** also gets deleted. This _'overall'_ execution context is also termed as the **Global Execution Context**.

> Now, how does JavaScript manage this _'chaining'_ of **Execution Contexts**?

- It actually manages a stack under the hood.
- This stack is also termed as the **Call Stack**.
- The **GEC** (Global Execution Context) is always at the bottom of this stack.
- Whenever a new Execution Context is created, it is pushed in this stack and on completion of its purpose, it gets popped.
- The control stays with the topmost element of this _Call Stack_.
- This _Call Stack_ is only used to manage the **Execution Context(s)**.
- On successful execution of the last statement, the **Call Stack** is emptied.

> **Call Stack** maintains the <mark>**_order of execution_**</mark> of Execution Contexts.

The **Call Stack** has the following fancy names by which it is also referred:

- Execution Context Stack
- Program Stack
- Control Stack
- Runtime Stack
- Machine Stack

---

### Hoisting in JavaScript

Let's take 2 code blocks for example.

First block:

```js
var x = 7;

function getName() {
  console.log("Namaste JavaScript");
}

getName();
console.log(x);
```

Output:

```
Namaste JavaScript
7
```

Second block:

```js
getName();
console.log(x);

var x = 7;

function getName() {
  console.log("Namaste JavaScript");
}
```

Output:

```
Namaste JavaScript
undefined
```

In most programming languages, code similar to the second block will throw errors, but that's not the case in JavaScript.

If we remove the statement `var x = 7;` from the second code block, which becomes like this:

```js
getName();
console.log(x);

function getName() {
  console.log("Namaste JavaScript");
}
```

It will now throw the following error:

```
Uncaught ReferenceError: x is not defined at...
```

Now, **not defined** and **undefined** are 2 different things.

All these scenarios are due to something called **Hoisting** in JavaScript.

**Hoisting**

- **Hoisting** is a phenomena in JavaScript by which we can access variables and functions even before initializing them.

- The result of **Hoisting**, i.e., the memory allocation that happens due to which we are able to access variables and functions even before initializing them, can be viewed in the developer tools. Also, we have a debugger in the developer tools by which we can stop execution of our code and check for ourselves the value that is stored against the variables and functions even before start of the execution.

**Not Defined** VS **Undefined**

> **Not Defined** means that the variable or function is not present in the memory, while **Undefined** acts simply as a placeholder for any value which is not defined yet, but memory is reserved for the same.

Suppose we now have the following code block where `getName` is an arrow function:

```js
getName();

var getName = () => {
  console.log("Namaste JavaScript");
};
```

On execution of the above code, we'll face the following error:

```
Uncaught TypeError: getName is not a function at ...
```

The reason for this error is that when we declare `getName()` as an arrow function, it behaves like any other variable. Meaning that the value stored in memory for `getName()` is `undefined` at the first line of the given code block.

Even if we declare a function as follows:

```js
var getName2 = function () {
	...
}
```

It will again behave like a variable.

Also, the **Call Stack** can be viewed in the developer tools. Hence the flow of control of the entire execution of the program can be easily monitored using the developer tools. The **GEC** (Global Execution Context) is referred to as **anonymous** in the **Call Stack** shown in the developer tools.

---

### How functions work in JavaScript?

Sample code:

```js
var x = 1;
a();
b();
console.log(x);

function a() {
  var x = 10;
  console.log(x);
}

function b() {
  var x = 100;
  console.log(x);
}
```

Sample output:

```
10
100
1
```

- Imagine the **Call Stack** and **Execution Contexts** for the above code.
- Each variable `x` is treated differently.
- The variable `x` declared inside each function is different than the `x` declared in the global context.
- Whenever we invoke a function, an **inner Execution Context** is created and we have a new `x` inside the _memory component_ of this **inner Execution Context**.
- When we reach the statement `console.log(x);`, we're actually retrieving `x` _from the memory of the local context and not the global context._

---

### SHORTEST JS Program

**`window` and `this` keyword**

The **shortest JavaScript program** would be **_an empty script_**.

Even though there's an empty script, still JavaScript engine is doing a lot of things behind the scenes.

Even on the execution of an empty script, a **GEC** (Global Execution Context) would be created and the JavaScript engine also sets up the memory space, though there is nothing to set as per our **empty** script.

On going to the console and typing the following:

```
> window
```

We do get some output.

Technically the `window` is like a big object with a lot of key-value pairs which typically stores different objects.

Even if our script is empty, the `window` is not.

The functions and variables in the `window` object are created by the JavaScript engine. Also, these are created into the global space, meaning we can access all these variables and functions anywhere in our JS program.

Also, just like the `window` object, the JavaScript engine creates something called `this`, which can also be logged on the console, just like the `window` object.

At the global level, the `this` points to the `window` object.

But, **What is `window`?**

`window` is actually a global object which is created along with the global execution context. So, whenever any JavaScript program is run, _a **global object** is created, a **GEC** is created, and along with that execution context a **`this`** variable is created._

Every JavaScript engine is responsible to create a global object similar to `window` on execution of a script. In case of browsers, this object is known as `window`. In case of **Node**, it's something else. But, wherever we might run our JavaScript program, one thing is for certain that _this global object would be created_, even if our script be empty.

At the global level, `this === window` returns `true` (in case of browsers).

Whenever an **Execution Context** is created, a `this` is created along with it, _even for functional execution contexts_. At the global level, the `this` points to the global object, which is `window` in case of browsers.

If we have a script with some code in it, then this global object would contain all the variables and functions which are at the global scope post **Hoisting**.

Suppose we have `var a = 10;` in the global scope, then all of the following statements are equivalent:

```js
console.log(window.a);
console.log(a);
console.log(this.a);
```

So, once again, the shortest program in JavaScript is:

```js

```

---

### Undefined VS Not Defined in JavaScript

`undefined` is a very special keyword in JavaScript, which is not there in other languages. This keyword has a lot to do with how JavaScript code is executed.

As discussed earlier, JavaScript code is executed in a different way. It creates a **GEC** (Global Execution Context) and allocates memory to all the variables and functions (which are present at the global scope) even before a single line of code is executed. Here is where `undefined` comes into picture.

When JavaScript allocates memory to all the variables and functions, to the variables it tries to put a placeholder. `undefined` is treated like a placeholder which is placed in the memory. So, technically, `undefined` takes up some memory space. It's totally different than `not defined`.

So, while creating the memory component, the JavaScript engine allocates `undefined` to all the variables before execution begins.

Sample code:

```js
console.log(a);
var a = 7;
console.log(a);
```

Sample output:

```
undefined
7
```

So, coming to `not defined`, `not defined` refers to something which has not been allocated memory.

`undefined` can be taken as a placeholder for the time being until a value is assigned replacing this placeholder.

Sample code:

```js
var a;

if (a === undefined) {
  console.log("a is undefined");
} else {
  console.log("a is not undefined");
}
```

Sample output:

```
a is undefined
```

**_JavaScript is a loosely typed language_**. Loosely typed means that it does not attach it's variables to any specific data type. That means that the data type of the data being stored in a variable can change from time to time. In this case, JavaScript is very flexible. In strict type languages like C, C++, Java, etc., every variable is associated with a data type, that means that a variable of type `String` in Java will only be able to store data of type `String`, so on and so forth.

Sample code:

```js
var a;
console.log(a);
a = 10;
console.log(a);
a = "hello world";
console.log(a);
```

Sample output:

```
undefined
10
hello world
```

Hence, JavaScript is a loosely typed language. Loosely typed language is also termed as weakly typed language. That does not mean that JavaScript is weak in any sense though.

Also, even though `undefined` is a placeholder in JavaScript, it really is a bad practice to use it in assignments. The reason being that this could lead to a lot of inconsistencies.

```js
// bad practice
a = undefined;
```

---

### The Scope Chain - Scope & Lexical Environment

Scope in JavaScript is directly related to Lexical Environment. The Lexical Environment is a very classic concept in JavaScript.

Sample code:

```js
function a() {
  console.log(b);
}
var b = 10;
a();
```

Sample output:

```
10
```

Justification:

When the function is invoked as `a();`, the JavaScript engine looks for `b` in the local memory space of `a()`. Failing to find it, it then searches for the same a level up, the global memory space in this case, and finds the value `10` assigned to `b`.

If, suppose, `b` weren't available in the upper level, and there were more levels preceding the current level, it'll search for `b` in the upper levels, till either it finds it or we run out of levels, which happens when one reaches the global memory space.

Sample code:

```js
function a() {
  c();
  function c() {
    console.log(b);
  }
}
var b = 10;
a();
```

Sample output:

```
10
```

Let's take another example.

Sample code:

```js
function a() {
  var b = 10;
  c();
  function c() {}
}
a();
console.log(b);
```

The above code will face the following error:

```
Uncaught ReferenceError: b is not defined at ...
```

Here's where scope comes into picture.

> Scope is basically where you can access a particular data member.

There are 2 aspects to Scope:

- What is the scope of this (referring to the current) variable? Meaning, where can I access this variable?
- Is this variable inside the local (referring to the Local Execution Context) scope?

The Scope is directly dependent on the Lexical Environment.

> _Whenever an **Execution Context** is created, a **Lexical Environment** is also created._

Lexical Environment is the local memory along with the Lexical Environment of its parent. Lexical, as a term, means in hierarchy, or in a sequence.

Corresponding to the above code block, we can say that the `c()` is lexically sitting inside `a()`. In terms of code, it's basically where a particular data member is located. Corresponding to that, we can say that `a()` is lexically inside the global scope.

Whenever a new Execution Context is created, in the memory component of this Execution Context, we also get a reference to the Lexical Environment to its parent. In case of the Global Execution Context, this reference points to `null`.

It can be visualized by the diagram below, in correspondence to the above code.

![lexical-env-visualization-js.png](https://i.ibb.co/Tm4WsH9/lexical-env-visualization-js.png)

So, technically when a variable is not found in the current local memory, the engine searches for that variable in the reference which points to the lexical parent and continues this process until the variable is found or we hit `null`. This search mechanism works on the basis of the **Scope Chain**. So, _the **Scope Chain** is the chain of all these lexical environments and their parent references._

So, whenever an Execution Context is created, a Lexical Environment is also created, which is a part of the memory component of this Execution Context. This Lexical Environment is actually a reference to the memory component of the lexical parent of the current Execution Context and in case of the **GEC** (Global Execution Context), this reference points to `null`.

> So, a variable is `not defined` when this **Scope Chain** is exhausted and the variable is not found.

The **Scope Chain** can be visualized by using the developer tools of any modern browser.

![scope-chain-dev-tools.png](https://i.ibb.co/pwKFHZK/scope-chain-dev-tools.png)

---

### `let` and `const` in JS - Temporal Dead Zone

- **What is a Temporal Dead Zone?**
- **Are `let` & `const` declarations hoisted?**
- **SyntaxError vs. ReferenceError vs. TypeError**

`let` and `const` declarations are hoisted. But, they are hoisted very differently than the `var` declarations. Technically the `let` and `const` declarations spend some time in the **Temporal Dead Zone**.

Sample code:

```js
console.log(b);
let a = 10;
var b = 100;
```

Sample output:

```
undefined
```

Sample code:

```js
console.log(a);
let a = 10;
var b = 100;
```

In the above code, we'll face the following issue:

```
Uncaught ReferenceError: Cannot access 'a' before initialization at ...
```

So, according to the above error, we can only access `a` after we've initialized it. But, _how to know whether this variable was hoisted or not?_

Let's go into the developer tools and check for ourselves what's going on.

![let-allocated-in-script-and-not-global.png](https://i.ibb.co/wpbMVrx/let-allocated-in-script-and-not-global.png)

As shown in the above screenshot, we can see that JavaScript has allocated memory to `a` in the `Script` section, but for `b` it has done the same in the `Global` section. Now the question arises is, _what is this `Script`?_

Memory was assigned to `b`, to the `var` declaration, and it was attached to the global object. But, in case of `let` and `const`, they are also allocated memory, which is what we call as **Hoisting**. But, the difference is that `let` and `const` declarations are stored in a different memory space than global, and we cannot access them unless they are assigned a value. This is how **Hoisting** works in case of `let` and `const` declarations.

In case of the above code, in which the execution is paused using a debugger, even if we run the entire script, the `let` and `const` declarations with still remain in the `Script` section of the memory. The point to consider is whether we'll be able to access them or not. Well, we'll be able to access only when a value is assigned to them.

**What is a Temporal Dead Zone?**

**Temporal Dead Zone** is the time interval for a certain data member, from when it is declared till when a value is assigned to it in the memory space. For example, in the code in the above screenshot, the **Temporal Dead Zone** is the time interval from the time the `let` declaration was hoisted till the time it was assigned a value. So, the **Temporal Dead Zone** for a data member ends the moment a value is assigned to it.

Whenever we try to access a variable when it is in the **Temporal Dead Zone**, we get a `ReferenceError`.

Another type of `ReferenceError` is when we're trying to access something which is `not defined`.

In case of `var` declarations, we can access the data members from the `window` object (in case of browsers). But that is not the case for `let` and `const` declarations. If we try to do something like `this.a` or `window.a` in the global scope relative to the above code, we'll get `undefined` in return, which we get for anything which is absent as a key in the `window` object.

Sample Code:

```js
let a = 10;
let a = 100;
```

In case of the above code, we'll face the following issue:

```
Uncaught SyntaxError: Identifier 'a' has already been declared
```

So, this means that we're unable to do a re-declaration using `let`.

Also, in case of `SyntaxError`s, none of the statements of the code are executed.

For instance, the code below:

```js
console.log("something");
let a = 10;
let a = 100;
```

Will face the same `SyntaxError` situation and nothing will be executed.

This phenomena is also observed in the following code:

```js
let a = 10;
var a = 100;
```

But, the following code snippet is totally valid:

```js
var b = 100;
var b = 1000;
```

Now, coming to `const`. `const` behaves much more similar than how `let` behaves, but it is a little more stricter than `let`.

For instance, we can do a `let` declaration without initialization and initialize the same later. But, we're unable to do the same in case of a `const` declaration.

Sample code:

```js
const b;
```

We'll face the following issue in the above code:

```
Uncaught SyntaxError: Missing initializer in const declaration
```

This means that whenever we're going for a `const` declaration, it expects to get initialized in the same line.

Sample code:

```js
const b = 10;
b = 100;
```

We'll face the following issue in the above code snippet:

```
Uncaught TypeError: Assignment to constant variable ...
```

Now, let us come to the difference between these 3 types of errors.

The `TypeError` which we see in case of trying to reinitialize a `const` variable is due to the fact that the variable is of type `const` and hence it is meant to be a constant.

We face `SyntaxError`s whenever we violate the rules of the syntax.

We face `ReferenceError`s when the JavaScript Engine tries to find some variable inside the memory space but it's unable to access it.

Now, according to certain conventions followed by the community, always use `const` wherever possible. This reduces the possibility of unexpected errors. If not `const` then we should use `let`, since due to the presence of the **Temporal Dead Zone**, we won't face unexpected errors.

Keep `var` aside. But, even if you ever feel the need to use `var`, use it very consciously. Otherwise, it is an un-preferred method of declaration.

Also, in an attempt to reduce issues being faced due to the **Temporal Dead Zone**, always try to put all the declarations and initializations at the top of the script. This technically shrinks the **Temporal Dead Zone** to zero.

---

### Block Scope & Shadowing in JS

`let` and `const` are **Block Scoped**.

The following is a perfectly valid JS code.

```js
{
}
```

Anything which is enclosed between `{` and `}` is a block. Even if there's nothing to enclose.

Blocks are also referred to as **Compound Statement**.

The use of a **Compound Statement** comes in handy when we need to group a bunch of statements into a single block and use in places where JavaScript expects only one statement.

For instance, `if` in JavaScript expects only one statement. But in case we need to execute multiple statements following an `if` case, we'll use a block, since a block is treated as a single statement, referred to as a **Compound Statement**.

**What is Block Scope?**

**Block Scope** means what all data members we can access within a block.

Let us take some code for instance and check out the developer tools for the same.

```js
{
  var a = 10;
  let b = 20;
  const c = 30;
}
```

![let-const-block-scoped-dev-tools.png](https://i.ibb.co/JpyL7ZK/let-const-block-scoped-dev-tools.png)

As shown in the image above, we've got `b` and `c` inside the `Block` scope. They are hoisted and assigned `undefined`. They are hoisted in a separate memory space which is reserved for their block. But, we see that `a` is hoisted inside the `Global` scope.

_`let` and `const` are block scoped_, which means that a separate memory space is reserved for the block of which the `let` and `const` declarations are a part of.

We would be able to access `a` outside the block, since it's hoisted as part of the `Global` scope. That's not the case for the `let` and `const` declarations.

**Shadowing**

Sample code:

```js
var a = 100;
{
  var a = 10;
  console.log(a);
}
console.log(a);
```

Sample output:

```
10
10
```

The inner `a` shadowed the outer `a` and the value was also overridden, reason for which we got the above output. This happened since both of them were pointing to the same memory location, the global memory space in this case.

Shadowing works differently in case of `let` and `const`.

Sample code:

```js
let b = 100;
{
  var b = 20;
  console.log(b);
}
console.log(b);
```

Sample output:

```
20
100
```

So, we see that the inner `b` shadowed the outer `b`, but since both of these are referring to different memory spaces, the inner `b` didn't override the outer `b`. If we analyze the code using developer tools, we see that the inner `b` is part of the `Block` scope, while the outer `b` is part of the `Script` scope. According to the dev tools, we see that there are 3 memory spaces or scopes, namely `Block`, `Script` and `Global`.

**Illegal Shadowing**

Sample code:

```js
let a = 20;
{
  var a = 200;
}
```

We'll face the following error in the above code.

```
Uncaught SyntaxError: Identifier 'a' has already been declared
```

This is an example of **Illegal Shadowing**.

But, if we tried the same thing with both the declarations being of either `let` or `const`, then legal shadowing could have occurred.

But the following is also a case of legal shadowing.

```js
var a = 200;
{
  let a = 20;
}
```

Let's dig deep with the earlier code.

```js
let a = 20;
{
  var a = 200;
}
```

If a data member is attempting to shadow another data member, it shouldn't cross the boundary of its scope. In a particular scope, `let` cannot be re-declared.

Now, `var` is **function scoped**. This means that the following piece of code is totally valid.

```js
let a = 20;
function x() {
  var a = 200;
}
```

**Block Scope** follows **Lexical Scope**.

Also, the concept of scope in case of arrow functions is exactly the same as what we encounter in case of normal functions in JavaScript.

---

### Closures in JavaScript

Sample code:

```js
function x() {
  var a = 7;
  function y() {
    console.log(a);
  }
  y();
}
x();
```

Sample output:

```
7
```

Dev tools snap:

![closure-dev-tools.png](https://i.ibb.co/Tw20htF/closure-dev-tools.png)

**Closure** basically means a function bind together with its lexical environment. In other words, a function along with its lexical scope forms a **Closure**.

In the above screenshot, we have paused the execution at `console.log(a);`. We see that inside `y()` it forms a closure with its lexical parent, `x()` in this case.

_Let's see what the MDN says about Closures:_

> A **closure** is the combination of a function bundled together (enclosed) with references to its surrounding state (the **lexical environment**). In other words, a closure gives you access to an outer function’s scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time.

In JavaScript, functions are very flexible in nature. We can assign functions to variables. We can even pass an entire function as a parameter to another function while calling.

The below code snippet it totally valid (provided that `x` is already defined):

```js
x(function y() {
  console.log(a);
});
```

Moreover, we are also allowed to return a function from a function in JavaScript. This is where the concept of closures kind of complicate things.

Sample code:

```js
function x() {
  var a = 7;
  function y() {
    console.log(a);
  }
  return y;
}
var z = x();
z();
```

Sample output:

```
7
```

Now here **closure** comes into picture. When a function is returned from another function, they still maintain their lexical scope. They remember where they were actually present. Meaning, `y()` remembers where it came from. It means that when we returned `y()`, it's not just that the function code was returned, but a closure enclosed function along with its lexical scope was returned.

The following code snippet is equivalent to the last one.

```js
function x() {
  var a = 7;
  return function y() {
    console.log(a);
  };
}
var z = x();
z();
```

Whenever we return a function, the references to the data members as part of the closure are retained instead of being garbage collected.

_Uses of **Closures**:_

- Module Design Pattern
- Currying
- Functions like once
- Memoize
- Maintaining state in async world
- setTimeouts
- Iterators
- and many more...

---

### `setTimeout` & Closures

**Interview Question:** _Print 1-5 with a lapse of 1 second between each digit._

Sample code:

```js
function x() {
  var i = 1;
  setTimeout(function () {
    console.log(i);
  }, 3000);
}
x();
```

The above code prints `1` after 3 seconds.

Sample code:

```js
function x() {
  var i = 1;
  setTimeout(function () {
    console.log(i);
  }, 3000);
  console.log("Namaste JavaScript");
}
x();
```

In the above code, `Namaste JavaScript` is printed before `1`, which in turn is printed after 3 seconds of a lapse.

What `setTimeout` does is, first its callback function forms a closure. Meaning it remembers the reference to `i`. Wherever this function goes, it takes the reference of `i` along with it. What `setTimeout` does is, it takes this callback function and stores it in some place and attaches a timer to it, and the rest of the code proceeds. Once the timer runs out, the `setTimeout` places that callback function to the call stack and runs it.

Sample code:

```js
function x() {
  for (var i = 1; i <= 5; i++) {
    setTimeout(function () {
      console.log(i);
    }, i * 1000);
  }

  console.log("Namaste JavaScript");
}
x();
```

Sample output:

```
Namaste JavaScript
6
6
6
6
6
```

**[NOTE:** All the lines with `6` are printed after a delay of 1 second.**]**

This behavior was observed because of the closure.

A closure is like a function along with its lexical environment. So, even when a function is taken out from its original scope, if it's executed in some other scope, it still remembers its lexical environment. Meaning it can access those variables of its lexical scope.

So what happens is, when the `setTimeout` takes this function and stores it somewhere and attaches a timer, the function remembers the reference to `i`. _It remembers the reference, not the value._

So, when the above code is executed, all 5 copies of the function are pointing to the same reference of `i`.

Sample code:

```js
function x() {
  for (let i = 1; i <= 5; i++) {
    setTimeout(function () {
      console.log(i);
    }, i * 1000);
  }

  console.log("Namaste JavaScript");
}
x();
```

Sample output:

```
Namaste JavaScript
1
2
3
4
5
```

**[NOTE:** All the digits are printed after a lapse of 1 second.**]**

Now, this behavior is observed in the case of this code since we've used `let`. The reason being that `let` is block scoped, which means that for every iteration, every `i` is a new copy all together and each time the `setTimeout` function is called, its callback function has a new copy of `i` with it. It forms a closure with a new variable each time. Each time the function is called, it is referring to a different memory location.

But, there's a workaround that we can achieve this even by using `var`.

Sample code:

```js
function x() {
  for (var i = 1; i <= 5; i++) {
    function close(i) {
      setTimeout(function () {
        console.log(i);
      }, i * 1000);
    }
    close(i);
  }

  console.log("Namaste JavaScript");
}
```

So, _we've formed a closure_. We created a function `close` and enclosed the `setTimeout` within it. Also, we're passing the value of `i` to this function `close`. This works because every time we call this `close` function with this `i`, it creates a new copy of `i` for itself when we **pass by value**.

---

### More on Closures

Sample code:

```js
function outer() {
  var a = 10;
  function inner() {
    console.log(a);
  }
  return inner;
}
outer()();
```

Sample output:

```
10
```

In the last statement of the code we have `outer()();`. This statement actually executes the inner function. It is able to do so as per the syntax since invoking the `outer()` returns `inner` which we are in turn calling by chaining another pair of `()`.

One example of data hiding using closures would be the following:

```js
function counter() {
  var count = 0;
  return function incrementCounter() {
    count++;
    console.log(count);
  };
}
var counter1 = counter();
counter1();
```

If we try to access `count` outside the function `counter()`, we'll get a `ReferenceError` of it being `not defined`.

Sample code:

```js
function counter() {
  var count = 0;
  return function incrementCounter() {
    count++;
    console.log(count);
  };
}

var counter1 = counter();
counter1();
counter1();

var counter2 = counter();
counter2();
```

Sample output:

```
1
2
1
```

This behavior is observed since each time we call `counter`, we get a fresh reference of the function being returned.

A more preferred way of creating a `Counter` would be using a constructor function.

Sample code:

```js
function Counter() {
  var count = 0;
  this.incrementCounter = function () {
    count++;
    console.log(count);
  };
  this.decrementCounter = function () {
    count--;
    console.log(count);
  };
}
var counter1 = new Counter();

counter1.incrementCounter();
counter1.incrementCounter();
counter1.decrementCounter();
```

Sample output:

```
1
2
1
```

**A disadvantage of closures:**

There could be an over consumption of memory in closures. This happens since the references enclosed in closures are not garbage collected and if not handled properly, might lead to memory leaks.

**What is a garbage collector and what does it do?**

Garbage collector is like a program in the browser or in the JavaScript engine which frees up the un-utilized memory. In languages like **C** and **C++**, it's up to the developers to allocate and de-allocate memory, but in high level languages like JavaScript, the garbage collector frees up the memory by releasing space attached to data members which are not in use, meaning when they are no longer needed.

**Relationship between garbage collector and closures**

Sample code:

```js
function a() {
  var x = 0;
  return function b() {
    console.log(x);
  };
}
var y = a();
y();
```

In the above code, `x` is enclosed with `b` and `b` is being returned and stored in `y`. The means that the garbage collector is unable to release the memory allocated to `x`, since the function stored in `y` can be invoked anywhere in the code, which in turn has a dependency on the variable `x`.

But in some modern browsers and engines, for instance V8 in Chrome, have smart garbage collection mechanisms where if it finds out that somehow this variable is unreachable then it smartly collects the garbage variables. For example, if we had another variable outside `b` and inside `a` alongside `x`, then once the function `b` was returned, then any variable other than `x` would be garbage collected in V8 of Chrome. This happens since it's clear from the code that `b` has a dependency on only `x` for the moment being, and unless we use any other variable from `b`'s lexical parent, there's no other dependency on its lexical parent.

---

### First Class Functions

- What is an Anonymous function?
- What is a First Class Function?
- Function Statement vs. Function Expression vs. Function Declaration

```js
// Function Statement AKA Function Declaration
function a() {
  console.log("a called");
}

// Function Expression
// [Functions in JavaScript act like values which we can assign to variables]
var b = function () {
  console.log("b called");
};

// Anonymous Function
// A function without a name
// The following will result in a syntax error.
// function () {
// }

// Named Function Expression
var b = function xyz() {
  console.log("b called");
};

// Difference between Parameters & Arguments
function fun(param1, param2) {
  console.log(param1, param2);
}
fun(arg1, arg2);
```

**Function Statement vs. Function Expression**

The major difference between these 2 is hoisting.

In the above code block, we have `a()` and `b()`. If we call `a()` before it's declaration, it's valid, but if we do the same for `b()`, we get a `TypeError` stating that `b is not a function`.

During the hoisting phase, during the memory creation phase, `a` is allocated some memory and the function definition of `a` is assigned to it. But, in case of our function expression, `b` is treated like another variable. `b` is assigned undefined initially until the code hits the line of `b`'s initialization.

**Anonymous Functions**

An anonymous function doesn't have its own identity.

The following will result in a syntax error.

```js
function () {

}
```

We'll face the following issue in the above code.

```
Uncaught SyntaxError: Function statements require a function name
```

Anonymous Functions don't have a name, but according to the ECMAScript specification, every function should have a name. Hence the above is an invalid syntax.

_**Anonymous functions** are used in places where functions are **used as values**._

**First Class Functions**

The ability to use functions as values is known as First Class Functions. Functions are First Class Citizens in JavaScript. The concept of First Class Functions and First Class Citizens is the same.

---

### Callback Functions

**What is a callback function in JavaScript?**

Functions are First Class Citizens in JavaScript. That means that we can take a function and pass it to another function. When we do so, this function which we've passed to another function is known as a callback function.

But, why the term **callback**? The reason being that we give the responsibility of calling our callback function to another function. It's up to that function, in which we've passed our callback, to decide when to invoke the callback.

**Blocking the main thread**

The **Call Stack** in JavaScript is AKA the **Main Thread**. Everything, whatever is executed inside your page, is executed through the **Call Stack** only.

If any operation blocks this **Call Stack**, then that is known as _blocking the main thread_.

Hence, we should always use `async` operations for anything that takes time and hence unblocks the **Call Stack**.

A niche example will be the `setTimeout` function.

Hence, this means that if JavaScript didn't have the concept of **First Class Citizens** and **Callback Functions**, then we wouldn't be able to perform `async` operations.

**Event Listeners**

Suppose we have a button with an id of `clickMe`, and we have the following event listener for the same.

```js
document.getElementById("clickMe").addEventListener("click", function xyz() {
  console.log("Botton Clicked");
});
```

So, here `xyz()` is our callback function. Although, we didn't need to name our function and could've kept it anonymous, this just helps to provide better visibility in the dev tools.

So, this function `xyz()` will be pushed to the call stack whenever we receive the `"click"` event.

But, suppose we want to add a counter of the frequency of clicks and log the same, we can achieve that by using a global variable, but that's not a conventional solution.

Instead, we can take the help of a closure here.

```js
function attachEventListener() {
  let count = 0;
  document.getElementById("clickMe").addEventListener("click", function xyz() {
    console.log("Button clicked", ++count);
  });
}
attachEventListener();
```

Now, once we run the above code, the callback function basically remembers the reference to `count` and remembers where it is present.

If we click the button 3 times, we'll get the following output:

```
Button Clicked 1
Button Clicked 2
Button Clicked 3
```

If we analyze the button in our HTML using the dev tools, we can see something like this:

![event-listener-and-handler-dev-tools.png](https://i.ibb.co/QvrbfPY/event-listener-and-handler-dev-tools.png)

Now, the `handler` which we see in the image is our callback function `xyz()`.

If we dig a little deep, we see that we can also see the `Scopes` of this `handler`.

![scopes-of-handler-dev-tools.png](https://i.ibb.co/YNQG144/scopes-of-handler-dev-tools.png)

This is the same scope that the function carries, the same lexical scope. Inside the scope, we'll have the `Closure`. So, we see that the `Global` environment and parent environment are all bundled up together in the lexical scope of the `handler`.

**INTERVIEW QUESTION**

> **_Why do we need to remove event listeners?_**

> Event listeners are heavy, meaning they take up a chunk of memory. Whenever we attach event listeners in our code, even if the **Call Stack** might be empty, the memory is still allocated to our event listeners and we can't simply free up this memory.

> This is the reason why we need to remove event listeners when we're not using them. Suppose our page has quite a bit of buttons with numerous event listeners attached, then our page might become much slower due to all the closures consuming a lot of memory for all the scopes and callback functions. If we remove our event listeners, then all the unnecessary data members will be garbage collected.

---

### Asynchronous JavaScript & Event Loop

So, JavaScript is a synchronous single threaded language. It has one call stack and it can only do one thing at a time. This call stack is present inside the JavaScript engine, and all the code is executed inside this call stack.

Whenever any JavaScript program is run, a **GEC** (Global Execution Context) is created and is pushed inside the call stack.

In case of any function invocation, a new Execution Context is created for that function and is pushed in the call stack. After executing all the statements of this invoked function, the execution context of this function is then popped from the call stack.

If we have more function invocations, more execution contexts will be created and the process of execution follows and the execution context is popped from the call stack once execution of all statements comes to completion.

Also, once we come to the end of our JavaScript program, the statements in the global scope also reach to an end, which means that after execution of all the statements in our global scope, the **GEC** (Global Execution Context) is also popped from the call stack.

So, the call stack of the JavaScript engine doesn't wait. Once something is pushed in this call stack, it quick executes it until all the statements are done with and the execution context is popped from the call stack.

Now, the call stack resides in the JavaScript engine and this JavaScript engine resides in another environment, which can typically be a browser in case of any web application. Many a times, the code needs to interact with elements which the browser has access to. This can be the local storage, bluetooth, timer, address bar, the HTML being rendered, etc.

The code does so by the help of web APIs.

Some of them would be:

- setTimeout()
- DOM APIs
- fetch()
- localStorage
- console
- location

**`setTimeout()` is not a part of JavaScript.**

_Even the DOM APIs are not a part of JavaScript_.

**_All the above mentioned web APIs are actually not a part of JavaScript. Instead they are the part of the browser._**

It is the browser which gives the JavaScript Engine access to all these web APIs.

Now, we're able to access these functionalities and execution follows in the call stack, all because of the global object.

In case of browsers, this global object is the `window`.

The reason we don't need to access the web APIs through the `window` keyword is due to the fact that the `window` is the global object. `window.setTimeout` is the same as `setTimeout`.

The browser wraps up the web APIs into the global object, `window` in this case, and hence provides access of these APIs to the JavaScript Engine.

**How `setTimeout` works? How does the callback function enter the call stack?**

The callback function of `setTimeout` enters the call stack through something called the **Callback Queue**. When we add a callback function to `setTimeout`, the callback function gets registered inside the web APIs environment with a timer. When the timer expires, the callback function is put inside this **Callback Queue**. We also have something called the **Event Loop**. The job of the **Event Loop** is to check this **Callback Queue** and put the functions of the **Callback Queue** into the call stack, and the call stack quickly executes.

**How does the callback function work in case of event listeners?**

When we add an event listener, the callback function is registered in the web APIs environment along with an event (`"click"` for example). Whenever this event occurs, the callback function is put inside the **Callback Queue** and it waits for its turn to get executed.

Now comes the **Event Loop**. The **Event Loop** has only one job. It's only job is to continuously monitor the **Call Stack** and the **Callback Queue**. If the **Event Loop** observes that the **Call Stack** is empty and also sees that there's something inside the **Callback Queue**, waiting to be executed, it takes that function and pushes it to the **Call Stack**.

**So, why do we need both the Event Loop and the Callback Queue?**

We need them for the fact that the **Callback Queue** helps us to manage the order of execution of the callbacks. The **Event Loop** on the other hand monitors the **Call Stack** and takes something from the **Callback Queue** only when it sees that the **Call Stack** is **_empty_**.

**How does fetch work?**

`fetch` is used to request an API call. The `fetch` function returns a promise. A callback is executed once this promise is resolved.

Now, we have something called the **Microtask Queue**, _which has higher priority over the_ **Callback Queue**, which implicitly means that whatever comes in the **Microtask Queue** will be executed prior to the elements of the **Callback Queue**.

In case of promises, in case of network calls using `fetch`, once the promise is resolved, the callback enters the **Microtask Queue**.

The **Event Loop** also monitors the **Microtask Queue** and hence pushes a callback from this queue to the **Call Stack** once it sees that the **Call Stack** is vacant.

Now, all callback functions which are registered against promises, will enter the **Microtask Queue**.

Also, we have something called the **Mutation Observer**, and callbacks against this also enters the **Microtask Queue**. The **Mutation Observer** keeps on checking whether there's a mutation in the **DOM** tree or not and executes a callback on the basis of that.

Hence, callbacks against **Promises** and **Mutation Observers** enter the **Microtask Queue**, where as all other callbacks enter the **Callback Queue**.

The **Callback Queue** is AKA the **Task Queue**.

**Starvation of Callback Queue**

Suppose we have callbacks lying in both the **Microtask Queue** and **Callback Queue**, and the **Microtask Queue** is adding more **Microtasks** to its queue. In such a scenario, the callbacks in the **Callback Queue** might never get a chance to get executed. This is known as **Starvation** of functions/tasks in the **Callback Queue**.

---

### JS Engine - Google's V8 Architecture

**JavaScript Runtime Environment** is like a container with capabilities required to execute JavaScript code. The **JavaScript Engine**, **APIs** to connect to the outer environment, **Event Loop**, **Callback Queue** and **Microtask Queue**, are all part of the **JavaScript Runtime Environment**. This Environment is not possible without the **JavaScript Engine**. Hence, the **JavaScript Engine** basically forms the heart of this environment.

Every web browser has a **JavaScript Runtime Environment**.

**Node.js** also provides a **JavaScript Runtime Environment**.

The **APIs** to connect to the outer environment differs from environment to environment. Also, the **APIs** which are common, like `setTimeout` for instance, are implemented differently for every **JavaScript Runtime Environment**.

Also, every browser uses it's own **JavaScript Engine**. **V8** for instance is used in Google Chrome, Node.js, Deno and V8.NET.

Anyone can create their own JavaScript Engine, the catch is to follow the protocols as portrayed in the **ECMAScript language standards**.

The world's first JavaScript engine was created by the creator of JavaScript himself, and has evolved to what we call today as **SpiderMonkey**, which is the JavaScript Engine used in Firefox.

**JS Engine is not a machine.**

JavaScript Engine is just like another program, written in a low-level language. For instance, V8 is written in C++.

**JS Engine Architecture**

The JavaScript Engine accepts the JavaScript code as input and this code goes through 3 major steps.

- Parsing
- Compilation
- Execution

During this **Parsing** phase, the code which we write is broken down into tokens.

We also have something called the **Syntax Parser**. The job of the **Syntax Parser** is to take the code and convert it to an **AST** (Abstract Syntax Tree). The **AST** is a detailed **JSON** which represents the tokens of our code in a tree-like structure.

This **AST** is then passed into the compilation phase.

Also, we can visualize **AST** using this tool -> [astexplorer.net](astexplorer.net)

The compilation and execution of JavaScript code goes hand in hand.

JavaScript has something called as **JIT** (Just In Time) **Compilation**.

JavaScript can behave as both an interpreted language and a compiled language, which actually depends on the **JavaScript Engine**.

When JavaScript was initially created, it _was supposed to be an interpreted language_.

But, in today's world of modern browsers, both interpretation and compilation goes hand in hand.

**JIT Compilation** uses best of the both worlds. It uses an interpreter as well as a compiler to execute JavaScript code.

Now, the **compilation** and **execution** steps go hand in hand.

After **parsing**, we received the **AST**. The **AST** goes to the interpreter and the interpreter converts the high-level code into byte code, which moves on to the **execution** step. Also, while it is doing so, the interpreter takes the help of the compiler to optimize the code. While the code gets interpreted line by line, the compiler also tries to optimize the code as much as it can. So, it's not a single phase process, but can happen in multiple phases. Every JavaScript Engine has its own algorithm of doing it. So, the job of the compiler is to optimize the code as much as it can on the runtime, that's why it's called **JIT** (Just In TIme) **Compilation**.

In some JavaScript Engines, there's something called as **AOT** (Ahead Of Time) **Compilation**. In that case the compiler basically take a piece of code, which is going to be executed later, and tries to optimize it as much as it can. And it also produces the byte code which goes to the **execution** phase.

This **execution** is not possible without 2 major components: the **Memory Heap** and the **Call Stack**.

The **Memory Heap** is where all the memory is stored. It is constantly in sync with the **Call Stack**, the garbage collector, and many other things. This is the place where all the data members (like variables and functions) are assigned memory. The garbage collector tries to free up memory space whenever possible.

The garbage collector works on the basis of the **Mark & Sweep Algorithm**.

Also, there are many forms of optimizations which a compiler does for the code. Some of which are:

- inlining
- copy elision
- inline caching
- etc...

All this information is very generic, as every JavaScript Engine has its own way of doing things.

At this instance, Google's V8 Engine is considered to be the fastest among all JavaScript Engines ever created.

The V8 Engine has an interpreter called **Ignition** and a compiler called **TurboFan**.

The V8 Engine Architecture can be visualized as follows:

![v8-engine-architecture.png](https://i.ibb.co/VQW0Qd1/v8-engine-architecture.png)

Also, the garbage collector in the V8 Engine is called **Orinoco**, which uses the **Mark & Sweep Algorithm**.

It also has another garbage collector called **Oilpam**, which is used for a different purpose.

---

### Trust issues with `setTimeout()`

The `setTimeout()` has some issues. A `setTimeout()` with a timer of 5 seconds doesn't always exactly wait for 5 seconds.

This depends on the **Call Stack**.

Suppose our **Call Stack** is busy with the **GEC** (Global Execution Context), and hence is not vacant, then the **Event Loop** is unable to push any callback from the **Callback Queue**. This is also what we refer to as the **Concurrency Model** in JavaScript.

So, this forms the basis of **not blocking the main thread**.

We can block our main thread by simulating an environment by simply creating a while loop which would run for 10 seconds.

```js
let startDate = new Date().getTime();
let endDate = startDate;
while (endDate < startDate + 10000) {
  endDate = new Date().getTime();
}
```

It is also due to this **Concurrency Model** of JavaScript that we are able to perform asynchronous operations.

---

### Higher-Order Functions

A function which takes _another function as an argument or returns a function from it_, is known as a **Higher-Order Function**.

A proper example of an **Higher-Order Function** would be the following code snippet, where we are calculating area, circumference and diameter of 4 circles.

```js
const radius = [3, 1, 2, 4];

const area = function (radius) {
  return Math.PI * radius * radius;
};

const circumference = function (radius) {
  return 2 * Math.PI * radius;
};

const diameter = function (radius) {
  return 2 * radius;
};

const calculate = function (arr, logic) {
  const output = [];
  for (let i = 0; i < arr.length; i++) {
    output.push(logic(arr[i]));
  }
  return output;
};

console.log(calculate(radius, area));
console.log(calculate(radius, circumference));
console.log(calculate(radius, diameter));
```

So, as given in the above code, `area`, `circumference` and `diameter` are our callback functions, and `calculate` is our higher-order function.

**Pollyfill for map function in JavaScript**

`map` is a very common higher-order function.

`console.log(calculate(radius, area));`

is equivalent to

`console.log(radius.map(area));`

So, technically `calculate` in the above code block is like our implementation of `map`, although it's a miniature implementation.

Suppose we replace, the following snippet

```js
const calculate = function (arr, logic) {
  const output = [];
  for (let i = 0; i < arr.length; i++) {
    output.push(logic(arr[i]));
  }
  return output;
};
```

with the following

```js
Array.prototype.calculate = function (logic) {
  const output = [];
  for (let i = 0; i < this.length; i++) {
    output.push(logic(this[i]));
  }
  return output;
};
```

Now, we'll be able to use the `calculate` function in the same manner as we did with the `map` function, i.e., like this

```js
console.log(radius.calculate(area));
```

We just put the function `calculate` in the `Array.prototype`. This way the function `calculate` will now be available on all the arrays.

---

### `map`, `filter` & `reduce`

`map`, `filter` & `reduce` are higher-order functions in JavaScript.

#### `map`

The `map` function is basically used to transform an array. Transformation means to operate on each of the elements of the array and get a new array out of it.

We have to pass in a function inside `map`, a function which basically tells what function do we need, what transformation logic do we need to apply on each of the elements.

```js
const arr = [5, 1, 3, 2, 6];

// returns double of a number
function double(x) {
  return x * 2;
}

// returns triple of a number
function triple(x) {
  return x * 3;
}

// returns binary of a number
function binary(x) {
  return x.toString(2);
}

console.log(arr.map(double));
console.log(arr.map(triple));
console.log(arr.map(binary));
```

Now

```js
arr.map(double);
```

is equivalent to

```js
arr.map(function double(x) {
  return x * 2;
});
```

which in turn is equivalent to

```js
arr.map((x) => {
  return x * 2;
});
```

which, yet again, is equivalent to

```js
arr.map((x) => x * 2);
```

#### `filter`

The `filter` function is used to filter the values inside an array.

```js
const arr = [5, 1, 3, 2 6];

function isOdd(x) {
  return x % 2;
}

function isEven(x) {
  return x % 2 === 0;
}

function greaterThan4(x) {
  return x > 4;
}

console.log(arr.filter(isOdd));
console.log(arr.filter(isEven));
console.log(arr.filter(greaterThan4));
```

#### `reduce`

`reduce` function is basically used at a place where you have to take all the elements of an array and come up with a single value out of them.

It takes a callback function with 2 parameters, an **accumulator** and a **current**. When the `reduce` iterates over the elements, the **current** points to each of the elements of the array. The **accumulator** is used to accumulate the result of what we need to reduce out of these values. Also, the `reduce` function takes in a second parameter, which serves as the initial value of the **accumulator**.

Suppose we have an array of numbers and we want to find the sum of the elements. We can use the `reduce` function as follows.

```js
const arr = [5, 1, 3, 2 6];

const output = arr.reduce(function (acc, curr) {
  acc = acc + curr;
  return acc;
}, 0);
```

So, the first parameter to our `reduce` function is the callback, which has 2 parameters, the first one being the **accumulator** `acc` and the second being the **current** `curr`. This function has the accumulation logic, which would be used to reduce the result. Also, we have a second parameter to the `reduce` function, which is the initial value for our **accumulator**, `0` in this case.

Suppose we want to find out the max from our array. We can use `reduce` as follows.

```js
const arr = [5, 1, 3, 2, 6];

const output = arr.reduce(function (max, curr) {
  if (curr > max) {
    max = curr;
  }
  return max;
}, 0);
```

**[NOTE:** We've initialized our **accumulator** to `0` here since our array only has +ve values.**]**

#### Some real world use cases

Let's take the following array.

```js
const users = [
  { firstName: "akshay", lastName: "saini", age: 26 },
  { firstName: "donald", lastName: "trump", age: 75 },
  { firstName: "elon", lastName: "musk", age: 50 },
  { firstName: "deepika", lastName: "padukone", age: 26 },
];
```

Suppose we want an array with full names. We can use the `map` function like shown below.

```js
const output = users.map((x) = > x.firstName + " " + x.lastName);
```

Suppose we want to create an object with the age as the key and its value as the frequency of the number of people with that age. In such a case, we case use the `reduce` as follows.

```js
// acc = {26: 2, 50: 1, 75: 1}
const output = users.reduce(function (arr, curr) {
  if (acc[curr.age]) {
    acc[curr.age] = ++acc[curr.age];
  } else {
    acc[curr.age] = 1;
  }
  return acc;
}, {});
```

Now, suppose we want the first names of all people with age < 30. We can do something like this.

```js
const output = users.filter((x) => x.age < 30).map((x) => x.firstName);
```

#### Challenge

Instead of chaining the `filter` and `map` function to achieve the above result, use only `reduce`.

**Solution**

```js
const output = users.reduce(function (acc, curr) {
  if (curr.age < 30) {
    acc.push(curr.firstName);
  }
  return acc;
}, []);
```

---

SOURCE: [Namaste 🙏 JavaScript by Akshay Saini]
