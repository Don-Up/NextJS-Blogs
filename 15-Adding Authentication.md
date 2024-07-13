# Adding Authentication in Next.js

## Introduction

Adding authentication to your Next.js application is essential for managing user sessions, protecting routes, and providing personalized experiences.

## Using NextAuth.js

NextAuth.js is a complete open-source authentication solution for Next.js applications.

### Installation

1. **Install dependencies**:
   ```bash
   npm install next-auth
   ```

2. **Create an API route for authentication**:
   Create a file at `/pages/api/auth/[...nextauth].js`:
   ```javascript
   import NextAuth from 'next-auth';
   import Providers from 'next-auth/providers';
   
   export default NextAuth({
     providers: [
       Providers.GitHub({
         clientId: process.env.GITHUB_ID,
         clientSecret: process.env.GITHUB_SECRET,
       }),
     ],
   });
   ```

3. **Environment Variables**:
   Add your environment variables in a `.env.local` file:
   ```plaintext
   GITHUB_ID=yourgithubid
   GITHUB_SECRET=yourgithubsecret
   ```

### Protecting Pages

1. **Create a higher-order component (HOC) to protect pages**:
   ```javascript
   import { getSession } from 'next-auth/client';
   
   export default function ProtectedPage({ session }) {
     if (!session) {
       return <p>You need to be authenticated to view this page.</p>;
     }
     return <p>Welcome, authenticated user!</p>;
   }
   
   export async function getServerSideProps(context) {
     const session = await getSession(context);
     return {
       props: { session },
     };
   }
   ```

2. **Using the HOC**:
   Apply the HOC to any page that requires authentication.

### Example Usage

Create a protected page (`/pages/protected.js`):

```javascript
import { getSession } from 'next-auth/client';

export default function ProtectedPage({ session }) {
  if (!session) {
    return <p>You need to be authenticated to view this page.</p>;
  }
  return <p>Welcome, {session.user.name}!</p>;
}

export async function getServerSideProps(context) {
  const session = await getSession(context);
  return {
    props: { session },
  };
}
```

## Conclusion

Adding authentication with NextAuth.js in Next.js involves setting up an API route, configuring providers, and protecting pages. This approach offers a robust solution for managing user sessions and securing your application.

For more detailed information, visit [Next.js Adding Authentication](https://nextjs.org/learn/dashboard-app/adding-authentication).