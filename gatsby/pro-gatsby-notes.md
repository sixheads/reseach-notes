# Pro Gatsby Course - LevelUp Tuts

- start with installing gatsby CLI if not done already:
  npm install --global gatsby-cli

- to create a new gatsby site:
  gatsby new gatsby-site

- then inside the new site folder (development mode - hot reloading):
  gatsby develop

- a built version:
  gatsby build

- server version for testing:
  gatsby serve

- creates a site running on:
  http://localhost:8000/
  http://localhost:8000/___graphql

## Files Explained

- public:
- src: entire application lives, this is where you are editing
- gatsby-browser.js:
- gastby-config.js: this holds key info about the project (meta) & plugin info
- gastby-node.js: where you are connecting to node APIs & graphql stuff

- pages: relates to our real pages - handles routing. So you could create a hello.js file in pages then go to the URL/hello and it will load that page

## Pages & Nav

- creates a new page in pages folder called about.js
- uses snippet react plugin 'rcc' to create new component
- now you can navigate to:
  http://localhost:8000/about

- links use the gatsby-link package that comes with the gatsby install
    <Link to="/about/">About</Link>

## Importing Assets

- you could just drop the images into the public > static folder
- better to let webpack package it up and use it that way.

- so we drop it in the src folder in 'images' folder
- to use this in our Header.js:
  import logo from "../images/logo.svg";
- then to use it in the component:
  <img src={logo} alt="Level Up Logo" />
- reduces the requests
- if your image is less that 10,000 bytes gatsby will return a dataURI instead

## Styled Components

- reusable css, scoped!
- can use other CSS options

- need to install the plugin(and packages):
  npm install --save styled-components gatsby-plugin-styled-components

```javascript
import React from 'react';
import Link from 'gatsby-link';
import styled from 'styled-components'; // import styled components firstg

import logo from '../images/logo.svg';

// then write some styled components
const HeaderWrapper = styled.div`
  background: #524763;
  margin-bottom: 1.45rem;

  h1 {
    img {
      height: 80px;
    }
  }
`;
const HeaderContainer = styled.div`
  margin: 0 auto;
  max-width: 960px;
  padding: 1.45rem 1.0875rem;
`;

const Header = ({ siteTitle }) => (
  <HeaderWrapper>
    // add the styled components to the html
    <HeaderContainer>
      <h1 style={{ margin: 0 }}>
        <Link
          to="/"
          style={{
            color: 'white',
            textDecoration: 'none'
          }}
        >
          <img src={logo} alt="Level Up Logo" />
        </Link>
      </h1>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/about">About</Link>
          </li>
        </ul>
      </nav>
    </HeaderContainer>
  </HeaderWrapper>
);

export default Header;
```

- we also need to add:
  gatsby-plugin-styled-components
- this goes in the gatsby-config.js file in the plugins section
- this compiles the CSS into a static file so we don't get that flash of unstyled content

## Understanding Data Layer with GraphQL

- the GraphiQL is basically a query tester:
  http://localhost:8000/___graphql

- GraphQl is just a specification for writing queries
- gives some nice auto complete
- clicking the DOCs button lets us look at the documentation
- lets you write a graphQL query and it returns a javascript object when you hit the play button

```javascript
{
	site {
    siteMetadata {
      title
      desc
    }
  }
}

// returns the object:

{
  "data": {
    "site": {
      "siteMetadata": {
        "title": "Gatsby Default Starter",
        "desc": "A new blog"
      }
    }
  }
}
```

- to add this to our file we can go to index.js in pages
- gatsby gives us a variable named query we can export from our page
- we need to give the query a name also
- we add it as an export:

```javascript
export const query = graphql`
  query SiteMeta {
    site {
      siteMetadata {
        title
        desc
      }
    }
  }
`;
```

- now we can access this query by adding it as a prop call {data} in our component
  const IndexPage = ({data}) => (

- not in our component we can use this data:

  <p>{data.site.siteMetadata.title}</p>

- you can't use data query on components only add queries on layouts & pages
- you then need to pass the data down to your components. eg. in index.js layout file by adding 'data':
  const Layout = ({ children, data }) => (
- you then need to add data to the component inside of our layout:
  <Header data={data} />
- you also need to make sure you use different query names when you add then int
- you can add onto a query as needed

## Image Transformation & Manipulation

- first need to install gatsby-image gatsby-transformer-sharp gatsby-plugin-sharp
- then add gatsby-transformer-sharp gatsby-plugin-sharp to gatsby config file

- we also need to query the file system. To do that we add an object into our plugins in the config
- we also need to install the gatsby-source-filesystem package

```javascript
{
  resolve: 'gatsby-source-filesystem',
  options: {
    name: 'img',
    path: `${__dirname}/src/images`
  }
}
```

- this sets up gatsby to look inside the images folder we created in
- now to add an example background image we need ot add to our graphql query:

```
background: imageSharp(id: {regex: "/bg.jpeg/" }) {
  sizes(maxWidth: 1240) {
    ...GatsbyImageSharpSizes
  }
}
```

- this assigns a variable to the image then sets up a regex point to the image running it through imageSharp and also exporting out a whole lot of sources
- can inspect it in react dev tools
- to pull in into our page (index.js):

```javascript
import Img from "gatsby-image" // import plugin first

// then in your html
<Img sizes={data.background.sizes} />
```

- 'data' is our props query, 'background' is the name we gave the image & we find 'sizes' when we view the props in the react dev tools
- gatsby progressively loads in the image for you

## Image Component

- you can pass in a whole lot of transformation into the imageSharp plugin
- like B&W
  sizes(maxWidth: 1240, grayscale: true) {

- you can add styles to elements like images with React as well:

```
<Img
 style={{
   position: "absolute",
   left: 0,
   top: 0,
   width: "100%",
   height: "100%"
 }}
 sizes={data.background.sizes}
/>
```

- this adds the styles as an object instead, hence the JS CSS version instead

## How to Query Markdown Files in Gatsby

- to access our markdown files we do something similar to what we did with the image
- first need to install another package:
  gatsby-transformer-remark
- then in our config file under plugins
- we also need to tell gatsby where to look so we can reuse bits of the image version

```javascript
{
  resolve: 'gatsby-source-filesystem',
  options: {
    name: 'src',
    path: `${__dirname}/src/`
  }
},
```

- this makes our file available to graphql
- now we can type File or AllFile in graphql, we can also access our markdown files using the remark plugin with allMarkdownRemark

```
allMarkdownRemark {
  edges {
    node {
      id
      frontmatter {
        title
        date(formatString: "MMMM DD YYYY")
      }
      html
    }
  }
}
```

- in our markdown file we need to start it with some frontmatter

```
---
title: "Welcome to the new blog"
date: "2018-02-22"
---

## Hello

- just start writing mardown
```

- to add this to a page and loop through the markdown files we first need to add the remark query to our main query (index.js in pages)
- then to query for it in the template:

```javascript
{
  data.allMarkdownRemark.edges.map(({ node }) => {
    return <PostListing post={node} />;
  });
}
```

- this queries the data and loops through each node with map() then returns a new component (need to create that separately) and get post which is equal to an individual node.

## Working with Markdown Posts

- first make a folder called 'posts' then move all markdown files into that folder
- we then need to create our new PostListing component
  components > posts > PostListing.js

```javascript
import React from "react";
import Link from "gatsby-link;

const PostListing = ({ post }) => (
  <article>
    <h3>{post.frontmatter.title}</h3>
    <span>{post.frontmatter.date}</span>
  </article>
)
```

- React/Gatsby also like an key/id assigned to arrays so we get an id automatically so we just add it to our component
  return <PostListing key={node.id} post={node} />;

- because the map() is only returning one component we can have a cleaner version:

```javascript
{
  data.allMarkdownRemark.edges.map(({ node }) => (
    <PostListing key={node.id} post={node} />
  ));
}
```

- to get the html from our markdown file we can't just return {post.html} as it just appears an html with all the tags. Instead we:

```html
<div
  dangerouslySetInnerHTML={{
    __html: post.html
  }}
/>
```

- remark also lets you grab the excerpt instead

1.  add 'excerpt' into our query
2.  then in our component: <p>{post.excerpt}</p>
3.  this returns a string so we don't have to use the dangerouslySet... stuff

- there are options for the excerpt like the length which you can change in the query:
  excerpt(pruneLength: 280)

## Adding Posts as Pages

- need to setup some queries in gatsby-node.js
- this will query for our posts then create pages out of that data

```javascript
const path = require('path');
const { createFilePath } = require('gatsby-source-filesystem');

// creates a node and a slug
exports.onCreateNode = ({ node, getNode, boundActionCreators }) => {
  const { createNodeField } = boundActionCreators;
  if (node.internal.type === 'MarkdownRemark') {
    const slug = createFilePath({
      node,
      getNode,
      basePath: 'posts'
    });
    createNodeField({
      node,
      name: 'slug',
      value: `/posts${slug}`
    });
  }
};

// uses a query to create a bunch of new pages
exports.createPages = ({ graphql, boundActionCreators }) => {
  const { createPage } = boundActionCreators;
  return new Promise((resolve, reject) => {
    graphql(`
      {
        allMarkdownRemark {
          edges {
            node {
              fields {
                slug
              }
            }
          }
        }
      }
    `).then(result => {
      result.data.allMarkdownRemark.edges.forEach(({ node }) => {
        createPage({
          path: node.fields.slug,
          component: path.resolve('./src/pages/about.js'),
          context: {
            slug: node.fields.slug
          }
        });
      });
      resolve();
    });
  });
};
```

## Context Queries for our Blog Posts

- a query using markdownRemark will give us the data for a single post
- look at the markdownRemark package to see the options
- here's the query we can add to our PostPage.js component:

```javascript
export const query = graphql`
query BlogPostQuery($slug: String!) {
  slug: {
    eq: $slug
  }
  }) {
    html
    frontmatter {
      title
    }
    excerpt
  }
}

`;
```

- this slug is being defined in our context query in node and equates to our post slug
- now in our template we can pull out the exerpt with:

  <p>{data.markdownRemark.excerpt}</p>

- here's the example PostPage.js component

```javascript
import React, { Component } from 'react';

export default class PostPage extends Component {
  render() {
    const { data } = this.props;
    return (
      <div>
        <span>{data.markdownRemark.frontmatter.date}</span>
        <h1>{data.markdownRemark.frontmatter.title}</h1>
        <div
          dangerouslySetInnerHTML={{
            __html: data.markdownRemark.html
          }}
        />
      </div>
    );
  }
}

export const query = graphql`
  query BlogPostQuery($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
        date(formatString: "MMMM DD YYYY")
      }
    }
  }
`;
```

## Animated Page Transitions with the web animation API

- gatsby uses React router which gives us access to the location
- to use location we add it as a prop in our layout template then pass it down to components that need it:
  const TemplateWrapper = ({ children, data, location }) =>
- then in our header component for example:
  <Header data={data} location={location} />

## Query sorting

- to order our post we can use markdownRemark again in the query

```
allMarkdownRemark(sort: {
  fields: [frontmatter___date],
  order: DESC
})
```

- this will order our posts in decending order

## Building for Production
- gatsby build creates a static version of our site in the public folder
- this can be dropped onto any hosting
- gatsby serve 