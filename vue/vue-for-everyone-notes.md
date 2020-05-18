# Vue for Everyone - LevelUpTuts

- to use Vue we need to create a Vue instance
- we can have multiple instances of Vue

```javascript
const app = new Vue({
  el: "#app",
  data: {
    hello: "Hello World",
    isTrue: true
  }
});
```

## Templating in Vue

- we could then use this data in our HTML
- {{ }} can output variables
- uses similar syntax to Moustache templating
- also have access to methods or ternary statements
- {{ hello.length }}
- {{ isTrue ? hello : 'No' }}
- Can't do full on JavaScript the brackets

```html
<div id="app">
  {{ hello }}
</div>
```

## Directives

- directives are live attributes on HTML elements that modify your HTML in some way
- all directives are prefixed by v-
- eg: v-bind or v-on:
- for example an if statement looks like this:

```html
<div id="app">
  <div v-if="isTrue">
    {{ hello }}
  </div>
</div>
```

## Events and Methods

- an on click within the HTML

```html
<button v-on:click"isTrue = !isTrue">
  Press Me
</button>
```

- Alternatively you could user a method instead
- we use a function to get access to THIS rather than an arrow function

```javascript
const app = new Vue({
  el: "#app",
  data: {
    hello: "Hello World",
    isTrue: true
  },
  methods: {
    toggleIsTrue: function() {
      this.isTrue = !this.isTrue;
    }
  }
});
```

- then call it in our HTML: <button v-on:click"toggleIsTrue">

## USER INPUT & DATA BINDING WITH V-MODEL

- using v-model we can bind an element to a data element
- <input type="text" v-model="hi">
- then in our script we can add the variable to data
  hi: "",

## VueCLI

- [Vue CLI 3](https://cli.vuejs.org/)
- first up we make sure its installed: npm install -g @vue/cli
- to create a project:
  vue create my-project
  OR
  vue ui
- we're going to create a project called vue-movie-db
- can't use capitals when creating new project
- just went with the default settings and NPM
- once setup we cd into the new folder and run
  npm run serve
- this sets up a local server on: http://localhost:8080/
- use npm run build for a production ready build version

## Vue Default Project Files

- src is where we're working
- main.js handles the new Vue instance and rendering it to the page by mounting it at \$app
- App.vue is our main app component
- we also get an additional component called HelloWorld.vue

## Vue Files Explained

- vue templates are broken up into 3 main sections:

### 1. template

- template is our actual HTML
- we can also use our Vue templating here with javascipt variables and expressions
- we can also call on other components here
- we can also use props in here
- its a bit like our render function in React

### 2. script

- this is where we import our components
- our JS has to happen here between our script tags
- here we do a export default of our instance
- includes things like our data, methods & components
- bit like our class object in React
- to use our data in here we need to pass it as a function instead and return the data

```javascript
data() {
  return {
    hello: 'Hello World'
  }
}
```

### 3. style

- here are all out style as normal CSS
- we can add a scoped attribute to the style tag to scope the styles just to this component

## Creating and using components

- first we'll create a new Header.vue component
- we can use our Vetur extension to add the structure 'scafold'

```html
<template>
  <header>
    <h1>Header</h1>
  </header>
</template>

<script>
  export default {
    name: "Header"
  };
</script>

<style scoped>
  header {
    background-color: #111;
    padding: 20px;
    color: white;
  }
  h1 {
    margin: 0;
  }
</style>
```

- to use this in our App we first import the component in our script
  import Header from "./components/Header.vue";
- then add it in our export as one of the components
- finally we can add it in our template as <Header />

## Props in Vue

- props are essentially data that you can pass along from one component to another
- to add a simple prop to our Header we can add a props object to our Header

```javascript
props: {
  title: String; // we define what sort of data this will accept
}
```

- We then switch out our h1: <h1>{{title}}</h1>
- and back in our App.vue file we add our title prop to our Header with the actual text string we want

  <Header title="Vue Movie DB"/>

- If we wanted to pull the title from our data instead as a variable we need to add it into the main App.vue as data

```javascript
data() {
  return {
    hello: 'Hello World',
    title: 'Vue Movie DB'
  }
}
```

- then to use it we need to bind the variable to our element
  <Header v-bind:title="title"/> or the shorthand for bind colon <Header :title="title"/>

## Looping in Vue

- in Vue we aren't using .map() or forEach to loop.
- instead we use the v-for directive
- there's not shorthand for v-for
- the v-for goes on the item you are looping not the parent.

```javascript
<li v-for="item in list" :key="item">{{ item }}</li>

// from a list we create in our data
list: ["Penquin", "Turtle", "Red Panda"]
```

- it also needs a :key for the virtual DOM to keep track of the items. Ideally you would have a separate id, in this case we've just used the item itself
- A better option for this from an array is:

```javascript
<li v-for="(item, index) in list" :key="index">{{ item }}</li>
```

- with an array of objects we can just use an id:

```javascript
<li v-for="person in people" :key="person.id">{{ person.name }}</li>

// our data
people: [
  {
    id: 1,
    name: "John"
  },
  {
    id: 2,
    name: "Bec"
  }
  ]
```

- with v-for we can also loop over an object which is tricky with normal js
- here we have access to the key, its value as well as the index

```javascript
<li v-for="(value, key, index) in profile" :key="index">{{ key }}: {{ value }}</li>

profile: {
  name: "Jim",
  age: 40,
  job: "Developer"
}
```

- you can also loop and output a component, but you definitely need a :key

## Conditional Directives in Depth

- when we use the v-if directive we also get access to v-else
- v-else will only work if placed on the very next HTML element

```javascript
<Header v-bind:title="title" v-if="isTrue" />
<img alt="Vue logo" src="./assets/logo.png" v-else />
```

- could be useful for a loading state
- we also have v-else-if as well if we want to chain some conditionals
- this would work if our condition wasn't a boolean.

```javascript
<div id="app" v-if="status === 'Ready'">
// some stuff
<div v-else-if="status === 'Loading'">
// then end with else
```

## Computed Properties in Vue

- lets us modify our data before its displayed
- we're able to grab data or props then create a function in computed and make some modifications.

## Hitting The MovieDB API with Methods

- first up we need to grab our API key from our account with the MovieDB
- our new MoviesList comp fetches the movies to then display on the page

```javascript
export default {
  name: "MoviesList",
  // first we need to declare the movie variable as a empty object so it exists
  data() {
    return {
      movie: {}
    };
  },
  // we then hit a lifecycle method created which runs our function to get the data
  created: function() {
    this.fetchData();
  },
  methods: {
    // we've create an async function to grab the data
    fetchData: async function() {
      try {
        // first we get the content
        const res = await fetch(
          `https://api.themoviedb.org/3/movie/550?api_key=YOUR_KEY_HERE`
        );
        // convert it to json
        const movie = await res.json();
        // then assign it to our empty movie variable
        this.movie = movie;
      } catch (e) {
        console.log(e);
      }
    }
  }
};
```

- now we have access to the movie

## Creating Our Movies Listing

- the movieDB gives you a lot of options on how to use the data
- checkout the API link to see what's possible
- we're trying the discover/most popular movies option
- so instead of the single movie above we change it to:
  https://api.themoviedb.org/3/discover/movie?sort_by=popularity.desc&api_key=YOUR_KEY_HERE
- the new bit is: /discover/movie?sort_by=popularity.desc
- also had to change the ? to a & before the API key
- you can check that it works in a browser
- now we have a list of movies we can list those out

```javascript
<li v-for="movie in movies" :key="movie.id">
  {{ movie.title }}
</li>
```

- to improve on this we should create a Movie.vue component instead

```javascript
<template>
  <div>{{movie.title}}</div>
</template>

<script>
export default {
  props: ["movie"]
};
</script>
```

- our movie data is passed in as props
- in our MoviesList comp we now use the Movie comp and use v-bind to add the props

```javascript
<li v-for="movie in movies" :key="movie.id">
  <Movie :movie="movie" />
</li>
```

- to list our images instead we could try the poster_path but it won't work because we need some extra URL info
  <img :src="movie.poster_path" :alt="movie.title" />
- here we are binding the src and alt attributes rather than src={{movie.poster_path}} which won't work
- the best way to add this extra info in the link is via computed properties - we modify the prop info this way

```javascript
  // here we pull in the computed property instead of the movie.poster_path
<template>
  <img :src="posterImage" :alt="movie.title" />
</template>

// this adds the extra URL part we need
<script>
const POSTER_PATH = "http://image.tmdb.org/t/p/w154";

export default {
  props: ["movie"],
  computed: {
    // we then combine the variable and the prop data here
    posterImage: function() {
      return `${POSTER_PATH}/${this.movie.poster_path}`;
    }
  }
};
</script>
```

## Global styles

- we can import a global stylesheet simply by adding one to our assets folder an importing the file directly into our main.js
  import "./assets/styles.css";

## Setting Up Vue Router

- to use routing in our Vue app we need to install vue router:
  npm install --save vue-router

- Now we need to create a new file called routes.js in our src folder
- this file controls all our routes for the application

```javascript
import Vue from "vue";
import Router from "vue-router";
import MoviesList from "./components/MoviesList.vue";

Vue.use(Router);

export default new Router({
  mode: "history",
  routes: [
    {
      path: "/",
      new: "Movies List",
      component: MoviesList
    }
  ]
});
```

- all our routes get outlined here
- uses the history state rather than reloading the page
- we also move our main MoviesList comp in here too
- So in our App.vue we no longer need to import MoviesList.vue so we need to remove that and add <router-view /> instead
- now depending on the view (page we're on) the router can pull in different components based on the path
- in our main.js file we also now need to tell Vue that we're using vue-router

```javascript
import Vue from "vue";
import App from "./App.vue";
import "./assets/styles.css";
import router from "./routes";

Vue.config.productionTip = false;

new Vue({
  render: h => h(App),
  router
}).$mount("#app");
```

## Creating Routes & Route Params

- now that we've added the router we have access to routes as parameters
- all we need to do is add another route with a path that includes a param, in this case the id

```javascript
{
  path: "/movie/:id",
  new: "Movies Detail",
  component: MovieDetail
}
```

- we also need to import our new MovieDetail comp
- to use a relative path we can write out import like this with and @:
  **import MovieDetail from "@/components/MovieDetail";**
- now if we go to /movie/anything we will see our new dynamic route
- in our component we can output our route with **{{ $route.params.id }}**
- in our dev tools we now should see our router-view and have access to this param
- to make use of the movie id para and load that particular movie id we need to add a router link to our Movie.vue comp

```javascript
<router-link :to="moviePath">
  <img :src="posterImage" :alt="movie.title" />
</router-link>

// we also add a new computed property to combine the movie.id
// with the extra path

moviePath: function() {
  return `/movie/${this.movie.id}`;
}
```

## Route Based API Calls

- in order to pull in the data for that movie based on the ID param being passed we basically copied the code for MoviesList but now we're replacing the most popular movies API URL with one that includes the id of that movie
- this gets passed in by the ID param

```javascript
export default {
  data() {
    return {
      movie: {} // movie instead of movies and an object rather than an array
    };
  },
  created: function() {
    this.fetchData();
  },
  methods: {
    fetchData: async function() {
      try {
        // now we grab the single movie with the passed in ID
        const res = await fetch(
          `https://api.themoviedb.org/3/movie/${this.$route.params.id}?api_key=YOUR_KEY_HERE`
        );
        const movie = await res.json(); // movie
        this.movie = movie; // movie
      } catch (e) {
        console.log(e);
      }
    }
  }
};
```

- now we can grab our movie content with {{ movie.title }} etc
- we can also target our styles with a computed property

```javascript
// here's the image path minus the id
const BACKDROP_PATH = "http://image.tmdb.org/t/p/w1280";

// here's our computed property referencing the variable and our backdrop_path
computed: {
  styles() {
    return {
      background: `url(${BACKDROP_PATH}/${this.movie.backdrop_path}) no-repeat`
    };
  }
},

// in our template we just have to bind the style to our styles computed property
<div class="movie-wrapper" :style="styles">
```

## Animation In Vue

- animation features are build into Vue all we need to do is wrap our element in a <transition> element

```javascript
<transition name="fade">
  // Our elements here that we want to animate
</transition>

// now we have access to some styles - using the name we set above
<style>
.fade-enter-active, fade-leave-active {
  transition: all 0.3s ease;
}
.fade-enter, fade-leave-to {
  opacity: 0;
}

// name-enter -> name-enter-to
// name-enter-active

// name-leave -> name-leave-to
// name-leave-active
</style>
```

## Lifecycle Methods

- https://vuejs.org/v2/api/#Options-Lifecycle-Hooks

## Where To Go From Here

- docs are great, so is their discord
- CHeckout https://github.com/vuejs/awesome-vue
-
