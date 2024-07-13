# Fetching Data in Next.js

## Introduction

Fetching data in Next.js can be done using various methods depending on the data source and the desired behavior of your application. The primary methods are `getStaticProps`, `getServerSideProps`, and `getStaticPaths`.

## `getStaticProps`

`getStaticProps` allows you to fetch data at build time, generating static HTML.

### Example

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

- Improves performance by generating static pages.
- Ideal for content that does not change frequently.

## `getServerSideProps`

`getServerSideProps` allows you to fetch data on each request, generating dynamic HTML.

### Example

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

- Provides the most up-to-date data.
- Suitable for content that changes frequently or requires authentication.

## `getStaticPaths`

`getStaticPaths` is used with `getStaticProps` to generate dynamic routes at build time.

### Example

```javascript
export async function getStaticPaths() {
  const res = await fetch('https://api.example.com/data');
  const items = await res.json();

  const paths = items.map(item => ({
    params: { id: item.id },
  }));

  return { paths, fallback: false };
}

export async function getStaticProps({ params }) {
  const res = await fetch(`https://api.example.com/data/${params.id}`);
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

- Generates static pages for each route at build time.
- Improves performance for dynamic routes.

## Conclusion

Choosing the right data fetching method in Next.js depends on your application's needs. Use `getStaticProps` for static content, `getServerSideProps` for dynamic content, and `getStaticPaths` for dynamic routes.

For detailed information, visit [Next.js Fetching Data](https://nextjs.org/learn/dashboard-app/fetching-data).