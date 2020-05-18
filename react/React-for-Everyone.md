# My Notes

## Setup

* use create-react-app to get a basic section
* [Create React App](https://github.com/facebookincubator/create-react-app)
* Install it with:
  npx create-react-app my-app
  cd my-app
  npm start
* npx is a newer option for npm when installing
* this will install all the dependencies, setup the site and run a development server.

* [great resource for React](https://reactjs.org/)

### File Structure

* public is where any publically accessible things go like images
* src is where our main working files are found

---

## Basic React Component

* index.js is our main file, we don't really need to edit this - this inserts out application into our index.html
* App.js is our starting file

```javascript
// First part imports our requirements
import React, { Component } from 'react'; // we're grabbing the base Component from React.
import logo from './logo.svg'; // could have also put it in public folder
import './App.css';

const welcome = 'Welcome to React';

class App extends Component {
  // here we extend the basic component
  render() {
    // Every component needs a render method that returns something
    return (
      // all this in the return is JSX syntax
      // have to use className rather than class in JSX
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">{welcome}</h1> // example of using a const from
          above // OR you could use the Welcome component from below instead of the
          h1 above
          <Welcome />
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}

// we always start by defining a class and extending the base react component
class Welcome extends Component {
  render() {
    const { text } = this.props;
    return <h1 className="App-title">{text}</h1>;
  }
}

export default App; // this exports the App component for use
```

---

## Using Props

* a prop is a property on a component.
* is the way components share data between them, talk to one another
* components general talk in a one way direction, they drill down
* anything can be passed into a prop
* below we are passing a prop from a parent to a child - App to Welcome
* generally data is passed down from a parent the children
* you are able to do more javascript in a component after the render but before the return

```javascript
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <Welcome text="Welcome to React" /> // defined a prop called text
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}

class Welcome extends Component {
  render() {
    // you can add javascript here between the render method and the return
    const { text } = this.props; // defines that text is a poperty of this.props object
    // we're deconstructing an object here - pulling out text varible from this.props object
    // this is more DRY and allows for multiple props eg. const {text, prop2, prop3 } = this.props;
    return (
      // <h1 className="App-title">{this.props.text}</h1> this is how it looks without the const variable
      <h1 className="App-title">{text}</h1> // now we need to access the prop
    );
  }
}

export default App;
```

## State & basic events

* use state to keep track of things changing
* can be used to keep track of an input or toggling something on and off like a navigation, turning something on or off
* lets you have dynamic components that you don't need to pass props too
* we set state for the initial render of a component just above the render function
* state is set as an object
* state can use boolen values, strings whatever
* we can change state and this will trigger a rerender of that component

* now we have access to this.state within this App component
* it wouldn't work in our Welcome component at the moment

- We can do inline conditional statements with React and state
  {this.state.toggle &&
    <p>This should show and hide</p>
  }
  - the && acts as a conditional statement - if the first statement is true then go to the next one which is our result

* When we want to change state we use setState and for that we need to use an arrow function, a normal function will cause errors with setState

* to pass state down to the Welcome component we would pass it in as a prop

```javascript
class App extends Component {
  state = {
    toggle: true // here we are setting the initial state on our component
  };

  toggle = () => {
    // must be an arrow function or errors will occur
    this.setState({
      // setState is the method we use to change state
      toggle: !this.state.toggle // this is basically setting toggle to the opposite of its current value
    });
  };

  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <Welcome text="Welcome to React" toggle={this.state.toggle} /> // pass
          state down to Welcome with prop
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
        // Creating our inline conditional if statement
        {this.state.toggle && <p>This should show and hide</p>}
        <button onClick={this.toggle}>Show / Hide </button> // we use an onClick
        event to run the toggle method
      </div>
    );
  }
}

class Welcome extends Component {
  render() {
    const { text, toggle } = this.props;
    console.log(toggle); // here we access state, but we haven't used this.state as its now a prop
    return <h1 className="App-title">{text}</h1>;
  }
}

export default App;
```

### Lifecycle Methods

* a component lifecycle goes through a number of phases
* these phases trigger events which you can tap into
* when component is first created the constructor method is called
* componentWillMount() is fired just before the component renders
* render() method happens between these two
* componentDidMount() is fired just after the component has finished rendering
* React documentation suggest that you use constructor() more than componentWillMount()
* you might do something with your props in the constructor() method before the comp renders
* you use componentDidMount() to mess with stuff in your comp after it has rendered
* so if you changed state in componentDidMount() it will cause a rerender
* there are other lifecycle methods - see the docs
* we're using a new why to set state with a state object rather than in the constructor() method
* example: you might use componentDidMount() to use another JS library and load that in but not hold up the render, something like pulling in a google map
* these lifecycle methods can be used with animation to set different classes at different phases

```javascript
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  // first lifecycle method
  constructor(props) {
    // this is a javascript class
    super(props); // because its a class we need a super() method
    // props don't exist yet so we pass them in
    console.log('constuctor');
  }
  // this fires just before the component renders
  componentWillMount() {
    console.log('will mount');
    // we do have access this.props here
  }
  // this fires after the component has finished rendering
  componentDidMount() {
    console.log('mounted');
  }

  state = {
    toggle: true
  };

  toggle = () => {
    this.setState({
      toggle: !this.state.toggle
    });
  };

  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <Welcome text="Welcome to React" toggle={this.state.toggle} />
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
        {this.state.toggle && <p>This should show and hide</p>}
        <button onClick={this.toggle}>Show / Hide </button>
      </div>
    );
  }
}

class Welcome extends Component {
  render() {
    const { text, toggle } = this.props;
    console.log(toggle);
    return <h1 className="App-title">{text}</h1>;
  }
}

export default App;
```

### Using Refs To Access DOM elements

* a reference to a DOM element
* often used with a form
* you can add a ref onto any DOM element
  <input type="text" ref={input => (this.text = input)} />

  * the input parameter could be anything really
    ref={bob => (this.text = bob)} // would work just as well

```javascript
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  submit = () => {
    console.log(this.text.value); // here we console.log the value entered
  };

  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <Welcome text="Welcome to React" />
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
        <input type="text" ref={input => (this.text = input)} /> // targeting
        the input with a ref
        <button onClick={this.submit}>Show value </button> // calls the submit function
        above
      </div>
    );
  }
}

class Welcome extends Component {
  render() {
    const { text } = this.props;
    return <h1 className="App-title">{text}</h1>;
  }
}

export default App;
```

### Inputs. Controlled vs Uncontrolled

* the ref example above is an example of an uncontrolled input - we are just accessing the value of the input
* with a controlled input we use state instead
* you could used a controlled input to filter results in a search input for example

```javascript
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  state = {
    input: 'Hello' // sets the value of the input initially
  };

  updateInput = event => {
    // captures the event - whatever user types into input
    this.setState({
      input: event.target.value // then set the new state to the value of the input
      // input: event.target.value.trim() // we can also use methods like trim on it now to get rid of spaces
    });
  };

  submit = () => {
    console.log(this.text.value);
  };

  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <Welcome text="Welcome to React" />
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
        <h3>{this.state.input}</h3> // we can even render the input in an
        element
        <input
          type="text"
          onChange={this.updateInput} // onChange event listens on this input and calls our updateInput function
          value={this.state.input} // sets the value of whats in state
        />
        <input type="text" ref={input => (this.text = input)} />
        <button onClick={this.submit}>Show value </button>
      </div>
    );
  }
}

class Welcome extends Component {
  render() {
    const { text } = this.props;
    return <h1 className="App-title">{text}</h1>;
  }
}

export default App;
```

### Iterating & Child Components

* when in react we need to output javascript to use it in our component
* so using something like map() is perfect as we can loop over an array and output a new array to work with in our component
* we can modify that output in the new array, such as adding some JSX

```javascript
import React, { Component } from "react";
import logo from "./logo.svg";
import "./App.css";

const movies = [ // setup an array to play with
  {
    id: 1, // React needs a unique id for elements to keep track of them
    title: "Star Wars"
  },
  {
    id: 2,
    title: "Spider Man"
  },
  {
    id: 3,
    title: "36th Chamber of Shaolin"
  }
];

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
        </header>
        {movies.map((movie) => {  // use the map() method to loop over the array and output a new array
          return (
            <div key={movie.id}>{movie.title}</div>; // we need return the array items
          )
        })}
      </div>
    );
  }
}

export default App;
```

* here we have the Movie component as its own thing:

```javascript
import React, { Component } from 'react';

export default class Movie extends Component {
  // the export default can be used here rather than at the bottom like App.js
  render() {
    return <div>{this.props.movie.title}</div>;
  }
}
```

* to use this new component in our App.js we first have to import it:
  import Movie from "./Movie";
* then to use it we replace this:

```javascript
{movies.map((movie) => {  // use the map() method to loop over the array and output a new array
  return (
    <div key={movie.id}>{movie.title}</div>; // we need return the array items
  )
})}
```

* we also pass in props to the component with
  movie={movie}
* with our component instead:

```javascript
{
  movies.map(movie => <Movie key={movie.id} movie={movie} />);
}
```

### STATIC, DEFAULTPROPS & PROPTYPES

* we can create default props as a fall back if data doesn't come through properly.
* for example a default image for our movies if the real movie image doesnt come through,
* this provides a nice fallback

- for this we need to install prop-types package
  npm install prop-types
- this allows use to specify what sort of prop types we are expecting

- you should have a propType for every prop coming into your app
- you should either use isRequired or have a defaultProps instead on everything
- that way you will get alerts in your linter that something is not as its meant to be
- makes for more robust code
- defaultProps doesn't work on nested props

```javascript
import React, { Component } from 'react';
import PropTypes from 'prop-types'; // importing PropTypes

export default class Movie extends Component {
  static propTypes = {
    // using propTypes to describe each prop
    movie: PropTypes.shape({
      // using .shape() means you can describe different date coming into the prop
      title: PropTypes.string.isRequired // using .isRequired for must have data
    }),
    desc: PropTypes.string // or here just directly
  };

  static defaultProps = {
    desc: 'Description not available' //here we create a default if desc isn't loaded for one of our movies
  };

  render() {
    return (
      <div>
        <h3>{this.props.movie.title}</h3>
        <p>{this.props.desc}</p>
      </div>
    );
  }
}
```

### Fetching Data from an API and async await

* for this project we're using a API from themoviedb.org
* in Account > Settings > API we need to grab the API Key (v3 auth)
* can also just grab the example API request
* if you go to API > Discover you get access to try out the API
  [https://developers.themoviedb.org/3/discover/movie-discover]
* then in the Try me tab add your API key
* grab a copy of the send request link - this will go in our fetch()

* in the App.js we need to go into the component
* we're best to add this in componentDidMount lifecycle phase
* this means the component has loaded and shows up before the data arrives

* we're using the fetch API to grab the data and use async await
* async await is good because it uses promises - this means we make sure the data exists and comes back from the API before we use it
* we're using await to fetch the data and wait for it, we then get the json from that data to use in our app

* to use this data in our app we need to add it as a prop via setState
* the movie info we need in in the results

- now we replace the movie variable with this.state.movies in our app
  {this.state.movies.map(movie => <Movie key={movie.id} movie={movie} />)}

```javascript
class App extends Component {
  // to use our setState from below we need to define state initially as a blank array otherwise we get an error
  // we get an error because we've used componentDidMount below so state didn't exist without defining it first
  state = {
    movies: []
  };

  // now we get our data from the API using async await
  async componentDidMount() {
    // adding data with this lifecycle phase so the user sees something
    try {
      // we try to get the data
      const res = await fetch(
        // await to get this data before moving onto the next step
        // fetching data and waiting until it arrives
        'https://api.themoviedb.org/3/discover/movie?api_key=809677ac41564518d039d2e5cbdabbfe&language=en-US&sort_by=popularity.desc&include_adult=false&include_video=false&page=1'
      );
      const movies = await res.json(); // once arrived we pull it onto our movies variable to use in our component
      // console.log(movies);
      this.setState({
        movies: movies.results
      });
    } catch (e) {
      // log error if the data doesn't arrive
      console.log(e);
    }
  }

  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
        </header>
        {this.state.movies.map(movie => (
          <Movie key={movie.id} movie={movie} />
        ))} // replaced movie with this.state.movie
      </div>
    );
  }
}

export default App;
```

### React Dev Tools

* direct access to our components & props
* you can see exactly what data you can access in your props
* with a component selected you can type $r in the console to output the entire component

- great for seeing inside the component and test it

### Eslinting React

* first up we need eslint plugin installed in VS code
* then we need to load some packages as dev dependences:
  npm install --save-dev eslint eslint-config-airbnb eslint-plugin-react

- now to use these pages we need a .eslintrc file
  eslint --init

- then use the airbnb styleguide
- yes to react
- javascript for the format for the config file

- Scott had configs we could grab straight from the code files
- clicking on an underlined error then hitting fn F8 key to show the rule you can then turn off as below
- can add a rule object to our .eslintrc file to switch off things we don't need
  rules: {
  'react/jsx-filename-extension': 0,
  'function-paren-newline': 0
  }

- can also go into the settings for VS Code and search of eslint then add changes to the user file like:
  "eslint.autoFixOnSave": true,
- eslint has a good tutorial video of getting setup

- you can turn off rules on a line or even for a whole component. EG. added to top of App.js
  eslint react/no-did-mount-set-state: 0 in a comment

* helps to set VS code to Javscript React as well

#### Functional Stateless Components

* you should be using a functional stateless component any time you aren't using state, refs or lifecycle methods
* more lightweight component
* may give performance benefits in the future

- this means we can clean up the Movie.js file:

- from this:

```javascript
import React, { Component } from 'react';
import PropTypes from 'prop-types';

export default class Movie extends Component {
  static propTypes = {
    movie: PropTypes.shape({
      title: PropTypes.string.isRequired
    })
  };

  render() {
    return (
      <div>
        <h3>{this.props.movie.title}</h3>
      </div>
    );
  }
}
```

* to this:

```javascript
import React from 'react'; // just import React
import PropTypes from 'prop-types';
// class becomes a function, props passed into the arrow function
const Movie = props => (
  // don't have the render or return methods
  <div>
    <h3>{props.movie.title}</h3> // remove this
  </div>
);

export default Movie; // need to export down here instead

Movie.propTypes = {
  // Movie.propTypes vs static propTypes
  movie: PropTypes.shape({
    title: PropTypes.string.isRequired
  }).isRequired // fixed the required for movie as well as title
};
```

* can refine further and destructure the props with ES6 destructuring
* the {} define the props object then we say we just want movie from it

```javascript
const Movie = (
  { movie } // props become ({movie})
) => (
  <div>
    <h3>{movie.title}</h3> // remove this
  </div>
);
```

#### Understanding React Router
