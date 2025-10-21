# SOLUTION: Multi-Platform Synchronization Rules
[FILE LENGTH: 45/600 lines]

## Overview
Standardization rules to ensure consistent behavior across local development, VPS deployment, and mobile platforms for the Spotless cleaning management system.

## 1. Naming Conventions (All Platforms)

### File Structure
```
spotless-apps/
├── backend/                 # Node.js API server
│   ├── src/
│   │   ├── controllers/     # Business logic
│   │   ├── models/         # Database models
│   │   ├── routes/         # API endpoints
│   │   ├── services/       # Business services
│   │   ├── middleware/     # Auth, validation
│   │   └── utils/          # Helper functions
│   ├── data/               # Database files
│   ├── tests/              # Backend tests
│   └── package.json
├── frontend/               # React web app
│   ├── src/
│   │   ├── components/     # Reusable UI components
│   │   ├── pages/         # Page components
│   │   ├── hooks/         # Custom React hooks
│   │   ├── services/       # API services
│   │   ├── utils/         # Utility functions
│   │   └── types/         # TypeScript definitions
│   ├── public/            # Static assets
│   └── package.json
├── mobile/                 # React Native app
│   ├── src/
│   │   ├── screens/       # Screen components
│   │   ├── components/    # Reusable components
│   │   ├── services/      # API services
│   │   ├── contexts/      # React contexts
│   │   └── utils/         # Utility functions
│   └── package.json
└── shared/                 # Common code
    ├── types/              # Shared TypeScript types
    ├── utils/              # Shared utilities
    └── constants/          # Application constants
```

### Naming Rules
- **Files**: kebab-case (e.g., `user-auth-service.js`)
- **Components**: PascalCase (e.g., `UserLoginForm.js`)
- **Variables**: camelCase (e.g., `userName`)
- **Constants**: SCREAMING_SNAKE_CASE (e.g., `API_BASE_URL`)
- **Database Tables**: snake_case (e.g., `user_sessions`)
- **API Endpoints**: kebab-case (e.g., `/api/user/login`)

### Platform-Specific Extensions
- **Backend**: `.js` (Node.js)
- **Frontend**: `.jsx` (React components)
- **Mobile**: `.js` (React Native)
- **Shared**: `.js` or `.ts` (TypeScript)

## 2. Database Schema (Identical Everywhere)

### Core Tables Structure
```sql
-- Users table (same on all platforms)
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT UNIQUE NOT NULL,
    password_hash TEXT NOT NULL,
    role TEXT NOT NULL CHECK (role IN ('admin', 'employee')),
    name TEXT NOT NULL,
    email TEXT,
    phone TEXT,
    hourly_rate REAL,
    language_preference TEXT DEFAULT 'en',
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    deleted_at DATETIME
);

-- Houses table (same on all platforms)
CREATE TABLE houses (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    address TEXT NOT NULL,
    latitude REAL,
    longitude REAL,
    payment_amount REAL NOT NULL,
    payment_type TEXT DEFAULT 'per_visit',
    cleaning_interval TEXT NOT NULL,
    last_cleaned_date DATE,
    base_cleaning_time INTEGER,
    base_employee_count INTEGER DEFAULT 1,
    preferred_start_time TIME,
    time_flexibility TEXT DEFAULT 'anytime',
    estimated_duration INTEGER,
    preferred_day_of_week INTEGER,
    preferred_day_of_week_2 INTEGER,
    employees_required INTEGER DEFAULT 1,
    preferred_employees TEXT,
    notes TEXT,
    daily_schedule TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    deleted_at DATETIME
);

-- User sessions table (for JWT token management)
CREATE TABLE user_sessions (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER NOT NULL,
    token_hash TEXT NOT NULL,
    expires_at DATETIME NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

### Database Configuration
```javascript
// shared/database-config.js
export const DATABASE_CONFIG = {
  // Local development
  local: {
    type: 'sqlite',
    filename: './data/spotless.db',
    options: { }
  },
  
  // Production (VPS)
  production: {
    type: 'postgres',
    host: process.env.DB_HOST || 'localhost',
    port: process.env.DB_PORT || 5432,
    database: process.env.DB_NAME || 'spotless',
    username: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    ssl: process.env.NODE_ENV === 'production'
  }
};
```

## 3. Build Process (Works for All Versions)

### Package.json Structure
```json
// Root package.json
{
  "name": "spotless-apps",
  "version": "1.0.0",
  "scripts": {
    "dev": "concurrently \"npm run dev:backend\" \"npm run dev:frontend\" \"npm run dev:mobile\"",
    "dev:backend": "cd backend && npm run dev",
    "dev:frontend": "cd frontend && npm run dev",
    "dev:mobile": "cd mobile && npm start",
    "build": "npm run build:backend && npm run build:frontend && npm run build:mobile",
    "build:backend": "cd backend && npm run build",
    "build:frontend": "cd frontend && npm run build",
    "build:mobile": "cd mobile && npm run build",
    "test": "npm run test:backend && npm run test:frontend",
    "test:backend": "cd backend && npm test",
    "test:frontend": "cd frontend && npm test"
  },
  "devDependencies": {
    "concurrently": "^8.2.2"
  }
}
```

### Platform-Specific Build Scripts
```json
// backend/package.json
{
  "scripts": {
    "dev": "nodemon src/server.js",
    "build": "echo 'Backend build complete'",
    "start": "node src/server.js",
    "test": "jest"
  }
}

// frontend/package.json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "test": "vitest"
  }
}

// mobile/package.json
{
  "scripts": {
    "start": "expo start",
    "android": "expo start --android",
    "ios": "expo start --ios",
    "build:android": "eas build --platform android",
    "build:ios": "eas build --platform ios"
  }
}
```

## 4. Git Structure (Easy Updates)

### Repository Structure
```
spotless-apps/
├── .github/                 # GitHub Actions
│   └── workflows/
│       ├── ci.yml          # Continuous integration
│       └── deploy.yml      # Deployment
├── backend/                # Backend source
├── frontend/               # Frontend source
├── mobile/                 # Mobile source
├── shared/                 # Shared code
├── docs/                   # Documentation
├── scripts/                # Build and deployment scripts
├── .gitignore              # Git ignore rules
├── README.md               # Project documentation
└── package.json            # Root package management
```

### Git Workflow
```bash
# Feature branch workflow
git checkout -b feature/user-authentication
# ... make changes ...
git add .
git commit -m "feat: add user authentication"
git push origin feature/user-authentication
# Create pull request
```

### .gitignore Configuration
```
# Dependencies
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Build outputs
dist/
build/
*.apk
*.aab
*.ipa

# Environment files
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# Database files
*.db
*.sqlite
*.sqlite3

# OS files
.DS_Store
Thumbs.db

# IDE files
.vscode/
.idea/
*.swp
*.swo
```

## 5. Environment Variables (Local vs Production)

### Environment Configuration
```javascript
// shared/environment.js
export const ENV = {
  // Local development
  development: {
    API_URL: 'http://localhost:3001',
    DATABASE_URL: 'sqlite:./data/spotless.db',
    JWT_SECRET: 'dev-secret-key',
    LOG_LEVEL: 'debug'
  },
  
  // Production
  production: {
    API_URL: process.env.API_URL || 'https://api.spotless-app.com',
    DATABASE_URL: process.env.DATABASE_URL,
    JWT_SECRET: process.env.JWT_SECRET,
    LOG_LEVEL: 'info'
  }
};

export const getEnvConfig = () => {
  const env = process.env.NODE_ENV || 'development';
  return ENV[env];
};
```

### Environment Files
```bash
# .env.local (local development)
NODE_ENV=development
API_URL=http://localhost:3001
DATABASE_URL=sqlite:./data/spotless.db
JWT_SECRET=your-local-secret-key

# .env.production (production)
NODE_ENV=production
API_URL=https://api.spotless-app.com
DATABASE_URL=postgresql://user:pass@localhost:5432/spotless
JWT_SECRET=your-production-secret-key
```

### Environment Variable Validation
```javascript
// shared/validate-env.js
import { getEnvConfig } from './environment';

export const validateEnvironment = () => {
  const config = getEnvConfig();
  const required = ['API_URL', 'JWT_SECRET'];
  
  required.forEach(key => {
    if (!config[key]) {
      throw new Error(`Missing required environment variable: ${key}`);
    }
  });
  
  return config;
};
```

## Implementation Guidelines

### 1. Consistent API Design
- Use RESTful endpoints with consistent naming
- Implement proper HTTP status codes
- Include error handling and validation
- Use JWT for authentication across all platforms

### 2. Data Synchronization
- Ensure database schema is identical across all environments
- Use migration scripts for schema changes
- Implement data validation on all platforms
- Handle offline scenarios for mobile

### 3. Code Organization
- Follow the established file structure
- Use shared types and utilities where possible
- Implement proper error handling
- Write comprehensive tests

### 4. Deployment Strategy
- Use environment-specific configurations
- Implement proper logging and monitoring
- Use containerization for production
- Set up automated testing and deployment

## Benefits
1. **Consistency**: Same behavior across all platforms
2. **Maintainability**: Easy to update and maintain
3. **Scalability**: Can handle growth and changes
4. **Reliability**: Reduced bugs and issues
5. **Efficiency**: Streamlined development process

## Tags
#standardization #multi-platform #database #build-process #git #environment-variables