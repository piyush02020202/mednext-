# MedNext+ Vercel Deployment Checklist

## Phase 1: Pre-Deployment Setup ✓

### Repository Setup
- [ ] Create GitHub repository for frontend
- [ ] Create GitHub repository for backend (or use monorepo)
- [ ] Push all code to GitHub
- [ ] Verify all files are committed

### Environment Configuration
- [x] Backend: `vercel.json` created
- [x] Frontend: `vercel.json` created
- [x] Backend: `.env.production` configured
- [x] Frontend: `.env.production` configured
- [ ] Update MongoDB Atlas IP whitelist to allow Vercel
- [ ] Generate secure JWT_SECRET for production

### Code Review
- [ ] Backend CORS properly configured ✓
- [ ] API routes tested locally
- [ ] Frontend build tested locally
- [ ] All environment variables documented
- [ ] No hardcoded URLs or secrets

---

## Phase 2: MongoDB Atlas Configuration

### IP Whitelist
1. Go to MongoDB Atlas Dashboard
2. Navigate to Network Access
3. Click "Add IP Address"
4. Options:
   - For Development: Allow `0.0.0.0/0` (all IPs)
   - For Production: Add Vercel IP ranges
     - `76.75.126.0/24` (Vercel US)
     - `2604:1380:0:2600::/42` (Vercel IPv6)
   - Or: Click "Allow Access from Anywhere" (less secure but easier)

### Database User
- [ ] Verify database user credentials are correct
- [ ] Password is URL encoded correctly in MONGODB_URI
- [ ] Database name is correct in URI

### Cluster Status
- [ ] Cluster is running and accessible
- [ ] Connection string is correct
- [ ] Test connection locally succeeds

---

## Phase 3: Vercel Backend Deployment

### Create Vercel Project
1. Go to https://vercel.com/dashboard
2. Click "Add New..." → "Project"
3. Select GitHub repository (backend)
4. Configure settings:
   - **Framework Preset**: Node.js
   - **Build Command**: Leave empty (Vercel auto-detects)
   - **Output Directory**: Leave empty
   - **Install Command**: `npm install`

### Environment Variables (Backend)
Set these in Vercel Dashboard → Settings → Environment Variables:

```
MONGODB_URI
JWT_SECRET
FRONTEND_URL
CORS_ORIGIN
NODE_ENV=production
```

### Deploy
- [ ] Click "Deploy"
- [ ] Wait for build to complete
- [ ] Verify deployment succeeded
- [ ] Note the deployment URL (e.g., `https://mednext-api.vercel.app`)

### Test Backend API
```bash
# Test health endpoint
curl https://mednext-api.vercel.app/api/health

# Test products endpoint
curl https://mednext-api.vercel.app/api/products
```

---

## Phase 4: Update Frontend Configuration

### Update API URL
1. Update `frontend/.env.production`:
   ```
   REACT_APP_API_URL=https://mednext-api.vercel.app
   ```

2. Update any hardcoded API URLs in code:
   - Check all API service files
   - Verify environment variable usage

### Verify Build Locally
```bash
cd frontend
npm run build
# Should complete without errors
```

---

## Phase 5: Vercel Frontend Deployment

### Create Vercel Project
1. Go to https://vercel.com/dashboard
2. Click "Add New..." → "Project"
3. Select GitHub repository (frontend)
4. Configure settings:
   - **Framework Preset**: Create React App
   - **Build Command**: `npm run build`
   - **Output Directory**: `build`
   - **Install Command**: `npm install`

### Environment Variables (Frontend)
```
REACT_APP_API_URL=https://mednext-api.vercel.app
```

### Custom Domain (Optional)
1. In Vercel Dashboard → Domains
2. Add custom domain
3. Configure DNS records as instructed

### Deploy
- [ ] Click "Deploy"
- [ ] Wait for build to complete
- [ ] Verify deployment succeeded
- [ ] Note the deployment URL (e.g., `https://mednext.vercel.app`)

---

## Phase 6: Post-Deployment Testing

### Frontend Testing
- [ ] Homepage loads without errors
- [ ] Products display with images
- [ ] Navigation works
- [ ] Responsive design works on mobile
- [ ] Images load properly from Unsplash

### API Connectivity
- [ ] Fetch products endpoint works
- [ ] Cart functionality works
- [ ] Login/auth works
- [ ] No CORS errors in browser console
- [ ] API response times acceptable

### Browser Console
- [ ] No JavaScript errors
- [ ] No 404 errors
- [ ] No CORS errors
- [ ] Network requests successful

### Database
- [ ] Data persists correctly
- [ ] New products can be added
- [ ] Database queries are fast

---

## Phase 7: Monitoring & Maintenance

### Vercel Logs
Check logs for any errors:
```bash
vercel logs [project-name] --tail
```

### Performance Monitoring
- [ ] Check Vercel Analytics
- [ ] Monitor API response times
- [ ] Check database query performance
- [ ] Monitor error rates

### Error Tracking
- [ ] Set up error logging/monitoring
- [ ] Configure alerts for deployment failures
- [ ] Monitor database connection issues

---

## Phase 8: Security Checklist

- [ ] Remove all hardcoded secrets
- [ ] All environment variables are set in Vercel
- [ ] MongoDB user credentials are secure
- [ ] JWT_SECRET is cryptographically secure
- [ ] CORS is properly configured
- [ ] HTTPS is enabled (automatic on Vercel)
- [ ] Rate limiting is configured
- [ ] Input validation is in place

---

## Common Issues & Solutions

### Issue: CORS Errors
**Solution:**
1. Verify `CORS_ORIGIN` matches frontend URL exactly
2. Restart backend deployment after env changes
3. Check browser console for exact error

### Issue: MongoDB Connection Timeout
**Solution:**
1. Verify IP whitelist includes Vercel IPs
2. Check MongoDB URI is correct
3. Verify database user credentials
4. Check cluster is running

### Issue: 404 on Frontend Routes
**Solution:**
1. Verify `vercel.json` rewrites are configured
2. Clear browser cache
3. Check routing in React app

### Issue: Build Fails
**Solution:**
1. Run `npm run build` locally to test
2. Check build logs in Vercel dashboard
3. Verify all dependencies are in package.json
4. Check for syntax errors

---

## Rollback Plan

If deployment goes wrong:
```bash
# View previous deployments
vercel ls

# Promote previous deployment to production
vercel promote [deployment-url]

# Or use Vercel Dashboard to switch production to previous deployment
```

---

## Performance Optimization

### Frontend
- [ ] Enable image optimization
- [ ] Implement code splitting
- [ ] Optimize bundle size
- [ ] Enable gzip compression

### Backend
- [ ] Implement caching
- [ ] Optimize database queries
- [ ] Use connection pooling
- [ ] Enable compression middleware

---

## Cost Optimization

- **Vercel Free Tier**: Up to 100 serverless function executions/month
- **MongoDB Atlas Free Tier**: 512MB storage
- For production: Consider upgrading to paid tiers

---

## Deployment Completion Checklist

- [ ] Backend deployed successfully
- [ ] Frontend deployed successfully
- [ ] All environment variables configured
- [ ] API endpoints working
- [ ] Frontend connects to API
- [ ] Database operations working
- [ ] No errors in logs
- [ ] Performance is acceptable
- [ ] Security measures in place
- [ ] Custom domain configured (if applicable)
- [ ] Monitoring and alerts configured
- [ ] Team notified of deployment

---

## Post-Deployment Communication

Email template for team:

```
Subject: MedNext+ is now live on Vercel! 🚀

Hi team,

MedNext+ has been successfully deployed to Vercel!

Frontend: https://mednext.vercel.app
Backend API: https://mednext-api.vercel.app

Features:
✓ 57 medicines with beautiful images
✓ Indian Rupee (₹) pricing
✓ Full shopping cart functionality
✓ Secure authentication
✓ Responsive design

Database: MongoDB Atlas (Cloud)
Deployment: Vercel Serverless

For issues or questions, please reach out!
```

---

## Useful Resources

- Vercel Docs: https://vercel.com/docs
- MongoDB Atlas: https://www.mongodb.com/cloud/atlas
- Express on Vercel: https://vercel.com/docs/concepts/functions/serverless-functions/runtimes/node-js
- React Deployment: https://create-react-app.dev/deployment

---

**Status**: Ready for deployment
**Last Updated**: Feb 5, 2026
