# Portfolio Strapi Site

- initial setup with git repo and separate strapi install
- [Repo: starter gatsby files](https://github.com/john-smilga/gatsby-personal-site-2019-starter)
- currently the gatsby and strapi installs are in separate folders - I've gone with frontend and server
- to install Strapi we've navigated to the server folder and:
  npx create-strapi-app . --quickstart

- because we've installed with npx supposedly we need to us npx when we restart the server: npx strapi develop - **not sure about this???**

## Content Type Setup
- when setting up a content type we navigate to **Content Type Builder**
- we then **Add a Content Type**
- when we name the new content type we use the singular name eg. course
- but then in our Content Types list we'll see it as Courses
- once setup you can add your content fields -eg. title, images etc
- then add your content
- to make this new content accessible we need to get Strapi to setup routes for it. We do this under **Roles & Permissions**
- we then go to Public and we should see our Course content type show up
- we need to select **find** & **findone** from the list
- under Advanced settings you'll now see GET/courses/ & GET/courses/:id routes
- once we save that we can now navigate to: http://localhost:1337/courses/
- we should see our courses listed in json format

## Connect to Strapi
- need to grab the [gatsby-source-strapi plugin](https://www.gatsbyjs.org/packages/gatsby-source-strapi/?=strapi)
- we then need to add the settings to our gatsby-config.js

```js
{
    resolve: `gatsby-source-strapi`,
    options: {
      apiURL: `http://localhost:1337`,
      queryLimit: 1000, // Default to 100
      contentTypes: [`course`, `user`],
      // Possibility to login with a strapi user, when content types are not publically available (optional).
      loginData: {
        identifier: "",
        password: "",
      },
    },
  },
```

- within this config we need add course as a content type (or whatever other ones you've added)
- don't need to edit anything else for now
- we then need to restart the Gatsby server

```js
// - to query our courses with graphql:
query Courses {
  allStrapiCourse(sort: {fields: published, order: DESC}) {
    nodes {
      id
      title
      size
      url
      image {
        childImageSharp {
          fluid(maxWidth: 600) {
            src
          }
        }
      }
    }
  }
}

```
## Getting our courses into Gatsby
- we've setup a Courses folder in components
- then added Course.js and Courses.js
- we import Course.js into Courses.js
- then in pages/courses.js we've imported the Courses.js component

- in our Courses.js component we use the graphql query from above and setup a map to loop over our data

```js
import React from "react"
import Course from "./Course"
import { graphql, useStaticQuery } from "gatsby"
import styles from "../../css/courses.module.css"
import Title from "../Title"

// we've changed the image query to use GatsbyImageSharpFluid_withWebp
const query = graphql`
  {
    allStrapiCourse(sort: { fields: published, order: DESC }) {
      nodes {
        id
        title
        size
        url
        image {
          childImageSharp {
            fluid(maxWidth: 600) {
              ...GatsbyImageSharpFluid_withWebp
            }
          }
        }
      }
    }
  }
`

const Courses = () => {
  const {
    allStrapiCourse: { nodes: courses }, // destructure allStrapiCourse and relabel nodes as courses
  } = useStaticQuery(query)

  return (
    <section className={styles.courses}>
      <Title title="all" subtitle="courses" />
      <div className={styles.center}>
        {courses.map(item => {
          return <Course key={item.id} {...item} />
          // using the items as a spread for the Course component
        })}
      </div>
    </section>
  )
}
export default Courses

```
- we then do much the same process for th home page with a new component called LastestCourses.js in Home
- this is just a copy of Courses.js with names changed and the limit added to the query
- the new component is added to home and we are set.

## Combining scripts
- you can setup a combined start script that runs the strapi server then gatsby develop
- add both to a single folder
- add a package.json
- then create a new script:

```js
"start": "(cd server && npm run develop) & (cd frontend && gatsby develop)"
```
- you may get errors from time to time. Best to clean the gatsby build
