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

#### What You’ll Learn in This Lesson

In this lesson, you’ll learn:

* How to add static files (images, etc) to Next.js.
* How to customize what goes inside the <head> for each page.
* How to create a reusable React component which is styled using CSS Modules.
* How to add global CSS in pages/_app.js.
* Some useful tips for styling in Next.js.

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
