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



### Polishing Layout

./pages/index.js -> Main Page of the Application

./pages/_app.js -> Import the .styles/global.css to set global css

./pages/posts/first-post.js -> first-post page

./components/layout.js -> Layout Component to pages that imported it

./components/layout.module.css --> CSS style to layout.js component

./public/images/ -> place to put all images 

./styles/global.css -> CSS to be import by _app.js and used to global 

./styles/Home.module.css -> CSS specific for the Home page

./styles/utils.module.css -> CSS styles utilities from many components but not set as global to pages

MODULES.CSS IS THE SCOPE STYLES AT COMPONENT LEVELS!



### Styling Tips

Here are some styling tips that might be helpful.

> You can just **read through** the following sections. No need to make changes to our app!

#### Using `classnames` library to toggle classes

[`classnames`](https://github.com/JedWatson/classnames) is a simple library that lets you toggle class names easily. You can install it using `npm install classnames` or `yarn add classnames`.

Please take a look at its [documentation](https://github.com/JedWatson/classnames) for more details, but here’s the basic usage:

- Suppose that you want to create an `Alert` component which accepts `type`, which can be `'success'` or `'error'`.
- If it’s `'success'`, you want the text color to be green. If it’s `'error'`, you want the text color to be red.

You can first write a CSS module (e.g. `alert.module.css`) like this:

```
.success {
  color: green;
}
.error {
  color: red;
}
```

And use `classnames` like this:

```
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

#### Customizing PostCSS Config

Out of the box, with no configuration, Next.js compiles CSS using [PostCSS](https://postcss.org/).

To customize PostCSS config, you can create a top-level file called [`postcss.config.js`](https://nextjs.org/docs/advanced-features/customizing-postcss-config#customizing-plugins). This is useful if you’re using libraries like [Tailwind CSS](https://tailwindcss.com/).

Here are the steps to add [Tailwind CSS](https://tailwindcss.com/). We recommend using `postcss-preset-env` and `postcss-flexbugs-fixes` to match [Next.js’s default behavior](https://nextjs.org/docs/advanced-features/customizing-postcss-config#default-behavior). First, install the packages:

```
npm install tailwindcss postcss-preset-env postcss-flexbugs-fixes
```

Then write the following for [`postcss.config.js`](https://nextjs.org/docs/advanced-features/customizing-postcss-config#customizing-plugins):

```
module.exports = {
  plugins: [
    'tailwindcss',
    'postcss-flexbugs-fixes',
    [
      'postcss-preset-env',
      {
        autoprefixer: {
          flexbox: 'no-2009'
        },
        stage: 3,
        features: {
          'custom-properties': false
        }
      }
    ]
  ]
}
```

We also recommend [removing unused CSS](https://tailwindcss.com/docs/controlling-file-size/#removing-unused-css) by specifying the `purge` option on `tailwind.config.js`:

```
// tailwind.config.js
module.exports = {
  purge: [
    // Use *.tsx if using TypeScript
    './pages/**/*.js',
    './components/**/*.js'
  ]
  // ...
}
```

> To learn more about custom PostCSS configuration, check out the [documentation for PostCSS](https://nextjs.org/docs/advanced-features/customizing-postcss-config).

#### Using Sass

Out of the box, Next.js allows you to import [Sass](https://nextjs.org/docs/basic-features/built-in-css-support#sass-support) using both the `.scss` and `.sass` extensions. You can use component-level Sass via [CSS Modules](https://nextjs.org/docs/basic-features/built-in-css-support#adding-component-level-css) and the `.module.scss` or `.module.sass` extension.

Before you can use Next.js' built-in Sass support, be sure to install [`sass`](https://github.com/sass/sass):

```
npm install sass
```

#### That’s it for this lesson!

To learn more about Next.js’s built-in CSS Support and CSS Modules, check out the [CSS Documentation](https://nextjs.org/docs/basic-features/built-in-css-support).





# Pre-rendering and Data Fetching

## Pre-rendering and Data Fetching

### Pre-rendering

By default, Next.js pre-renders every page. This means that Next.js *generates HTML for each page in advance*, instead of having it all done by client-side JavaScript. Pre-rendering can result in better performance and [SEO](https://en.wikipedia.org/wiki/Search_engine_optimization).

Each generated HTML is associated with minimal JavaScript code necessary for that page. When a page is loaded by the browser, its JavaScript code runs and makes the page fully interactive. (This process is called **hydration**.)

If the App has interactive components like <Link />, they will be active after JS loads.



#### Two Forms of Pre-rendering

Next.js has two forms of pre-rendering: [**Static Generation**](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) and [**Server-side Rendering**](https://nextjs.org/docs/basic-features/pages#server-side-rendering). The difference is in **when** it generates the HTML for a page.

- [**Static Generation**](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) is the pre-rendering method that generates the HTML at **build time**. The pre-rendered HTML is then *reused* on each request.
- [**Server-side Rendering**](https://nextjs.org/docs/basic-features/pages#server-side-rendering) is the pre-rendering method that generates the HTML on **each request**.



![image-20210416232954007](/home/roger/.config/Typora/typora-user-images/image-20210416232954007.png)



![image-20210416233017594](/home/roger/.config/Typora/typora-user-images/image-20210416233017594.png)



In development mode (when you run `npm run dev` or `yarn dev`), every page is [pre-rendered](https://nextjs.org/docs/basic-features/pages#pre-rendering) on each request — even for pages that use [Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended).



##  Pre-rendering and Data Fetching

[1](https://nextjs.org/learn/basics/data-fetching)

[2](https://nextjs.org/learn/basics/data-fetching/setup)

[3](https://nextjs.org/learn/basics/data-fetching/pre-rendering)

[4](https://nextjs.org/learn/basics/data-fetching/two-forms)

[5](https://nextjs.org/learn/basics/data-fetching/with-data)

[6](https://nextjs.org/learn/basics/data-fetching/blog-data)

[7](https://nextjs.org/learn/basics/data-fetching/implement-getstaticprops)

[8](https://nextjs.org/learn/basics/data-fetching/getstaticprops-details)

[9](https://nextjs.org/learn/basics/data-fetching/request-time)

## Two Forms of Pre-rendering

Next.js has two forms of pre-rendering: [**Static Generation**](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) and [**Server-side Rendering**](https://nextjs.org/docs/basic-features/pages#server-side-rendering). The difference is in **when** it generates the HTML for a page.

- [**Static Generation**](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) is the pre-rendering method that generates the HTML at **build time**. The pre-rendered HTML is then *reused* on each request.
- [**Server-side Rendering**](https://nextjs.org/docs/basic-features/pages#server-side-rendering) is the pre-rendering method that generates the HTML on **each request**.

![Static Generation](https://nextjs.org/static/images/learn/data-fetching/static-generation.png)

![Server-side Rendering](https://nextjs.org/static/images/learn/data-fetching/server-side-rendering.png)

> In development mode (when you run `npm run dev` or `yarn dev`), every page is [pre-rendered](https://nextjs.org/docs/basic-features/pages#pre-rendering) on each request — even for pages that use [Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended).



#### Per-page Basis

Importantly, Next.js lets you **choose** which pre-rendering form to use for each page. You can create a "hybrid" Next.js app by using [Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) for most pages and using [Server-side Rendering](https://nextjs.org/docs/basic-features/pages#server-side-rendering) for others.



![image-20210416233223615](/home/roger/.config/Typora/typora-user-images/image-20210416233223615.png)



### When to Use [Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) v.s. [Server-side Rendering](https://nextjs.org/docs/basic-features/pages#server-side-rendering)

We recommend using [**Static Generation**](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) (with and without data) whenever possible because your page can be built once and served by CDN, which makes it much faster than having a server render the page on every request.

You can use [Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) for many types of pages, including:

- Marketing pages
- Blog posts
- E-commerce product listings
- Help and documentation

You should ask yourself: "Can I pre-render this page **ahead** of a user's request?" If the answer is yes, then you should choose [Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended).

On the other hand, [Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) is **not** a good idea if you cannot pre-render a page ahead of a user's request. Maybe your page shows frequently updated data, and the page content changes on every request.

In that case, you can use [**Server-side Rendering**](https://nextjs.org/docs/basic-features/pages#server-side-rendering). It will be slower, but the pre-rendered page will always be up-to-date. Or you can skip pre-rendering and use client-side JavaScript to populate frequently updated data.



### Static Generation with and without Data

[Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) can be done with and without data.

So far, all the pages we’ve created do not require fetching external data. Those pages will automatically be statically generated when the app is built for production.



![image-20210416233753148](/home/roger/.config/Typora/typora-user-images/image-20210416233753148.png)



However, for some pages, you might not be able to render the HTML without first fetching some external data. Maybe you need to access the file system, fetch external API, or query your database at build time. Next.js supports this case — [Static Generation **with data**](https://nextjs.org/docs/basic-features/pages#static-generation-with-data) — out of the box.

![image-20210416233946614](/home/roger/.config/Typora/typora-user-images/image-20210416233946614.png)



### Static Generation with Data using `getStaticProps`

How does it work? Well, in Next.js, when you export a page component, you can also export an `async` function called [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation). If you do this, then:

- [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) runs at build time in production, and…
- Inside the function, you can fetch external data and send it as props to the page.

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

Essentially, [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) allows you to tell Next.js: *“Hey, this page has some data dependencies — so when you pre-render this page at build time, make sure to resolve them first!”*

> **Note**: In development mode, [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) runs on each request instead.
>
> 

### Let’s Use `getStaticProps`

It’s easier to learn by doing, so starting from the next page, we’ll use [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) to implement our blog.



#### Blog Data

We’ll now add blog data to our app using the file system. Each blog post will be a markdown file.

- Create a new top-level directory called **`posts`** (this is not the same as `pages/posts`).
- Inside `posts`, create two files: **`pre-rendering.md`** and **`ssg-ssr.md`**.

Now, copy the following code to `posts/pre-rendering.md`:

```markdown
---
title: 'Two Forms of Pre-rendering'
date: '2020-01-01'
---

Next.js has two forms of pre-rendering: **Static Generation** and **Server-side Rendering**. The difference is in **when** it generates the HTML for a page.

- **Static Generation** is the pre-rendering method that generates the HTML at **build time**. The pre-rendered HTML is then _reused_ on each request.
- **Server-side Rendering** is the pre-rendering method that generates the HTML on **each request**.

Importantly, Next.js lets you **choose** which pre-rendering form to use for each page. You can create a "hybrid" Next.js app by using Static Generation for most pages and using Server-side Rendering for others.
```



Then, copy the following code to `posts/ssg-ssr.md`:

```markdown
---
title: 'When to Use Static Generation v.s. Server-side Rendering'
date: '2020-01-02'
---

We recommend using **Static Generation** (with and without data) whenever possible because your page can be built once and served by CDN, which makes it much faster than having a server render the page on every request.

You can use Static Generation for many types of pages, including:

- Marketing pages
- Blog posts
- E-commerce product listings
- Help and documentation

You should ask yourself: "Can I pre-render this page **ahead** of a user's request?" If the answer is yes, then you should choose Static Generation.

On the other hand, Static Generation is **not** a good idea if you cannot pre-render a page ahead of a user's request. Maybe your page shows frequently updated data, and the page content changes on every request.

In that case, you can use **Server-Side Rendering**. It will be slower, but the pre-rendered page will always be up-to-date. Or you can skip pre-rendering and use client-side JavaScript to populate data.
```

> You might have noticed that each markdown file has a metadata section at the top containing `title` and `date`. This is called YAML Front Matter, which can be parsed using a library called [gray-matter](https://github.com/jonschlinkert/gray-matter).





##  Pre-rendering and Data Fetching

[1](https://nextjs.org/learn/basics/data-fetching)

[2](https://nextjs.org/learn/basics/data-fetching/setup)

[3](https://nextjs.org/learn/basics/data-fetching/pre-rendering)

[4](https://nextjs.org/learn/basics/data-fetching/two-forms)

[5](https://nextjs.org/learn/basics/data-fetching/with-data)

[6](https://nextjs.org/learn/basics/data-fetching/blog-data)

[7](https://nextjs.org/learn/basics/data-fetching/implement-getstaticprops)

[8](https://nextjs.org/learn/basics/data-fetching/getstaticprops-details)

[9](https://nextjs.org/learn/basics/data-fetching/request-time)

## Blog Data

We’ll now add blog data to our app using the file system. Each blog post will be a markdown file.

- Create a new top-level directory called **`posts`** (this is not the same as `pages/posts`).
- Inside `posts`, create two files: **`pre-rendering.md`** and **`ssg-ssr.md`**.

Now, copy the following code to `posts/pre-rendering.md`:

```
---
title: 'Two Forms of Pre-rendering'
date: '2020-01-01'
---

Next.js has two forms of pre-rendering: **Static Generation** and **Server-side Rendering**. The difference is in **when** it generates the HTML for a page.

- **Static Generation** is the pre-rendering method that generates the HTML at **build time**. The pre-rendered HTML is then _reused_ on each request.
- **Server-side Rendering** is the pre-rendering method that generates the HTML on **each request**.

Importantly, Next.js lets you **choose** which pre-rendering form to use for each page. You can create a "hybrid" Next.js app by using Static Generation for most pages and using Server-side Rendering for others.
```

Then, copy the following code to `posts/ssg-ssr.md`:

```
---
title: 'When to Use Static Generation v.s. Server-side Rendering'
date: '2020-01-02'
---

We recommend using **Static Generation** (with and without data) whenever possible because your page can be built once and served by CDN, which makes it much faster than having a server render the page on every request.

You can use Static Generation for many types of pages, including:

- Marketing pages
- Blog posts
- E-commerce product listings
- Help and documentation

You should ask yourself: "Can I pre-render this page **ahead** of a user's request?" If the answer is yes, then you should choose Static Generation.

On the other hand, Static Generation is **not** a good idea if you cannot pre-render a page ahead of a user's request. Maybe your page shows frequently updated data, and the page content changes on every request.

In that case, you can use **Server-Side Rendering**. It will be slower, but the pre-rendered page will always be up-to-date. Or you can skip pre-rendering and use client-side JavaScript to populate data.
```

> You might have noticed that each markdown file has a metadata section at the top containing `title` and `date`. This is called YAML Front Matter, which can be parsed using a library called [gray-matter](https://github.com/jonschlinkert/gray-matter).



### Parsing the Blog Data on `getStaticProps`

Now, let’s update our index page (`pages/index.js`) using this data. We’d like to:

- Parse each markdown file and get `title`, `date`, and file name (which will be used as `id` for the post URL).
- List the data on the index page, sorted by date.

To do this on pre-render, we need to implement [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation).



![image-20210416235004087](/home/roger/.config/Typora/typora-user-images/image-20210416235004087.png)





## Implement getStaticProps

First, install [gray-matter](https://github.com/jonschlinkert/gray-matter) which lets us parse the metadata in each markdown file.

```shell
yarn add gray-matter
```



Next, we’ll create a simple library for fetching data from the file system.

- Create a top-level directory called `lib`, and…
- Inside `lib`, create a file called `posts.js` with the following content:

```js
import fs from 'fs'
import path from 'path'
import matter from 'gray-matter'

const postsDirectory = path.join(process.cwd(), 'posts')

export function getSortedPostsData() {
  // Get file names under /posts
  const fileNames = fs.readdirSync(postsDirectory)
  const allPostsData = fileNames.map(fileName => {
    // Remove ".md" from file name to get id
    const id = fileName.replace(/\.md$/, '')

    // Read markdown file as string
    const fullPath = path.join(postsDirectory, fileName)
    const fileContents = fs.readFileSync(fullPath, 'utf8')

    // Use gray-matter to parse the post metadata section
    const matterResult = matter(fileContents)

    // Combine the data with the id
    return {
      id,
      ...matterResult.data
    }
  })
  // Sort posts by date
  return allPostsData.sort((a, b) => {
    if (a.date < b.date) {
      return 1
    } else {
      return -1
    }
  })
}
```



Now, we need to add an import for `getSortedPostsData` and call it inside [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) in `pages/index.js`.

Open `pages/index.js` in your editor and add the following code above the exported `Home` component:



```js
import { getSortedPostsData } from '../lib/posts'

export async function getStaticProps() {
  const allPostsData = getSortedPostsData()
  return {
    props: {
      allPostsData
    }
  }
}
```



By returning `allPostsData` inside the `props` object in `getStaticProps`, the blog posts will be passed to the `Home` component as a prop. Now you can access the blog posts like so:

```js
export default function Home ({ allPostsData }) { ... }
```

To display the blog posts, let's update the `Home` component to add another `<section>` tag with the data below the section with your self introduction. Don't forget to also change the props from `()` to `({ allPostsData })`:

```jsx
export default function Home({ allPostsData }) {
  return (
    <Layout home>
      {/* Keep the existing code here */}

      {/* Add this <section> tag below the existing <section> tag */}
      <section className={`${utilStyles.headingMd} ${utilStyles.padding1px}`}>
        <h2 className={utilStyles.headingLg}>Blog</h2>
        <ul className={utilStyles.list}>
          {allPostsData.map(({ id, date, title }) => (
            <li className={utilStyles.listItem} key={id}>
              {title}
              <br />
              {id}
              <br />
              {date}
            </li>
          ))}
        </ul>
      </section>
    </Layout>
  )
}
```



You should now see the blog data if you access [http://localhost:3000](http://localhost:3000/).



![image-20210417000157237](/home/roger/.config/Typora/typora-user-images/image-20210417000157237.png)



Congratulations! We’ve successfully fetched external data (from the file system) and pre-rendered the index page with this data.

![image-20210417000348169](/home/roger/.config/Typora/typora-user-images/image-20210417000348169.png)



## getStaticProps Details

Here is some essential information you should know about [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation).

### Fetch External API or Query Database

In `lib/posts.js`, we’ve implemented `getSortedPostsData` which fetches data from the file system. But you can fetch the data from other sources, like an external API endpoint, and it’ll work just fine:

```js
export async function getSortedPostsData() {
  // Instead of the file system,
  // fetch post data from an external API endpoint
  const res = await fetch('..')
  return res.json()
}
```

> Note: Next.js polyfills fetch() on both the client and server. You don't need to import it.



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

This is possible because [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) only **runs on the server-side**. It will never run on the client-side. It won’t even be included in the JS bundle for the browser. That means you can write code such as direct database queries without them being sent to browsers.

#### Development vs. Production

- In **development** (`npm run dev` or `yarn dev`), [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) runs on *every request*.
- In **production**, [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) runs at *build time*. However, this behavior can be enhanced using the [`fallback` key](https://nextjs.org/docs/basic-features/data-fetching#the-fallback-key-required) returned by [`getStaticPaths`](https://nextjs.org/docs/basic-features/data-fetching#getstaticpaths-static-generation)

Because it’s meant to be run at build time, you won’t be able to use data that’s only available during request time, such as query parameters or HTTP headers.



##  Pre-rendering and Data Fetching

[1](https://nextjs.org/learn/basics/data-fetching)

[2](https://nextjs.org/learn/basics/data-fetching/setup)

[3](https://nextjs.org/learn/basics/data-fetching/pre-rendering)

[4](https://nextjs.org/learn/basics/data-fetching/two-forms)

[5](https://nextjs.org/learn/basics/data-fetching/with-data)

[6](https://nextjs.org/learn/basics/data-fetching/blog-data)

[7](https://nextjs.org/learn/basics/data-fetching/implement-getstaticprops)

[8](https://nextjs.org/learn/basics/data-fetching/getstaticprops-details)

[9](https://nextjs.org/learn/basics/data-fetching/request-time)

## getStaticProps Details

Here is some essential information you should know about [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation).

### Fetch External API or Query Database

In `lib/posts.js`, we’ve implemented `getSortedPostsData` which fetches data from the file system. But you can fetch the data from other sources, like an external API endpoint, and it’ll work just fine:

```
export async function getSortedPostsData() {
  // Instead of the file system,
  // fetch post data from an external API endpoint
  const res = await fetch('..')
  return res.json()
}
```

> **Note**: Next.js polyfills [`fetch()`](https://nextjs.org/docs/basic-features/supported-browsers-features) on both the client and server. You don't need to import it.

You can also query the database directly:

```
import someDatabaseSDK from 'someDatabaseSDK'

const databaseClient = someDatabaseSDK.createClient(...)

export async function getSortedPostsData() {
  // Instead of the file system,
  // fetch post data from a database
  return databaseClient.query('SELECT posts...')
}
```

This is possible because [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) only **runs on the server-side**. It will never run on the client-side. It won’t even be included in the JS bundle for the browser. That means you can write code such as direct database queries without them being sent to browsers.

### Development vs. Production

- In **development** (`npm run dev` or `yarn dev`), [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) runs on *every request*.
- In **production**, [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) runs at *build time*. However, this behavior can be enhanced using the [`fallback` key](https://nextjs.org/docs/basic-features/data-fetching#the-fallback-key-required) returned by [`getStaticPaths`](https://nextjs.org/docs/basic-features/data-fetching#getstaticpaths-static-generation)

Because it’s meant to be run at build time, you won’t be able to use data that’s only available during request time, such as query parameters or HTTP headers.

#### Only Allowed in a Page

[`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) can only be exported from a [**page**](https://nextjs.org/docs/basic-features/pages). You can’t export it from non-page files.

One of the reasons for this restriction is that React needs to have all the required data before the page is rendered.

#### What If I Need to Fetch Data at Request Time?

[Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) is **not** a good idea if you cannot pre-render a page ahead of a user's request. Maybe your page shows frequently updated data, and the page content changes on every request.

In cases like this, you can try [**Server-side Rendering**](https://nextjs.org/docs/basic-features/pages#server-side-rendering) or skip pre-rendering. Let’s talk about these strategies before we move on to the next lesson.







