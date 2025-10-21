# GLOBAL DEBUGGING RULES FOR ALL PROJECTS

## **MANDATORY DEBUGGING PROTOCOLS FOR NEW PROJECTS**

### 1. Server Restart Requirements
**WHENEVER you start a new project or make changes to any server component, YOU MUST restart that server:**

#### Backend Changes (Node.js/Express)
**RESTART REQUIRED for:**
- Server entry files (server.js, app.js, index.js)
- Route files (any file in routes/ directory)
- Model files (any file in models/ directory)
- Service files (any file in services/ directory)
- Middleware files (any file in middleware/ directory)
- Environment configuration files (.env, .env.local)
- Package.json files

**Command to restart:**
```bash
# For Windows
taskkill /F /IM node.exe  # Kill existing processes
node server.js

# For Linux/Mac
pkill -f "node.*server.js"
node server.js
```

#### Frontend Changes (React/Vue/Angular)
**RESTART REQUIRED for:**
- Main application files (App.js, main.js)
- Component files (any file in components/)
- Configuration files (any file in config/)
- Context files (any file in contexts/)
- Utility files (any file in utils/)
- Package.json files

**Command to restart:**
```bash
# For npm
npm start

# For yarn
yarn start
```

#### Database Changes
**RESTART REQUIRED for:**
- Database schema changes
- Migration files
- Database configuration files
- Database service restarts

**Command to restart:**
```bash
# PostgreSQL
sudo systemctl restart postgresql

# MySQL/MariaDB
sudo systemctl restart mysql
```

### 2. Logging Requirements for Problem Tracking
**WHENEVER you debug issues, YOU MUST add appropriate logging to track and fix problems:**

#### Backend Logging (Node.js)
**Add these logging statements:**
- **File loading**: Add `console.log('🔧 File loaded:', __filename);` at top of modified files
- **API endpoints**: Add `console.log('📝 API call:', req.method, req.url);` at start of route handlers
- **Database queries**: Add `console.log('🗃️ Query:', text, params);` before pool.query() calls
- **Environment variables**: Add `console.log('🔗 ENV:', key, value);` for critical env vars
- **Error handling**: Add `console.error('❌ ERROR:', error.message, error.stack);` in catch blocks

#### Frontend Logging (React/Vue)
**Add these logging statements:**
- **Component renders**: Add `console.log('🖼️ Component render:', component_name);` in useEffect or render functions
- **API calls**: Add `console.log('🌐 API call:', endpoint, data);` before fetch() calls
- **State changes**: Add `console.log('🔄 State change:', state_name, new_value);` in setState calls
- **Error boundaries**: Add `console.error('❌ Frontend error:', error);` in error boundaries

#### Database Logging
**Add these logging statements:**
- **Connection attempts**: Add `console.log('🔌 DB Connect attempt:', host, port, database);`
- **Query execution**: Add `console.log('🗃️ Executing query:', query_text);`
- **Connection errors**: Add `console.error('❌ DB Connection error:', error);`

### 3. Log Removal Protocol
**ONCE the problem is IDENTIFIED and FIXED (only after approval by project lead):**

1. **Create a backup** of the logged version
2. **Remove all debug console.log statements** 
3. **Test functionality** without logs
4. **Commit the clean version** with a message like "Remove debug logs after issue resolution"

### 4. Problem Resolution Workflow
**WHEN encountering issues:**

1. **Add comprehensive logging** to track the problem
2. **Restart the affected server(s)** after making changes
3. **Monitor logs** for error patterns and root causes
4. **Test the fix** thoroughly
5. **Get approval** from project lead before removing logs
6. **Clean up** debug statements once confirmed working

### 5. Common Issues to Check
**When things aren't working:**

1. **Server not running**: Check if processes are active on correct ports
2. **Database connection**: Verify remote database accessibility and credentials
3. **Environment variables**: Confirm correct .env files are loaded
4. **CORS issues**: Verify backend CORS configuration matches frontend URL
5. **Port conflicts**: Ensure no other services are using the same ports
6. **Network connectivity**: Check firewalls and VPN settings

### 6. Quick Reference Commands

#### Backend Management
```bash
# Start backend
cd backend-directory
node server.js

# Restart backend (Windows)
taskkill /F /IM node.exe
node server.js

# Check backend is running
netstat -ano | findstr :3001

# Check running processes
tasklist | findstr node
```

#### Frontend Management
```bash
# Start frontend
cd frontend-directory
npm start

# Check frontend is running
netstat -ano | findstr :3000
```

#### Database Testing
```bash
# Test database connection
psql -h host -p port -U user -d database

# Test PostgreSQL service
sudo systemctl status postgresql

# Check database logs
tail -f /var/log/postgresql/postgresql-*.log
```

#### API Testing
```bash
# Test backend API
curl http://localhost:3001/api/test

# Test specific endpoints
curl http://localhost:3001/api/health
curl http://localhost:3001/api/database-status
```

---

## **REMEMBER: Always restart the appropriate server after making changes!**

## **REMEMBER: Add comprehensive logging before debugging issues!**

## **REMEMBER: Get approval before removing debug logs!**