# Headless WordPress - LevelUp Tuts

## 1. Getting started

- clone the repo from Postlight [https://github.com/postlight/headless-wp-starter.git]
- then cd into the new directory and use:
  yarn install && yarn start
- you should then be able to login at:
  http://localhost:8080/wp-admin/
  u: nedstark
  p: winteriscoming
- we can view our REST API at: http://localhost:8080/wp-json

- NOTE: i've switch to running my own install of WP via docker so:

  1.  cd into wordpress folder
  2.  docker-compose up -d

- Stop your docker compose instance in that folder
  docker-compose stop

---

## 4. Setting up our Next.js Frontend

- cd into frontend folder:
  yarn install && yarn start
- our frontend should show up on:
  http://localhost:3000/

---

## 6. getInitialProps and our first API calls

- to create a list of posts (archive template) we create a new file in pages folder called postIndex.js & setup the component
- Next.js automatically creates a route for this file for us at http://localhost:3000/postIndex
- to get a more traditional URL we could change the file name to post-index.js
- to get the base layout we need to import the Layout:
  import Layout from "../components/Layout.js";
- into our component then change the div to <Layout> instead
- this is because we don't have access to the router directly with this system

- to get our posts we need to use a lifestyle method only available with Next.js called getInitialProps
- if we weren't using Next.js we'd use the componentDidMount lifecycle method
- with this we need to fetch our data from the WordPress API (http://localhost:8080/wp-json)
- do a search for 'posts'
- collapse the 'endpoints' & you should see the link to self: http://localhost:8080/wp-json/wp/v2/posts

```javascript
import React, { Component } from 'react';
import Layout from '../components/Layout.js';
import fetch from 'isomorphic-unfetch'; // this is a polyfill for fetch
import { Config } from '../config'; // this is the file that gives us our default URL

export default class postIndex extends Component {
  // here we have the getInitialProps lifecycle method
  static async getInitialProps() {
    const postsRes = await fetch(`${Config.apiUrl}/wp-json/wp/v2/posts`); // fetches the posts from our API
    const posts = await postsRes.json(); // waits then converts the data into a json format
    return {
      posts
    }; // then we can return the posts to use
  }

  render() {
    const { posts } = this.props; //destructuring do we don't have to write this.props.posts each time
    // we then loop over the posts with map to return the title
    // its title.rendered as that's how we see it in our React dev tools
    return (
      <Layout>
        <h1>Post Index</h1>
        <ul>
          {posts.map(post => (
            <li>{post.title.rendered}</li>
          ))}
        </ul>
      </Layout>
    );
  }
}
```

---

## Custom Server Page Routes

- server.js file exposes the express server that Next.js runs on
- gives us access to writing out own routes
- the Postlight setup comes with a post route already:

```javascript
server.get('/post/:slug', (req, res) => {
  const actualPage = '/post';
  const queryParams = { slug: req.params.slug, apiRoute: 'post' };
  app.render(req, res, actualPage, queryParams);
});

// or you could write it like this
server.get('/post/:slug', (req, res) => {
  app.render(req, res, '/post', { slug: req.params.slug });
});
```

- NOTE: you need to restart the frontend process to see changes in the server.js

- Now back in our version we're creating a route for /articles/post

```javascript
server.get('/articles/:slug', (req, res) => {
  app.render(req, res, '/blogPost', { slug: req.params.slug });
});
```

- we then create a blogPost.js file in our pages folder
- to access the correct data we need to go back to the wp-json and search for slug
- the one we're looking for is: "/postlight/v1/post"
  http://localhost:8080/wp-json/postlight/v1/post
- this needs an additional parameter, the slug
- so to show the 'sample post' post we add a parameter query to the URL: slug=sample-post
  http://localhost:8080/wp-json/postlight/v1/post?slug=sample-post
- then back in our blogPost.js we need to change the getInitialProps:
- the slug is actually part of the route as a query parameter
- the slug doesn't automatically get added to props. Instead we have to use something called 'context'

```javascript
static async getInitialProps(context) { // add context as an argument
  const slug = context.query.slug; // now we pull the slug from the query parameter

  const postRes = await fetch(
    `${Config.apiUrl}/wp-json/postlight/v1/post?slug=${slug}` // fetching the new data depending on the page slug
  );
  const post = await postRes.json();
  return {
    post
  };
}
```

- then to use it in our render it looks like this:

```javascript
render() {
  const { post } = this.props; // destructuring
  return (
    <div>
      <h1>{post.title.rendered}</h1>
    </div>
  );
}
```

## API Calls with componentWillMount

- getInitialProps only works on Pages not on Components
- instead we need to use componentWillMount in our components
- we basically replace one for the other
- we do need to update the return to use setState instead
- we also need to set the state to an empty array

- so now we've created a new component called PostIndex.js

```javascript
import React, { Component } from 'react';
import fetch from 'isomorphic-unfetch';
import Link from 'next/link'; // we import this so we can use links
import { Config } from '../config';

export default class PostIndex extends Component {
  // first we set state and make posts and empty array
  // this means its available from the start
  state = {
    posts: []
  };

  // out getInitialProps component now becomes componentWillMount
  async componentWillMount() {
    const postsRes = await fetch(`${Config.apiUrl}/wp-json/wp/v2/posts`);
    const posts = await postsRes.json();
    this.setState({
      // the return becomes a this.setState
      posts
    });
  }

  render() {
    const { posts } = this.state; //this.state rather than this.props
    return (
      <section>
        <h3>Archive</h3>
        <ul>
          {posts.map(post => (
            <li key={post.id}>
              <Link
                href={`/post?slug=${post.slug}&apiRoute=post`}
                as={`/post/${post.slug}`}
              >
                <a>{post.title.rendered}</a>
              </Link>
            </li>
          ))}
        </ul>
      </section>
    );
  }
}
```

- within the render method we:

1.  set a key for each item using the post.id
2.  we wrap the text in a <Link> tag
3.  this tag requires a href and an as attribute
4.  the href relies on the route in server.js, hence the apiRoute call and the query params
    e.g. const queryParams = { slug: req.params.slug, apiRoute: "post" };
5.  We also need to wrap the text in an <a> tag with no properties - very Next.js thing
6.  much of this came up in the errors in the console
7.  The original index.js file had this link code present in the file
8.  We were able to remove the const post section and ref to this in index.js as its all now contained within the component
9.  makes for something much more reusable

## Passing Props to API calls

- by default WP has set the number of posts to retreave to 10
- you can find this in the wp-json file if you search for 'posts' then look at the args
- you'll see per_page
- to change this in our component we have to add a query param to our fetch

```javascript
const postsRes = await fetch(
  `${Config.apiUrl}/wp-json/wp/v2/posts?per_page=20`
);
```

- an even better way to deal with this is with props instead
- this makes the component more reusable again

```javascript
  // first we add default props of 5
  static defaultProps = {
    limit: 5
  };

  state = {
    posts: []
  };

  async componentWillMount() {
    const { limit } = this.props; // destructuring again
    const postsRes = await fetch(
      `${Config.apiUrl}/wp-json/wp/v2/posts?per_page=${limit}` // then reference the limit here
    );
    const posts = await postsRes.json();
    this.setState({
      posts
    });
  }
```

- then to set the limit differently in the component we simple set the props:
  <PostIndex limit={20} />
- to add another param to the fetch just separate the next one with & - eg.
  ?per_page=20&param=value

## Basic Layout and Starting our menu
