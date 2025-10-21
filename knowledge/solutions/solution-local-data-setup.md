# SOLUTION: Local Data Setup Guide
[FILE LENGTH: 45/600 lines]

## Overview
Guide for setting up local development using the existing real data files from the production server, ensuring the local environment matches production exactly.

## 1. Locate Existing Data Files

### Original Data Source
The original project files contain real data exported from the production server. These files are located at:

```
C:\Users\dusty\spotless-app\
├── data/
│   ├── users.sql           # User data export
│   ├── houses.sql          # House data export
│   ├── schedules.sql       # Schedule data export
│   └── backups/           # Historical backups
├── database/
│   ├── schema.sql         # Production database schema
│   └── migrations/        # Migration scripts
└── exports/
    ├── users.json         # User data in JSON format
    ├── houses.json        # House data in JSON format
    └── schedules.json     # Schedule data in JSON format
```

### Data File Locations
- **Primary data files**: `C:\Users\dusty\spotless-app\data\`
- **Schema definitions**: `C:\Users\dusty\spotless-app\database\`
- **JSON exports**: `C:\Users\dusty\spotless-app\exports\`

## 2. Verify Data Structure

### Check Data Integrity
```bash
# Navigate to the original data directory
cd "C:\Users\dusty\spotless-app\data"

# List available data files
ls -la

# Check file sizes to verify data presence
du -sh *.sql *.json
```

### Examine Data Structure
```sql
-- Check the structure of users.sql
head -20 users.sql

-- Check the structure of houses.sql  
head -20 houses.sql

-- Verify data format
tail -10 users.sql
```

### JSON Data Format Check
```bash
# Check JSON data structure
jq '.[0]' exports/users.json
jq '.[0]' exports/houses.json
jq '.[0]' exports/schedules.json
```

## 3. Set Up Local Database

### Create Local Database Directory
```bash
# Navigate to sandbox project
cd "D:\Kilo Code\projects\sandbox-spotless-apps"

# Create data directory
mkdir -p backend/data

# Set proper permissions
chmod 755 backend/data
```

### Initialize SQLite Database
```javascript
// backend/scripts/init-database.js
const sqlite3 = require('sqlite3').verbose();
const fs = require('fs');
const path = require('path');

// Create database connection
const dbPath = path.join(__dirname, '../data/spotless.db');
const db = new sqlite3.Database(dbPath);

// Read and execute schema
const schemaPath = path.join(__dirname, '../../C:/Users/dusty/spotless-app/database/schema.sql');
const schema = fs.readFileSync(schemaPath, 'utf8');

db.exec(schema, (err) => {
  if (err) {
    console.error('Error creating database schema:', err);
    return;
  }
  console.log('Database schema created successfully');
  
  // Import data
  importData();
});

function importData() {
  // Import users data
  const usersPath = path.join(__dirname, '../../C:/Users/dusty/spotless-app/exports/users.json');
  const usersData = JSON.parse(fs.readFileSync(usersPath, 'utf8'));
  
  usersData.forEach(user => {
    db.run(
      `INSERT INTO users (username, password_hash, role, name, email, phone, hourly_rate, language_preference)
       VALUES (?, ?, ?, ?, ?, ?, ?, ?)`,
      [user.username, user.password_hash, user.role, user.name, user.email, user.phone, user.hourly_rate, user.language_preference],
      (err) => {
        if (err) console.error('Error inserting user:', err);
      }
    );
  });
  
  console.log('Users data imported');
  
  // Import houses data
  const housesPath = path.join(__dirname, '../../C:/Users/dusty/spotless-app/exports/houses.json');
  const housesData = JSON.parse(fs.readFileSync(housesPath, 'utf8'));
  
  housesData.forEach(house => {
    db.run(
      `INSERT INTO houses (name, address, latitude, longitude, payment_amount, payment_type, cleaning_interval, 
       base_cleaning_time, base_employee_count, preferred_start_time, time_flexibility, estimated_duration,
       preferred_day_of_week, preferred_day_of_week_2, employees_required, preferred_employees, notes, daily_schedule)
       VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)`,
      [house.name, house.address, house.latitude, house.longitude, house.payment_amount, house.payment_type,
       house.cleaning_interval, house.base_cleaning_time, house.base_employee_count, house.preferred_start_time,
       house.time_flexibility, house.estimated_duration, house.preferred_day_of_week, house.preferred_day_of_week_2,
       house.employees_required, house.preferred_employees, house.notes, house.daily_schedule],
      (err) => {
        if (err) console.error('Error inserting house:', err);
      }
    );
  });
  
  console.log('Houses data imported');
  db.close();
}
```

### Database Configuration
```javascript
// backend/src/config/database.js
const sqlite3 = require('sqlite3').verbose();
const path = require('path');

const dbPath = path.join(__dirname, '../../../data/spotless.db');

// Create database connection
const db = new sqlite3.Database(dbPath, (err) => {
  if (err) {
    console.error('Error connecting to database:', err);
  } else {
    console.log('Connected to SQLite database');
  }
});

// Enable foreign keys
db.run('PRAGMA foreign_keys = ON');

module.exports = db;
```

## 4. Configure App to Read from Local Data

### Backend Configuration
```javascript
// backend/src/config/app.js
require('dotenv').config();

const config = {
  // Database
  database: {
    type: 'sqlite',
    path: process.env.DATABASE_PATH || './data/spotless.db',
    options: {
      pool: {
        max: 5,
        min: 0,
        acquire: 30000,
        idle: 10000
      }
    }
  },
  
  // Server
  server: {
    port: process.env.PORT || 3001,
    host: process.env.HOST || 'localhost'
  },
  
  // JWT
  jwt: {
    secret: process.env.JWT_SECRET || 'your-secret-key',
    expiresIn: process.env.JWT_EXPIRES_IN || '24h'
  },
  
  // CORS
  cors: {
    origin: process.env.CORS_ORIGIN || ['http://localhost:3000', 'exp://localhost:19000'],
    credentials: true
  }
};

module.exports = config;
```

### Frontend Configuration
```javascript
// frontend/src/config/api.js
export const API_CONFIG = {
  // Local development
  development: {
    baseURL: 'http://localhost:3001/api',
    timeout: 10000,
    headers: {
      'Content-Type': 'application/json'
    }
  },
  
  // Production
  production: {
    baseURL: process.env.REACT_APP_API_URL || '/api',
    timeout: 10000,
    headers: {
      'Content-Type': 'application/json'
    }
  }
};

export const getApiConfig = () => {
  const env = process.env.NODE_ENV || 'development';
  return API_CONFIG[env];
};
```

### Mobile Configuration
```javascript
// mobile/src/config/api.js
export const API_CONFIG = {
  baseURL: __DEV__ 
    ? 'http://localhost:3001/api'
    : 'https://api.spotless-app.com/api',
  
  timeout: 10000,
  
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json'
  }
};
```

## 5. Data Structure Validation

### Schema Comparison Script
```javascript
// scripts/validate-schema.js
const sqlite3 = require('sqlite3').verbose();
const fs = require('fs');

// Connect to local database
const db = new sqlite3.Database('./data/spotless.db');

// Check table structure
const checkTableStructure = (tableName) => {
  return new Promise((resolve, reject) => {
    db.all(`PRAGMA table_info(${tableName})`, (err, rows) => {
      if (err) reject(err);
      else resolve(rows);
    });
  });
};

// Validate data integrity
const validateData = async () => {
  try {
    // Check users table
    const usersStructure = await checkTableStructure('users');
    console.log('Users table structure:', usersStructure);
    
    // Check houses table
    const housesStructure = await checkTableStructure('houses');
    console.log('Houses table structure:', housesStructure);
    
    // Count records
    const userCount = await new Promise((resolve, reject) => {
      db.get('SELECT COUNT(*) as count FROM users', (err, row) => {
        if (err) reject(err);
        else resolve(row.count);
      });
    });
    
    const houseCount = await new Promise((resolve, reject) => {
      db.get('SELECT COUNT(*) as count FROM houses', (err, row) => {
        if (err) reject(err);
        else resolve(row.count);
      });
    });
    
    console.log(`Data validation: ${userCount} users, ${houseCount} houses`);
    
    db.close();
  } catch (error) {
    console.error('Validation error:', error);
    db.close();
    process.exit(1);
  }
};

validateData();
```

### Data Validation Commands
```bash
# Run schema validation
node scripts/validate-schema.js

# Check database integrity
sqlite3 backend/data/spotless.db "PRAGMA integrity_check;"

# Verify data counts
sqlite3 backend/data/spotless.db "SELECT COUNT(*) FROM users;"
sqlite3 backend/data/spotless.db "SELECT COUNT(*) FROM houses;"
```

## 6. Environment Setup Script

### Automated Setup Script
```bash
#!/bin/bash

# setup-local-env.sh
echo "Setting up local development environment..."

# Create necessary directories
mkdir -p backend/data
mkdir -p backend/logs
mkdir -p frontend/build
mkdir -p mobile/dist

# Copy environment files
if [ ! -f backend/.env ]; then
    cp backend/.env.example backend/.env
    echo "Created backend/.env from template"
fi

if [ ! -f frontend/.env ]; then
    cp frontend/.env.example frontend/.env
    echo "Created frontend/.env from template"
fi

# Initialize database
echo "Initializing database..."
cd backend
npm run db:init

# Install dependencies
echo "Installing dependencies..."
cd ..
npm install
cd backend && npm install
cd ../frontend && npm install
cd ../mobile && npm install

echo "Setup complete! You can now run:"
echo "npm run dev    # Start all services"
echo "npm run test   # Run tests"
```

### Database Initialization Script
```javascript
// backend/scripts/db-init.js
const sqlite3 = require('sqlite3').verbose();
const fs = require('fs');
const path = require('path');

console.log('Initializing local database...');

// Create database directory
const dataDir = path.join(__dirname, '../data');
if (!fs.existsSync(dataDir)) {
  fs.mkdirSync(dataDir, { recursive: true });
}

// Initialize database
const dbPath = path.join(dataDir, 'spotless.db');
const db = new sqlite3.Database(dbPath);

// Read schema
const schemaPath = path.join(__dirname, '../../C:/Users/dusty/spotless-app/database/schema.sql');
if (!fs.existsSync(schemaPath)) {
  console.error('Schema file not found:', schemaPath);
  process.exit(1);
}

const schema = fs.readFileSync(schemaPath, 'utf8');

db.exec(schema, (err) => {
  if (err) {
    console.error('Error creating database:', err);
    process.exit(1);
  }
  
  console.log('Database schema created successfully');
  
  // Import data
  importData();
});

function importData() {
  console.log('Importing data from production exports...');
  
  try {
    // Import users
    const usersPath = path.join(__dirname, '../../C:/Users/dusty/spotless-app/exports/users.json');
    if (fs.existsSync(usersPath)) {
      const usersData = JSON.parse(fs.readFileSync(usersPath, 'utf8'));
      console.log(`Importing ${usersData.length} users...`);
      
      usersData.forEach((user, index) => {
        db.run(
          `INSERT INTO users (username, password_hash, role, name, email, phone, hourly_rate, language_preference)
           VALUES (?, ?, ?, ?, ?, ?, ?, ?)`,
          [user.username, user.password_hash, user.role, user.name, user.email, user.phone, user.hourly_rate, user.language_preference],
          (err) => {
            if (err && !err.message.includes('UNIQUE constraint failed')) {
              console.error(`Error inserting user ${index}:`, err);
            }
          }
        );
      });
    }
    
    // Import houses
    const housesPath = path.join(__dirname, '../../C:/Users/dusty/spotless-app/exports/houses.json');
    if (fs.existsSync(housesPath)) {
      const housesData = JSON.parse(fs.readFileSync(housesPath, 'utf8'));
      console.log(`Importing ${housesData.length} houses...`);
      
      housesData.forEach((house, index) => {
        db.run(
          `INSERT INTO houses (name, address, latitude, longitude, payment_amount, payment_type, cleaning_interval, 
           base_cleaning_time, base_employee_count, preferred_start_time, time_flexibility, estimated_duration,
           preferred_day_of_week, preferred_day_of_week_2, employees_required, preferred_employees, notes, daily_schedule)
           VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)`,
          [house.name, house.address, house.latitude, house.longitude, house.payment_amount, house.payment_type,
           house.cleaning_interval, house.base_cleaning_time, house.base_employee_count, house.preferred_start_time,
           house.time_flexibility, house.estimated_duration, house.preferred_day_of_week, house.preferred_day_of_week_2,
           house.employees_required, house.preferred_employees, house.notes, house.daily_schedule],
          (err) => {
            if (err && !err.message.includes('UNIQUE constraint failed')) {
              console.error(`Error inserting house ${index}:`, err);
            }
          }
        );
      });
    }
    
    console.log('Data import completed');
    db.close();
    console.log('Local database setup complete!');
    
  } catch (error) {
    console.error('Error importing data:', error);
    db.close();
    process.exit(1);
  }
}
```

## 7. Verification Steps

### Post-Setup Verification
```bash
# 1. Check database files
ls -la backend/data/

# 2. Verify database connectivity
sqlite3 backend/data/spotless.db "SELECT name FROM sqlite_master WHERE type='table';"

# 3. Check record counts
sqlite3 backend/data/spotless.db "SELECT COUNT(*) FROM users;"
sqlite3 backend/data/spotless.db "SELECT COUNT(*) FROM houses;"

# 4. Test API connectivity
curl http://localhost:3001/api/test

# 5. Check frontend build
cd frontend && npm run build
```

### Data Consistency Check
```javascript
// scripts/check-data-consistency.js
const sqlite3 = require('sqlite3').verbose();

const db = new sqlite3.Database('./data/spotless.db');

const checkConsistency = () => {
  return new Promise((resolve, reject) => {
    db.all(`
      SELECT 
        u.id as user_id,
        u.username,
        u.role,
        COUNT(h.id) as house_count,
        SUM(h.payment_amount) as total_revenue
      FROM users u
      LEFT JOIN houses h ON u.id = h.preferred_employees
      GROUP BY u.id, u.username, u.role
    `, (err, rows) => {
      if (err) reject(err);
      else resolve(rows);
    });
  });
};

checkConsistency().then(results => {
  console.log('Data consistency check:');
  results.forEach(row => {
    console.log(`${row.username} (${row.role}): ${row.house_count} houses, $${row.total_revenue}`);
  });
  db.close();
}).catch(error => {
  console.error('Consistency check failed:', error);
  db.close();
});
```

## Benefits
1. **Real Data**: Development with actual production data
2. **Consistency**: Local environment matches production exactly
3. **Realistic Testing**: Test with real user and house data
4. **Efficient Setup**: Automated data import process
5. **Data Integrity**: Validation ensures data consistency

## Tags
#local-setup #database #data-import #configuration #development-environment