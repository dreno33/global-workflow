# SOLUTION: Spotless App Architecture Analysis
[FILE LENGTH: 45/600 lines]

## Problem
Need to understand and recreate a complex production cleaning management system with web, Android, and iOS versions.

## Solution
Analyzed the existing system architecture and created a simplified sandbox approach for local development.

## Analysis Findings

### System Architecture
```
Production System:
├── Backend: Node.js + Express + PostgreSQL
├── Frontend: React.js (CRA) 
├── Mobile: React Native + Expo
├── Server: Ubuntu VPS (178.128.9.12)
├── Web Server: Caddy (auto HTTPS)
└── Database: PostgreSQL (8 core tables)
```

### Key Technologies Identified
- **Backend**: Express, JWT auth, bcrypt, rate limiting, CORS
- **Frontend**: React 19, React Router, Axios, Mapbox GL
- **Mobile**: Expo, background GPS, push notifications
- **Database**: PostgreSQL with geocoded addresses
- **Security**: Helmet.js, rate limiters, parameterized queries

### Core Features
1. **Authentication**: JWT with 24h expiration
2. **User Management**: Admin/employee roles
3. **House Management**: Properties with GPS coordinates
4. **Route Scheduling**: A-J routes with optimization
5. **Time Tracking**: GPS-validated clock in/out
6. **GPS Tracking**: Real-time while clocked in
7. **Messaging**: With translation support

## Problems Identified

### 1. File Naming Inconsistencies
```javascript
// Local vs Production mismatches
// Build files differ between platforms
// No standardized naming convention
```

### 2. Database Complexity
```sql
-- Production has complex PostgreSQL schema
-- Local development needs full PostgreSQL setup
-- Migration scripts scattered
```

### 3. Deployment Complexity
```bash
# Multiple platforms to update
# Complex server configuration
# External service dependencies
```

## Simplified Sandbox Approach

### Architecture Decisions
1. **SQLite instead of PostgreSQL** - Single file, no setup
2. **Vite instead of CRA** - Faster development
3. **Local-only** - No external dependencies
4. **Core features only** - Remove GPS/push/emails

### Sandbox Structure
```
sandbox-spotless-apps/
├── backend/          # Node.js + SQLite
├── frontend/         # React + Vite
├── mobile/           # React Native + Expo
└── shared/           # Common types/utils
```

## Implementation Strategy

### Phase 1: Backend
```javascript
// 1. Setup Express with SQLite
// 2. Implement JWT auth
// 3. Create core API routes
// 4. Simplified database schema
```

### Phase 2: Frontend
```javascript
// 1. Vite + React setup
// 2. Authentication flow
// 3. Dashboard components
// 4. API integration
```

### Phase 3: Mobile
```javascript
// 1. Expo setup
// 2. Shared API client
// 3. Simplified navigation
// 4. Core features only
```

## Code Examples

### Database Schema (SQLite)
```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT UNIQUE NOT NULL,
    password_hash TEXT NOT NULL,
    role TEXT NOT NULL, -- 'admin' or 'employee'
    name TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE houses (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    address TEXT NOT NULL,
    latitude REAL,
    longitude REAL,
    frequency TEXT,
    price REAL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### API Route Example
```javascript
// Simplified auth route
app.post('/api/auth/login', async (req, res) => {
  const { username, password } = req.body;
  
  const user = await db.get(
    'SELECT * FROM users WHERE username = ?',
    [username]
  );
  
  if (!user || !await bcrypt.compare(password, user.password_hash)) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }
  
  const token = jwt.sign(
    { id: user.id, role: user.role },
    process.env.JWT_SECRET,
    { expiresIn: '24h' }
  );
  
  res.json({ token, user });
});
```

## Benefits of This Approach
1. **Fast setup** - No PostgreSQL installation
2. **Portable** - Everything runs locally
3. **Simple** - Focus on core business logic
4. **Consistent** - Same codebase across platforms
5. **Learning** - Understand full stack architecture

## Next Steps
1. Create backend with SQLite
2. Implement authentication
3. Build frontend with Vite
4. Setup mobile app
5. Document all learnings

## Tags
#architecture #nodejs #react #sqlite #fullstack #analysis