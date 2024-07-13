# Mutating Data in Next.js

## Introduction

Mutating data in a Next.js application involves creating, updating, or deleting data on the server. This guide explains how to handle data mutations using API routes and form handling.

## Creating an API Route

API routes in Next.js allow you to create server-side functions for data mutations.

### Example

Create a new API route (`/pages/api/create.js`):

```javascript
export default async function handler(req, res) {
  if (req.method === 'POST') {
    const { name, email } = req.body;

    // Handle data creation logic here
    // Example: Save to database

    res.status(200).json({ message: 'Data created successfully' });
  } else {
    res.status(405).json({ message: 'Method not allowed' });
  }
}
```

## Form Handling

### Example

Create a form component to handle user input and send data to the API route:

```javascript
import { useState } from 'react';

export default function CreateForm() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');

  const handleSubmit = async (e) => {
    e.preventDefault();

    const res = await fetch('/api/create', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ name, email }),
    });

    const data = await res.json();
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        placeholder="Name"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <input
        type="email"
        placeholder="Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

## Conclusion

Mutating data in Next.js involves setting up API routes to handle server-side data changes and creating forms to collect and submit data. This approach allows for efficient data management within your application.

For more detailed information, visit [Next.js Mutating Data](https://nextjs.org/learn/dashboard-app/mutating-data).