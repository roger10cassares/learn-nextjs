# [ L E A R N - N E X T J  S ]

From: https://nextjs.org/learn/



# Create Next App

```bash
yarn create next-app
yarn add next react react-dom
cd learn-nextjs
yarn dev

tree .
├── Instrunctions.md
├── node_modules
├── package.json
├── pages
│   ├── api
│   │   └── hello.js
│   ├── _app.js
│   └── index.js
├── public
│   ├── favicon.ico
│   └── vercel.svg
├── README.md
├── styles
│   ├── globals.css
│   └── Home.module.css
└── yarn.lock

```



# Navigate Between Pages

- Create a new page using the integrated [file system routing](https://nextjs.org/docs/routing/introduction).
- Learn how to use the [`Link`](https://nextjs.org/docs/api-reference/next/link) component to enable client-side navigation between pages.
- Learn about built-in support for code splitting and prefetching.



```bash
./pages/index.js
----
import Link from 'next/link'

<h1 className="title">
  Read{' '}
  <Link href="/posts/first-post">
    <a>this page!</a>
  </Link>
</h1>

{' '} adds an empty space, which is used to divide text over multiple lines.
----


./pages/posts/first-post.js
----
import Link from 'next/link'

export default function FirstPost() {
  return (
    <>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </>
  )
}

----


```



# Assets, Metadata, and CSS

- How to add [static files](https://nextjs.org/docs/basic-features/static-file-serving) (images, etc) to Next.js.
- How to customize what goes inside the `<head>` for each page.
- How to create a reusable React component which is styled using [CSS Modules](https://nextjs.org/docs/basic-features/built-in-css-support#adding-component-level-css).
- How to [add global CSS](https://nextjs.org/docs/basic-features/built-in-css-support#adding-a-global-stylesheet) in [`pages/_app.js`](https://nextjs.org/docs/advanced-features/custom-app).
- Some useful tips for styling in Next.js.



```bash
mkdir -p ./public/images
wget https://github.com/vercel/next-learn-starter/raw/master/basics-final/public/images/profile.jpg ./public/images
rm ./public/vercel.svg
rm ./public/favicon.ico
```



## Assets

Next.js can serve **static assets**, like images, under **the top-level [`public` directory](https://nextjs.org/docs/basic-features/static-file-serving)**. Files inside `public` can be referenced from the root of the application similar to [`pages`](https://nextjs.org/docs/basic-features/pages).

The `public` directory is also useful for `robots.txt`, Google Site Verification, and any other static assets. 



```bash
...
├── public
│   ├── images
│       └── profile.jpeg
...
```

> profile.jpg image can be around 400px x 400px



next/image is am Image Component and Image Optimizaion

Supports WebP image format

Example:

```js
import Image from 'next/image'

const YourComponent = () => (
  <Image
    src="/images/profile.jpg" // Route of the image file
    height={144} // Desired size with correct aspect ratio
    width={144} // Desired size with correct aspect ratio
    alt="Your Name"
  />
)
```

 

### Metadata

Modifying <title> HTML Tag

./pages/index.js

```bash
<Head>
  <title>Create Next App</title>
  <link rel="icon" href="/favicon.ico" />
</Head>
```

<Head> is a React Component that uses <head> from HTML Tag. It Allow to modify <head> tag! 

Head comes from  [`next/head`](https://nextjs.org/docs/api-reference/next/head) module.



Modufy ./pages/posts/first-post.js

```js
import Head from 'next/head'
...
export default function FirstPost() {
  return (
    <>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </>
  )
}
```



> Note: f you want to customize the `<html>` tag, for example to add the `lang` attribute, you can do so by creating a `pages/_document.js` file. Learn more in the [custom `Document` documentation](https://nextjs.org/docs/advanced-features/custom-document).
>





### CSS

[styled-jsx](https://github.com/vercel/styled-jsx) is a “CSS-in-JS” library — it lets you write CSS within a React component, and the CSS styles will be *scoped* (other components won’t be affected).

Next.js has built-in support for [styled-jsx](https://github.com/vercel/styled-jsx), but you can also use other popular CSS-in-JS libraries such as [styled-components](https://github.com/vercel/next.js/tree/canary/examples/with-styled-components) or [emotion](https://github.com/vercel/next.js/tree/canary/examples/with-emotion).

#### Layout Component

**Layout** component which will be shared across all pages

- Create a top-level directory called `components`.
- Inside `components`, create a file called `layout.js` with the following content:

../components/layout.js

```jsx
export default function Layout({ children }) {
  return <div>{children}</div>
}
```

Then, import Layout component in first-post.js and make it the outermos Component!

./pages/posts/first-post.js

```js
import Head from 'next/head';
import Link from 'next/link';
import Layout from '../../components/layout';

export default function FirstPost() {
  return (
    <Layout>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </Layout>
  )
}
```



#### Adding CSS

Use CSS Modules to import  CSS files in React Component:

./components/layout.module.css

```css
.container {
  max-width: 36rem;
  padding: 0 1rem;
  margin: 3rem auto 6rem;
}
```

> **Important:** To use [CSS Modules](https://nextjs.org/docs/basic-features/built-in-css-support#adding-component-level-css), the CSS file name must end with `.module.css`.



To use this `container` class inside `components/layout.js`, you need to:

- Import the CSS file and assign a name to it, like `styles`
- Use `styles.container` as the `className`

Open `components/layout.js` and replace its content with the following:

```js
import styles from './layout.module.css'

export default function Layout({ children }) {
  return <div className={styles.container}>{children}</div>
}
```

[CSS Modules](https://nextjs.org/docs/basic-features/built-in-css-support#adding-component-level-css) are extracted from the JavaScript bundles at build time and generate `.css` files that are loaded automatically by Next.js.



#### Global Styles

[CSS Modules](https://nextjs.org/docs/basic-features/built-in-css-support#adding-component-level-css) are useful for component-level styles. But if you want some CSS to be loaded by **every page**, Next.js has support for that as well.

To load [global CSS](https://nextjs.org/docs/basic-features/built-in-css-support#adding-a-global-stylesheet) files, **create a file called [`pages/_app.js`](https://nextjs.org/docs/advanced-features/custom-app)** with the following content:

./pages/_app.js

```js
function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}

export default App
```

This `App` component is the top-level component which will be common across all the different pages. You can use this `App` component to keep state when navigating between pages, for example.



##### Restart the Development Server

**Important:** You need to restart the development server when you add [`pages/_app.js`](https://nextjs.org/docs/advanced-features/custom-app). Press Ctrl + c to stop the server and run:

```bash
yarn dev
```



#### Adding Global CSS

In Next.js, you can add [global CSS](https://nextjs.org/docs/basic-features/built-in-css-support#adding-a-global-stylesheet) files by importing them from [`pages/_app.js`](https://nextjs.org/docs/advanced-features/custom-app). You **cannot** import global CSS anywhere else.

You can place the global CSS file anywhere and use any name. So let’s do the following:

- Create a top-level `styles` directory and create `global.css` inside.
- Add the following content to `styles/global.css`. It resets some styles and changes the color of the `a` tag:

./styles/global.css

```css
html,
body {
  padding: 0;
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen, Ubuntu,
    Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
  line-height: 1.6;
  font-size: 18px;
}

* {
  box-sizing: border-box;
}

a {
  color: #0070f3;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

img {
  max-width: 100%;
  display: block;
}
```



Finally, open `pages/_app.js` add import the CSS file like so:

```js
import '../styles/global.css'

export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}
```















































