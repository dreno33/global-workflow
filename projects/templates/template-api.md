# API PROJECT TEMPLATE

## PROJECT SETUP
```bash
# Create new project directory
mkdir project-name
cd project-name
npm init -y
```

## INSTALL DEPENDENCIES
```bash
# Core dependencies
npm install express cors helmet morgan dotenv
npm install mongoose  # for MongoDB
# or
npm install pg        # for PostgreSQL

# Development dependencies
npm install -D nodemon jest supertest
```

## FOLDER STRUCTURE
```
project-name/
├── src/
│   ├── controllers/   # Route handlers
│   ├── models/        # Database models
│   ├── routes/        # API routes
│   ├── middleware/    # Custom middleware
│   ├── services/      # Business logic
│   ├── utils/         # Helper functions
│   ├── config/        # Configuration files
│   └── app.js         # Express app setup
├── tests/             # Test files
├── .env               # Environment variables
├── .gitignore
├── package.json
└── server.js          # Server entry point
```

## BASIC SERVER.JS
```javascript
const app = require('./src/app');
const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

## BASIC APP.JS
```javascript
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
const morgan = require('morgan');

const app = express();

// Middleware
app.use(helmet());
app.use(cors());
app.use(morgan('combined'));
app.use(express.json());

// Routes
app.use('/api/v1/users', require('./routes/users'));
app.use('/api/v1/posts', require('./routes/posts'));

// Error handling
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Something went wrong!' });
});

module.exports = app;
```

## SAMPLE ROUTE (routes/users.js)
```javascript
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');

// GET /api/v1/users
router.get('/', userController.getAllUsers);

// GET /api/v1/users/:id
router.get('/:id', userController.getUserById);

// POST /api/v1/users
router.post('/', userController.createUser);

// PUT /api/v1/users/:id
router.put('/:id', userController.updateUser);

// DELETE /api/v1/users/:id
router.delete('/:id', userController.deleteUser);

module.exports = router;
```

## SAMPLE CONTROLLER (controllers/userController.js)
```javascript
const userService = require('../services/userService');

const getAllUsers = async (req, res) => {
  try {
    const users = await userService.getAllUsers();
    res.json(users);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};

const getUserById = async (req, res) => {
  try {
    const user = await userService.getUserById(req.params.id);
    if (!user) {
      return res.status(404).json({ error: 'User not found' });
    }
    res.json(user);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};

// Create other controller methods...

module.exports = {
  getAllUsers,
  getUserById,
  // Export other methods...
};
```

## PACKAGE.JSON SCRIPTS
```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js",
    "test": "jest",
    "test:watch": "jest --watch"
  }
}
```

## .ENV TEMPLATE
```
PORT=3000
NODE_ENV=development
MONGODB_URI=mongodb://localhost:27017/project-name
# or
DATABASE_URL=postgresql://user:password@localhost:5432/project-name
JWT_SECRET=your-secret-key
```

## GIT INITIALIZATION
```bash
git init
git add .
git commit -m "feat: initial Express API setup"
```

## NEXT STEPS
1. Set up database connection
2. Create models/schemas
3. Implement authentication
4. Add input validation
5. Set up testing framework