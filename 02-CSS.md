# CSS Styling in Next.js

Learn how to add and manage CSS styles in a Next.js application. This guide covers global styles, Tailwind CSS, CSS Modules, and the `clsx` library.

## Global Styles

Add a global CSS file to apply site-wide styles:

```javascript
// /app/layout.tsx
import '@/app/ui/global.css';

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

## Tailwind CSS

Tailwind allows utility-first CSS directly in TSX:

```javascript
// /app/page.tsx
<main className="flex min-h-screen flex-col p-6">
  <div className="flex h-20 items-end rounded-lg bg-blue-500 p-4">...</div>
</main>
```

## CSS Modules

CSS Modules scope CSS to components, preventing style collisions:

```css
/* /app/ui/home.module.css */
.shape {
  height: 0;
  width: 0;
  border-bottom: 30px solid black;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
}
```

```javascript
// /app/page.tsx
import styles from '@/app/ui/home.module.css';

<div className={styles.shape} />
```

## clsx Library

Toggle class names conditionally with `clsx`:

```javascript
// /app/ui/invoices/status.tsx
import clsx from 'clsx';

export default function InvoiceStatus({ status }) {
  return (
    <span className={clsx('inline-flex items-center rounded-full px-2 py-1 text-sm', {
      'bg-gray-100 text-gray-500': status === 'pending',
      'bg-green-500 text-white': status === 'paid',
    })}>
      {status}
    </span>
  );
}
```

## Other Styling Solutions

- **Sass**: Import `.css` and `.scss` files.
- **CSS-in-JS**: Use libraries like `styled-jsx`, `styled-components`, and `emotion`.

Check out the [CSS documentation](https://nextjs.org/learn/dashboard-app/css-styling) for more details.