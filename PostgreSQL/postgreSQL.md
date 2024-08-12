# PostgreSQL

PostgreSQL is a powerful, open-source relational database management system (RDBMS) known for its robustness, extensibility, and standards compliance. 

It supports both SQL (relational) and JSON (non-relational) querying, offering flexibility for diverse applications. PostgreSQL boasts features like ACID compliance, full-text search, and advanced indexing techniques. Its extensibility allows users to define custom functions, types, and operators. PostgreSQL also supports complex queries, foreign keys, triggers, and views. With its strong community and extensive documentation, PostgreSQL is widely used in web applications, data warehousing, and various enterprise-level solutions, ensuring high performance, scalability, and reliability.



## Set Up PostgreSQL

**Install PostgreSQL**: If you haven't installed PostgreSQL, you can download and install it from [here](https://www.postgresql.org/download/).



## Create a database

 Create a new database for your application.

![image-20240714135446457](assets/image-20240714135446457.png)

<img src="assets/image-20240714135559333.png" alt="image-20240714135559333" style="zoom: 80%;" />

## Install Required Packages

Install `pg` (node-postgres) and `dotenv` for environment variable management.

```bas
npm install pg dotenv
```



##  Configure Environment Variables

Create a `.env` file in the root of your project and add your PostgreSQL connection string.

```bash
DATABASE_URL=postgres://username:password@localhost:5432/todos
```



##  Create Database Connection

```js
// database/index.js
const { Pool } = require('pg');
const dotenv = require('dotenv');

dotenv.config();

const pool = new Pool({
    connectionString: process.env.DATABASE_URL
});

module.exports = pool;
```



## Create Tables

1. Create a `database/init.js` file to set up your database schema.

   ```js
   const pool = require('./index');
   
   const createTableText = `
   CREATE TABLE IF NOT EXISTS todos (
       id SERIAL PRIMARY KEY,
       text TEXT NOT NULL
   )`;
   
   pool.query(createTableText)
       .then(res => {
           console.log('Table is successfully created');
       })
       .catch(err => {
           console.error('Error creating table', err);
       })
       .finally(() => {
           pool.end();
       });
   ```

2. Run the `init.js` file to create the table.

   ```bash
   node database/init.mjs
   ```



## Update API Routes

Update `pages/api/todos.js` to use PostgreSQL.

```js
import pool from '../../database';

export default async function handler(req, res) {
    if (req.method === 'GET') {
        try {
            const result = await pool.query('SELECT * FROM todos');
            res.status(200).json({ todos: result.rows });
        } catch (err) {
            res.status(500).json({ error: err.message });
        }
    } else if (req.method === 'POST') {
        const { text } = req.body;
        try {
            const result = await pool.query(
                'INSERT INTO todos (text) VALUES ($1) RETURNING *',
                [text]
            );
            res.status(201).json(result.rows[0]);
        } catch (err) {
            res.status(500).json({ error: err.message });
        }
    } else if (req.method === 'DELETE') {
        const { id } = req.body;
        try {
            await pool.query('DELETE FROM todos WHERE id = $1', [id]);
            res.status(200).json({ message: 'Todo deleted' });
        } catch (err) {
            res.status(500).json({ error: err.message });
        }
    } else {
        res.status(405).json({ message: 'Method not allowed' });
    }
}
```



## Complement

### SyntaxError: Unexpected token 'export'

The error you're encountering is because you're using ES module syntax (`import`/`export`), but Node.js expects CommonJS syntax (`require`/`module.exports`) by default unless you specify `"type": "module"` in your `package.json` or use the `.mjs` extension.

To resolve this, you have two options:

#### Option 1: Use CommonJS Syntax

Convert your import/export statements to CommonJS `require`/`module.exports` syntax.

**`database/index.js`**
```javascript
const { Pool } = require('pg');
const dotenv = require('dotenv');

dotenv.config();

const pool = new Pool({
    connectionString: process.env.DATABASE_URL
});

module.exports = pool;
```

**`database/init.js`**
```javascript
const pool = require('./index');

const createTableText = `
CREATE TABLE IF NOT EXISTS todos (
    id SERIAL PRIMARY KEY,
    text TEXT NOT NULL
)`;

pool.query(createTableText)
    .then(res => {
        console.log('Table is successfully created');
    })
    .catch(err => {
        console.error('Error creating table', err);
    })
    .finally(() => {
        pool.end();
    });
```

#### Option 2: Use ES Modules by Specifying `"type": "module"`

1. **Specify `"type": "module"` in `package.json`**
    ```json
    {
      "name": "myblog",
      "version": "1.0.0",
      "scripts": {
        "dev": "next dev",
        "build": "next build",
        "start": "next start"
      },
      "dependencies": {
        "next": "latest",
        "react": "latest",
        "react-dom": "latest",
        "pg": "^8.7.1",
        "dotenv": "^10.0.0"
      },
      "type": "module"
    }
    ```

2. **Rename files to `.mjs` extension**

Rename `index.js` to `index.mjs` and `init.js` to `init.mjs`.

**`database/index.mjs`**
```javascript
import { Pool } from 'pg';
import dotenv from 'dotenv';

dotenv.config();

const pool = new Pool({
    connectionString: process.env.DATABASE_URL
});

export default pool;
```

**`database/init.mjs`**
```javascript
import pool from './index.mjs';

const createTableText = `
CREATE TABLE IF NOT EXISTS todos (
    id SERIAL PRIMARY KEY,
    text TEXT NOT NULL
)`;

pool.query(createTableText)
    .then(res => {
        console.log('Table is successfully created');
    })
    .catch(err => {
        console.error('Error creating table', err);
    })
    .finally(() => {
        pool.end();
    });
```

#### Run the Initialization Script

Run the script again using `node`:

```bash
node database/init.mjs
```

By following one of these options, you will be able to resolve the issue and successfully create your PostgreSQL table.



## View tables in pgAdmin

<img src="assets/image-20240714141548150.png" alt="image-20240714141548150" style="zoom:80%;" />

