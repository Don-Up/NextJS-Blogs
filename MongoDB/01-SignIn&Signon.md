# Signin & Signon

### 1. Create Custom `useAuth` Hook

**`hooks/useAuth.ts`**
```typescript
import { useEffect, useState } from 'react';
import { useRouter } from 'next/router';

export const useAuth = () => {
  const [user, setUser] = useState<string | null>(null);
  const router = useRouter();

  useEffect(() => {
    const token = localStorage.getItem('token');
    if (token) {
      const username = localStorage.getItem('username');
      setUser(username);
    }
  }, []);

  const logout = () => {
    localStorage.removeItem('token');
    localStorage.removeItem('username');
    setUser(null);
    router.push('/login');
  };

  return { user, logout };
};
```

### 2. Update Login API Endpoint

Ensure the login API endpoint returns a token and username.

**`pages/api/login.ts`**
```typescript
import { NextApiRequest, NextApiResponse } from 'next';
import connectToDatabase from '../../database/mongo';
import jwt from 'jsonwebtoken';

const SECRET_KEY = 'your-secret-key'; // Use environment variable in production

export default async function handler(req: NextApiRequest, res: NextApiResponse) {
  if (req.method === 'POST') {
    const { username, password } = req.body;

    if (!username || !password) {
      return res.status(400).json({ error: 'Username and password are required' });
    }

    const db = await connectToDatabase();
    const collection = db.collection('users');

    try {
      const user = await collection.findOne({ username, password });
      if (!user) {
        return res.status(400).json({ error: 'Invalid username or password' });
      }

      const token = jwt.sign({ username }, SECRET_KEY, { expiresIn: '1h' });
      res.status(200).json({ token, username }); // Return token and username
    } catch (error) {
      res.status(500).json({ error: (error as Error).message });
    }
  } else {
    res.status(405).json({ message: 'Method not allowed' });
  }
}
```

### 3. Update Login Page

Save the token and username to local storage on successful login.

**`app/login/page.tsx`**
```typescript
'use client';

import { useState } from 'react';
import { useRouter } from 'next/navigation';

export default function LoginPage() {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');
  const router = useRouter();

  const handleLogin = async () => {
    setError('');
    const res = await fetch('/api/login', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ username, password }),
    });

    const data = await res.json();
    if (res.ok) {
      localStorage.setItem('token', data.token);
      localStorage.setItem('username', username);
      router.push('/');
    } else {
      setError(data.error);
    }
  };

  const handleRegisterRedirect = () => {
    router.push('/register');
  };

  return (
    <div className="flex flex-col items-center justify-center min-h-screen py-2">
      <main className="flex flex-col items-center justify-center flex-1 w-full px-20 text-center">
        <h1 className="text-6xl font-bold">Login</h1>
        <div className="mt-8 w-full max-w-md">
          <input
            type="text"
            value={username}
            onChange={(e) => setUsername(e.target.value)}
            className="w-full p-2 border border-gray-300 rounded-md"
            placeholder="Username"
          />
          <input
            type="password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            className="w-full p-2 border border-gray-300 rounded-md mt-4"
            placeholder="Password"
          />
          <button
            onClick={handleLogin}
            className="mt-4 px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600"
          >
            Login
          </button>
          {error && <p className="mt-4 text-red-500">{error}</p>}
          <button
            onClick={handleRegisterRedirect}
            className="mt-4 px-4 py-2 bg-gray-500 text-white rounded-md hover:bg-gray-600"
          >
            Register
          </button>
        </div>
      </main>
    </div>
  );
}
```

### 4. Create Registration Page

Create a registration page that allows users to register and redirect to the login page upon successful registration.

**`app/register/page.tsx`**
```typescript
'use client';

import { useState } from 'react';
import { useRouter } from 'next/navigation';

export default function RegisterPage() {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');
  const [success, setSuccess] = useState('');
  const router = useRouter();

  const handleRegister = async () => {
    setError('');
    setSuccess('');
    const res = await fetch('/api/register', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ username, password }),
    });

    const data = await res.json();
    if (res.ok) {
      setSuccess('Registration successful!');
      setUsername('');
      setPassword('');
      setTimeout(() => router.push('/login'), 2000);
    } else {
      setError(data.error);
    }
  };

  return (
    <div className="flex flex-col items-center justify-center min-h-screen py-2">
      <main className="flex flex-col items-center justify-center flex-1 w-full px-20 text-center">
        <h1 className="text-6xl font-bold">Register</h1>
        <div className="mt-8 w-full max-w-md">
          <input
            type="text"
            value={username}
            onChange={(e) => setUsername(e.target.value)}
            className="w-full p-2 border border-gray-300 rounded-md"
            placeholder="Username"
          />
          <input
            type="password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            className="w-full p-2 border border-gray-300 rounded-md mt-4"
            placeholder="Password"
          />
          <button
            onClick={handleRegister}
            className="mt-4 px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600"
          >
            Register
          </button>
          {error && <p className="mt-4 text-red-500">{error}</p>}
          {success && <p className="mt-4 text-green-500">{success}</p>}
        </div>
      </main>
    </div>
  );
}
```

### 5. Protect Home Page

Check for token on the home page and redirect to the login page if not found.

**`pages/index.tsx`**
```typescript
import { useEffect } from 'react';
import { useRouter } from 'next/router';
import { useAuth } from '../hooks/useAuth';

export default function HomePage() {
  const { user, logout } = useAuth();
  const router = useRouter();

  useEffect(() => {
    const token = localStorage.getItem('token');
    if (!token) {
      router.push('/login');
    }
  }, [router]);

  return (
    <div className="flex flex-col items-center justify-center min-h-screen py-2">
      <main className="flex flex-col items-center justify-center flex-1 w-full px-20 text-center">
        <h1 className="text-6xl font-bold">My TODO List</h1>
        {/* ... */}
      </main>
      {user ? (
        <div className="absolute top-4 right-4 flex items-center space-x-4">
          <span>{user}</span>
          <button
            onClick={logout}
            className="px-4 py-2 bg-red-500 text-white rounded-md hover:bg-red-600"
          >
            Logout
          </button>
        </div>
      ) : (
        <div className="absolute top-4 right-4">
          <button
            onClick={() => router.push('/login')}
            className="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600"
          >
            Login
          </button>
        </div>
      )}
    </div>
  );
}
```

### Summary

1. **Create Custom Hook**: `useAuth` to manage authentication state.
2. **Update Login API**: Ensure it returns token and username.
3. **Login Page**: Save token and username to local storage on successful login.
4. **Registration Page**: Allow users to register and redirect to login page.
5. **Protect Home Page**: Check for token and redirect to login if not found.

This setup ensures that users are redirected to the login page if

 they are not authenticated and provides a seamless login and registration experience.