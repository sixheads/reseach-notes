# React for Beginners - cont...

## Persisting our State with Firebase

- we then need to mirror our fish state within our database
- we do this in App.js
- we do this in componentDidMount as we need to wait for the component to render
- first we import base
- then in componentDidMount:

```javascript
componentDidMount() {
  const { params } = this.props.match;
  this.ref = base.syncState(`${params.storeId}/fishes`, {
    context: this,
    state: 'fishes'
  });
}
```

- we destructure our params to get the storeId
- this.ref is a firebase method that refers to a piece of data in the database
- the state we are mirroring is fishes
- adding the /fishes puts the data into that structure within the database
- now this will push out data to our database

- before we finish we also need to make sure if the user moves off this page/route that we remove the binding for this.ref
- if we don't do this it can lead to a memory leak

```javascript
componentWillUnmount() {
  base.removeBinding(this.ref);
}
```

## Persisting Order State with localstorage

- bit like cookies
- key:value pairs
- find it in Dev Tools > Application > Local Storage

- first thing we need to do is save our order to local storage when we add to the order
- we do this in **componentDidUpdate**

```javascript
// in App.js
componentDidUpdate() {
  localStorage.setItem(
    // this is the key - store id
    this.props.match.params.storeId,
    // this is the order state, but converted to a string from an object
    JSON.stringify(this.state.order)
  );
}
```

- problem is when we refresh the page the order state is reset to an empty object
- so back in componentDidMount we need to first define our localStorage as a variable
- then we check if localStorage contains our orders and reinstate it

```javascript
// first reinstate our localStorage
const localStorageRef = localStorage.getItem(params.storeId);
if (localStorageRef) {
  // we need to convert it back into an object to use in state
  this.setState({ order: JSON.parse(localStorageRef) });
}
```

- we then hit an issue in our order component that Firebase takes a few seconds to update and add the fish back into out inventory - **localStorage is immediate**
- so the order is looping over fish and checking the status, but there's no fish at first
- so we first need to update our isAvailable variable

```javascript
// from this
const isAvailable = fish.status === 'available';
// to this and check if there is fish first up
const isAvailable = fish && fish.status === 'available';
// then if there's no fish just return null
if (!fish) return null;
```

- if we return **null** on something it just renders out nothing at all

## Bi-directional Data Flow and Live State Editing

- now we want to be able to edit our fish in our inventory
- first up we need to pass the fishes into Inventory

```javascript
// in App.js
<Inventory
  addFish={this.addFish}
  loadSampleFishes={this.loadSampleFishes}
  fishes={this.state.fishes}
/>
```

- we then create a new EditFishForm.js component
- which we will loop over with map()
- as fishes is an object we need to set Object.keys so we can map over just the keys - otherwise it won't work
- we then pass the fishes props down and specify each one with [key]

```javascript
{
  Object.keys(this.props.fishes).map(key => (
    <EditFishForm key={key} fish={this.props.fishes[key]} />
  ));
}
```

- this will throw an error as React doesn't like it if you don't also setup an onChange event on the text field
- this is because changing the text field here won't reflect in state just yet so it will disable it.
- so we first setup a handleChange function

```javascript
handleChange = event => {
  // update that fish
  // 1. Take a copy of the current fish
  const updatedFish = {
    ...this.props.fish,
    // it then sees which input is being changed & gets the new value
    // this is why we needed to give our inputs a name and value
    [event.currentTarget.name]: event.currentTarget.value
  };
  this.props.updateFish(this.props.index, updatedFish);
};
```

- we then pass this up to Inventory.js then App.js by adding this to both:
  updateFish={this.props.updateFish}
- now that we've passed up that function we can pull that along with the key to identify that fish into a new updateFish function

```javascript
updateFish = (key, updatedFish) => {
  // 1. Take a copy of the current state
  const fishes = { ...this.state.fishes };
  // 2. Update that state
  fishes[key] = updatedFish;
  // 3. Set that to state
  this.setState({ fishes });
};
```

- this then passes the updated fish into state (also gets passed into Firebase as well)

## Removing Items from State

- first we create a deleteFish function in App.js
- for this to work we need to set the deleted fish to **null** for Firebase to delete it also

```javascript
deleteFish = key => {
  // 1. take a copy of state
  const fishes = { ...this.state.fishes };
  // 2. update the state (Firebase needs us to set it to null)
  fishes[key] = null;
  // 3. update state
  this.setState({ fishes });
};
```

- we then pass it down to Inventory then EditFishForm as props:
  deleteFish={this.deleteFish}
- then at the bottom of our edit fish we add a button
- we can just call the function inline

```javascript
<button onClick={() => this.props.deleteFish(this.props.index)}>
  Remove Fish
</button>
```

- removing from the Order is similar:

```javascript
removeFromOrder = key => {
  const order = { ...this.state.order };
  delete order[key]; // not using firebase for this so can use delete
  this.setState({ order });
};
```

- we then pass it down to Order as props:
  removeFromOrder={this.removeFromOrder}
- then in Order we add a button
  <button onClick={() => this.props.removeFromOrder(key)}>&times;</button>

## Animating React Components

- we're using react-transition-group to add animation to the site
- we can use simple css transitions on our components targeting when they mount and unmount

- so in Order.js we first import react-transition-group:
  import { TransitionGroup, CSSTransition } from 'react-transition-group';

- we then change our ul into a TransitionGroup tag

```javascript
<TransitionGroup Cmponent="ul" className="order">
  {orderIds.map(this.renderOrder)}
</TransitionGroup>
```

- we then wrap the li in a CSSTransition tag
- that tag needs classNames, key and timeout (the speed of the animation)

```javascript
<CSSTransition classNames="order" key={key} timeout={{ enter: 250, exit: 250 }}>
  <li key={key}>
    {count} lbs {fish.name}
    {formatPrice(count * fish.price)}
    <button onClick={() => this.props.removeFromOrder(key)}>&times;</button>
  </li>
</CSSTransition>
```

- so now when we add an <li> it gets mounted & a class of .order-enter is added immediately, then after the timeout .order-enter-active is added
- so we can transition between those 2 classes
- in the revers we get .order-exit and .order-exit-active in the same way

- so we don't have to repeat the CSSTransition and all its options we can pull out those options into a variable then apply them to the tag instead - easy to manage.
- then spread them into our tag

```javascript
const transitionOptions = {
  classNames: 'order',
  key,
  timeout: { enter: 250, exit: 250 }

  // then spread them like this:
  <CSSTransition {...transitionOptions}>
};
```

## Component Validation with PropTypes

- when we pass props into a component we can use proptypes to check that we're adding the correct data
- if we pass in the wrong sort of data it will throw an error in the console
- so for example in Header.js we can import PropTypes then add a propType for the tagline

```javascript
Header.propTypes = {
  tagline: PropTypes.string.isRequired
};
```

- on a class like Fish.js we add it within the class above the render() - we do this because we are applying it the the class
- for ones like details which was returning and object we can break it out using shape()

```javascript
static propTypes = {
  details: PropTypes.shape({
    image: PropTypes.string,
    name: PropTypes.string,
    desc: PropTypes.string,
    status: PropTypes.string,
    price: PropTypes.number
  }),
  addToOrder: PropTypes.func
};
```

## Authentication

- first up we need to setup Authentication login settings for Facebook & GitHub (didn't bother with Twitter as it asked too many questions)
- then we setup a Login.js component
- then add it to Inventory.js just above our return statement

```javascript
 render() {
    return <Login />;
    return (
```

- basically it will return the login and ignore our other return() for now
- we then add a button for each login option which calls a function called authenticate that also supplies the provider eg. Github

```javascript
<button
  className="github"
  onClick={() => props.authenticate('Github')}>
  Log In with Github
</button>
<button
  className="facebook"
  onClick={() => props.authenticate('Facebook')}
>
  Log In with Facebook
</button>
```

- back in our inventory we need to create this new function and pass it down to Login

```javascript
authenticate = provider => {
  // 1. create a new authProvider depending on what they are siging in with
  const authProvider = new firebase.auth[`${provider}AuthProvider`]();
  // we then use firebase to signin
  firebaseApp
    .auth()
    .signInWithPopup(authProvider)
    // then when the user comes back with that authentication
    // we run this function & pass the data to the authHandler
    .then(this.authHandler);
};

// this is the authHandler function
authHandler = async authData => {
  // 1. Look up the current store in the firebase database
  const store = await base.fetch(this.props.storeId, { context: this });
  // 2. Claim it if there is no owner
  if (!store.owner) {
    await base.post(`${this.props.storeId}/owner`, {
      data: authData.user.uid
    });
  }
  // 3. Set the state of the inventory component to reflect the current user
  this.setState({
    uid: authData.user.uid,
    owner: store.owner || authData.user.uid
  });
};
```

- in order to do this and use firebase base we need to import it into the file
  import base, { firebaseApp } from '../base';
- we also needed to pass the storeId down to Inventory from App.js
  storeId={this.props.match.params.storeId}

- we also need to set a null state for uid and owner

```javascript
state = {
  uid: null,
  owner: null
};
```

- finally add a logout module which calls a function to sign us out of firebase and set the state of uid to null

```javascript
const logout = <button onClick={this.logout}>Log Out!</button>;

// this is the function
logout = async () => {
  await firebase.auth().signOut();
  this.setState({ uid: null });
};
```

- we then set 3 states for checking

1.  if no owner logged in render the login
2.  if their is a uid but they are not the owner render the 'Sorry you are not the owner" - logout
3.  otherwise render the Inventory

- to reset the privacy on our database Wes has provided some new rules to paste in

```javascript
// These are your firebase security rules - put them in the "Security & Rules" tab of your database
{
  "rules": {
    // won't let people delete an existing room
    ".write": "!data.exists()",
    ".read": true,
    "$room": {
      // only the store owner can edit the data
      ".write":
        "auth != null && (!data.exists() || data.child('owner').val() === auth.uid)",
      ".read": true
    }
  }
}
```
