# Error Handling in Next.js

## Introduction

Error handling in Next.js is essential for creating robust applications. It involves managing errors gracefully both in the frontend and backend to enhance user experience and maintain application stability.

## Handling Errors in API Routes

### Example

Catch and respond to errors in API routes:

```javascript
export default async function handler(req, res) {
  try {
    // Your logic here
    res.status(200).json({ message: 'Success' });
  } catch (error) {
    res.status(500).json({ error: 'Internal Server Error' });
  }
}
```

## Custom Error Pages

Create custom error pages for better UX.

### Example

Custom 404 page (`/pages/404.js`):

```javascript
export default function Custom404() {
  return <h1>404 - Page Not Found</h1>;
}
```

Custom 500 page (`/pages/500.js`):

```javascript
export default function Custom500() {
  return <h1>500 - Server Error</h1>;
}
```

## Client-Side Error Handling

Handle errors in components using try-catch or error boundaries.

### Example with Try-Catch

```javascript
function MyComponent() {
  try {
    // Component logic here
  } catch (error) {
    return <p>An error occurred: {error.message}</p>;
  }
}
```

### Example with Error Boundaries

Create an error boundary component:

```javascript
import React from 'react';

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // Log error to an error reporting service
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```

Wrap your component with the error boundary:

```javascript
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>
```

## Conclusion

Effective error handling in Next.js involves catching errors in API routes, creating custom error pages, and managing client-side errors with try-catch or error boundaries. This ensures a more stable and user-friendly application.

For more detailed information, visit [Next.js Error Handling](https://nextjs.org/learn/dashboard-app/error-handling).