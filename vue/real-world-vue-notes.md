# Real World Vue - Vue Mastery

- with the vue cli or ui you can edit settings and determine how you set up our project
- in this project we've done a manual setup with Vue Router, Vuex, Linting with Prettier
- with the Vue UI you can add plugins, change settings and see a whole lot of stats from your project

### Files

- **public** is for assets you don't want processed by webpack
- **src** is all our project files
  - **assets** is where we keep all our assets we do want webpack to process
  - **components** are where we keep all our reusable components
  - **views** contains our page templates or views
  - **App.vue** is our root component
  - main.js is the file that renders our app and mounts it to the DOM

### Process

- our main.js file renders the App.vue file with all our components nested within it, this then renders to the index.html file which mounts our app in the div with #app id.

### Setup

- we can setup a .prettierrc.js file and add these settings:

```javascript
module.exports = {
  singleQuote: true,
  semi: false
}
```

- add Vue VSCode Snippets plugin for VSCode - its by Sarah Drasner
- shortcuts include: vbase, vif, von, vdata
- might need to add to settings: "vetur.completion.useScaffoldSnippets": false

## 4. Vue Router Basics

- uses client-side routing rather than server-side
- doesn't need to hit the server all the time
- Vue is good at SPA - uses Vue Router to achieve this
- Vue router loads in the router folder
- this is where we setup our different routes
- <router-link> is the comp we use for linking pages and <router-view/> is what we render the component into:

```javascript
<div id="nav">
  <router-link to="/">Home</router-link> |
  <router-link to="/about">About</router-link>
</div>
<router-view />
```

- you can also use a named route instead:
  <router-link :to="{ name: 'home' }">Home</router-link>
- this loads the route based on our named routes instead
- this is good because you can then change paths in a single file and it will update every link using this named route - the alternative is changing it in every component using that link
- you can also setup redirects in router if you change paths

```javascript
{
  path: "/about",
  redirect: { name: "about" }
}
```

## 5. Dynamic Routing & History mode

- we can create a dynamic route with a dynamic segment:

```javascript
{
  path: "/user/:username",
  name: "user",
  component: User,
  props: true // lets us send in our parameters into our comp with props
}
```

- now we can used a parameter in our route
- in our template we can access these route parameters with:
  {{ $route.params.username }}
- alternatively if we've set proper: true then we can pass it in as a proper in our script:
  props: ["username"]
- now in our template we can just use: {{ username }}

- for our dynamic link:

```javascript
<router-link :to="{ name: 'event-show', params: { id: '1' } }">Show Event #1</router-link>
```

### the # in the url

- Vue router by default runs in hash mode
- it uses the # to simulate a full URL so that every time a URL changes it doesn't have to refresh the page
- so we need to add a mode to our router:

```javascript
const router = new VueRouter({
  mode: 'history',
  routes
})
```

- now it enables history mode which uses the history.pushstate API
- need to enable some settings when pushing to production - check the docs

## 6. Single File Vue Components

- you can edit the languages using in your components for templates, scripts or styles
- if you want to load **Sass** you can go to the UI and **dependencies** and add:
  sass-loader & node-sass

## 7. Global Components

- base components are ones you use throughout your entire app so make a good candidate for a global component
- things like BaseIcon.vue
- to register a global component you need to import it into main and assign it to our Vue instance

```javascript
import BaseIcon from '@/components/BaseIcon.vue'

Vue.component('BaseIcon', BaseIcon) // the comp we're importing and the name of the component
```

- Now we can use it in any of our components without importing it
- to add more than one global comp you just import it and repeat the Vue.comp line with the new component name
- this can become unwieldy with lots of base components, instead you can use **Automatic Global Registration**
- for the code see the docs: https://vuejs.org/v2/guide/components-registration.html#Automatic-Global-Registration-of-Base-Components
- you also need to import lodash as a dependency

### BaseIcon component

- first we added an svg file to the public directory containing a number of icons within it
- the name, width and height are passed in as props

```javascript
<template>
  <div class="icon-wrapper">
    <svg class="icon" :width="width" :height="height">
      <use v-bind="{ 'xlink:href': '/feather-sprite.svg#' + name }" />
    </svg>
  </div>
</template>

<script>
export default {
  props: {
    name: String,
    width: {
      type: [Number, String],
      default: 24
    },
    height: {
      type: [Number, String],
      default: 24
    }
  }
}
</script>
```

- the name prop comes into play when we add the component
  <BaseIcon name="users" />
- the reusable comp can be used all over the place and is very flexible
- its and svg with use statement, hence the multiple icons

## 8. Slots

- slots can be used in our base components like buttons, form elements etc
- for example in our BaseButton comp:

```javascript
<template>
  <div>
    <button><slot>SUBMIT</slot></button>
  </div>
</template>

// then...
<BaseButton>SAVE</BaseButton>
```

- Adding the text in between the slot sets a default
- named slots let us use multiple lots in a component:

```javascript
<slot name="heading"></slot>
<slot name="paragraph"></slot>

// then
<template>
  <MediaBox>
    <h2 slot="heading">Adam Jahr</h2>
    <p slot="paragraph">My words...</p>
  </MediaBox>
</template>
```

- you can import multiple elements into a slot as well by using a template tag:

```javascript
<template>
  <MediaBox>
    <h2 slot="heading">Adam Jahr</h2>
    <template>
      <p>My words...</p>
      <BaseIcon name="book" />
    </template>
  </MediaBox>
</template>
```

- using a template tag rather than a div means the tag disappears when rendered whereas a div would still show
- here we are only naming one slot as Vue knows to use the other one by default

## 9. API calls with Axios

- Vue doesn't have a built in way for doing API calls
- one of the most popular is Axios
- allows for GET, POST, PUT and DELETE requests

### A basic axios get request

```javascript
axios.get('https://example.com/events') // Call out to this URL
  .then(response =>
    console.log(response.data);  // When the response returns, log it to the console
  })
  .catch(error =>
    console.log(error);  // If an error is returned log it to the console
  })
```

- Axios calls run asynchronously - means the code continue and doesn't wait for tha call to finish

- first up we've grabbed our mock database file - db.json - now in our root directory
- then we need to install json-server: npm install -g json-server
- once installed we can run json-server in the terminal and watch this json file:
  json-server --watch db.json
- this runs a local api server - on http://localhost:3000/events we have our api
- now we need to install Axios as a dependency in our Vue UI

**NOTE:** you can update plugins and dependencies from the Vue UI

- in the vue cli just run: npm outdated
- this shows you a list of outdated dependencies
- you then either update ones specificially with: npm update 'DEPENDENCY_NAME'
- or do them all with: npm update

### getting our data

- in our EventList.vue file we add our axios call inside the created() method

```javascript
// we also needed to import axios above out export default {}

created() {
    axios
      .get(`http://localhost:3000/events`) // grab the api
      .then(response => {
        console.log(response.data) // return the data as a response
      })
      .catch(error => {
        console.log('There was an error:' + error.response) // have an error incase it fails
      })
  }
```

- above this we create our data function and create an empty events array

```javascript
data() {
  return {
    events: []
  }
},
// and make the response:
this.events = response.data
```

- this makes the events date array we setup equal to our returned data
- now we pass that data into our EventCard component:
  <EventCard v-for="event in events" :key="event.id" :event="event" />

  - we loop over the events
  - add a key with the id
  - then we sent in each of the events we are iterating through as a prop to the comp

- now in our EventCard.vue file we replace our fake data with the event prop:

```javascript
props: {
  event: Object
}
```

- and out events should be loading
- lastly we replace our params id to fix the link:

```javascript
<router-link
    class="event-link"
    :to="{ name: 'event-show', params: { id: '1' } }"
  >

// becomes
<router-link
    class="event-link"
    :to="{ name: 'event-show', params: { id: event.id } }"
  >
```

### Reorganising our code

- we're about to do a axios call in the EventShow.vue comp
- this complicates things because we're doing to separate calls with 2 axios instances
- so instead we create a new files in a new services folder called EventService.js
- inside of that we setup a function doing out axios call

```javascript
import axios from 'axios'

const apiClient = axios.create({
  baseURL: `http://localhost:3000`,
  withCredentials: false, // This is the default
  headers: {
    Accept: 'application/json',
    'Content-Type': 'application/json'
  }
})

export default {
  getEvents() {
    return apiClient.get('/events')
  }
}
```

- now in our EventList.vue we remove the axios import and replace with:
  import EventService from '@/services/EventService.js'
- in our created() method we remove axios and the get request to our events URL and replace with:

```javascript
created() {
  EventService.getEvents()
    .then(response => {
      this.events = response.data
    })
    .catch(error => {
      console.log('There was an error:' + error.response)
    })
}
```

### Creating our EventShow comp with our data

- first we had to add another function to our EventService.js

```javascript
getEvent(id) {
  return apiClient.get('/events/' + id)
}
```

- then in our EventShow.vue component we:

```javascript
import EventService from '@/services/EventService.js' // import out service
export default {
  props: ['id'],
  data() {
    return {
      event: {} // add our empty object
    }
  },
  created() {
    EventService.getEvent(this.id) // create out api call using the id
      .then(response => {
        this.event = response.data
      })
      .catch(error => {
        console.log('There was an error:', error.response)
      })
  }
}
```

- see the final file for the template and style code
- we do get an error on the **event.attendees.length** because when it loads initially there is nothing in it so no length. TO fix this we add a ternary statement defaulting to 0 if nothing is found

```javascript
<li
  v-for="(attendee, index) in event.attendees"
  :key="index"
  class="list-item"
>
```
