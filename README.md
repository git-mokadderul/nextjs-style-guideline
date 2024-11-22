# NEXT JS Style Guide

## Table of Contents

1. [Next.js Lazy Loading](#nextjs-lazy-loading)
1. [Next.js Code Splitting with `next/dynamic`](#nextjs-code-splitting-with-nextdynamic)
1. [Lazy Loading of Images](#lazy-loading-of-images)
1. [Using Built-in Components](#using-built-in-components)
1. [Use CSS Modules](#use-css-modules)
1. [Caching Data in Next.js](#caching-data-in-nextjs)
1. [Use Route Handlers](#use-route-handlers)
1. [Use Multiple Data Rendering Modes](#use-multiple-data-rendering-modes)
1. [TypeScript Support](#typescript-support)
1. [Enhance SEO in Next.js](#enhance-seo-in-nextjs)
1. [Remove Unused Dependencies & Code](#remove-unused-dependencies--code)
1. [Deploy Your Next.js App to Vercel](#deploy-your-nextjs-app-to-vercel)
1. [Client Component Placement Strategy in Next.js](#client-component-placement-strategy-in-nextjs)

---

# Next.js Best Practices for Excellence

Next.js is a powerful framework that enables developers to build performant and scalable web applications. To make the most out of Next.js, it’s essential to follow best practices that ensure code quality, maintainability, and optimal performance. Here are some Next.js best practices to help you achieve excellence in your projects:

## Next.js Lazy Loading

In Next.js, there’s a neat technique called lazy loading. It means that instead of bringing everything onto the webpage all at once, things only show up when you really need them. This helps the webpage use less data and load faster.

Next.js makes this happen using something called dynamic imports. Think of it like getting deliveries – you only ask for a package when you’re ready to use it, not all of them at once. By doing this, you can split your website’s code into smaller parts. Each part only shows up when it’s actually required. This makes your webpage start up faster because it doesn’t have to load everything from the beginning. It’s like setting up your webpage piece by piece instead of all at the same time.

**[⬆ back to top](#table-of-contents)**

## Next.js Code Splitting with `next/dynamic`

Next.js code splitting is a technique used to split the JavaScript code of a Next.js application into smaller chunks, allowing the browser to load only the necessary code for each page or component.

Next.js uses automatic code splitting by default. When you build your Next.js application, it analyzes your pages and components and generates separate bundles for each one. These bundles are then loaded on-demand as the user navigates through the application.

`next/dynamic` is a feature offered by Next.js that allows you to import components dynamically. This is useful when you want to load a component only when it’s needed, such as when the user scrolls to a certain point on the page. To use `next/dynamic`, follow these steps:

1. Import the `dynamic` function from `next/dynamic` in your Next.js component file:

   ```javascript
   import dynamic from "next/dynamic";
   ```

2. Wrap the component you want to dynamically load with the `dynamic` function:

   ```javascript
   const DynamicComponent = dynamic(() => import("./YourComponent"));
   ```

3. Render the `DynamicComponent` in your component’s JSX:

   ```javascript
   <DynamicComponent />
   ```

That’s it! Now, the `YourComponent` will be lazy loaded and rendered only when it’s needed.

You can also pass additional configuration options to the `dynamic` function. For example, you can specify a loading component to be shown while the `DynamicComponent` is being loaded:

```javascript
const DynamicComponent = dynamic(() => import("./YourComponent"), {
  loading: () => <div>Loading...</div>,
});
```

Additionally, you can configure other options like `ssr` (server-side rendering), `ssg` (static site generation), and `noSsr` (disable server-side rendering).

**[⬆ back to top](#table-of-contents)**

## Lazy Loading of Images

Next.js provides a component called `Image` that makes it easy to implement lazy loading of images. The `Image` component automatically optimizes and lazy loads images based on the viewport of the user’s device.

To use the `Image` component, follow these steps:

1. Import the `Image` component in your Next.js page or component file:

   ```javascript
   import Image from "next/image";
   ```

2. Replace the `img` tag with the `Image` component and specify the `src` and `alt` attributes:

   ```javascript
   <Image src="/path/to/image.jpg" alt="Description of the image" />
   ```

3. You can specify the `width` and `height` attributes of the image. This helps Next.js optimize the image and generate multiple sizes for responsive layouts:

   ```javascript
   <Image
     src="/path/to/image.jpg"
     alt="Description of the image"
     width={500}
     height={300}
   />
   ```

That’s it! Next.js will take care of lazy loading the image and optimizing it for performance. The `Image` component also provides additional features like automatic responsive layouts, priority loading, and placeholder images.

If you want to learn about chart libraries using React with Next.js, you should go through the React chart libraries guide that we’ve published previously.

**[⬆ back to top](#table-of-contents)**

## Using Built-in Components

In Next.js, there are several built-in components that provide useful functionality out of the box. Here are some commonly used ones along with their use cases and code examples:

### Image Component

The `Image` component allows optimized image loading and lazy loading, improving performance and user experience. It automatically optimizes images based on the device and network conditions.

```javascript
import Image from "next/image";

const MyComponent = () => (
  <div>
    <Image
      src="/path/to/image.jpg"
      alt="Description"
      width={500}
      height={300}
    />
  </div>
);
```

### Link Component

The `Link` component is used for client-side navigation in Next.js applications. It ensures that navigation between pages is faster by pre-fetching the necessary data.

```javascript
import Link from "next/link";

const MyComponent = () => (
  <div>
    <Link href="/about">About</Link>
  </div>
);
```

### Script Component

The `Script` component is used to load external scripts into your Next.js application. It ensures that the scripts are loaded asynchronously and won’t block the rendering of the page.

```javascript
import Script from "next/script";

export default function MyApp({ Component, pageProps }) {
  return (
    <>
      <Component {...pageProps} />
      <Script src="https://example.com/script.js" />
    </>
  );
}
```

This script will load and execute when any route in your application is accessed. Next.js will ensure the script will only load once, even if a user navigates between multiple pages.

### Head Component

The `Head` component allows you to modify the `<head>` tag of the document. It is useful for adding meta tags, managing the document title, and including external stylesheets or scripts.

```javascript
import Head from "next/head";

function IndexPage() {
  return (
    <div>
      <Head>
        <title>My page title</title>
      </Head>
      <p>Hello world!</p>
    </div>
  );
}

export default IndexPage;
```

To avoid duplicate tags in your head you can use the `key` property, which will make sure the tag is only rendered once, as in the following example:

```javascript
import Head from "next/head";

function IndexPage() {
  return (
    <div>
      <Head>
        <title>My page title</title>
        <meta property="og:title" content="My page title" key="title" />
      </Head>
      <Head>
        <meta property="og:title" content="My new title" key="title" />
      </Head>
      <p>Hello world!</p>
    </div>
  );
}

export default IndexPage;
```

These built-in components in Next.js provide convenient ways to handle common tasks and improve the performance and user experience of your application.

**[⬆ back to top](#table-of-contents)**

## Use CSS Modules

CSS Modules are a technique for localizing the scope of CSS class names in your web applications. They provide a way to encapsulate styles and prevent them from conflicting with each other when building complex user interfaces. CSS Modules are particularly useful in projects where multiple developers are working on different parts of the application, as they reduce the risk of inadvertently affecting styles in other components.

Here’s a Next.js example of how to use CSS Modules:

1. Open or create `styles.module.css` and add some CSS code:

   ```css
   /* styles.module.css */
   .button {
     background-color: blue;
     color: white;
     padding: 10px 20px;
     border: none;
     cursor: pointer;
   }
   ```

2. Create a new React component that uses the CSS Module:

   ```javascript
   import styles from "../styles/styles.module.css";

   export default function Home() {
     return (
       <div>
         <button className={styles.button}>Click me</button>
       </div>
     );
   }
   ```

In this example, the `styles.button` class is imported from the `styles.module.css` file using the `styles` object. This allows you to use the CSS classes defined in the module as properties of the `styles` object. This approach ensures that the class names are scoped to the component and won’t clash with global styles.

**[⬆ back to top](#table-of-contents)**

## Caching Data in Next.js

Caching data is an effective technique to improve the performance and user experience of Next.js applications. By storing frequently accessed data in a cache, subsequent requests can be served faster, reducing the load on the server and improving overall performance. Next.js provides various methods and strategies to implement caching in your application.

### Client-Side Caching

Next.js utilizes client-side caching to store and reuse data fetched from APIs or server-side rendering. This caching mechanism reduces the need to make repetitive API calls, resulting in faster page rendering and improved user experience.

To implement client-side caching in Next.js, you can take advantage of the SWR library. SWR is a powerful data fetching library that provides caching, revalidation, and deduplication of requests. Here’s a Next.js example of how to use SWR for caching data:

```javascript
import useSWR from "swr";

const fetcher = (url) => fetch(url).then((res) => res.json());

function MyComponent() {
  const { data } = useSWR("/api/data", fetcher);

  if (!data) {
    return <div>Loading...</div>;
  }

  return (
    <div>
      {data.map((item) => (
        <div key={item.id}>{item.name}</div>
      ))}
    </div>
  );
}
```

In the example above, the `useSWR` hook is used to fetch data from the `/api/data` endpoint. The `fetcher` function is responsible for performing the actual API call. The fetched data is automatically cached by SWR, and subsequent renders of the component will reuse the cached data until it expires.

### Server-Side Caching

By default, Next.js automatically caches the returned values of `fetch` in the Data Cache on the server. This means that the data can be fetched at build time or request time, cached, and reused on each data request.

```javascript
// 'force-cache' is the default, and can be omitted
fetch("https://...", { cache: "force-cache" });
```

`fetch` requests that use the POST method are also automatically cached. Unless it’s inside a Route Handler that uses the POST method, then it will not be cached.

If you are using Next.js 12 or an older version, here’s an example of how to use `getServerSideProps` for server-side caching:

```javascript
// pages/index.js

import React from "react";

function HomePage({ data }) {
  return (
    <div>
      <h1>Server Side Caching Example</h1>
      <ul>
        {data.map((item) => (
          <li key={item.id}>{item.title}</li>
        ))}
      </ul>
    </div>
  );
}

export async function getServerSideProps() {
  // Fetch data from an API or any other source
  const response = await fetch("https://api.example.com/data");
  const data = await response.json();

  // Cache the data for 10 seconds
  const maxAge = 10;
  const cacheControl = `public, max-age=${maxAge}`;

  return {
    props: {
      data,
    },
    // Set the cache control header
    headers: {
      "Cache-Control": cacheControl,
    },
  };
}

export default HomePage;
```

In the above example, the `getServerSideProps` function fetches data from an API and sets the `Cache-Control` header to cache the data for 10 seconds. The fetched data is then passed as props to the `HomePage` component.

Server-side caching is just one approach to improving performance in Next.js applications. You can also use client-side caching libraries like SWR to cache data on the client side.

**[⬆ back to top](#table-of-contents)**

## Use Route Handlers

Route Handlers allow you to create custom request handlers for a given route using the Web Request and Response APIs. They are defined in a [route.js|ts file](https://nextjs.org/docs/app/api-reference/file-conventions/route) inside the app directory.

Here is an example:

```typescript
// app/items/route.ts

import { NextResponse } from "next/server";

export async function GET() {
  const res = await fetch("https://data.mongodb-api.com/...", {
    headers: {
      "Content-Type": "application/json",
      "API-Key": process.env.DATA_API_KEY,
    },
  });
  const data = await res.json();

  return NextResponse.json({ data });
}

// POST Request
export async function POST() {
  const res = await fetch("https://data.mongodb-api.com/...", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "API-Key": process.env.DATA_API_KEY,
    },
    body: JSON.stringify({ time: new Date().toISOString() }),
  });

  const data = await res.json();

  return NextResponse.json(data);
}
```

**[⬆ back to top](#table-of-contents)**

## Use Multiple Data Rendering Modes

Data fetching and rendering data on the webpage is a core part of any application. Here’s how you can fetch, cache, and revalidate data in React and Next.js:

### Fetching Data on the Server with `fetch`

Next.js extends the native [fetch Web API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) to allow you to configure the caching and revalidating behavior for each fetch request on the server. React extends fetch to automatically memoize fetch requests while rendering a React component tree.

```typescript
async function getData() {
  const res = await fetch("https://api.example.com/...");
  // The return value is *not* serialized
  // You can return Date, Map, Set, etc.

  if (!res.ok) {
    // This will activate the closest `error.js` Error Boundary
    throw new Error("Failed to fetch data");
  }

  return res.json();
}

export default async function Page() {
  const data = await getData();

  return <main>{/* render data here */}</main>;
}
```

### Revalidating Data

Revalidation is the process of purging the Data Cache and re-fetching the latest data. This is useful when your data changes and you want to ensure you show the latest information.

```typescript
fetch("https://...", { next: { revalidate: 3600 } });
```

Alternatively, to revalidate all fetch requests in a route segment, you can use the Segment Config Options:

```typescript
export const revalidate = 3600; // revalidate at most every hour
```

### Fetching Data on the Server with Third-Party Libraries

If you’re using a third-party library that doesn’t support or expose fetch (for example, a database, CMS, or ORM client), you can configure the caching and revalidating behavior of those requests using the Route Segment Config Option and React’s cache function.

```typescript
import { cache } from "react";

export const revalidate = 3600; // revalidate the data at most every hour

export const getItem = cache(async (id: string) => {
  const item = await db.item.findUnique({ id });
  return item;
});
```

### Fetching Data on the Client with Route Handlers

If you need to fetch data in a client component, you can call a Route Handler from the client. Route Handlers execute on the server and return the data to the client. This is useful when you don’t want to expose sensitive information to the client, such as API tokens.

```typescript
// app/data/route.ts

import { NextResponse } from "next/server";

export async function GET() {
  const res = await fetch("https://data.mongodb-api.com/...", {
    headers: {
      "Content-Type": "application/json",
      "API-Key": process.env.DATA_API_KEY,
    },
  });
  const data = await res.json();

  return NextResponse.json({ data });
}
```

### Fetching Data on the Client with Third-Party Libraries

You can also fetch data on the client using a third-party library such as SWR or React Query. These libraries provide their own APIs for memoizing requests, caching, revalidating, and mutating data.

Example using SWR:

```typescript
import useSWR from "swr";

function Profile() {
  const { data, error, isLoading } = useSWR("/api/user", fetcher);

  if (error) return <div>failed to load</div>;
  if (isLoading) return <div>loading...</div>;
  return <div>hello {data.name}!</div>;
}
```

**[⬆ back to top](#table-of-contents)**

## TypeScript Support

One of the best practices when working with Next.js projects is to leverage TypeScript for an enhanced development experience and improved code quality. TypeScript is a statically-typed superset of JavaScript that adds type annotations to the language. It provides several benefits that make it a best practice for Next.js projects:

### Type Safety

TypeScript introduces static typing, allowing developers to catch errors during development rather than at runtime. By specifying types for variables, function parameters, and return values, TypeScript helps identify potential bugs and provides better code completion and refactoring tools. This reduces the likelihood of runtime errors, leading to more reliable and maintainable code.

```typescript
// Example: TypeScript Type Annotation
function greet(name: string): string {
  return `Hello, ${name}!`;
}
```

### Better Tooling and IntelliSense

TypeScript integrates well with modern development tools and IDEs such as Visual Studio Code. These tools provide powerful features like IntelliSense, autocompletion, and code navigation, making it easier to write, refactor, and maintain code. TypeScript’s type system enables tools to provide accurate suggestions and catch potential errors, resulting in a more productive development experience.

### Improved Collaboration

In a collaborative project, TypeScript can greatly enhance the development process. With type annotations, developers can understand the structure and expected behavior of functions and objects without having to dive into the implementation details. This improves communication between team members and reduces the chances of misinterpreting code.

### Ecosystem and Community Support

TypeScript has gained significant popularity and has a thriving ecosystem and community. Many popular libraries and frameworks have official TypeScript support, including Next.js. This means that when using TypeScript in Next.js projects, you can take advantage of type definitions and improved interoperability with these libraries. Additionally, the TypeScript community provides helpful resources, tutorials, and support, making it easier to learn and troubleshoot.

**[⬆ back to top](#table-of-contents)**

## Enhance SEO in Next.js

To enhance SEO in a Next.js app, one effective approach is to use the [next-seo](https://www.npmjs.com/package/next-seo) package. This package provides a set of components and utilities that make it easy to add meta tags, structured data, and other SEO-related elements to your Next.js application.

To implement meta tags using [next-seo](https://www.npmjs.com/package/next-seo), you can make use of the NextSeo component provided by the package. This component allows you to define meta tags such as the title, description, and canonical URL for each page of your application.

Here’s an example of how you can implement meta tags using [next-seo](https://www.npmjs.com/package/next-seo):

```typescript
import { NextSeo } from "next-seo";

function MyPage() {
  return (
    <>
      <NextSeo
        title="My Page"
        description="This is my awesome page"
        canonical="https://www.example.com/my-page"
      />
      {/* Rest of your page content */}
    </>
  );
}

export default MyPage;
```

### Benefits of Better SEO

- **Visibility**: SEO helps your website appear higher in search engine results. Better SEO increases the chances of your site showing up, leading to more visibility and organic traffic.
- **Organic Traffic**: Search engines like Google are major sources of traffic. Improving your SEO means you’re more likely to attract visitors who are actively looking for the content or products your website offers.
- **Credibility**: High-ranking websites often appear more credible to users. People tend to trust search engine results, so a strong presence in these results can positively impact your site’s credibility and reputation.
- **User Experience**: Many SEO practices focus on improving the structure and usability of your website. This benefits not only search engines but also your visitors. A well-optimized site is easier to navigate and provides a better user experience.
- **Long-Term Benefits**: SEO is an investment that can pay off over time. While it might take time to see significant results, the efforts you put into optimizing your site can have lasting benefits.
- **Cost-Effective**: Organic traffic from search engines is essentially free. While you might invest in SEO strategies, the long-term benefits often outweigh the initial costs, making it a cost-effective marketing approach.
- **Ad Blockers**: Many users employ ad blockers or simply ignore online ads. Having a strong organic presence through effective SEO ensures that your website still gets seen even when users avoid ads.
- **Analytics and Insights**: Properly implemented SEO often comes with analytics tools that provide insights into your website’s performance and user behavior. This data can help you refine your strategies and improve your site further.

**[⬆ back to top](#table-of-contents)**

## Remove Unused Dependencies & Code

The act of removing unused dependencies and files within a software project holds substantial importance for a variety of reasons:

- **Efficiency and Performance**: Unused dependencies and files can bloat the application’s package size, slowing down loading times. By eliminating these

extraneous elements, the codebase becomes more streamlined, resulting in quicker load times and enhanced overall performance.

- **Resource Management**: Unused dependencies consume storage space within the project’s structure, and while this might seem negligible in smaller projects, it can accumulate and pose challenges in larger endeavors. Effective resource management is essential for maintaining a well-organized and clutter-free codebase.
- **Security**: Each dependency introduced into a project carries potential security risks. By removing unused dependencies, the attack surface is reduced, minimizing vulnerabilities and enhancing the application’s overall security posture.

Use the [depcheck](https://www.npmjs.com/package/depcheck) package to find unused dependencies in your project, and use [unimported](https://www.npmjs.com/package/unimported) to remove all the unimported files.

**[⬆ back to top](#table-of-contents)**

## Deploy Your Next.js App to Vercel

Deploying your Next.js app to Vercel is a seamless and efficient way to bring your website or application to life. Vercel, a cloud platform for static sites and serverless functions, is the perfect choice for hosting your Next.js projects. Here are a few reasons why deploying to Vercel is a great option:

### Easy Deployment Process

Vercel provides a simple and intuitive deployment process for Next.js apps. With just a few clicks, you can deploy your app and have it up and running in no time. Vercel automatically handles the build and deployment process, so you can focus on developing your app without worrying about complex deployment configurations.

### Automatic Scaling

Vercel's infrastructure is designed to scale your app effortlessly. Whether you have a small personal blog or a high-traffic e-commerce website, Vercel can handle the load. With automatic scaling, your app will be able to handle increased traffic without any performance issues.

### Global Network

Vercel has a global network of edge servers that ensure fast and reliable content delivery to users worldwide. This means that your app will load quickly regardless of the user’s geographical location. With Vercel, you can provide a smooth and optimized user experience to your visitors.

### Serverless Functions

Vercel supports serverless functions, allowing you to build and deploy serverless APIs alongside your Next.js app. This enables you to create dynamic and interactive features without the need for a separate backend infrastructure.

### Custom Domains and SSL

Vercel makes it easy to connect your custom domain to your Next.js app. You can easily configure your domain settings and enable SSL certificates for secure communication. This gives your app a professional and polished look while ensuring the privacy and security of your users.

### Real-time Collaboration

Vercel provides a collaborative environment where you can work with your team members on the same project. You can easily share preview URLs and collaborate on code changes, making it a breeze to iterate and ship your app faster.

Overall, deploying your Next.js app to Vercel offers a range of benefits, including an easy deployment process, automatic scaling, global network, serverless functions, custom domains, SSL support, and real-time collaboration. With Vercel, you can focus on building your app while enjoying the power and flexibility of a reliable hosting platform.

## Client Component Placement Strategy in Next.js

To ensure optimal performance and efficient rendering in a Next.js application, it's crucial to structure your components in a way that maximizes the benefits of server-side rendering and client-side interactivity. One effective approach is to place client components as lower leaves in the component tree hierarchy. This strategy leverages Next.js's ability to pre-render pages at build time or request time while allowing dynamic components to handle interactive features on the client side.

### Why Place Client Components as Lower Leaves

Placing client components, such as those utilizing dynamic data fetching or state management, as lower leaves in the component tree offers several advantages:

- **Enhanced Initial Page Load**: Components rendered on the server provide faster initial page load times as they are pre-rendered and delivered to the client.
- **Improved SEO and Accessibility**: Server-rendered content is accessible to search engines and users with JavaScript disabled, ensuring better SEO performance and accessibility compliance.

- **Progressive Enhancement**: By loading basic content server-side and enhancing it with client-side interactivity, you provide a smooth user experience that gracefully degrades for users with slower connections or less capable devices.

### Example Implementation

```jsx
import React from "react";
import { useEffect, useState } from "react";

const DynamicContent = () => {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetchData();
  }, []);

  const fetchData = async () => {
    // Example: Fetch data dynamically on the client side
    const response = await fetch("https://api.example.com/data");
    const newData = await response.json();
    setData(newData);
  };

  return (
    <div>
      {data.map((item) => (
        <div key={item.id}>{item.name}</div>
      ))}
    </div>
  );
};

export default DynamicContent;
```

In this example, `DynamicContent` fetches data dynamically on the client side, making it suitable for placement as a lower leaf component in a Next.js application. This ensures that server-side rendering optimizes initial page load performance while maintaining flexibility for client-side interactions.
