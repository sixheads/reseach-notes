# MDX Gatsby Blog - Udemy

- Starter Theme - https://www.gatsbyjs.org/starters/gatsbyjs/gatsby-starter-hello-world/

## MDX

- the plugin we use is: https://www.gatsbyjs.org/packages/gatsby-plugin-mdx/?=mdx
- a simple installation just requires we add it to our gatsby-config.js like so:
  plugins: ["gatsby-plugin-mdx"],
- installed the **MDX** plugin for VSCode to get syntax highlighting
- we can add inline code to our MDX: `<h1>this is my heading 1 - inline code</h1>`
- we can also add a code block:

<div className="code">

```javascript
const name = "John Smith"
function Hello(name) {
  console.log(name)
}
hello(name) // john smith
```

</div>

- its important to add the spaces around the code block for it to render properly
- now we can also style that with our css

### Import components into MDX

- to import a component into our MDX file we just use a normal import statement at the top of the file:
  import Counter from '../components/counter.js'

### Post setup

- add folders for each post inside of posts:
- 1-first-post
- inside of that we add our mdx file
- for images we add an image folder in each post folder

### Frontmatter

- we use this data to get our post with GraphQL
- also lets us display some information
- we add frontmatter to the top of the mdx file between ---:

---

title: first post
slug: first-post
image: ./images/image-1.jpeg
date: 2019-12-30
author: John Smith

---

### Plugins

- to access the images in our posts we need a few plugins
- we also need:
  1. gatsby-source-filesystem // see the file system
  2. gatsby-image + gatsby-transformer-sharp & gatsby-plugin-sharpe // transform our images

### Query for Posts

- now we can query these MDX files and all our posts with GraphQL

```javascript
{
  allMdx(sort: {
    fields: frontmatter___date, order: DESC
  }) {
    totalCount
    edges {
      node {
        frontmatter {
          title
          slug
          date(formatString: "MMMM Do, YYYY")
          author
          image {
            childImageSharp {
              fluid {
                srcSet
              }
            }
          }
        }
        excerpt
      }
    }
  }
}
```

- we then setup a PostList.js and PostCard.js files to display our posts
- in our index.js page we create a new static query using the above query and pass it as props to our PostList component
- to test if we can getting our posts into the PostList first up you can just add a console:

```javascript
const PostList = ({ posts }) => {
  console.log(posts)
```

- now we can add some styles and map over our PostCard component
- in our PostCard component we need to pull in the post prop from our PostList.js file
- then destructure our props to make things easier
- then we can output them in our component

### Creating our Post Pages

- we need to setup a template for the posts to use
- we do that in templates/post-template.js
- then we need to setup the gatsby-node.js file with the createPages function

```javascript
exports.createPages = async ({ actions, graphql }) => {
  const { createPage } = actions
  const {
    data: {
      allMdx: { edges: posts }, // destructure edges and give it an alias of posts
    },
    // this is our query for just the slug
  } = await graphql(`
    {
      allMdx {
        edges {
          node {
            frontmatter {
              slug
            }
          }
        }
      }
    }
  `)
  // we then loop over the posts and call the createPage method
  posts.forEach(({ node }) => {
    const { slug } = node.frontmatter // destructured slug
    createPage({
      path: slug,
      component: require.resolve("./src/templates/post-template.js"),
      context: {
        slug: slug, // we use this variable later in our template
      },
    })
  })
}
```

- now if you go to the 404 page you will see pages created for each of our posts
- to test our new page context created in the gatsby-node.js file inside our post-template.js file just console log it:

```javascript
const postTemplate = ({ pageContext }) => {
  console.log(pageContext)
```

- we use this slug variable to query data from that particular post - so very important
- our query will look like this:

```javascript
query getPost($slug:String!){ // query our slug & make it required
  mdx(frontmatter: {slug: {eq:$slug}}) { // pass that in as the slug required
    frontmatter {
      title
      slug
      date(formatString: "MMMM Do, YYYY")
      author
      image {
        childImageSharp {
          fluid {
            ...GatsbyImageSharpFluid_withWebp
          }
        }
      }
    }
    body // this exports the body of the mdx file for us
  }
}
```

- you can use the Query Variables field in the GraphiQL explorer to test this:

```javascript
{
  "slug": "first-post"
}
```

- this slug variable changes for each time a page is created so we get the data related to that particular post when it creates our page programmatically

- now using this query we can access the data in our component
- to show the body of the MDX file we need to use the MDXRenderer
   <MDXRenderer>{body}</MDXRenderer>

