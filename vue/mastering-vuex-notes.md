# Mastering Vuex - Vue Mastery (uses Real World Vue app as basis)

## 1. Intro to Vuex

- Vuex is Vue.js own state management pattern and library
- state is the data your component depend on and render - eg. blog posts, todo items etc
- without vuex passing state between components that share it depends on your passing events up then props down to the other comp
- with Vuex we can create a global state that all comp can access - a single source of truth
- this state is reactive so when a comp updates state any other comp that relies on that state is automatically updated
- to make this work effectively Vuex has a state management pattern that standardizes the way to make state changes

```javascript
const store = new.Vuex.Store({
  state: {
    ...
  },
  mutations: {
    ...// used to commit and track state changes
  },
  actions: {
    ...// can update the state (like methods)
  },
  getters: {
    ... // getters let us access our state (like computed)
  }
})
```

- works in a similar way to our vue instance with its data, methods and computed
- its best practice for our actions to use mutations to update out state - means dev tools can to timetravel debugging

## 3. State & Getters

- when we setup our app we included Vuex which created the store directory and a basic setup
- it also imported vuex into our main.js and included it before our render method
- to create a user that is available to all out components we add it to our state inside of store:

```javascript
state: {
  user: { id: 'abc123', name: 'Adam Jahr' }
},

// and use it in our EventCreate.vue comp with:
<h1>Create Event {{ $store.state.user }}</h1>

// or for just the name:
<h1>Create Event {{ $store.state.user.name }}</h1>
```

- if you plan to use this state in multiple places its best to use it with a computed property
- might be better for rerendering

```javascript
<template>
  <div>
    <h1>Create an Event, {{ userName }}</h1>
    <p>This event was created by {{ userName }}</p>
  </div>
</template>

<script>
export default {
  computed: {
    userName() {
      return this.$store.state.user.name // need to use this. as well
    }
  }
}
</script>
```

- if we wanted the id we could add another computed property
- this can get repetitive so we can use the mapState helper, which generates computed properties for us.

```javascript
import { mapState } from 'vuex'

export default {
  computed: mapState({
    userName: state => state.user.name,
    userID: state => state.user.id,
    categories: state => state.categories
  })
}

// or simplify further an just get the top level
computed: mapState({
  user: 'user',
  categories: 'categories'
})

// now call it in our template as:
{{ user.name }} or {{ user.id }}

// or loop over categories
<ul>
  <li v-for="cat in categories" :key="cat">{{ cat }}</li>
</ul>
```

- you could simplify our mapState further and just call the strings:
  computed: mapState(['user', 'categories'])

### Object Spread Operator

- with the operator spread we can use local computed properties and mix it in

```javascript
export default {
  computed: {
    localComputed() {
      return something
    },
    ...mapState(['user', 'categories'])
  }
}
```

### Getters

- if we want to access our categories length for example we could use a computed property. But if we need to access this length in multiple locations in our app we can use a getter for that instead:

```javascript
// first we setup our getter
getters: {
  catLength: state => {
    return state.categories.length
  }
},
// Our getter catLength takes in state then returns categories length

// then in EventCreate.vue we reference it in our computed
catLength() {
  return this.$store.getters.catLength
},

// then in out template
<p>There are {{ catLength }} categories</p>
```

### Passing in Getters

- sometime you need to pass the results from one getter into another one:

```javascript
doneTodos: state => {
  return state.todos.filter(todo => todo.done)
},
// here we pass state plus the getters into our new property
activeTodos: (state, getters) => {
  return state.todos.length - getters.doneTodos.length
}
```

- getting our length of done todos doesn't need the other getter (just an example above):

```javascript
activeTodosCount: state => {
  return state.todos.filter(todo => !todo.done).length
}
```

### Dynamic Getters

- sometimes we might want to make our getters dynamic by passing arguments into it

```javascript
// here we pass in state and then an id into that and return the event with the id we add
getters: {
  getEventById: state => id => {
    return state.events.find(event => event.id === id)
  }
},

// in are EventCreate.vue we reference the getter with our computed property
getEvent() {
  return this.$store.getters.getEventById
},
// then in out template:
<p>{{ getEvent(1) }}</p>
```

### mapGetters

- we can also map over our getters with mapGetters method, much like we did with mapState

```javascript
import { mapState, mapGetters } from 'vuex' // import mapGetters

export default {
  computed: {
    ...mapGetters(['getEventById']), // map over getters
    ...mapState(['user', 'categories'])
  }
}
```
