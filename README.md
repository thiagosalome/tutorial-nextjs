# tutorial-nextjs

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
