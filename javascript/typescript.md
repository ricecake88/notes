# Typescript set up

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

# Interference
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
	name: string,
	stars: number,
	loyaltyUser: boolean,
	date: string
}[] = [....]
```
# Union Types
If I wanted an array that have a mix of strings and numbers, to declare a variable as a union then the syntax would be as follows:
`datesStayed: (string | number)[]`

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

