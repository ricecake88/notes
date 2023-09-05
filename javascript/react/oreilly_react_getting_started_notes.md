# From the O’Reilly Course I took React: Getting Started by Samer Buna

https://jscomplete.com/react-beyond-basics (which addresses the above questions)

Components should not have this much responsibility. Whole app should not depend on a library like axios.

One should have a small agent-type module that has one responsibility - to communicate with external APIs and make your code only depend on that agent module.

## O’Reilly Course React: Getting Started GitHub Card Basic App

*Using Class Components:*

```reactjs
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
```

*Using Hooks*

```react
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
```