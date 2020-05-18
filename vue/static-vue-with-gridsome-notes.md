# Static Vue with Gridsome

- to get started we need to install the gridsome CLI:
  npm install --global @gridsome/cli

- now we can create a new project with:
  gridsome create my-gridsome-site
- we can then run gridsome develop to start our development server
- our site appears on:
  http://localhost:8080/
- our graphql playground on:
  http://localhost:8080/___explore

## File structure

- most of our work takes place in src
- gridsome is similar to gatsby with its pages folder for main page routes
- also includes a templates folder for dynamic pages
- static contains any static files we need
- gridsome.config.js has main config settings for the projects and is where we bring in plugins
- gridsome.server.js is where we interact with the server for our dynamic routes and pages

## Creating a Page

- just by adding a vue file to our pages directory we're able to create a new page
- Gridsome makes our Default.vue layout available to all templates from the main.js file
- this means we don't need to import Layout every time.
- this ability to make a component globally available is a Vue feature
- if you had different Layout components then that would be different

## Routing In Gridsome

- you can also add folders in pages directory to add a sub page
- Gridsome makes the link comp available to us automatically, without the need to import something like Link
  <g-link class="nav__link" to="/posts/">Posts</g-link>

## Layouts In Gridsome

- <slot/> is used in Layouts for adding content
- it works the same as {children} in React
- you can override the global layout by importing the alternative layout manually
- you can also add the alternative layout to main.js to make it global
- layouts re-render on page change which can cause some interesting effect when using transitions
- you can use multiple slots by naming them
- you can pass props into layouts to do things like control if a component shows/hides depending on what is set in the prop for that comp

## Plugins And Transformers

- in order to access markdown files within our site we need to install @gridsome/source-filesystem
- to transform our markdown into something we can use in our graphql queries we need to use a transformer:
  @gridsome/transformer-remark
- with both installed we can access our markdown files
- gridsome splits plugins and transformers into separate sections in the config file
- for the path in the source file system plugin we need to add src/ in
- if you needed another section with markdown files you would create another source-filesystem with a different name and path, but you could used the same transformer remark

## Static Queries

- good for query simple data that doesn't need any parameters added to it
- runs once at page build
- Gridsome comes with Page queries and Static queries
- page queries can be used in pages and templates while static queries can be used in components as well
- we create a separate block for our static query:

```javascript
<static-query>
  query LatestPosts {
    allBlogPost {
      edges {
        node {
          id
          title
          content
        }
      }
    }
  }
</static-query>
```

- we can then access the query in our template with a v-for

```javascript
<article v-for="edge in $static.allBlogPost.edges" :key="edge.node.id">
  <div v-html="edge.node.content" />
</article>
```

## Frontmatter And Formatting

- frontmatter lets us add some meta data to our markdown files
- we can add frontmatter to our md file between 3 dashes:

```javascript
---
title: Hello World
date: 2020-03-10
tags: ["intro", "soft skills"]
slug: hello-world
---

// Now we can query for that:
allBlogPost {
  edges {
    node {
      id
      title
      date(format: "MMM Do YYYY")
      content
      slug
    }
  }
}
// as well as doing some formatting on our date output
```

## Creating Posts Automatically With Templates

- now we have access to these markdown files with the added benefit of frontmatter.
- to get our dynamic pages we need to:

1. setup a vue file in our templates folder. This is what Gridsome uses for dynamic templates
2. the name of our file should be the same as typeName we set in our config - eg. BlogPost.vue
3. We also need to setup templates in our config which also sets the path and slug parameter - this is now separate to the plugins

```javascript
templates: {
    BlogPost: "/posts/:slug"
  },
```

4. We have to add the slug manually to the frontmatter
5. Now if we restart the server and go to our url and slug: http://localhost:8080/posts/hello-world
6. We should see our basic template

- for a link from our LatestPosts we use a g-link comp and bind to the to:
  <g-link :to="`/posts/${edge.node.slug}`">Read More</g-link>

## Page Queries And GraphQL Variables

- page-queries let you use dynamic parameters coming in from our route to display content in our template
- can only use them in pages and templates
- first up we test out query in the graphiql explorer

```javascript
{
  blogPost(path: "/posts/hello-world") {
    // here we've tested a single path
    id
    path
    title
    date(format: "MMM Do YYYY")
    tags
    content
  }
}
```

- Now we can drop that into our BlogPost.vue template as a page-query

```javascript
<page-query>
// set path as an argument and pull from path
query Post ($path: String!) {
  blogPost(path: $path) {
    id
    path
    title
    date(format: "MMM Do YYYY")
    tags
    content
  }
}
</page-query>
```

- in order to make this dynamic we can now use the path as an argument in our query
- in our template we can access the graphql data like so:
  {{$page.blogPost.title}}
- you can also use any of the other elements like tags, date etc

## Amazing Image Loading

- Gridsome gives us some shortcuts for directories
- a tilda ~ relative path the src folder:
  src="~/assets/images/brekky.jpg"

- with images Gridsome gives us access to a tag called <g-image /> that does lots of magic
- <g-image src="~/assets/images/brekky.jpg" />
- adding a height="200" will crop the image to the height of 200px but leave the width
- setting a width="200" instead proportionally resizes the image
- <g-image src="~/assets/images/brekky.jpg" height="200" width="200" fit="cover" />
  will resize the image and crop it to 200x200
- you can also add quality="100" to set the quality of the image - defaults to 75
- fit also has contain, fill
- you can also set the blur amount
- and also turn off lazy loading
- https://gridsome.org/docs/images/#images

## Images In GraphQL

- we can also access images via graphql from our markdown files
- in our frontmatter we need to add a relative path to the image
  image: ./images/bg.jpg
- now we can add image to our query
- when we test it we can see all the different options available and conversions.
- now we can access it in our template with the g-link tag:
  <g-image :src="$page.blogPost.image" />
- to add attributes we need to do that in the query instead: eg. image (height: 400)

## Sass In Gridsome

- to get Sass into Gridsome we need to install it first:
  npm install -D sass-loader node-sass
- now in our vue files we can add a lang="scss" to our style tag and start using sass

### setup a global scss stylesheet

- to have a global style where we have access to variable we need to install another package:
  npm i -D style-resources-loader

- now in our config we add some Webpack configs:

```javascript
const path = require("path"); // so we can use paths in our js

// a new function for loading our styles with the loader
function addStyleResource(rule) {
  rule
    .use("style-resource")
    .loader("style-resources-loader")
    .options({
      patterns: [path.resolve(__dirname, "./src/assets/*.scss")]
    });
}

// above the module.exports
```

- then in the module.exports we add our weboack config settings

```javascript
chainWebpack: config => {
  const types = ["vue-modules", "vue", "normal-modules", "normal"];
  types.forEach(type =>
    addStyleResource(config.module.rule("scss").oneOf(type))
  );
};
```

- this runs our new function and loads our stylesheet for all these files types
- all this comes from the docks
- now we can setup our scss in our assets folder

- to use variables we add them in our global stylesheet

```css
$baseFontSize: 20px;

body {
  background-color: red;
}

/* then reference them in any vue file with */
font-size: $baseFontSize;
```
