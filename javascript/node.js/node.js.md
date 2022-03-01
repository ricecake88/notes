# What is Node.js?

It is a Javscript Runtime "version" so to speak that is built on Javascript and adds some features and puts into a different environment. It allows any machine to run javascript more than just on the browser (which was the original purpose).

Javascript uses V8 which is the javascript engine built by Google that runs Javascript in the browser.

What does an engine mean? An engine takes javascript code in the browser and takes the javascript browser and converts it into machine code.

node.js uses V8 then compiles the Javascript and outputs machine code. V8 is written in C++. Node.js takes that V8 codebase and adds different features such as working with your local file system. Those things are not possible in the browser. Node.js adds these JS features to V8's engine so that you can do that. Node.js does not run in the browser, only using vanilla V8. Once you install node.js, you can then you can use the extended v8 versions to run your javascript script on the computer. So they are ran directly without having to be ran through the browser.

# Node.js' Role in Web Development

- Run Server: Create Server & Listen to Incoming Requests
- Business Logic: Handle Requests, Validate Input, Connect to Database
- Responses: Return Responses (Rendered HTML with Dynamic Contents, JSON)
Alternatives:

Python with Flask, Django
PHP
Vanilla JS
Ruby on Rails
ASP.NET

## What is a Node Module?

It is a way to separate concerns for our applications.
App has database calls, interface and we will change different files for each of these files.

### Exporting Modules

To export a function in a module (ie. javascript file named `tutorial.js`), for example
`const sum = (num1, num2) => num1 + num2;`

This line will export the sum function to other functions:
`module.exports = sum`

To import the exports in another file (ie `app.js`):
`const tutorial = require(./tutorial);`

### Event Module

#### Event emitter
Brings event driven programming to nodejs.
Event module is built into node js.

The below example creates a new eventEmitter. When the 'tutorial' event is triggered, the following function is called

```node
const EventEmitter = require('events');
const eventEmitter = new EventEmitter();

eventEmitter.on('tutorial', () => {
	console.log('tutorial event has been emitted';
})```

To initiate the tutorial event:
```node
eventEmitter.emit('tutorial')';
```

If we wanted to modify the tutorial callback to take in parameters for example:
```node
eventEmitter.on('tutorial', (num1, num2) => {
	console.log(num1+num2);
})```
And to trigger the tutorial event:
```node
eventEmitter.emit('tutorial', 1,2);
```

Event Emitters can be extended from for use. 
Example:
```node
class Person extends EventEmitter {
 constructor(name) {
 super();
 this._name = name;
 }

 get name() {
	 return this._name;
 }
}

  

let pedro = new Person('Pedro');
let christina = new Person('Christina');

pedro.on('name', () => {
 console.log('My name is ' + pedro.name);
})

pedro.emit('name');
christina.on('name', () => {
 console.log('My name is ' + christina.name)
})

christina.emit('name');
```

EventEmitters are synchronous. The above output would print their console messages one after the other.

### Readline Module

The readline module performs various activities including reading input.

#### createInterface(INPUT, output, completer)

It takes in two streams (input and output) and creates a readline interface. The completer is used for autocompletion. When given a substring it returns `[[substr1, substr2, ...], originalsubstring]`

The completer can be run in async mode if it accepts two arguments.

**Example

const readline = require('readline'),
readline.createInterface(process.stdin, process.stdout)
# Commands

## Get the version of Node after installation

`node -v`


