# Gatsby & WordPress course - Udemy

## Section 1: Intro & Setup

- first up do an install of Wordpress
- and another for a new Gatsby site

- we then install the gatsby-source-wordpress plugin
- then paste in the config setup in gatsby-config.js
- for now we just add our baseUrl: gatsby-wordpress-portfolio.local

## Section 2: Rendering pages and querying data with GraphiQL & GraphQL

### 6. Querying WordPress data with GraphiQL browser tool

- by default the gatsby-source-wordpress plugin queries some main routes in the config:

```javascript
includedRoutes: [
  "**/categories",
  "**/posts",
  "**/pages",
  "**/media",
  "**/tags",
  "**/taxonomies",
  "**/users",
],
```

- to start a query in GraphiQL CTRL + SPACE
- now with this plugin we can query our Wordpress site

```javascript
{
  allWordpressPage {
    edges {
      node {
        id
        title
        date
      }
    }
  }
}
```

### 7. Querying WordPress data with GraphiQL from within Gatsby

- after importing graphql and StaticQuery from gatsby we need to setup our StaticQuery
- this pulls in the data with graphql then renders it to the page

```javascript
import React from 'react';
import { graphql, StaticQuery } from 'gatsby';

import Layout from '../components/layout';
import SEO from '../components/seo';

const IndexPage = () => (
  <Layout>
    <SEO title="Home" />
    <StaticQuery
      query={graphql`
        {
          allWordpressPage {
            edges {
              node {
                id
                title
                date
                content
              }
            }
          }
        }
      `}
      render={props => (
        <div>
          {props.allWordpressPage.edges.map(page => (
            <div key={page.node.id}>
              <h1>{page.node.title}</h1>
              <div dangerouslySetInnerHTML={{ __html: page.node.content }} />
            </div>
          ))}
        </div>
      )}
    />
  </Layout>
);

export default IndexPage;
```

### 9. Generating Gatsby pages dynamically from Wordpress pages & Posts

- in the previous lecture he supplies an older version of the gatsby-node.js setup previously supplied by gatsby-source-wordpress plugin
- the new version they now have uses async await - **might need to research if we can use than instead**
- with the old version we also need to add: npm install --save bluebird

- in the query for both pages and posts we need to add: title, content
- we also change the slug for the posts to also include /post/
- next we create a template folder and add post.js and page.js to use
- at first we just setup a simple component in each:

```javascript
import React from 'react';

export default ({ pageContext }) => (
  <div>
    <h1>{pageContext.title}</h1>
  </div>
);
```

- the **context: edge.node** in our gatsby-node.js file gives us access to the props coming from this

- to get the home page working (now we've deleted the index.js page) is we change the name of the page in WordPress to 'home'
- then in our gatsby-node.js we add a createRedirect

```javascript
const { createPage, createRedirect } = actions;
createRedirect({
  fromPath: '/',
  toPath: '/home',
  redirectInBrowser: true,
  isPermanent: true
});
```

- [source of redirect tip](https://www.gatsbyjs.org/docs/actions/#createRedirect)

## Section 3: Creating the Menu

### 12. Removing wordpress frontend & create Wordpress menu

- first up Tom has supplied a simple pared back theme to use
- [Tom's Github](https://github.com/tomphill)
- first up we grab the wp-gatsby-js-theme-starter and install

- we then setup the new home page and portfolio pages with dummy content
- next we setup a new Main menu menu
- to query this menu with the Rest API we need to download a plugin called WP API Menus & install
- https://wordpress.org/plugins/wp-api-menus/
- now if we query our menus they should show up (http://gatsby-wordpress-portfolio.local/wp-json/wp-api-menus/v2/menus/)
- we also need to add it as a route in the gatsby-config.js we added with gatsby-source-wordpress
- any route we want to use needs to be register with this plugin
- we can also remove the excluded routes text

- back in GraphiQL we can query for our menu

```javascript
{
  allWordpressWpApiMenusMenusItems {
    edges {
      node {
        items {
          title
          object_slug
        }
      }
    }
  }
}
```

### 14. Creating the Menu component in Gatsby

- next we create a MainMenu.js component which pulls in the query above and rendered the links to the page

```javascript
import React from 'react';
import { graphql, StaticQuery, Link } from 'gatsby';

const MainMenu = () => (
  <StaticQuery
    query={graphql`
      {
        allWordpressWpApiMenusMenusItems {
          edges {
            node {
              items {
                title
                object_slug
              }
            }
          }
        }
      }
    `}
    render={props => (
      <div>
        {props.allWordpressWpApiMenusMenusItems.edges[0].node.items.map(
          item => (
            <Link to={item.object_slug} key={item.title}>
              {item.title}
            </Link>
          )
        )}
      </div>
    )}
  />
);

export default MainMenu;
```

- we also cleaned up the layout.js file and included the new MainMenu.js comp
- the code above assumes at this point we only want to render the first menu
- we then added the Layout comp to our page.js template
- to avoid odd code preview like the & we can render the h1 with:

  <h1 dangerouslySetInnerHTML={{ __html: pageContext.title }} />

- if we create a extra menu we can set queries on our graphql
- allWordpressWpApiMenusMenusItems(skip: 1) { // skips the first menu
- (limit: 1) // shows only one menu
- to filter with more control we can do:

```javascript
{
  allWordpressWpApiMenusMenusItems(filter: {
    name: {
      eq: "Main menu"
    }
  }) {
    edges {
      node {
        items {
          title
          object_slug
        }
      }
    }
  }
}
```

- this is what we use in our MainMenu component instead

### 16. Styling the Menu with React styled-components

- here we added styled components
- added global styles to the Layout
- also looked at how you can import some **google fonts**
- In main menu showed how you could target the Link element with styled components

### 17. Retrieving and rendering the Wordpress site title and tagline

- to grab the site meta from WP our query looks like this:

```javascript
{
  allWordpressSiteMetadata {
    edges {
      node {
        name
        description
      }
    }
  }
}
```

- we can setup a SiteInfo.js comp to render this then add it to our main menu

## Section 5: Querying & rending custom portfolio post types in Gatsby

- prior to this we added a portfolio CPT and some content in WP
- we can now access our portfolio posts on:
  http://gatsby-wordpress-portfolio.local/wp-json/wp/v2/portfolio
- we now need to add this to our gatsby-config.js file as an included route for gatsby-source-wordpress plugin
- now we can query for this in the graphiQL explorer

```javascript
{
  allWordpressWpPortfolio {
    edges {
      node {
        id
        title
        slug
        excerpt
        content
        featured_media {
          source_url
        }
      }
    }
  }
}
```

- we can then render this with a static query inside our new PortfolioItems.js comp
- to render our single portfolio items we need to edit the gatsby-node.js file
- basically we've just converted the Posts setup to portfolio instead

  1. replace query with allWordpressWpPortfolio query
  2. const portfolioTemplate = path.resolve("./src/templates/portfolio.js")
  3. ...

  ```javascript
    _.each(result.data.allWordpressWpPortfolio.edges, edge => {
      createPage({
        path: `/portfolio/${edge.node.slug}/`,
        component: slash(portfolioTemplate),
        context: edge.node,
      })
  ```

  4. then convert out post.js template to portfolio.js
  5. the context passes all the data in as props to **pageContext** within our template

### 23. Create custom page template in Wordpress to render portfolio items

- setup a new page template in WP
- only really needs the Template name comment, no other content
- set the portfolio page to that page template
- back in gatsby-node.js make sure we have **template** within our allWordpressPage query
- we then add an additional template path

```javascript
const pageTemplate = path.resolve("./src/templates/page.js")
const portfolioUnderContentTemplate = path.resolve(
  "./src/templates/portfolioUnderContentTemplate.js"
)

// and a conditional for the component based on the wp template chosen
component: slash(
  edge.node.template === "portfolio_under_content.php"
    ? portfolioUnderContentTemplate
    : pageTemplate
),
```

- we then setup our portfolioUnderContentTemplate.js template with the PortfolioItems.js component included

## Section 6: Advanced custom fields

- first up we install **ACF** plugin and also **ACF to REST API** plugin
- we've setup a url in the portfolio item to render
- to query it we just do

```javascript
{
  allWordpressWpPortfolio {
    edges {
      node {
        title
        acf {
          portfolio_url
        }
      }
    }
  }
}
```

- so we need to add this to our gatsby-node.js for portfolio
- then we can use it within our portfolio.js template

## Section 8: Create a blog with pagination

- need to make sure we're looking for posts route in gatsby-config.js
- once we've setup some posts we can query it with

```javascript
{
  allWordpressPost {
    edges {
      node {
        id
        date
        title
        excerpt
      }
    }
  }
}
```

- we then add a createPage in gatsby-node.js for generating the blog listing page and also adding pagination
- date formatting in graphql can be done in the query:
  date(formatString: "Do MMM YYYY HH:mm") // 21s July 2019 05:35
- moment.js has a great guide for how to format date strings on their site

- to create the single blog post pages we can add a createPage within our existing Posts query in gatsby-node.js
- we can also just use the pageTemplate for now as it does all we need

```javascript
const pageTemplate = path.resolve('./src/templates/page.js');
_.each(posts, post => {
  createPage({
    path: `/post/${post.node.slug}`,
    component: slash(pageTemplate),
    context: post.node
  });
});
```
