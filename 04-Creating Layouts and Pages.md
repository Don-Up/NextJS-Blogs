# Creating Layouts and Pages in Next.js

## Introduction

Creating layouts and pages efficiently in Next.js helps maintain consistent design and structure across your application. This guide covers setting up basic layouts and pages, and how to use them effectively.

## Setting Up Layouts

Layouts in Next.js are used to wrap around page components, providing a consistent structure such as headers, footers, or navigation menus.

### Example Layout Component

Create a layout component (`/app/layout.tsx`):

```javascript
export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <header>Header</header>
        <main>{children}</main>
        <footer>Footer</footer>
      </body>
    </html>
  );
}
```

### Applying Layout to Pages

Use the layout component to wrap around your page components:

```javascript
// pages/_app.tsx
import RootLayout from '../app/layout';

function MyApp({ Component, pageProps }) {
  return (
    <RootLayout>
      <Component {...pageProps} />
    </RootLayout>
  );
}

export default MyApp;
```

## Creating Pages

Next.js uses a file-based routing system where each file in the `pages` directory corresponds to a route.

### Example Pages

#### Home Page

Create a home page (`/pages/index.tsx`):

```javascript
export default function HomePage() {
  return (
    <div>
      <h1>Home Page</h1>
      <p>Welcome to the home page!</p>
    </div>
  );
}
```

#### About Page

Create an about page (`/pages/about.tsx`):

```javascript
export default function AboutPage() {
  return (
    <div>
      <h1>About Page</h1>
      <p>This is the about page.</p>
    </div>
  );
}
```

## Dynamic Routes

Dynamic routing allows creating pages with dynamic content. For example, a blog post page where the URL contains the post ID.

### Example Dynamic Route

Create a dynamic route (`/pages/posts/[id].tsx`):

```javascript
import { useRouter } from 'next/router';

export default function PostPage() {
  // Initialize the router to access route parameters and methods
  const router = useRouter();
  // Extract the 'id' parameter from the router query
  const { id } = router.query;

  return (
    <div>
      <h1>Post {id}</h1>
      <p>This is the content for post {id}.</p>
    </div>
  );
}
```

## Conclusion

By setting up layouts and creating both static and dynamic pages, you can build a structured and consistent Next.js application. This approach allows for reusable components and easier maintenance.

For detailed information, visit [Next.js Creating Layouts and Pages](https://nextjs.org/learn/dashboard-app/creating-layouts-and-pages).