# Partial Prerendering in Next.js

## Introduction

Partial prerendering in Next.js allows you to statically generate parts of a page at build time while other parts are rendered dynamically. This approach optimizes both performance and flexibility.

## Incremental Static Regeneration (ISR)

ISR enables **updating static content without rebuilding the entire site**. You can use ISR to refresh data at a specified interval.

> Main Differences between ISR and SSG
>
> * **ISR**: Updates static content incrementally, providing flexibility and performance.
> * **SSG (Static Site Generation)**: Generates static pages at build time and requires a full rebuild to update content.

### Example

```javascript
export async function getStaticProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return {
    props: { data },
    revalidate: 60, // Revalidate every 60 seconds
  };
}

export default function Page({ data }) {
  return <div>{/* Render your data here */}</div>;
}
```

### Benefits

- **Performance**: Combines the speed of static generation with the flexibility of server-side rendering.
- **SEO**: Ensures content is always up-to-date for search engines.

## Conclusion

Partial prerendering with ISR in Next.js enhances performance by updating static content incrementally. This method ensures your application is fast, up-to-date, and SEO-friendly.

For detailed information, visit [Next.js Partial Prerendering](https://nextjs.org/learn/dashboard-app/partial-prerendering).