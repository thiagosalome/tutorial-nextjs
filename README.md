# tutorial-nextjs

[Link do Tutorial](https://nextjs.org/learn/basics/create-nextjs-app)

## Create a Next.js APP

To build a complete wbe application with React from scratch, there are many important details you need to consider:

* Code has to be bundled using a bundler like webpack and transformed using a compiler like Babel.
* You need to do production optimizations such as code splitting.
* You might have to write some server-side code to connect your React app to your data store.

### Setup

To create a Next.JS app, open your terminal and run the following command:

```bash
npx create-next-app nextjs-blog --use-yarn --example "https://github.com/vercel/next-learn-starter/tree/master/learn-starter"
```

---

## Navigate Between Pages

### Link Component

Creating a Link in Next.js

```js
import Link from 'next/link'
<Link href="/posts/first-post">Next.js!</Link>
```

### Client-Side Navigation

The ```Link``` component enables client-side navigation between two pages in the same Next.js app.

#### Code splitting and prefetching

Next.js does code splitting automatically, so each page only loads what’s necessary for that page.

Furthermore, in a production build of Next.js, whenever ```Link``` components appear in the browser’s viewport, Next.js automatically prefetches the code for the linked page in the background. By the time you click the link, the code for the destination page will already be loaded in the background, and the page transition will be near-instant!

---

## Assets, Metadata, and CSS

Next.js has built-in support for CSS and Sass. For this course, we will use CSS.

### Assets

Next.js can serve static files, like images, under the top-level public directory. Files inside public can be referenced from the root of the application similar to pages.

### Metadata

```jsx
<Head>
  <title>Create Next App</title>
  <link rel="icon" href="/favicon.ico" />
</Head>
```

Notice that <Head> is used instead of the lowercase <head>. <Head> is a React Component that is built into Next.js. It allows you to modify the <head> of a page.

### CSS Styling

```jsx
<style jsx>{`
  …
`}</style>
```

This is using a library called styled-jsx. It’s a “CSS-in-JS” library — it lets you write CSS within a React component, and the CSS styles will be scoped (other components won’t be affected).

### Layout Component

#### Automatically Generates Unique Class Names

![CSS Modules](https://nextjs.org/static/images/learn/assets-metadata-css/devtools.png)

This is what CSS Modules does: It automatically generates unique class names. As long as you use CSS Modules, you don’t have to worry about class name collisions.

Furthermore, Next.js’s code splitting feature works on CSS Modules as well. It ensures the minimal amount of CSS is loaded for each page. This results in smaller bundle sizes.

CSS Modules are extracted from the JavaScript bundles at build time and generate .css files that are loaded automatically by Next.js.

### Global Styles

To load global CSS files, create a file called _app.js under pages and add the following content:

```jsx
export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}
```

This App component is the top-level component which will be common across all the different pages. You can use this App component to keep state when navigating between pages, for example.

#### Adding Global CSS

In Next.js, you can add global CSS files by importing them from pages/_app.js. You cannot import global CSS anywhere else.

### Polishing Layout

[...]

### Styling Tips

#### Using classnames library to toggle classes

classnames is a simple library that lets you toggle class names easily. You can install it using npm install classnames or yarn add classnames.

```css
.success {
  color: green;
}
.error {
  color: red;
}
```

```jsx
import styles from './alert.module.css'
import cn from 'classnames'

export default function Alert({ children, type }) {
  return (
    <div
      className={cn({
        [styles.success]: type === 'success',
        [styles.error]: type === 'error'
      })}
    >
      {children}
    </div>
  )
}
```

---

## Pre-rendering and Data Fetching

### Pre-rendering

By default, Next.js pre-renders every page. This means that Next.js generates HTML for each page in advance, instead of having it all done by client-side JavaScript. **Pre-rendering can result in better performance and SEO.**

If your app is a plain React.js app (without Next.js), there’s no pre-rendering, so you won’t be able to see the app if you disable JavaScript.

#### Summary: Pre-rendering vs No Pre-rendering

![Pre-rendering](https://nextjs.org/static/images/learn/data-fetching/pre-rendering.png)

![No Pre-rendering](https://nextjs.org/static/images/learn/data-fetching/no-pre-rendering.png)

### Two Forms of Pre-rendering

Next.js has two forms of pre-rendering: Static Generation and Server-side Rendering. The difference is in when it generates the HTML for a page.

* [Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) is the pre-rendering method that generates the HTML at **build time**. The pre-rendered HTML is then reused on each request.

![Static Generation](https://nextjs.org/static/images/learn/data-fetching/static-generation.png)

* [Server-side Rendering](https://nextjs.org/docs/basic-features/pages#server-side-rendering) is the pre-rendering method that generates the HTML on **each request**.

![Server-side Rendering](https://nextjs.org/static/images/learn/data-fetching/server-side-rendering.png)

**OBS:** In development mode (when you run npm run dev or yarn dev), every page is pre-rendered on each request — even for pages that use Static Generation.

#### Per-page Basis

Importantly, Next.js lets you choose which pre-rendering form to use for each page. You can create a "hybrid" Next.js app by using Static Generation for most pages and using Server-side Rendering for others.

![Per-page Basis](https://nextjs.org/static/images/learn/data-fetching/per-page-basis.png)

#### When to Use Static Generation v.s. Server-side Rendering

#### When to use Static Generation and Sever-side Rendering

We recommend using Static Generation (with and without data) whenever possible because your page can be built once and served by CDN, which makes it much faster than having a server render the page on every request.

You should ask yourself: "Can I pre-render this page ahead of a user's request?" If the answer is yes, then you should choose Static Generation.

On the other hand, Static Generation is not a good idea if you cannot pre-render a page ahead of a user's request. Maybe your page **shows frequently updated data**, and the **page content changes on every request**. In that case, you can use Server-side Rendering. It will be slower, but the pre-rendered page will always be up-to-date

### Static Generation with and without Data

So far, all the pages we’ve created do not require fetching external data. **Those pages will automatically be statically generated** when the app is built for production.

![Static Generation without Data](https://nextjs.org/static/images/learn/data-fetching/static-generation-without-data.png)

However, for some pages, you might not be able to render the HTML without first fetching some external data.

![Static Generation with Data](https://nextjs.org/static/images/learn/data-fetching/static-generation-with-data.png)

#### Static Generation with Data using ```getStaticProps```

In Next.js, when you export a page component, you can also export an async function called getStaticProps. If you do this, then:

```js
export default function Home(props) { ... }

export async function getStaticProps() {
  // Get external data from the file system, API, DB, etc.
  const data = ...

  // The value of the `props` key will be
  //  passed to the `Home` component
  return {
    props: ...
  }
}
```

```
Note: In development mode, getStaticProps runs on each request instead.
```

### getStaticProps Details

As a pattern, when we need fetch data from a source, we create a file in lib/posts.js for example.

#### Fetch External API or Query Database

You can fetch the data from other sources, like an external API endpoint, and it’ll work just fine:

```js
export async function getSortedPostsData() {
  // Instead of the file system,
  // fetch post data from an external API endpoint
  const res = await fetch('..')
  return res.json()
}
```

You can also query the database directly:

```js
import someDatabaseSDK from 'someDatabaseSDK'

const databaseClient = someDatabaseSDK.createClient(...)

export async function getSortedPostsData() {
  // Instead of the file system,
  // fetch post data from a database
  return databaseClient.query('SELECT posts...')
}
```

**OBS**: This is possible because getStaticProps **runs only on the server-side**. It will never run on the client-side.

#### Development vs. Production

* In development (npm run dev or yarn dev), getStaticProps runs on every request.
* In production, getStaticProps runs at build time.

#### Only Allowed in a Page

```getStaticProps``` can only be exported from a page. You can’t export it from non-page files.

#### What If I Need to Fetch Data at Request Time

Static Generation is **not** a good idea if you cannot pre-render a page ahead of a user's request. Maybe your page shows frequently updated data, and the page content changes on every request.

In cases like this, you can try **Server-side** Rendering or skip pre-rendering.

### Fetching Data at Request Time

If you need to fetch data at **request time** instead of at build time, you can try **Server-side Rendering**:

![Server-side Rendering with Data](https://nextjs.org/static/images/learn/data-fetching/server-side-rendering-with-data.png)

To use Server-side Rendering, you need to export ```getServerSideProps``` instead of ```getStaticProps``` from your page. Exemple:

```js
export async function getServerSideProps(context) {
  return {
    props: {
      // props for your component
    }
  }
}
```

Because ```getServerSideProps``` is called at request time, its parameter ```(context)``` contains request specific parameters.

OBS: You should use getServerSideProps **only if you need to pre-render**. Time to first byte (TTFB) will be slower than getStaticProps because the server must compute the result on every request.

#### Client-side Rendering

If you do not need to pre-render the data, you can also use the following strategy (called Client-side Rendering):

![Fetch Data on the Client-Side](https://nextjs.org/static/images/learn/data-fetching/client-side-rendering.png)

This approach works well for user dashboard pages, for example. Because a dashboard is a private, user-specific page, SEO is not relevant, and the page doesn’t need to be pre-rendered.

#### SWR

The team behind Next.js has created a React hook for data fetching called SWR. We highly recommend it if you’re fetching data on the client side. Exemple:

```js
import useSWR from 'swr'

function Profile() {
  const { data, error } = useSWR('/api/user', fetch)

  if (error) return <div>failed to load</div>
  if (!data) return <div>loading...</div>
  return <div>hello {data.name}!</div>
}
```

---

## Dynamic Routes

### Page Path Depends on External Data

In this lesson, we’ll talk about the case where each page path depends on external data.

![Page Path Depends on External Data](https://nextjs.org/static/images/learn/dynamic-routes/page-path-external-data.png)

#### Overview of the Steps

![Overview of the Steps](https://nextjs.org/static/images/learn/dynamic-routes/how-to-dynamic-routes.png)

### Dynamic Routes Details

Here is some essential information you should know about dynamic routes.

#### Fetch External API or Query Database

Like getStaticProps, getStaticPaths can fetch data from any data source. In our example, getAllPostIds (which is used by getStaticPaths) may fetch from an external API endpoint:

```js
export async function getAllPostIds() {
  // Instead of the file system,
  // fetch post data from an external API endpoint
  const res = await fetch('..')
  const posts = await res.json()
  return posts.map(post => {
    return {
      params: {
        id: post.id
      }
    }
  })
}
```

#### Development v.s. Production

* In development (```npm run dev``` or ```yarn dev```), ```getStaticPaths``` runs on every request.
* In production, ```getStaticPaths``` runs at build time.

#### Fallback

Recall that we returned ```fallback: false``` from getStaticPaths. What does this mean?

If ```fallback``` is ```false```, **then any paths not returned by getStaticPaths will result in a 404 page**.

If ```fallback``` is ```true```, then the behavior of getStaticProps changes:

* The paths that have not been generated at build time will not result in a 404 page. Instead, Next.js will serve a “fallback” version of the page on the first request to such a path.

* In the background, Next.js will statically generate the requested path. Subsequent requests to the same path will serve the generated page, just like other pages pre-rendered at build time.

#### Catch-all Routes

Dynamic routes can be extended to catch all paths by adding three dots (...) inside the brackets. For example:

* pages/posts/[...id].js matches /posts/a, but also /posts/a/b, /posts/a/b/c and so on.

If you do this, in getStaticPaths, you must return an array as the value of the id key like so:

```js
return [
  {
    params: {
      // Statically Generates /posts/a/b/c
      id: ['a', 'b', 'c']
    }
  }
  //...
]
```

And params.id will be an array in getStaticProps:

```js
export async function getStaticProps({ params }) {
  // params.id will be like ['a', 'b', 'c']
}
```

---

## API Routes

### Creating API Routes

API Routes let you create an API endpoint inside a Next.js app. You can do so by creating a function inside the pages/api directory that has the following format:

```js
// req = request data, res = response data
export default (req, res) => {
  // ...
}
```

They can be deployed as Serverless Functions (also known as Lambdas).

### API Routes Details

#### Do Not Fetch an API Route from getStaticProps or getStaticPaths

You should not fetch an API Route from getStaticProps or getStaticPaths. Instead, write your server-side code directly in getStaticProps or getStaticPaths (or call a helper function).

Here’s why: getStaticProps and getStaticPaths runs only on the server-side. It will never be run on the client-side.

#### A Good Use Case: Handling Form Input

A good use case for API Routes is handling form input. For example, you can create a form on your page and have it send a POST request to your API Route.

#### Preview Mode

Static Generation is useful when your pages fetch data from a headless CMS. However, it’s not ideal when you’re writing a draft on your headless CMS and want to preview the draft immediately on your page.

Next.js has a feature called Preview Mode to solve the problem above, and it utilizes API Routes.
