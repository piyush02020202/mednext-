# MedNext+ Vercel Deployment Guide

## Deployment Setup Instructions

### Prerequisites
- Vercel account (https://vercel.com)
- GitHub account with repositories
- MongoDB Atlas URI
- JWT Secret key

---

## FRONTEND DEPLOYMENT (React App)

### Step 1: Prepare Frontend
The `vercel.json` file is already configured in the frontend directory.

### Step 2: Deploy to Vercel
```bash
# Option A: Using Vercel CLI
npm install -g vercel
cd frontend
vercel --prod

# Option B: Using GitHub (Recommended)
# 1. Push your repo to GitHub
# 2. Go to https://vercel.com/new
# 3. Import your repository
# 4. Set Environment Variables (see below)
# 5. Deploy
```

### Step 3: Set Environment Variables (Frontend)
In Vercel Dashboard → Settings → Environment Variables:
```
REACT_APP_API_URL=https://mednext-api.vercel.app
```

### Step 4: Build Settings
- **Framework**: Create React App
- **Build Command**: `npm run build`
- **Output Directory**: `build`

---

## BACKEND DEPLOYMENT (Node.js API)

### Step 1: Prepare Backend
The `vercel.json` file is already configured for Node.js serverless functions.

### Step 2: Update Backend URL Reference
Update `frontend/.env.production` with your backend Vercel URL:
```
REACT_APP_API_URL=https://your-backend-app.vercel.app
```

### Step 3: Deploy Backend
```bash
# Option A: Using Vercel CLI
cd backend
vercel --prod

# Option B: Using GitHub
# 1. Push backend repo to GitHub (or same repo, different branch)
# 2. Go to https://vercel.com/new
# 3. Import backend repository
# 4. Set Environment Variables
# 5. Deploy
```

### Step 4: Set Environment Variables (Backend)
In Vercel Dashboard → Settings → Environment Variables, add:
```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/mednext?retryWrites=true&w=majority
JWT_SECRET=your_jwt_secret_key_here
NODE_ENV=production
CORS_ORIGIN=https://your-frontend-app.vercel.app
FRONTEND_URL=https://your-frontend-app.vercel.app
PORT=3001
```

---

## Configuration Files Overview

### Frontend - `vercel.json`
- **buildCommand**: Runs React build
- **outputDirectory**: Points to `build` folder
- **rewrites**: Handles client-side routing

### Backend - `vercel.json`
- **builds**: Specifies Node.js serverless function
- **routes**: Routes all requests to server.js
- **env**: Environment variables mapping

---

## Update Backend Configuration for Production

### 1. Update CORS Settings
Edit `backend/src/server.js`:
```javascript
app.use(cors({
  origin: process.env.FRONTEND_URL || 'http://localhost:3000',
  credentials: true
}));
```

### 2. Environment Variables (.env.production)
```
NODE_ENV=production
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/mednext?retryWrites=true&w=majority
JWT_SECRET=your_secure_jwt_secret
FRONTEND_URL=https://mednext-frontend.vercel.app
PORT=
```

---

## Frontend Configuration Updates

### Update API Base URL
Create `frontend/.env.production`:
```
REACT_APP_API_URL=https://mednext-api.vercel.app
REACT_APP_API_TIMEOUT=5000
```

Update API calls in your services to use:
```javascript
const API_BASE_URL = process.env.REACT_APP_API_URL || 'http://localhost:5000';

// Example API call
fetch(`${API_BASE_URL}/api/products`)
```

---

## Deployment Checklist

### Pre-Deployment
- [ ] Test locally with production environment variables
- [ ] Ensure all environment variables are set in Vercel
- [ ] MongoDB Atlas cluster is accessible
- [ ] API endpoints are tested and working
- [ ] Frontend builds successfully without errors
- [ ] CORS settings are correct

### Deployment
- [ ] Deploy backend first
- [ ] Test backend API endpoints
- [ ] Deploy frontend with correct backend URL
- [ ] Test frontend connectivity to API

### Post-Deployment
- [ ] Verify all pages load correctly
- [ ] Test login/authentication
- [ ] Test product fetching
- [ ] Test cart functionality
- [ ] Check browser console for errors
- [ ] Monitor Vercel logs for issues

---

## Useful Vercel Commands

```bash
# View deployment logs
vercel logs [project-name]

# Check current deployment
vercel inspect

# List all deployments
vercel ls

# View environment variables
vercel env list

# Preview deployment
vercel --prod --preview

# Remove deployment
vercel rm [deployment-url]
```

---

## MongoDB Atlas IP Whitelist

Ensure your MongoDB Atlas cluster allows Vercel IPs:
1. Go to MongoDB Atlas → Network Access
2. Add IP: `0.0.0.0/0` (allows all IPs - for development)
3. Or add specific Vercel IPs (more secure for production)

---

## Troubleshooting

### CORS Errors
- Verify `FRONTEND_URL` environment variable matches your Vercel frontend URL
- Check CORS configuration in backend

### MongoDB Connection Issues
- Verify `MONGODB_URI` is correct and IP whitelist includes Vercel IPs
- Check MongoDB cluster status on Atlas dashboard

### 404 Errors on Routes
- Add `vercel.json` rewrites for client-side routing (frontend)
- Verify API routes in backend `vercel.json`

### Environment Variables Not Loading
- Restart deployment after updating env vars
- Verify variable names match exactly in code
- Check for typos in variable names

---

## Deployment Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      Vercel Platform                         │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌─────────────────────┐        ┌──────────────────────┐   │
│  │  Frontend (React)   │        │  Backend (Node.js)   │   │
│  │  https://mednext... │        │  https://mednext-api │   │
│  │  - Home            │        │  - API Routes        │   │
│  │  - Products        │◄───────┤  - Database Connect  │   │
│  │  - Cart            │        │  - Auth              │   │
│  │  - Checkout        │        │  - Product CRUD      │   │
│  └─────────────────────┘        └──────────────────────┘   │
│           │                              │                    │
│           └──────────────────────────────┘                    │
│                  Serverless Functions                          │
│                                                                │
└────────────────────────────────────────────────────────────────┘
                            │
                            │ (MongoDB Connection)
                            ▼
                  ┌──────────────────┐
                  │   MongoDB Atlas  │
                  │  (mednext cluster)
                  └──────────────────┘
```

---

## Costs

- **Frontend**: Free tier available (up to 100GB/month)
- **Backend**: Free tier available (up to 100 Serverless Function calls/month)
- **MongoDB Atlas**: Free tier available (512MB storage)

---

## Next Steps

1. Create `vercel.json` files (already done ✓)
2. Set up GitHub repositories for frontend and backend
3. Connect repositories to Vercel
4. Configure environment variables
5. Deploy and test
6. Monitor performance and logs

For more information, visit: https://vercel.com/docs
