[What is React](#what-is-react)  
[Efficiency React Dom vs Browser Dom](#efficiency-react-dom-vs-browser-dom)
    [How Much?](#how-much?)
[Setting Up React](#setting-up-react)

# What is React

It is a JS library of user interfaces, built with reusable components. Only those elements that change what are displayed are updated.

React is a Model View Controller.

**Model** - manages the data and rules of the application (React component)

**View**- the output through the browser’s DOM (render)

**Controller**- takes user input and converts it into commands for the Model or View layers via click events or api requests.

**User -> Controller -> Model -> View -> User**

In some MVC setups the controller can modify the browser dom (view) directly, but React does not allow this to happen.

# Efficiency React Dom vs Browser Dom

Virtual DOM (React DOM) is much quicker than the Browser DOM

React uses a Diffing Algorithm  to find the differences based on two assumptions

Two elements of different types will produce different trees

The developer can hint at which child elements may be stable across different renders with a key prop.

When comparing two REACT DOM elements of the same type, React looks at the attributes of both, keeps the same underlying DOM node, and only updates the changed attributes. Works the same way for the style tag.

## How much?

Minimal difference can be calculated most optimally using an algorithm with O(n^3)

React uses optimizations to enhance their algorithm to O(n)

If a node is found to be different (meaning a different type or component) it will re-render the entire subtree.

React traverses the tree breadth first. This ensures that a node isn’t added to the update if one of its parents also needs to be updated

```
      X
    /   \
   A     B
  /|\   /|\
 C D E F G H
```

So it will start at X, then A, then B. If it finds B has been updated then it will just update B entirely (without using the diffing algorithm)

React DOM then creates a new virtual DOM, diffs it vs the old one, and creates a list of the minimum possible changes to the browser DOM.

# Setting Up React

## To Load Up React right within the webpage

```html
<script  
 src="https://cdnjs.cloudflare.com/ajax/libs/react/16.8.3/umd/react.development.js"  
 crossorigin  
 ></script>

<script  
 src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/16.8.3/umd/react-dom.production.min.js"  
 crossorigin  
 ></script>
```

These are the two libraries that are used, and need to be updated with the latest React features (so upgrade!) And these need to be listed first.

To use JSX you need an extra step: load Babel

```html
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

and load your scripts with the special text/babel MIME type:

```html
<script src="app.js" type="text/babel"><;/script>
```

Now you can add JSX in your app.js file:

```jsx
const Button = () => {  
 return <button>Click me!</button>  
}  
ReactDOM.render(<Button />, document.getElementById('root'))
```

Example:
index.html

```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
<title>Hello!</title>  
<meta charset="utf-8">  
<meta http-equiv="X-UA-Compatible" content="IE=edge">  
<meta name="viewport" content="width=device-width, initial-scale=1">  

<!-- import the webpage's stylesheet -->  
<link rel="stylesheet" href="/style.css">  

<!-- import the webpage's javascript file -->  
</head>   
<body>  
<h1>Hi there!</h1>  

<p>  
I'm your cool new webpage. Made by Flavio <a href="https://flaviocopes.com">flaviocopes.com</a>!  
</p>  

<div id="root">  

</div>  

<script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>

<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>

<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>

<script src="script.js" type="text/babel"></script>

</body>  
</html>
```

Script.js

```js
/* If you're feeling fancy you can add interactivity  
 to your site with Javascript */  

// prints "hi" in the browser's dev tools console  

console.log(React);  

const Button = () => {  
 return (  
 <button>  
 Click me!  
 </button>  
 )  
}  

ReactDOM.render(  
 <Button />,  
 document.getElementById('root')  
)
```

## Using Create-React-App

Install node.js. ([https://nodejs.org](https://nodejs.org/))

# Styling

Styling can be in between {}

For example:

<div style={{margin: ‘1em’, color: blue}}>Yes</div>

<div style={{ color: Math.random < 0.5 ? ‘blue’ : ‘green’}}</div>

Look for CSS options in

https://github.com/MicheleBertoli/css-in-js

CSS in JS

Handling Inputs

# Refs

Get reference to an element.

Example:

userNameInput = React.createRef();

handleSubmit = (event) => {

event.preventDefault();

console.log(

this.userNameInput.current.value

);

};

<form onSubmit={this.handleSubmit}

<input type=”text” placeholder=”Enter username” ref={this.usernameInput} required />

Or State Objects

Example:

state = {

userName: ‘’

};

<form onSubmit={this.handleSubmit}

<input type=”text” placeholder=”Enter username” value={this.state.userName} onChange={this.onChange} required />

The above is what I learned from Flatiron.

Retrieving from an API without using Redux

For example, if we need to retrieve a username from the github api, we can use axios instead of fetch. (fetch is native code). Axios is installed in the js playground.

 handleSubmit = async (event) => {

 event.preventDefault();

 const resp = await axios.get(`https://api.github.com/users/${this.state.newName}`);

 this.props.onSubmit(resp.data);

 }

When using a spread, and modifying just one element, the spread goes first, followed by the element. Example:

 addNewProfile = (profileData) => {

 console.log('App',profileData);

 this.setState(prevState => ({

  ...prevState,

 profiles: [

 ...prevState.profiles,

 profileData],

 }))

 }

Questions that need to be asked - Handling Errors

These concerns need to be addressed right away during an interview

1. What should the UI do if the user types in an invalid GitHub handle?

2. What should the UI do if the request of data fails over the network?

3. What should the UI do if the request is taking too long?

From the O’Reilly Course I took React: Getting Started by Samer Buna

https://jscomplete.com/react-beyond-basics (which addresses the above questions)

Components should not have this much responsibility. Whole app should not depend on a library like axios.

One should have a small agent-type module that has one responsibility - to communicate with external APIs and make your code only depend on that agent module.

O’Reilly Course React: Getting Started GitHub Card Basic App

Using Class Components:

const CardList = (props) => {  
 console.log(props.search)  
 return (<div>  
 {!props.search  
 ? props.data.map((profile, idx) => <Card key={idx} profile={profile}/>)  
 : props.data.filter(profile => {  
 return profile.name.toUpperCase().includes(props.search.toUpperCase())  
 }  
 ).map((profile,idx) => <Card key={idx} profile={profile} />  
 )}  
 </div>)  
}

class Card extends React.Component {  
render() {  
 const profile = this.props.profile;  
 return (  
 <div className="github-profile">  
  <img src={profile.avatar_url} />  
 <div className="info">  
 <div className="name">{profile.name}</div>  
 <div className="company">{profile.company}</div>  
 </div>  
 </div>  
 );  
 }  
}  

class FormSearch extends React.Component {  

 render() {  
 return (  
 <>  
 <label htmlFor="search">Search</label>  
 <input type="text" name="search" value={this.props.search} onChange={this.props.onChange}/>  
 </>  
 )  
 }  
}  

class FormAdd extends React.Component {  

 state = {  
 newName: ''  
 };  

 handleSubmit = async (event) => {  
 event.preventDefault();  
 const resp = await axios.get(`https://api.github.com/users/${this.state.newName}`);  
 this.props.onSubmit(resp.data);  
 this.setState({  
 newName: ''  
 })  
 }  

 onChange = (event) => {  
 this.setState({  
 newName: event.target.value  
 })  
 }  

 render() {  
 return (  
 <form onSubmit={this.handleSubmit}>  
 <input  
 type="text"  
 name="newName"  
 value={this.state.newName}  
 placeholder="Github username"  
 onChange={this.onChange}  
 />  
 <button>Add card</button>  
 </form>)  
 }  
}  

class App extends React.Component {  

 state = {  
 search: '',  
 profiles: []  
 }  

 onChange = (event) => {  
 this.setState({  
 [event.target.name]: event.target.value  
 })  
 }  

 addNewProfile = (profileData) => {  
 console.log('App',profileData);  
 this.setState(prevState => ({  
 ...prevState,  
 profiles: [  
 ...prevState.profiles,  
 profileData],  
 }))  
 }  

render() {  
 return (  
 <div>  
  <div className="header">{this.props.title}</div>  
 <FormSearch search={this.state.search} onChange={this.onChange}/>  
 <FormAdd data={this.state.profiles} onSubmit={this.addNewProfile}/>  
 <CardList data={this.state.profiles} search={this.state.search}/>  
 </div>  
 );  
 }  
}  

ReactDOM.render(  
<App title="The GitHub Cards App" />,  
 mountNode,  
);

# Using Hooks

//Github usernames: gaearon, sophiebits, sebmarkbage, bvaughn  

const CardList = (props) => {  
 console.log(props.search)  
 return (<div>  
 {!props.search  
 ? props.data.map((profile, idx) => <Card key={idx} profile={profile}/>)  
 : props.data.filter(profile => {  
 return profile.name.toUpperCase().includes(props.search.toUpperCase())  
 }  
 ).map((profile,idx) => <Card key={idx} profile={profile} />  
 )}  
 </div>)  
}  

function Card(props) {  
 const profile = props.profile;  
 return (  
 <div className="github-profile">  
  <img src={profile.avatar_url} />  
 <div className="info">  
 <div className="name">{profile.name}</div>  
 <div className="company">{profile.company}</div>  
 </div>  
 </div>  
 );  
}  

function FormSearch(props) {  
 return (  
 <>  
 <label htmlFor="search">Search</label>  
 <input type="text" name="search" value={props.search} onChange={props.onChange}/>  
 </>  
 )  
}  

function FormAdd (props) {  

 const [newName, setNewName] = useState("");  

 const handleSubmit = async (event) => {  
 event.preventDefault();  
 const resp = await axios.get(`https://api.github.com/users/${newName}`);  
 props.onSubmit(resp.data);  
 setNewName("");  
 };  

 return (  
 <form onSubmit={handleSubmit}>  
 <input  
 type="text"  
 name="newName"  
 value={newName}  
 placeholder="Github username"  
 onChange={(event) => setNewName(event.target.value)}  
 />  
 <button>Add card</button>  
 </form>);  
}  

function App (props) {  

 const [profiles, updateProfile] = useState([]);  
 const [search, setSearch] = useState("");  

 const onChange = (event) => {  
 setSearch(event.target.value);  
 }  

 const addNewProfile = (profileData) => {  
 const newProfile = [...profiles, profileData];  
 console.log('App',profileData);  
 updateProfile(newProfile);  
 }  

return (  
 <div>  
  <div className="header">{props.title}</div>  
 <FormSearch search={search} onChange={onChange}/>  
 <FormAdd data={profiles} onSubmit={addNewProfile}/>  
 <CardList data={profiles} search={search}/>  
 </div>  
 );  
}  

ReactDOM.render(  
<App title="The GitHub Cards App" />,  
 mountNode,  
);

# Tips

It is a good idea to keep computational logic away from the return statement. 

For example:

```html
 <div className="left">

 {(availableNums.length === 0) ?

 <PlayButton reset={reset} />

 : <StarsDisplay count={stars}/>

 }

 </div>
```

Do the following instead:

```react
const gameIsDone = (availableNums.length === 0) ? true : false;



 <div className="left">

 {gameIsDone ?

 <PlayButton reset={reset} />

 : <StarsDisplay count={stars}/>

 }

 </div>
```

# React-Router-Dom: Changes Needed In v6
[React-Router-Docs](#https://reactrouterdotcom.fly.dev/docs/en/v6/getting-started/overview#installation)

- Route component now uses element.
`<Route path={'/'} element={<Dashboard />} />`
Previously it was:
`<Route path={''/'} component={Dashboard} />`

- Link cannot be within BrowserRouter

# Resources

[2021 02 02 week34 react state exercise - YouTube](https://www.youtube.com/watch?v=Yj8AaeO1fAM&authuser=1)

[2021 02 09 week35 react lifecycle methods - YouTube](https://www.youtube.com/watch?v=sx26dEbE8_0&authuser=1)

[2021 04 06 react concepts to review for live coding - YouTube](https://www.youtube.com/watch?v=N_J9UHgzWJ0&authuser=1)

[What the heck is the event loop anyway? | Philip Roberts | JSConf EU - YouTube](https://www.youtube.com/watch?v=8aGhZQkoFbQ&authuser=1)

Huge Guidebook

[The React Handbook (freecodecamp.org)](https://www.freecodecamp.org/news/the-react-handbook-b71c27b0a795/#an-introduction-to-the-react-view-library)

Samer Buren’s courses

[jsComplete: Learn Modern Full-stack JavaScript with Node and React](https://jscomplete.com/learn)

**
