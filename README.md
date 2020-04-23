## [`Next.JS`](https://nextjs.org/)

### Creact a Next.js app

```sh
npm init next-app nextjs-blog --example "https://github.com/zeit/next-learn-starter/tree/master/learn-starter"

cd nextjs-blog

npm run dev
```

### Editing the page

Next.js development server has the **Hot Reloaing** feature.
When you make changes to files Next.js automatically applies the changes in the browser.

### Creating Pages

- Create a new page using the file system routing feature.
- Learn how to use the Link component to enable client-side navigation between pages.
- Learn about built-in support for code splitting and prefetching.

detailed documentation on Next.js routing, take a look at the [`routing documentation`](https://nextjs.org/docs/routing/introduction)

```ts
// pages/posts/first-post.js
export default function FirstPost() {
  return <h1>First Post</h1>;
}
```

http://localhost:3000/posts/first-post

pages diretory 안에 js file을 생성하면, url path로 생성된다.

### usung `<Link>` - Link Component

```ts
// pages/index.js
import Link from 'next/link'

Read <Link href="/posts/first-post"><a>this page!</a></Link>

// pages/posts/first-post.js
import Link from 'next/link'

export default function FirstPost() {
  return (
    <>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a className="foo" target="_blank" rel="noopener noreferrer">
        Hello World
      </a>
        </Link>
      </h2>
    </>
  )
}
```

### Assets, Metadata, and CSS

- Assets

Next.js can serve static files, like images, under the top-level **public** directory. Files inside public can be referenced from the root of the application similar to **pages**

```ts
<img src='/vercel.svg' alt='Vercel Logo' className='logo' />
```

- Metadata

wanted to modify the metadata of the page

```ts
import Head from 'next/head';

<Head>
  <title>Create Next App</title>
  <link rel='icon' href='/favicon.ico' />
</Head>;
```

- CSS Styling

```ts
<style jsx>{`
  …
`}</style>
```

Writing and Importing CSS<br/>
Next.js has built-in support for CSS and Sass which allows you to import .css and .scss files.

### Layout Component

- Create a top-level directory called **components**.

```ts
// components/layout.js
function Layout({ children }) {
  return <div>{children}</div>;
}

export default Layout;

// pages/posts/first-post.js
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
        <Link href='/'>
          <a>Back to home</a>
        </Link>
      </h2>
    </Layout>
  );
}
```

### Adding CSS

Create a file called **layout.module.css** in the **components** directory with the following content:

```ts
/// components/layout.module.css
.container {
  max-width: 36rem;
  padding: 0 1rem;
  margin: 3rem auto 6rem;
}

// components/layout.js
import styles from './layout.module.css'

export default function Layout({ children }) {
  return <div className={styles.container}>{children}</div>
}
```

- Automatically Generates Unique Class Names
  browser 가서 browser’s devtools(f12) 로 확인해보면
  class name이 자동 설정되어 unique하게 해준다.

### Adding Global Styles

```ts
// pages/_app.js
import '../styles/global.css';

export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />;
}
```

This **App** component is the top-level component which will be common across all the different pages. You can use this App component to keep state when navigating between pages

- To use CSS Modules, import a CSS file named **\*.module.css** from any component.
- To use global CSS, import a CSS file in **pages/\_app.js**.

### Polishing Layout

```ts
// public/images/profile.jpg

// upate componenets/layout.module.css

// styles/utils.module.css

// components/layout.js
import Head from 'next/head';
import styles from './layout.module.css';
import utilStyles from '../styles/utils.module.css';
import Link from 'next/link';

const name = 'Your Name';
export const siteTitle = 'Next.js Sample Website';

export default function Layout({ children, home }) {
  return (
    <div className={styles.container}>
      <Head>
        <link rel='icon' href='/favicon.ico' />
        <meta
          name='description'
          content='Learn how to build a personal website using Next.js'
        />
        <meta
          property='og:image'
          content={`https://og-image.now.sh/${encodeURI(
            siteTitle
          )}.png?theme=light&md=0&fontSize=75px&images=https%3A%2F%2Fassets.zeit.co%2Fimage%2Fupload%2Ffront%2Fassets%2Fdesign%2Fnextjs-black-logo.svg`}
        />
        <meta name='og:title' content={siteTitle} />
        <meta name='twitter:card' content='summary_large_image' />
      </Head>
      <header className={styles.header}>
        {home ? (
          <>
            <img
              src='/images/profile.jpg'
              className={`${styles.headerHomeImage} ${utilStyles.borderCircle}`}
              alt={name}
            />
            <h1 className={utilStyles.heading2Xl}>{name}</h1>
          </>
        ) : (
          <>
            <Link href='/'>
              <a>
                <img
                  src='/images/profile.jpg'
                  className={`${styles.headerImage} ${utilStyles.borderCircle}`}
                  alt={name}
                />
              </a>
            </Link>
            <h2 className={utilStyles.headingLg}>
              <Link href='/'>
                <a className={utilStyles.colorInherit}>{name}</a>
              </Link>
            </h2>
          </>
        )}
      </header>
      <main>{children}</main>
      {!home && (
        <div className={styles.backToHome}>
          <Link href='/'>
            <a>← Back to home</a>
          </Link>
        </div>
      )}
    </div>
  );
}

// pages/index.js
import Head from 'next/head';
import Layout, { siteTitle } from '../components/layout';
import utilStyles from '../styles/utils.module.css';

export default function Home() {
  return (
    <Layout home>
      <Head>
        <title>{siteTitle}</title>
      </Head>
      <section className={utilStyles.headingMd}>
        <p>hello world i`m here</p>
        <p>
          (This is a sample website - you’ll be building a site like this on{' '}
          <a href='https://nextjs.org/learn'>our Next.js tutorial</a>.)
        </p>
      </section>
    </Layout>
  );
}
```

### Pre-rendering and Data Fetching

- Next.js’s pre-rendering feature.
- The two forms of pre-rendering: Static Generation and Server-side Rendering.
- Static Generation with and without data.
- **getStaticProps** and how to use it to import external blog data into the index page.
- Some useful information on **getStaticProps**

```sh
// posts/pre-rendering.md
// posts/ssg-ssr.md

npm install gray-matter
```

```ts
// lib/posts.js
import fs from 'fs';
import path from 'path';
import matter from 'gray-matter';

const postsDirectory = path.join(process.cwd(), 'posts');

export function getSortedPostsData() {
  // Get file names under /posts
  const fileNames = fs.readdirSync(postsDirectory);
  const allPostsData = fileNames.map((fileName) => {
    // Remove ".md" from file name to get id
    const id = fileName.replace(/\.md$/, '');

    // Read markdown file as string
    const fullPath = path.join(postsDirectory, fileName);
    const fileContents = fs.readFileSync(fullPath, 'utf8');

    // Use gray-matter to parse the post metadata section
    const matterResult = matter(fileContents);

    // Combine the data with the id
    return {
      id,
      ...matterResult.data,
    };
  });
  // Sort posts by date
  return allPostsData.sort((a, b) => {
    if (a.date < b.date) {
      return 1;
    } else {
      return -1;
    }
  });
}
```

### Dynamic Routes

- How to statically generate pages with dynamic routes using getStaticPaths.
- How to write getStaticProps to fetch the data for each blog post.
- How to render markdown using remark.
- How to pretty-print date strings.
- How to link to a page with dynamic routes.
- Some useful information on dynamic routes.

첫번째, page clled **[id].js**
두번째, async function called getStaticPaths from this page. In this function, we need to return a list of possible values for id.

```ts
export async function getStaticPaths() {
  // Return a list of possible value for id
}
```

세번쨰, implement getStaticProps again - this time, to fetch necessary data for the blog post with a given id. getStaticProps is given params, which contains id.

```ts
export async function getStaticProps({ params }) {
  // Fetch necessary data for the blog post using params.id
}
```

```ts
// lib/posts.js
export function getAllPostIds() {
  const fileNames = fs.readdirSync(postsDirectory);

  return fileNames.map((fileName) => {
    return {
      params: {
        id: fileName.replace(/\.md$/, ''),
      },
    };
  });
}

export function getPostData(id) {
  const fullPath = path.join(postsDirectory, `${id}.md`);
  const fileContents = fs.readFileSync(fullPath, 'utf8');

  // Use gray-matter to parse the post metadata section
  const matterResult = matter(fileContents);

  // Combine the data with the id
  return {
    id,
    ...matterResult.data,
  };
}

// pages/posts/[id].js
import { getAllPostIds, getPostData } from '../../lib/posts';

export default function Post({ postData }) {
  return (
    <Layout>
      {postData.title}
      <br />
      {postData.id}
      <br />
      {postData.date}
    </Layout>
  );
}

export async function getStaticPaths() {
  const paths = getAllPostIds();
  return {
    paths,
    fallback: false,
  };
}

export async function getStaticProps({ params }) {
  const postData = getPostData(params.id);
  return {
    props: {
      postData,
    },
  };
}
```

#### Render Markdown

```sh
npm install remark remark-html
```

```ts
// lib/posts.js
import remark from 'remark';
import html from 'remark-html';

export async function getPostData(id) {
  const fullPath = path.join(postsDirectory, `${id}.md`);
  const fileContents = fs.readFileSync(fullPath, 'utf8');

  // Use gray-matter to parse the post metadata section
  const matterResult = matter(fileContents);

  // Use remark to convert markdown into HTML string
  const processedContent = await remark()
    .use(html)
    .process(matterResult.content);
  const contentHtml = processedContent.toString();

  // Combine the data with the id and contentHtml
  return {
    id,
    contentHtml,
    ...matterResult.data,
  };
}
```

```ts
// pages/posts/[id].js
export async function getStaticProps({ params }) {
  // Add the "await" keyword like this:
  const postData = await getPostData(params.id);
  // ...
}
```

#### Polishing the post page

```ts
// [id].js
<Layout>
  <Head>
    <title>{postData.title}</title>
  </Head>
  ...
</Layout>
```

```sh
npm install date-fns
```

```ts
// components/date.js
import { parseISO, format } from 'date-fns';

export default function Date({ dateString }) {
  const date = parseISO(dateString);
  return <time dateTime={dateString}>{format(date, 'LLLL d, yyyy')}</time>;
}

// pages/posts/[id].js
import Date from '../../components/date';

export default function Post({ postData }) {
  return (
    <Layout>
      ...
      {/* Replace {postData.date} with this */}
      <Date dateString={postData.date} />
      ...
    </Layout>
  );
}
```

#### Polishing the Index Page

```ts
// pages/index.js
import Link from 'next/link';
import Date from '../components/date';

{
  allPostsData.map(({ id, date, title }) => (
    <li className={utilStyles.listItem} key={id}>
      <Link href='/posts/[id]' as={`/posts/${id}`}>
        <a>{title}</a>
      </Link>
      <br />
      <small className={utilStyles.lightText}>
        <Date dateString={date} />
      </small>
    </li>
  ));
}
```
