[Typescript Set up](#typescript-set-up)  
[Create a React App Using TypeScript](#create-a-react-app-using-typescript)  
[Install React Router](#install-react-router)  
[Install Redux  Dependencies](#install-redux-dependencies)  
[Install Typescript Redux Helper Library](#install-typescript-redux-helper-library)
[If I want to install redux and thunk](#if-i-want-to-install-redux-and-thunk)  
[Install axios (for node)](#install-axios-for-node)  
[Inference](#inference) 

# . Typescript set up

With node, install type script for all users (global)
    `npm install -g typescript`

If you want to compile it from command line (which will create a js file):
    `tsc <file.ts`

If you want to compile and run, make sure `ts-node` is installed.
    `ts-node <file.ts>`

# Create a React App Using Typescript

    npx create-react-app <name_of_project> --template typescript

# Install React Router

	npm i react-router-dom @types/react-router-dom

# Install Redux Dependencies

    npm i -D @types/react @types/react-dom @types/react-redux

# Install typescript redux helper library

    npm install -D typesafe-actions

# If I want to install redux and thunk

    npm install redux redux-thunk react-redux

# Install axios (for node)
	npm -i axios @types/axios

# Inference
Typescript can detect what type an assignment is when a variable is set to a value.
Example:
```typescript
let name = 'Grace'
let isTrue = true
let numOfApples = 3
```
In the above assignments, Typescript can detect that `name`, `isTrue`, `numofApples`, is associated with a `string`, `boolean`, and a `number` respectively.

In the instances where you a variable is empty or has no value, the type is then set as `any`. If you know what type that variable will eventually be used for, then the type should be specified.

Example:
```typescript
let name : string;
```
In the instance of `const`s, since they cannot be changed (when they are not objects), the variables are assigned directly to a specific value.

Example:
```typescript
const name = 'Grace'
const isTrue = true
const numOfApples = 3 
```
`name` is simply just set to `Grace`, and is not associated with type `string`. The same can be said of `isTrue`, set to `true`, and `numOfApples` set to `3`, where they are not associated with `boolean` nor `number`. and `isTrue` is simply set to always `true` and `numOfApples` will always be 3.

# Javascript Object vs Type for Objects
In the following example:
```typescript
const you = {
	userName: 'Bobby',
	isReturning: true,
}
```
the type for the object is:
```typescript
const you: { userName: string; isReturning: true; }
```
which looks very similar to a Javascript object. So make a note of this that this is NOT a Javascript object.

# Arrays
Typescript has objects with type 'Array' whereas Javascript does not recognize an Array as an array, it recognizes it as an object.

To specify a type of an Array of strings, then to declare a variable as an array of strings, then the syntax would be as follows:
`deliciousRestaurants: string[]`

To specify a an Array of objects, the syntax would read as (for example):
```typescript
const reviews: {
	name: string;
	stars: number;
	loyaltyUser: boolean;
	date: string
}[] = [....]
```
# Union Types
If I wanted an array that have a mix of strings and numbers, to declare a variable as a union then the syntax would be as follows:
`datesStayed: (string | number)[]`

# Tuple Type
Expresses an array in which it has a fixed number of elements with their types known at which index.

**Example**:
Tuple of: `[string, string, number]`
corresponds or matches with an array of:
`['hello', 'bye', 88]`
and would fail to match:
`['wow', 22]`
It has to match exactly.

If one were to use it in an object to specify the tuple type, say for example you wanted a customer contact to provide both an email address and a phone number, you could specify this tuple in the object:
```ts
{
	...
	contactName: [string, number],
	...
}
```

# Enums
An enum is a way of giving more friendly names to sets of numeric values.

Enums can be used when we want to be very specific about what strings a variable can take.

For example:
In permissions, we only want specific strings between `admin` and `read_only` permissions. If we set the following:
```ts
const ADMIN = 'admin'
const READ_ONLY= 'read_only'

let permissions = ADMIN
```
`permissions` appear to be able to be set to any string. But we want it to be set to only either `ADMIN` or `READ_ONLY` and not any other string.

enums allow us to group like variables together (like in C). So we would change the above code to:

```ts
enum Permissions {
	ADMIN = 'admin',
	READ_ONLY = 'read_only'
}

let permissions = Permissions.ADMIN
```
# Literal Types
One can use an alias type - it creates a new name that refers to new types.

Example:
```ts
type Price = 35 | 40 | 45
type Countries = 'Poland' | 'United Kingdom' | 'Columbia

let price: Price
let country: Countries
```


# Sort Function
Sorting an array of objects that have a date (as type string)

```typescript
var compareDate = function (emp1: Review, emp2: Review) {
	var emp1Date = new Date(emp1.date).getTime();
	var emp2Date = new Date(emp2.date).getTime();
	return emp1Date > emp2Date ? 1 : -1;
}
arraysOfSomething.sort(compareDate);
```

# Function Types
Syntax example:

Writing a function like the following add, Typescript already infers from the below function that it will be returning a number.
```ts
function add ( firstValue: number, secondValue: number ) {
	return firstValue + secondValue
}```

To explicity return a variable, one can modify the syntax to look like the following:
```ts
function add ( firstValue: number, secondValue: number) : number {
	return firstValue + secondValue
}
```
If we want to return undefined from a function, we would want to `return undefined` as the last line in the function. Otherwise we would have to modify the function to specifically return `void` as the type that it is return such as below:
```ts
function sayHello(name: string) : void { 
	console.log("Hi", name)
}
```
```ts
function makeMultiple(value: number) {
	if (value > 1) {
		return 's'
	}
}
```

# Classes
The syntax for writing a class in Typescript is as follows:
```ts
class Class {
	property: type;
	property2: type;
	constructor(nameOfProperty: property, nameOfProperty2: property2) {
		this.nameOfProperty = nameOfProperty;
		this.nameOfProperty2 = nameOfProperty2;
	}
}
```