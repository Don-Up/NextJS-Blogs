# Streaming in Next.js

## Introduction

Next.js supports streaming, allowing you to **send parts of a response to the client as soon as they are available**. This can significantly improve the user experience by reducing the time it takes for content to be displayed.

## Benefits of Streaming

- **Improved Performance**: Content is rendered and sent to the client incrementally, reducing load times.
- **Better User Experience**: Users can see content sooner rather than waiting for the entire page to load.

## Implementation

To implement streaming, use the `React.lazy` function to **lazy-load components** and the `Suspense` component to **display fallback content** while loading.

### Example

```jsx
import { Suspense } from 'react';
import dynamic from 'next/dynamic';

// Dynamic import of the LazyComponent with suspense support
// P1-import('..'): Dynamically import the LazyComponent
// P2-{suspense: true,}: Enable suspense mode for the imported component
// Ret-LazyComponent: Assign the dynamically imported component to LazyComponent
const LazyComponent = dynamic(() => import('../components/LazyComponent'), {
  suspense: true,
});

export default function Page() {
  return (
    <div>
      <h1>My Page</h1>
      {/* Suspense component with a fallback while LazyComponent is loading */}
      <Suspense fallback={<div>Loading...</div>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}
```

### Steps:

1. **Use `React.lazy`**: Lazy-load components to split the bundle and only load them when needed.
2. **Wrap with `Suspense`**: Provide a fallback UI while the lazy-loaded component is being fetched and rendered.

## Conclusion

Streaming in Next.js enhances performance and user experience by incrementally sending content to the client. This approach ensures users see parts of the page sooner, leading to a smoother browsing experience.

For more detailed information, visit [Next.js Streaming](https://nextjs.org/learn/dashboard-app/streaming).