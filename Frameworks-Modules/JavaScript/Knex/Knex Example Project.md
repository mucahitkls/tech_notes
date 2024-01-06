Let's design a real-time conventional example project for an online bookstore. This project will include basic functionalities such as user management, book inventory, and order processing. We'll focus on the backend structure using Node.js and Knex for database interactions.

### Project Structure:

- **/migrations** - Contains database schema migrations.
- **/seeds** - Contains seed files to populate the database with initial data.
- **/models** - The data layer where Knex is used to interact with the database.
  - `user.js` - Interacts with the 'users' table.
  - `book.js` - Interacts with the 'books' table.
  - `order.js` - Interacts with the 'orders' table.
- **/routes** - API routes for handling HTTP requests.
  - `userRoutes.js` - Routes for user-related operations.
  - `bookRoutes.js` - Routes for book-related operations.
  - `orderRoutes.js` - Routes for order-related operations.
- **/controllers** - Logic for handling requests and interacting with models.
  - `userController.js` - Handles user-related logic.
  - `bookController.js` - Handles book-related logic.
  - `orderController.js` - Handles order-related logic.
- `knexfile.js` - Configuration for Knex.
- `app.js` - The main application file that brings everything together.

### Knex in Action:

#### Knex Configuration (`knexfile.js`):

```javascript
module.exports = {
  development: {
    client: 'postgresql',
    connection: {
      database: 'online_bookstore',
      user:     'username',
      password: 'password'
    },
    migrations: {
      tableName: 'knex_migrations'
    },
    seeds: {
      directory: './seeds'
    }
  }
};
```

#### Migration Example (`/migrations/20210106123456_create_users_table.js`):

```javascript
exports.up = function(knex) {
  return knex.schema.createTable('users', table => {
    table.increments('id').primary();
    table.string('username').notNullable();
    table.string('email').notNullable().unique();
    table.string('password_hash').notNullable();
    table.timestamps(true, true);
  });
};

exports.down = function(knex) {
  return knex.schema.dropTable('users');
};
```

#### Model Example (`/models/user.js`):

```javascript
const knex = require('../knexfile');

class User {
  static async create(userData) {
    const [userId] = await knex('users').insert(userData).returning('id');
    return userId;
  }
  
  static async findByEmail(email) {
    const user = await knex('users').where({ email }).first();
    return user;
  }
  
  // Additional methods...
}
```

#### Controller Example (`/controllers/userController.js`):

```javascript
const User = require('../models/user');

exports.register = async (req, res) => {
  try {
    const userId = await User.create(req.body);
    res.status(201).json({ userId });
  } catch (error) {
    res.status(500).json({ error: 'Error registering new user' });
  }
};

// Additional handlers...
```

#### Route Example (`/routes/userRoutes.js`):

```javascript
const express = require('express');
const userController = require('../controllers/userController');
const router = express.Router();

router.post('/register', userController.register);

// Additional routes...

module.exports = router;
```

#### Main Application (`app.js`):

```javascript
const express = require('express');
const userRoutes = require('./routes/userRoutes');
const bookRoutes = require('./routes/bookRoutes');
const orderRoutes = require('./routes/orderRoutes');

const app = express();
app.use(express.json());

app.use('/users', userRoutes);
app.use('/books', bookRoutes);
app.use('/orders', orderRoutes);

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server is running on port ${PORT}`));
```

### Conclusion:

In this setup:
- **Migrations** create the necessary tables for users, books, and orders.
- **Models** use Knex to interact with the database, performing CRUD operations.
- **Controllers** handle the business logic, interacting with models to fetch and manipulate data.
- **Routes** define the endpoints of the API, connecting HTTP requests to the appropriate controllers.

This structure provides a clear separation of concerns, making the application modular, maintainable, and scalable. The use of Knex in models abstracts the database interactions, allowing developers to write cleaner, more readable code while still harnessing the power of SQL.