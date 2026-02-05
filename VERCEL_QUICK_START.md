# MedNext+ Vercel Deployment - Quick Start

## 🚀 5-Minute Quick Start

### Step 1: Prepare Code (Local)
```bash
# Clone/verify your repository
cd d:\mednext+

# Backend - verify build
cd backend
npm install
npm run build  # If you have a build script

# Frontend - verify build
cd ../frontend
npm install
npm run build
# Should create 'build' folder without errors
```

### Step 2: Push to GitHub
```bash
# Initialize Git (if not already done)
git init
git add .
git commit -m "Ready for Vercel deployment"
git remote add origin https://github.com/yourname/mednext-backend.git
git push -u origin main

# Repeat for frontend in separate repo or branch
```

### Step 3: Deploy Backend
1. Go to https://vercel.com/new
2. Click "Import Git Repository"
3. Select your backend repository
4. Framework: **Node.js**
5. Set Environment Variables:
   ```
   MONGODB_URI=mongodb+srv://mednext:mednext%402641@cluster0.4xa81hy.mongodb.net/mednext?retryWrites=true&w=majority
   JWT_SECRET=mednext_production_jwt_secret_2026
   FRONTEND_URL=https://mednext.vercel.app
   CORS_ORIGIN=https://mednext.vercel.app
   NODE_ENV=production
   ```
6. Click **Deploy**
7. Copy your backend URL: `https://your-project.vercel.app`

### Step 4: Deploy Frontend
1. Update `frontend/.env.production`:
   ```
   REACT_APP_API_URL=https://your-backend-url.vercel.app
   ```
2. Push changes to GitHub
3. Go to https://vercel.com/new
4. Select frontend repository
5. Framework: **Create React App**
6. Environment Variables:
   ```
   REACT_APP_API_URL=https://your-backend-url.vercel.app
   ```
7. Click **Deploy**

### Step 5: Test
```
✓ Visit https://your-frontend-url.vercel.app
✓ Verify products load
✓ Check browser console for errors
✓ Test add to cart
```

---

## Files Already Created

✓ `backend/vercel.json` - Backend serverless config
✓ `frontend/vercel.json` - Frontend build config  
✓ `backend/.env.production` - Production environment variables
✓ `frontend/.env.production` - Frontend production variables
✓ `VERCEL_DEPLOYMENT_GUIDE.md` - Complete deployment guide
✓ `DEPLOYMENT_CHECKLIST.md` - Full checklist with troubleshooting

---

## MongoDB Atlas Setup (IMPORTANT)

Before deploying, allow Vercel to access MongoDB:

1. Go to https://cloud.mongodb.com/
2. Select your MedNext+ cluster
3. Network Access → Add IP Address
4. Choose: **Allow access from anywhere** (easier for testing)
   - Or add Vercel IP ranges manually
5. Click Confirm

---

## Environment Variables Summary

### Backend (.env.production)
```
NODE_ENV=production
MONGODB_URI=your_mongodb_atlas_uri
JWT_SECRET=your_secret_key
FRONTEND_URL=https://your-frontend.vercel.app
CORS_ORIGIN=https://your-frontend.vercel.app
```

### Frontend (.env.production)
```
REACT_APP_API_URL=https://your-backend.vercel.app
```

---

## Vercel Project Naming

Suggested names:
- Backend: `mednext-api`
- Frontend: `mednext` or `mednext-app`

This makes URLs clean:
- Backend: `mednext-api.vercel.app`
- Frontend: `mednext.vercel.app`

---

## After Deployment

1. **Test API**
   ```bash
   curl https://your-backend.vercel.app/api/health
   curl https://your-backend.vercel.app/api/products
   ```

2. **Monitor Logs**
   - Backend: Vercel Dashboard → Deployments → Logs
   - Frontend: Vercel Dashboard → Deployments → Logs

3. **Check Database**
   - MongoDB Atlas → Collections → Verify data

4. **Performance**
   - Vercel Analytics → Monitor response times
   - Check for 4xx/5xx errors

---

## Rollback

If something goes wrong:
```bash
# In Vercel Dashboard
1. Click your project
2. Go to Deployments
3. Select previous deployment
4. Click "Promote to Production"
```

---

## Support

For issues:
- ✓ Check `DEPLOYMENT_CHECKLIST.md` for troubleshooting
- ✓ Check `VERCEL_DEPLOYMENT_GUIDE.md` for detailed steps
- ✓ Review Vercel logs in dashboard
- ✓ Check MongoDB logs in Atlas dashboard

---

## File Structure for Deployment

```
mednext/
├── frontend/
│   ├── src/
│   ├── public/
│   ├── vercel.json ✓
│   ├── .env.production ✓
│   └── package.json
├── backend/
│   ├── src/
│   ├── vercel.json ✓
│   ├── .env.production ✓
│   └── package.json
├── VERCEL_DEPLOYMENT_GUIDE.md ✓
└── DEPLOYMENT_CHECKLIST.md ✓
```

---

**Ready to deploy? Start with Step 1 above! 🚀**
