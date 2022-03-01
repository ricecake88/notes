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

