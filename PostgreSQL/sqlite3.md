# Sqlite3

## db

### Code Explanation

```javascript
import sqlite3 from 'sqlite3';

// Create a new SQLite database connection
const db = new sqlite3.Database('./database/todos.db', (err) => {
    if (err) {
        // If there is an error opening the database, log the error message
        console.error('Error opening database', err);
    } else {
        // If the database is opened successfully, run the following SQL query
        db.run(`
            CREATE TABLE IF NOT EXISTS todos (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                text TEXT NOT NULL
            )
        `, (err) => {
            // If there is an error creating the table, log the error message
            if (err) {
                console.error('Error creating table', err);
            }
        });
    }
});

// Export the database connection so it can be used in other parts of the application
export default db;
```

## api/todos.js

### Detailed Breakdown

1. **Import the `sqlite3` module:**
   ```javascript
   import sqlite3 from 'sqlite3';
   ```
   This line imports the `sqlite3` module, which is used to interact with SQLite databases.

2. **Create a new database connection:**
   ```javascript
   const db = new sqlite3.Database('./database/todos.db', (err) => {
   ```
   This line creates a new SQLite database connection to a file named `todos.db` located in the `database` directory. If the file does not exist, it will be created. The callback function handles any errors that occur during the opening of the database.

3. **Error handling for database opening:**
   ```javascript
   if (err) {
       console.error('Error opening database', err);
   } else {
   ```
   If there is an error while opening the database, it logs the error message. Otherwise, it proceeds to the next step.

4. **Create the `todos` table if it does not exist:**
   ```javascript
   db.run(`
       CREATE TABLE IF NOT EXISTS todos (
           id INTEGER PRIMARY KEY AUTOINCREMENT,
           text TEXT NOT NULL
       )
   `, (err) => {
   ```
   This SQL query creates a table named `todos` with two columns:
    - `id`: an integer that serves as the primary key and is automatically incremented for each new row.
    - `text`: a text field that cannot be null.

   The `CREATE TABLE IF NOT EXISTS` statement ensures that the table is only created if it does not already exist.

5. **Error handling for table creation:**
   ```javascript
   if (err) {
       console.error('Error creating table', err);
   }
   ```
   If there is an error while creating the table, it logs the error message.

6. **Export the database connection:**
   ```javascript
   export default db;
   ```
   This line exports the `db` object, which represents the database connection, so that it can be used in other parts of the application.

### Summary
The code establishes a connection to an SQLite database located at `./database/todos.db`. If the database file does not exist, it creates it. It then attempts to create a table named `todos` with columns for an auto-incrementing primary key (`id`) and a text field (`text`). If any errors occur during the opening of the database or the creation of the table, they are logged to the console. The `db` object is exported for use in other parts of the application.


Sure, let's break down this code step-by-step to understand how it works:

### Code Explanation

```javascript
import db from "../../database/db";
```

This line imports the `db` object, which is the SQLite database connection created in the `db.js` file located in the `../../database/` directory.

### Handler Function

```javascript
export default function handler(req, res) {
```

This function is the default export of the module and acts as an API route handler in Next.js. It takes two parameters: `req` (the request object) and `res` (the response object).

### Handling GET Requests

```javascript
    if (req.method === 'GET') {
        db.all('SELECT * FROM todos', (err, rows) => {
            if (err) {
                res.status(500).json({ error: err.message });
            } else {
                res.status(200).json({ todos: rows });
            }
        });
    }
```

If the request method is `GET`, it retrieves all records from the `todos` table in the SQLite database. Here's what happens step-by-step:

1. **SQL Query Execution:** The `db.all` method executes the SQL query `SELECT * FROM todos`, which selects all records from the `todos` table.
2. **Error Handling:** If there is an error during the query, it sends a response with a 500 status code and the error message.
3. **Successful Retrieval:** If the query is successful, it sends a response with a 200 status code and the retrieved rows as JSON.

### Handling POST Requests

```javascript
    else if (req.method === 'POST') {
        const { text } = req.body;
        db.run('INSERT INTO todos (text) VALUES (?)', [text], function(err) {
            if (err) {
                res.status(500).json({ error: err.message });
            } else {
                res.status(201).json({ id: this.lastID, text });
            }
        });
    }
```

If the request method is `POST`, it inserts a new record into the `todos` table. Here's the process:

1. **Extract Text:** It extracts the `text` property from the request body.
2. **SQL Insert Execution:** The `db.run` method executes the SQL query `INSERT INTO todos (text) VALUES (?)`, inserting the `text` value into the `todos` table. The question mark (`?`) is a placeholder for the value provided in the array `[text]`.
3. **Error Handling:** If there is an error during the insertion, it sends a response with a 500 status code and the error message.
4. **Successful Insertion:** If the insertion is successful, it sends a response with a 201 status code, including the new record's ID (`this.lastID`) and the text.

### Handling DELETE Requests

```javascript
    else if (req.method === 'DELETE') {
        const { id } = req.body;
        db.run('DELETE FROM todos WHERE id = ?', [id], function(err) {
            if (err) {
                res.status(500).json({ error: err.message });
            } else {
                res.status(200).json({ message: 'Todo deleted' });
            }
        });
    }
```

If the request method is `DELETE`, it deletes a record from the `todos` table. The steps are:

1. **Extract ID:** It extracts the `id` property from the request body.
2. **SQL Delete Execution:** The `db.run` method executes the SQL query `DELETE FROM todos WHERE id = ?`, deleting the record with the specified `id`. The question mark (`?`) is a placeholder for the value provided in the array `[id]`.
3. **Error Handling:** If there is an error during the deletion, it sends a response with a 500 status code and the error message.
4. **Successful Deletion:** If the deletion is successful, it sends a response with a 200 status code and a message indicating the todo was deleted.

### Handling Unsupported Methods

```javascript
    else {
        res.status(405).json({ message: 'Method not allowed' });
    }
}
```

If the request method is not `GET`, `POST`, or `DELETE`, it sends a response with a 405 status code and a message indicating that the method is not allowed.

### Summary

- **GET Requests:** Retrieve all todos from the database.
- **POST Requests:** Insert a new todo into the database.
- **DELETE Requests:** Delete a todo from the database by ID.
- **Unsupported Methods:** Respond with a 405 status code.

This handler function allows basic CRUD operations on the `todos` table in the SQLite database through a Next.js API route.

## Page.tsx

Sure, let's break down the code snippet step-by-step to understand its functionality.

### Code Explanation

#### 1. `useEffect` Hook
```javascript
useEffect(() => {
    fetch('/api/todos')
        .then((response) => response.json())
        .then((data) => setTodos(data.todos));
}, []);
```

- **Purpose**: This `useEffect` hook is used to fetch the list of todos from the server when the component is first rendered.
- **Details**:
    - The empty dependency array `[]` means this effect runs only once, after the initial render.
    - `fetch('/api/todos')` makes a GET request to the `/api/todos` endpoint to retrieve the todos.
    - `.then((response) => response.json())` converts the response to JSON.
    - `.then((data) => setTodos(data.todos))` updates the `todos` state with the data received from the server.

#### 2. `handleAddTodo` Function
```javascript
const handleAddTodo = () => {
    if (newTodo.trim()) {
        fetch('/api/todos', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ text: newTodo.trim() })
        })
            .then(response => response.json())
            .then(data => {
                setTodos([...todos, { id: data.id, text: data.text }]);
                setNewTodo("");
            });
    }
};
```

- **Purpose**: This function handles adding a new todo to the list.
- **Details**:
    - `if (newTodo.trim()) { ... }` ensures that the new todo is not an empty string.
    - `fetch('/api/todos', { ... })` makes a POST request to the `/api/todos` endpoint to add a new todo.
        - `method: 'POST'` specifies the HTTP method as POST.
        - `headers: { 'Content-Type': 'application/json' }` sets the request header to indicate that the request body contains JSON.
        - `body: JSON.stringify({ text: newTodo.trim() })` converts the new todo text to a JSON string.
    - `.then(response => response.json())` converts the response to JSON.
    - `.then(data => { ... })` updates the state:
        - `setTodos([...todos, { id: data.id, text: data.text }])` adds the new todo to the existing list of todos.
        - `setNewTodo("")` clears the input field for the new todo.

#### 3. `handleRemoveTodo` Function
```javascript
const handleRemoveTodo = (id: number) => {
    fetch('/api/todos', {
        method: 'DELETE',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({ id })
    })
        .then(() => {
            setTodos(todos.filter(todo => todo.id !== id));
        });
};
```

- **Purpose**: This function handles removing a todo from the list.
- **Details**:
    - `fetch('/api/todos', { ... })` makes a DELETE request to the `/api/todos` endpoint to delete a todo.
        - `method: 'DELETE'` specifies the HTTP method as DELETE.
        - `headers: { 'Content-Type': 'application/json' }` sets the request header to indicate that the request body contains JSON.
        - `body: JSON.stringify({ id })` converts the todo ID to a JSON string.
    - `.then(() => { ... })` updates the state:
        - `setTodos(todos.filter(todo => todo.id !== id))` filters out the deleted todo from the list of todos.

### Summary
- **`useEffect` Hook**: Fetches and sets the initial list of todos from the server when the component mounts.
- **`handleAddTodo` Function**: Adds a new todo by sending a POST request to the server and updates the local state.
- **`handleRemoveTodo` Function**: Removes a todo by sending a DELETE request to the server and updates the local state.

These functions ensure that the todo list displayed in the UI is synchronized with the data stored on the server.