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











































