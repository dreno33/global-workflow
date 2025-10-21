# SOLUTION: Spotless App - Environment Standardization

**Date Created:** October 20, 2025
**Project:** Spotless Improved
**Status:** 🚧 In Progress

#tags: #spotless #deployment #devops #standardization

---

## PROBLEM

Deployment between local development and production is difficult and error-prone due to:
- Inconsistent file structures between environments
- Unclear differences between local and production configurations
- No documented procedure for syncing databases
- Unclear what files/configurations are environment-specific
- Difficult to maintain parity between environments

---

## SOLUTION OVERVIEW

Create a standardized environment structure with:
1. **Documented credentials** (secure, not in git)
2. **Environment configuration matrix** (what's different, what's the same)
3. **Automated deployment scripts** (one-command deployment)
4. **Database sync procedures** (production → local)
5. **Cleanup procedures** (remove unnecessary files)

---

## ENVIRONMENT COMPARISON MATRIX

### File Structure - SHOULD BE IDENTICAL

Both local and production should have:
```
spotless-improved/
├── backend/
│   ├── server.js
│   ├── package.json
│   ├── .env (different content per environment)
│   ├── routes/
│   ├── middleware/
│   ├── models/
│   ├── services/
│   └── uploads/
├── frontend/
│   ├── src/
│   ├── public/
│   ├── package.json
│   └── build/ (production only)
└── spotless-mobile/
    ├── App.js
    ├── screens/
    ├── services/
    ├── config/
    └── package.json
```

### Environment Variables - DIFFERENT PER ENVIRONMENT

**Local (.env):**
```env
NODE_ENV=development
PORT=3001
DB_HOST=localhost
DB_PORT=5432
DB_NAME=spotless_db
DB_USER=postgres
DB_PASSWORD=[YOUR_LOCAL_PASSWORD]
JWT_SECRET=your_local_dev_secret_key_here
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=dtreno33@gmail.com
EMAIL_PASSWORD=lciwuwhhxywufwco
MAPBOX_ACCESS_TOKEN=pk.eyJ1IjoiZHRyZW5vMzMiLCJhIjoiY21nY2pvcmZzMDR2ODJpcTJyM2hqaTJ5cSJ9.kf26M0WFquRuhywBf5vvLA
```

**Production (.env):**
```env
NODE_ENV=production
PORT=3001
DB_HOST=localhost
DB_PORT=5432
DB_NAME=spotless_db
DB_USER=dtreno33
DB_PASSWORD=Westley33Hotel
JWT_SECRET=[SECURE_64_CHAR_SECRET]
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=dtreno33@gmail.com
EMAIL_PASSWORD=lciwuwhhxywufwco
MAPBOX_ACCESS_TOKEN=pk.eyJ1IjoiZHRyZW5vMzMiLCJhIjoiY21nY2pvcmZzMDR2ODJpcTJyM2hqaTJ5cSJ9.kf26M0WFquRuhywBf5vvLA
```

### Key Differences

| Component | Local | Production |
|-----------|-------|------------|
| **Backend Server** | localhost:3001 | localhost:3001 (behind Caddy) |
| **Frontend Dev** | localhost:3000 | N/A (uses build/) |
| **Frontend Prod** | N/A | Caddy serves from /var/www/spotless/build |
| **Database User** | postgres | dtreno33 |
| **Database Password** | [Your choice] | Westley33Hotel |
| **API Base URL** | http://localhost:3001 | https://spotlessadmin.com |
| **CORS Origins** | Allow all in dev | Strict in production |
| **Process Manager** | Manual npm start | PM2 or nohup |
| **Web Server** | React dev server | Caddy (ports 80/443) |
| **SSL/HTTPS** | No | Yes (automatic via Caddy) |
| **Logs** | Console | output.log file |

---

## STANDARDIZATION PROCEDURES

### 1. Database Standardization

**Goal:** Local database should be a copy of production.

**Production → Local Sync:**
```bash
# 1. Backup production database
ssh root@178.128.9.12 "pg_dump -U dtreno33 spotless_db > /tmp/spotless_backup.sql"

# 2. Download backup
scp root@178.128.9.12:/tmp/spotless_backup.sql "d:/temp/spotless_backup.sql"

# 3. Drop and recreate local database
psql -U postgres -c "DROP DATABASE IF EXISTS spotless_db;"
psql -U postgres -c "CREATE DATABASE spotless_db;"

# 4. Import production data
psql -U postgres -d spotless_db -f "d:/temp/spotless_backup.sql"

# 5. Verify data
psql -U postgres -d spotless_db -c "SELECT COUNT(*) FROM users;"
psql -U postgres -d spotless_db -c "SELECT COUNT(*) FROM houses;"
```

**Automated Script:** See `scripts/sync-database-from-production.bat`

### 2. Code Standardization

**Goal:** Code should be identical except for environment-specific configs.

**Files that MUST be identical:**
- All JavaScript/TypeScript source files
- All routes, controllers, models, services
- Package.json dependencies
- Database schema and migrations

**Files that SHOULD differ:**
- `.env` (different credentials)
- `node_modules/` (locally installed)
- `build/` (frontend production build)
- `uploads/` (user-uploaded content)
- `output.log` (production logs)

**Verification:**
```bash
# Compare package.json versions
diff backend/package.json backend-production/package.json

# Compare key source files
diff backend/server.js backend-production/server.js
```

### 3. Deployment Standardization

**Goal:** One-command deployment to production.

**Before Deployment:**
1. Test locally first
2. Commit changes to git
3. Update version numbers if needed
4. Document changes in changelog

**Deployment Process:**
```bash
# Backend deployment
cd backend
npm install
npm test (if tests exist)
tar --exclude='node_modules' --exclude='.git' -czf /tmp/backend.tar.gz .
scp /tmp/backend.tar.gz root@178.128.9.12:/tmp/
ssh root@178.128.9.12 "cd /root/spotless-backend && tar -xzf /tmp/backend.tar.gz && npm install && pm2 restart spotless-backend"

# Frontend deployment
cd frontend
npm install
npm run build
tar -czf /tmp/frontend.tar.gz build/
scp /tmp/frontend.tar.gz root@178.128.9.12:/tmp/
ssh root@178.128.9.12 "cd /var/www/spotless && rm -rf build/* && tar -xzf /tmp/frontend.tar.gz && systemctl reload caddy"

# Mobile deployment
cd spotless-mobile
npm install
eas build --platform android
eas build --platform ios
```

**Automated Script:** See `deployment-scripts/deploy-all-standardized.bat`

---

## FILES TO KEEP VS. DELETE

### Local Machine - Keep
```
✅ Source code (all .js, .jsx, .json files)
✅ Configuration templates (.env.example)
✅ Documentation (.md files)
✅ Package files (package.json, package-lock.json)
✅ Git repository (.git/)
✅ Test data/databases (for development)
```

### Local Machine - Delete
```
❌ Old backup directories (spotless-mobile-backup)
❌ Duplicate production folders (backend-production, frontend-production)
❌ Corrupted files (files with special characters)
❌ Old build artifacts (outdated build folders)
❌ Temporary files (.log, .tmp)
❌ Unused dependencies (check package.json)
```

### Production Server - Keep
```
✅ Running backend code
✅ Frontend build files
✅ Database (spotless_db)
✅ Uploaded files (message photos)
✅ Logs (recent output.log)
✅ SSL certificates
✅ Caddy configuration
```

### Production Server - Delete
```
❌ Old backup files (older than 30 days)
❌ Unused code directories
❌ Development dependencies
❌ Temporary build files
❌ Old log files (older than 7 days)
❌ Test/debug files
```

---

## IMPLEMENTATION CHECKLIST

### Phase 1: Documentation (✅ In Progress)
- [x] Create credentials document
- [ ] Audit local environment
- [ ] Audit production environment
- [ ] Document all differences
- [ ] Create sync procedures

### Phase 2: Standardization
- [ ] Sync production database to local
- [ ] Verify local database has all data
- [ ] Update local .env with correct credentials
- [ ] Test local development environment
- [ ] Ensure code matches between environments

### Phase 3: Cleanup
- [ ] Remove duplicate directories locally
- [ ] Remove corrupted/renamed files
- [ ] Clean up old backups locally
- [ ] Clean up old files on production
- [ ] Remove unused dependencies

### Phase 4: Automation
- [ ] Create database sync script
- [ ] Create deployment scripts
- [ ] Test deployment process
- [ ] Document deployment procedures
- [ ] Create rollback procedures

### Phase 5: Verification
- [ ] Test full deployment local → production
- [ ] Verify all features work
- [ ] Test database migrations
- [ ] Test mobile app builds
- [ ] Document any issues found

---

## NEXT STEPS

1. **Get PostgreSQL Password:** Need your local PostgreSQL password
2. **Audit Local:** Document current local setup
3. **Audit Production:** Document current production setup
4. **Sync Database:** Copy production DB to local
5. **Test Everything:** Ensure local environment works
6. **Clean Up:** Remove unnecessary files
7. **Automate:** Create deployment scripts
8. **Document:** Update all documentation

---

## RELATED DOCUMENTS

- [CREDENTIALS.md](../../projects/spotless-improved/.credentials/CREDENTIALS.md) - Secure credentials storage
- [DEPLOYMENT_GUIDE_FINAL.md](../../projects/spotless-improved/DEPLOYMENT_GUIDE_FINAL.md) - Current deployment guide
- [state-current.md](../../projects/spotless-improved/state-current.md) - Current project state
- [fix-database-connection.md](../../projects/spotless-improved/fix-database-connection.md) - Database troubleshooting

---

**Status:** 🚧 Documentation phase complete, moving to audit phase
**Next Action:** Prompt user for PostgreSQL password and begin environment audit
