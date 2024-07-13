# Setting Up Your Database in Next.js

## Introduction

Setting up a database in your Next.js application involves selecting a database, connecting to it, and integrating it with your application.

## Choosing a Database

You can choose between SQL (e.g., PostgreSQL, MySQL) or NoSQL (e.g., MongoDB) databases based on your application's needs.

## Connecting to the Database

### Example using PostgreSQL with Prisma:

1. **Install Prisma**:
   ```bash
   npm install @prisma/client
   npx prisma init
   ```

2. **Configure the database connection** in the `.env` file:
   ```plaintext
   DATABASE_URL="postgresql://user:password@localhost:5432/mydatabase"
   ```

3. **Define your schema** in `prisma/schema.prisma`:
   ```prisma
   datasource db {
     provider = "postgresql"
     url      = env("DATABASE_URL")
   }
   
   generator client {
     provider = "prisma-client-js"
   }
   
   model User {
     id    Int     @id @default(autoincrement())
     name  String
     email String  @unique
   }
   ```

4. **Generate the Prisma Client**:
   ```bash
   npx prisma generate
   ```

5. **Use the Prisma Client in your application**:
   ```javascript
   import { PrismaClient } from '@prisma/client';
   
   const prisma = new PrismaClient();
   
   async function main() {
     const newUser = await prisma.user.create({
       data: {
         name: 'Alice',
         email: 'alice@prisma.io',
       },
     });
     console.log(newUser);
   }
   
   main()
     .catch(e => {
       throw e;
     })
     .finally(async () => {
       await prisma.$disconnect();
     });
   ```

## Conclusion

Setting up a database in Next.js involves choosing the right database, configuring the connection, and integrating it with your application using an ORM like Prisma. This allows for efficient data management and interaction within your Next.js project.

For detailed information, visit [Next.js Setting Up Your Database](https://nextjs.org/learn/dashboard-app/setting-up-your-database).