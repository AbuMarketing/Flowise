# InstaAgents Deployment Checklist

## Pre-Deployment Verification

### ✅ Code Quality

-   [x] All files modified successfully
-   [x] No syntax errors in modified files
-   [x] No TypeScript/ESLint errors
-   [x] All imports are correct
-   [x] No broken references

### ⚠️ Assets (Action Required)

-   [ ] Replace `packages/ui/public/favicon.ico` with InstaAgents favicon
-   [ ] Replace `packages/ui/public/logo192.png` with 192x192 PNG logo
-   [ ] Replace `packages/ui/public/logo512.png` with 512x512 PNG logo
-   [ ] (Optional) Replace SVG logos with custom designs if available

### 🔨 Build Process

-   [ ] Run `pnpm install` (if dependencies changed)
-   [ ] Run `pnpm build` successfully
-   [ ] No build errors or warnings
-   [ ] Build output is clean

### 🧪 Local Testing

#### Visual Testing

-   [ ] Light mode displays correctly
-   [ ] Dark mode displays correctly
-   [ ] Logo shows in header (both themes)
-   [ ] Sidebar has glass effect
-   [ ] Active menu items show gradient
-   [ ] Cards have rounded corners
-   [ ] Buttons have hover effects
-   [ ] Inputs have proper styling
-   [ ] No "Flowise" branding visible
-   [ ] All colors match brand palette

#### Functional Testing

-   [ ] Navigation works (all menu items)
-   [ ] Chatflows list loads
-   [ ] Can create new chatflow
-   [ ] Can edit existing chatflow
-   [ ] Chatflow builder/canvas works
-   [ ] Drag-and-drop nodes works
-   [ ] Can save chatflow
-   [ ] Agentflows list loads
-   [ ] Can create new agentflow
-   [ ] Can edit existing agentflow
-   [ ] Agentflow builder/canvas works
-   [ ] Templates Hub (marketplace) loads
-   [ ] Can browse templates
-   [ ] Can use templates
-   [ ] Credentials page works
-   [ ] API Keys page works
-   [ ] Variables page works
-   [ ] Tools page works
-   [ ] Settings page works
-   [ ] Account settings work
-   [ ] Dark mode toggle works
-   [ ] Profile menu works

#### Canvas/Builder Specific

-   [ ] Nodes render correctly
-   [ ] Node connections work
-   [ ] Edge buttons show brand colors
-   [ ] Can add new nodes
-   [ ] Can delete nodes
-   [ ] Can configure nodes
-   [ ] Zoom in/out works
-   [ ] Pan/drag canvas works
-   [ ] Node tooltips work
-   [ ] Validation works

#### Chat Interface

-   [ ] Chat widget loads
-   [ ] Can send messages
-   [ ] Receives responses
-   [ ] Chat history works
-   [ ] File attachments work (if enabled)

### 🌐 Browser Testing

-   [ ] Chrome (latest)
-   [ ] Firefox (latest)
-   [ ] Safari (latest)
-   [ ] Edge (latest)
-   [ ] Mobile Chrome
-   [ ] Mobile Safari

### 📱 Responsive Testing

-   [ ] Desktop (1920x1080)
-   [ ] Laptop (1366x768)
-   [ ] Tablet (768x1024)
-   [ ] Mobile (375x667)
-   [ ] Mobile landscape

### 🎨 Theme Testing

-   [ ] Toggle between light/dark multiple times
-   [ ] All components adapt correctly
-   [ ] No color contrast issues
-   [ ] Shadows appropriate for each theme
-   [ ] Text readable in both themes
-   [ ] Icons visible in both themes

### ⚡ Performance Testing

-   [ ] Page load time acceptable
-   [ ] Animations smooth (60fps)
-   [ ] No lag when dragging nodes
-   [ ] No memory leaks
-   [ ] Canvas performance good with many nodes
-   [ ] Sidebar transitions smooth

## Deployment Steps

### 1. Backup

```bash
# Create backup of current installation
cp -r /path/to/flowise /path/to/flowise-backup-$(date +%Y%m%d)
```

### 2. Version Control

```bash
# Commit changes
git add .
git commit -m "Rebrand: Flowise → InstaAgents"

# Tag release
git tag -a v1.0.0-instaagents -m "InstaAgents rebrand release"

# Push to repository
git push origin main
git push origin v1.0.0-instaagents
```

### 3. Build for Production

```bash
# Clean previous builds
pnpm clean

# Install dependencies
pnpm install

# Build all packages
pnpm build

# Verify build
ls -la packages/ui/dist
ls -la packages/server/dist
```

### 4. Environment Variables

```bash
# Verify all environment variables are set
# Note: FLOWISE_* variables remain unchanged
cat .env

# Required variables (examples):
# FLOWISE_USERNAME=admin
# FLOWISE_PASSWORD=***
# DATABASE_PATH=/path/to/database
# etc.
```

### 5. Database

```bash
# Backup database
cp flowise.db flowise.db.backup

# No migrations needed (schema unchanged)
```

### 6. Docker Deployment (if applicable)

```bash
# Build Docker image
docker build -t instaagents:latest .

# Or using docker-compose
docker-compose build

# Start containers
docker-compose up -d

# Check logs
docker-compose logs -f
```

### 7. Standard Deployment

```bash
# Start the application
pnpm start

# Or with PM2
pm2 start packages/server/dist/index.js --name instaagents

# Check status
pm2 status
```

### 8. Verify Deployment

```bash
# Check if server is running
curl http://localhost:3000/api/v1/health

# Check UI is accessible
curl http://localhost:3000
```

## Post-Deployment Verification

### Immediate Checks (First 5 minutes)

-   [ ] Application starts without errors
-   [ ] UI loads in browser
-   [ ] Can log in
-   [ ] Dashboard displays
-   [ ] No console errors
-   [ ] API endpoints responding

### Short-term Checks (First hour)

-   [ ] All pages accessible
-   [ ] Existing chatflows load
-   [ ] Existing agentflows load
-   [ ] Can create new flows
-   [ ] Chat functionality works
-   [ ] No error logs

### Medium-term Checks (First day)

-   [ ] User feedback positive
-   [ ] No reported issues
-   [ ] Performance acceptable
-   [ ] All features working
-   [ ] Analytics normal

## Rollback Plan

### If Issues Occur

#### Quick Rollback (Docker)

```bash
# Stop current containers
docker-compose down

# Restore previous image
docker-compose up -d --force-recreate
```

#### Standard Rollback

```bash
# Stop application
pm2 stop instaagents
# or
pkill -f flowise

# Restore from backup
rm -rf /path/to/current
cp -r /path/to/flowise-backup-YYYYMMDD /path/to/current

# Restore database
cp flowise.db.backup flowise.db

# Restart
pnpm start
```

#### Git Rollback

```bash
# Revert to previous commit
git revert HEAD

# Or reset to specific commit
git reset --hard <commit-hash>

# Rebuild
pnpm build
pnpm start
```

## Monitoring

### What to Monitor

-   [ ] Server uptime
-   [ ] Response times
-   [ ] Error rates
-   [ ] Memory usage
-   [ ] CPU usage
-   [ ] Disk space
-   [ ] Database size
-   [ ] User activity

### Logs to Check

```bash
# Application logs
tail -f logs/app.log

# PM2 logs
pm2 logs instaagents

# Docker logs
docker-compose logs -f

# System logs
journalctl -u instaagents -f
```

## User Communication

### Before Deployment

-   [ ] Notify users of scheduled maintenance
-   [ ] Explain rebrand changes
-   [ ] Provide timeline
-   [ ] Share documentation

### After Deployment

-   [ ] Announce successful deployment
-   [ ] Share new branding
-   [ ] Provide updated documentation
-   [ ] Collect feedback

### Communication Template

```
Subject: InstaAgents Platform Update

Dear Users,

We're excited to announce that our platform has been rebranded to InstaAgents!

What's New:
- Fresh, modern interface with Apple-inspired design
- New brand colors (purple and pink)
- Enhanced user experience with smooth animations
- Improved visual hierarchy and spacing

What Stays the Same:
- All your existing chatflows and agentflows
- All functionality and features
- Your data and credentials
- API endpoints and integrations

The update will be deployed on [DATE] at [TIME].
Expected downtime: [DURATION]

Questions? Contact support@instaagents.com

Best regards,
The InstaAgents Team
```

## Documentation Updates

### Update These Documents

-   [ ] User guide
-   [ ] API documentation
-   [ ] Installation guide
-   [ ] Configuration guide
-   [ ] Troubleshooting guide
-   [ ] FAQ
-   [ ] Screenshots in docs
-   [ ] Video tutorials (if any)

### Update These Links

-   [ ] Website
-   [ ] Social media profiles
-   [ ] Email signatures
-   [ ] Support portal
-   [ ] Knowledge base

## Success Criteria

### Technical

-   ✅ Zero downtime deployment
-   ✅ No data loss
-   ✅ All features working
-   ✅ Performance maintained
-   ✅ No critical errors

### User Experience

-   ✅ Positive user feedback
-   ✅ No usability issues
-   ✅ Smooth transition
-   ✅ Clear branding
-   ✅ Professional appearance

### Business

-   ✅ Brand identity established
-   ✅ User adoption smooth
-   ✅ No support spike
-   ✅ Positive reception
-   ✅ Goals achieved

## Final Sign-off

-   [ ] Technical lead approval
-   [ ] Design lead approval
-   [ ] Product owner approval
-   [ ] QA team approval
-   [ ] Stakeholder approval

---

## Emergency Contacts

**Technical Issues:**

-   Lead Developer: [contact]
-   DevOps: [contact]
-   Database Admin: [contact]

**Business Issues:**

-   Product Owner: [contact]
-   Project Manager: [contact]

**Support:**

-   Support Team: [contact]
-   On-call: [contact]

---

**Deployment Date:** ******\_\_\_******
**Deployed By:** ******\_\_\_******
**Verified By:** ******\_\_\_******
**Sign-off:** ******\_\_\_******

---

🎉 **Ready to deploy InstaAgents!**
