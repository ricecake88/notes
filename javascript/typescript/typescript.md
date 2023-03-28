**TABLE OF CONTENTS**

[[#Typescript set up]]
[[#Create a React App Using Typescript]]
[[#Install React Router]]
[[#Install Redux Dependencies]]
[[#Install typescript redux helper library]]
[[#If I want to install redux and thunk]]
[[#Install axios for node]]
[[#Inference]]
[[#Javascript Object vs Type for Objects]]
[[#Arrays]]
[[#Union Types]]
[[#Tuple Type]]
[[#Enums]]
[[#Literal Types]]
[[#Sort Function]]
[[#Function Types]]
[[#Function Type Definitions]]
[[#Optional Parameters]]
[[#Initial or Default Parameters]]
[[#Rest Parameters]]
[[#Classes]]
[[#Generics]]
[[#Extra Tips]]
[[#New Types In Typescript]]
[[#Narrowing]]
[[#Resources]]

# Typescript set up

With node, install type script for all users (global)
    `npm install -g typescript`

If you want to compile it from command line (which will create a js file):
    `tsc <file.ts`

If you want to compile and run, make sure `ts-node` is installed.
    `ts-node <file.ts>`

Setup a new typescript project by entering in the project folder and creating a `tsconfig.json` file
   `tsc --init`

To always look for changes in typescript files and to compile the code.
  `tsc -w`

# tsconfig.json

This is the configuration file that uses various different compiler options. [Here](https://www.typescriptlang.org/docs/handbook/compiler-options.html) is the full list. Most common ones:
* `noImplicitAny` - instructs compiler to raise errors on expressions and declarations with an implied `any` type.
* `noEmitOnError`
* `target` (specifies the ECMAScript target version for the Javascript file)
These can be used in command line, or placed directly in the config file.

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

### Function Type Definitions

```typescript

// A function that returns of type string
function PublicationMessage(year: number): string {
 return 'Date published: ' + year;
}

// this is the Function Type publishFunc. This "=>" indicates what type it returns. Not to be confused with Javscript function declarations
let publishFunc: ( someYear: number ) => string

publishFunc = PublicationMessage

let message: string = publishFunc(2021);

/*If the return type from publishFunc does not match the type of string on the left hand side, Typescript will throw an error */
```
The parameter names don't have to match up. As long as the return type is the same, that is all that matters.

function overloads

### Optional Parameters

Optional Parameters must _follow_ required parameters. And they are denoted with a question mark after the variable name.
Example:
```Typescript
function CreateCustomer(name: string, age?: number, city?: string): void {
	console.log('Creating Customer' + name);

	if (age) {
		console.log('Age: ' + age);
	}

	if (city) {
		console.log('City: ' + city);
	}
}
```

### Initial or Default Parameters

```Typescript
function GetBookTitlesByCategory(categoryFilter: Category = Category.Fiction) : Array<string> {
	console.log('Getting books in category: ' + Category[categoryFilter]);

	const allBooks = GetAllBooks();
	const filteredTitles: string[] = [];

	for (let currentBook of allBooks) {
		if (currentBook.category === categoryFilter) {
			filteredTitles.push(crrentBook.title);
		}
	}
	return filteredTitles;
}

let poetryBooks = GetBookTitlesByCategory(Category.Poetry);
let fictionBooks = GetBookTitlesByCategory();
```

Can also set default parameter to a function
```Typescript
function LogFirstAvailable(book = GetFirstBook()) {
	...
}
```
### Rest Parameters

Rest Parameters can be used to indicate multiple paramters being passed in by comma. The example below passes a name of type string, and also `bookIDs` which will be of type number[ ]
Example:
```Typescript
fnction GetBooksReadForCustomer(name: string, ...bookIDs: number[])
```

Use:
```Typescript
let books = GetBooksReadForCustomer('Leigh', 2, 5)
let books = GetBooksReadForCustomer('Daniel', 2, 5, 12, 42)
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

### Private vs Public vs Protected

https://stackoverflow.com/questions/38713052/understanding-public-private-in-typescript-class

## Generics

## Extra Tips

If I want to change the values of an object into a string without caring about order:
```
const stringAddress = (addressInfo: Address): string => {

const addressStr: string[] = [];

for (const [key, val] of Object.entries(addressInfo)) {

	if (!!key) {

		addressStr.push(val);

	}
}
 

return addressStr.filter(Boolean).join(', ');

};
```


# New Types In Typescript
T is type or interface
K is key of a property

Record
Omit
Pick
Exclude

# Narrowing
[Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html) is when typescript reads your code, and rules out certain type possibilities because of conditions in your code. This is common when you have a _union_ type. Any time you're performing an operation in an `if` statement, or a `switch` you might be leveraging _narrowing_.

## Union of Literals
Setting types to be restricted to a set of strings or values
Example:
`type Actions = "create" | "update" | "delete"`

## typeof-utility
typeof is a utility to get the type of a variable
Example:
`typeof val`

## keyof utility
Use the keys of an object as your types.

So you have an Object:
```
{
	name: string,
	age: number
}
```

`keyof { name: string, age: number}`

would then resolve to "name" | " age"

## generic constraints

place constraints on generics, the syntax would be like
`<T extends string>`

## function type generics

These assign the type from a parameter into subsequent parameters or into the return type,  and can be used to constrain parameter types.
`function returnMe<T>(a: T): T { return a }`

## indexed access types
construct an object type out of keys from another type, one can use indexed access types.

Syntax:
```
[key in SomeUnionType]: string
```

can also all for any key by doing: 
`{ [key: string]: string}

Example:
```typescript
type OptionalProps = "name" | "age" | "location"
type PartialUser = {[ key in OptionalProps]: string | number}

```

When you don't particularly care about the keys and just want it to set it to either string or number, the above example can be used.

Or you can say I don't care what the key is set to and set it to any. For example:
```typescript
type AnyKeyOptions = {[ k: string ]: any}
```

## conditional types
need to read more on

## infer
need to read more on

# Utility Types
## Partial
All parameters not required,

Syntax:
`Partial<Object>`

## Required
Opposite of Partial. Wants any **optional** properties required.

Syntax:
`Required<Object>`

## Pick
Syntax:
`Pick<Object, value_I_want | value2_I_want | ...>`

## Omit
Syntax:
`Omit<Object, value_I_dont_want | value2_I_don't_want | ...>`

## Union Intersections - Extract
Extract can be used to intersect the properties/types that exist in both types.

`Extract<'a'|'b'|'c', 'b'|'d'|'e'>`
will just return a type that has 'b'.

`type T1 = Extract<string | number | (() => void), Function>;

therefore, type T1 = () => void

## Exclude
Only takes members of both union types that do not exist in the other. The opposite of `Extract`

`Exclude<'a'|'b'|'c', 'b'|'d'|'e'>`
will give you a type that has 'a' | 'd' | 'e'

## Parameters
Parameters takes in a function and makes a type out of the parameters passed into the function.

Example:
```typescript
function updateUser(
	url: string,
	data: { name: string },
	config: RequestInit,
) {
	return fetch(url, {
		body: JSON.stringify(data),
		...config,
	});
}

type Url = Parameters<typeof updateUser>[0]; // url parameter of function
type Data = Parameters<typeof updateUser>[1]; // data parameter type
type Config = Parameters<typeof updateUser>[2]; // config parameter type
```

## ReturnType

ReturnType takes a function and returns the function type of that function. can pass an anonymous function, or something like `ReturnType<typeof nameOfFunction>

## Map
Takes an object of key list pairs and converts it to Map. Can set, remove new key pairs.

```ts
let myMap = new Map<string, string>([ ["key1", "value1"], ["key2", "value2"] ]);
```
Resource:
https://howtodoinjava.com/typescript/maps/
`
# Resources

Typescript Handbook
https://www.staging-typescript.org/docs/handbook/

Deeper Resources:
[Design Patterns with Typescript](https://refactoring.guru/design-patterns/typescript)
Book - Programming Typescript
Book - Effective Typescript

