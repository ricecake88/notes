# Javascript Weirdness

## Variables & Block Scopes

In a block

```js
for (var i = 0; i <= 10; i++) {

// block scope

}
```

The var of i can still be accessed outside of the function. (Which is really weird!) as the above is a block scope.

To remedy this, const and let should be used instead:

```js
for (let i = 0; i <= 10; i++) {

// block scope

}
```

Nested block scopes, to not be able to access in outside nested block scopes, should have the variables declared as const and let.

Variables declared as const cannot have their REFERENCE modified. However, they are NOT immutable, and therefore the value can be changed.  However if the variable defined with const is a SCALAR one, like a string or an integer, they can be thought of as mutable.

### What is a Scalar?

(Scalar = A scalar variable, or scalar field, is a variable that holds one value at a time. It is a single component that assumes a range of number or string values. A scalar value is associated with every point in a space. In computing, the term scalar is derived from the scalar processor, which processes one data item at a time.)

The term "scalar" comes from linear algebra, where it is used to differentiate a single number from a vector or matrix. The meaning in computing is similar. It distinguishes a single value like an integer or float from a data structure like an array. This distinction is very prominent in Perl, where the $ sigil (which resembles an 's') is used to denote a scalar variable and an @ sigil (which resembles an 'a') denotes an array. It doesn't have anything to do with the type of the element itself. It could be a number, character, string, or object. What matters to be called a scalar is that there is one of them.

What does this mean?

So `const greeting = ‘merry’;` cannot be modified to `greeting = ‘yes’;``

This will throw an error, as will `const greeting = 12;` and `greeting =13`;

BUT, if `const greeting = [];` The elements can be modified inside. So consts that are declared as arrays and objects, the contents are MUTABLE.

### Const vs let?

With using const and setting it to a value, user won’t have to trace back to see if the variable changed. With a let declaration, the user will have to trace back to see if the value changed.

Use let for counters is a good choice.

Arrow Functions

### Closures

Best description of a closure:

[https://medium.com/@prashantramnyc/javascript-closures-simplified-d0d23fa06ba4](https://medium.com/@prashantramnyc/javascript-closures-simplified-d0d23fa06ba4)

An arrow function does not care who calls it. `this` is bound to the parent/scope. An arrow function will close over the value of the function of the `this` keyword for its scope at the time it was defined. (under the scope that defined the function.)

A regular function binds `this` to its caller.

What makes an arrow function so great (and is my preference) is that this can still have access to the element when you need a delay such as for event listeners, and not the calling environment.

# Templating

Template strings in Javascript are indicated by ${}.

Example:

```js
Const html = `

<div>${Math.random()}</div>

`;
```

Output will be

```html
<div>0.4767816436886543</div>
```

# Asynchronous

Look into MacroTasks vs MicroTasks

Understanding the Event Loop:

[https://www.youtube.com/watch?v=8aGhZQkoFbQ](https://www.youtube.com/watch?v=8aGhZQkoFbQ)

Dakota Martinez explaining async vs sync

[https://www.youtube.com/watch?v=xONU7lUzIRM](https://www.youtube.com/watch?v=xONU7lUzIRM)

## What is a Promise?

A promise is an object that might deliver data at a later point in the program.

The web fetch returns a promise. When a promise is returned, a “.then” is appended, and handles information based on the information that has been returned from the promise.

Example:

```js
const fetchData = () => {

    fetch(‘[https://api.github.com](https://api.github.com)’).then(resp => { // returns the raw API object

    resp.json().then(data => { //convert the raw API object to json, which then returns the data in JSON format

        console.log(data); // print the JSON object

    }
}
```

Note that fetch() is an asynchronous method, as is json() so both return promises that need to be operated after on with .then()

## Async/Await

Modern method of  using

```js
const fetchData = async() => {

const resp = await fetch(‘[https://api.github.com](https://api.github.com)’);

const data = await resp.json();

   console.log(data);

}
```

fetchData itself will return a promise Object.

Resources

Samer Buren’s courses:

[https://jscomplete.com/learn](https://jscomplete.com/learn)

Closures:

[https://jscomplete.com/closures](https://jscomplete.com/closures)
