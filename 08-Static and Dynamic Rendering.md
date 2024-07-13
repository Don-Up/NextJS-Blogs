# Static and Dynamic Rendering in Next.js

## Introduction

Next.js supports both static and dynamic rendering, allowing developers to choose the best approach for their specific use case. This flexibility helps optimize performance and user experience.

## Static Rendering

### Static Generation

Static Generation is a method where HTML is generated at build time. It can be done with or without data. Use `getStaticProps` and `getStaticPaths` to fetch data and generate pages.

#### Example

```javascript
export async function getStaticProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return {
    props: {
      data,
    },
  };
}

export default function Page({ data }) {
  return <div>{/* Render your data here */}</div>;
}
```

### Benefits

- Improved performance with fast load times.
- Better SEO as content is available at build time.

## Dynamic Rendering

### Server-Side Rendering (SSR)

Server-Side Rendering generates HTML on each request. Use `getServerSideProps` to fetch data for SSR.

#### Example

```javascript
export async function getServerSideProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return {
    props: {
      data,
    },
  };
}

export default function Page({ data }) {
  return <div>{/* Render your data here */}</div>;
}
```

### Benefits

- Ensures the most up-to-date data.
- Suitable for pages that require authentication or frequently changing data.

## Conclusion

Next.js provides powerful rendering options with Static Generation and Server-Side Rendering. Choose the method that best suits your application's needs to optimize performance and user experience.

For more details, visit [Next.js Static and Dynamic Rendering](https://nextjs.org/learn/dashboard-app/static-and-dynamic-rendering).