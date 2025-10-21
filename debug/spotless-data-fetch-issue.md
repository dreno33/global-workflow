# ERROR: Spotless App - Cannot Fetch Data from Server

**Date:** October 20, 2025
**Project:** Spotless Improved
**Status:** 🔍 ROOT CAUSE IDENTIFIED

---

## SYMPTOMS
- Cannot fetch any data when running app locally
- Frontend cannot connect to backend API
- All API calls returning errors or failing

## ERROR MESSAGE
```
{"error":"Failed to fetch employees"}
```

## ENVIRONMENT
- **OS:** Windows 11
- **Node.js:** v22.20.0
- **PostgreSQL:** Installed locally
- **Backend Port:** 3001
- **Database:** spotless_db
- **Working Directory:** `D:\Kilo Code\projects\spotless-improved\backend`

---

## ROOT CAUSE IDENTIFIED ✅

### Problem 1: Backend was running from WRONG directory
**Issue:** An old backend process was running from `C:\Users\dusty\spotless-app\backend`
**Current Project:** Located at `D:\Kilo Code\projects\spotless-improved\backend`

**Solution:** ✅ FIXED
- Killed old process (PID 11172)
- Started backend from correct directory
- Backend now running properly

### Problem 2: Database Password Authentication Failed ❌ NEEDS FIX
**Issue:** PostgreSQL rejecting password in `.env` file

**Backend .env Configuration:**
```
DB_USER=postgres
DB_PASSWORD=Westley33Hotel
DB_HOST=localhost
DB_PORT=5432
DB_NAME=spotless_db
```

**Error:**
```
❌ Database connection test failed: password authentication failed for user "postgres"
FATAL code: '28P01'
```

---

## CAUSES (Most likely first)

### 1. PostgreSQL Password Incorrect
The password `Westley33Hotel` in the `.env` file doesn't match the actual PostgreSQL password.

**Check if:**
- PostgreSQL password was changed
- Different password set during PostgreSQL installation
- Password is for a different user

### 2. Database Doesn't Exist
The database `spotless_db` might not exist in your local PostgreSQL.

**Check if:**
- Database was never created locally
- Database name is different (maybe `spotless` without `_db`)

### 3. PostgreSQL User Doesn't Exist
The `postgres` user might not have been set up properly.

---

## SOLUTIONS

### Solution 1: Find Correct PostgreSQL Password
**Try these steps:**

1. **Reset PostgreSQL password:**
```bash
# Open Windows Command Prompt as Administrator
psql -U postgres
# If it asks for password, try common ones:
# - Your Windows password
# - postgres
# - Westley33Hotel
# - (blank/empty)
```

2. **If you can't remember password, reset it:**
```bash
# Edit pg_hba.conf to allow trust authentication temporarily
# Location usually: C:\Program Files\PostgreSQL\[version]\data\pg_hba.conf
# Change this line:
# host    all             all             127.0.0.1/32            md5
# To:
# host    all             all             127.0.0.1/32            trust

# Restart PostgreSQL service
# Then connect without password and reset:
psql -U postgres
ALTER USER postgres WITH PASSWORD 'Westley33Hotel';

# Change pg_hba.conf back to md5
# Restart PostgreSQL again
```

### Solution 2: Check if Database Exists
**Create the database if it doesn't exist:**

```bash
# Connect to PostgreSQL
psql -U postgres

# List databases
\l

# If spotless_db doesn't exist, create it:
CREATE DATABASE spotless_db;

# Grant permissions
GRANT ALL PRIVILEGES ON DATABASE spotless_db TO postgres;
```

### Solution 3: Get Data from Production Server
**If local database is empty or doesn't exist, pull from production:**

```bash
# SSH into production server
ssh root@178.128.9.12

# Dump production database
pg_dump -U dtreno33 -h localhost spotless_db > /tmp/spotless_backup.sql

# Exit SSH
exit

# Download dump to local
scp root@178.128.9.12:/tmp/spotless_backup.sql d:/temp/

# Import to local database
# First create database if needed:
psql -U postgres -c "CREATE DATABASE spotless_db;"

# Then import data:
psql -U postgres -d spotless_db -f d:/temp/spotless_backup.sql
```

### Solution 4: Use .env.local for Remote Database
**Point backend to production database temporarily:**

Create `backend/.env.local`:
```
# Remote Production Database
DB_USER=dtreno33
DB_PASSWORD=Westley33Hotel
DB_HOST=178.128.9.12
DB_PORT=5432
DB_NAME=spotless_db
```

**NOTE:** This requires opening port 5432 on the production server firewall.

---

## CURRENT STATUS

### ✅ Fixed
- [x] Backend running from correct directory
- [x] Backend server starting successfully
- [x] All services (email, photo cleanup) loading correctly

### ❌ Still Needs Fix
- [ ] Database connection authentication
- [ ] Determine correct PostgreSQL password
- [ ] Verify database exists locally
- [ ] Test API endpoints return data

---

## NEXT STEPS

**Immediate Actions Required:**

1. **Try to connect to PostgreSQL with various passwords**
   ```bash
   psql -U postgres
   # Try: Westley33Hotel, postgres, blank
   ```

2. **Check what databases exist**
   ```bash
   psql -U postgres -c "\l"
   ```

3. **If can't connect, reset PostgreSQL password** (see Solution 1 above)

4. **Once connected, check if spotless_db has data:**
   ```sql
   \c spotless_db
   SELECT COUNT(*) FROM users;
   SELECT COUNT(*) FROM houses;
   ```

5. **If no data locally, import from production** (see Solution 3 above)

---

## TESTING PLAN

Once database is fixed, test these:

1. **Database connection:**
   ```bash
   psql -U postgres -d spotless_db -c "SELECT COUNT(*) FROM users;"
   ```

2. **Backend API:**
   ```bash
   curl http://localhost:3001/api/employees
   # Should return JSON array, not error
   ```

3. **Frontend connection:**
   - Open http://localhost:3000
   - Login should work
   - Dashboard should load data

---

## PREVENTION

### Going Forward:
1. **Document PostgreSQL credentials** in a secure password manager
2. **Keep local database synced** with production via regular dumps
3. **Use `.env.local` files** that are in `.gitignore` for local overrides
4. **Add database health check script** to verify connection on startup

---

## RELATED FILES
- [Backend .env](../projects/spotless-improved/backend/.env)
- [Database connection](../projects/spotless-improved/backend/models/db.js)
- [Backend server](../projects/spotless-improved/backend/server.js)

---

## SUMMARY

**Problem:** Cannot fetch data from server
**Root Cause:** PostgreSQL password authentication failing
**Immediate Fix:** Need to verify/reset PostgreSQL password and confirm database exists
**Alternative:** Import production data to local database or connect to remote database

**Status:** Backend is running correctly, just needs database credentials fixed.
