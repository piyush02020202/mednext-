# Quick Start Guide - MedNext+ E-Commerce Platform

## 🚀 Get Started in 5 Minutes

### Step 1: Install MongoDB (if not already installed)
Download from [MongoDB Community Server](https://www.mongodb.com/try/download/community)

### Step 2: Backend Setup

```bash
# Navigate to backend directory
cd backend

# Install dependencies
npm install

# Create .env file (copy from .env.example)
cp .env.example .env

# Edit .env with your credentials
# At minimum, update:
# - MONGODB_URI (or leave default for local)
# - JWT_SECRET (can be any random string)
```

### Step 3: Start MongoDB
```bash
# On Windows (Command Prompt as Admin)
net start MongoDB

# Or start mongod directly
mongod
```

### Step 4: Start Backend Server
```bash
cd backend
npm run dev
```

Server will start on `http://localhost:5000`

### Step 5: Frontend Setup (New Terminal)
```bash
cd frontend

# Install dependencies
npm install

# Start React development server
npm start
```

Frontend will open at `http://localhost:3000`

## 📱 Sample Test Data

### Test User (Customer)
```
Email: customer@test.com
Password: password123
```

### Test Admin
```
Email: admin@test.com
Password: admin123
```

### Create a Test User
Register a new account directly through the UI at `http://localhost:3000/register`

## 🧪 API Testing with cURL

### Register a User
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@test.com",
    "password": "password123",
    "phone": "9876543210"
  }'
```

### Login
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@test.com",
    "password": "password123"
  }'
```

### Get Products
```bash
curl http://localhost:5000/api/products
```

### Get Products by Category
```bash
curl "http://localhost:5000/api/products?category=Antibiotics"
```

## 📋 Project Features Checklist

- ✅ User Authentication (Register/Login)
- ✅ Product Catalog (Browse by category)
- ✅ Shopping Cart (Add/Remove items)
- ✅ Checkout Process
- ✅ Order Management
- ✅ User Profiles
- ✅ Product Reviews
- ✅ Admin Features
- ✅ Responsive Design

## 🔧 Useful Commands

### Backend
```bash
npm run dev     # Start with hot reload
npm start       # Start production
npm test        # Run tests
```

### Frontend
```bash
npm start       # Start development server
npm build       # Build for production
npm test        # Run tests
```

## 🐛 Troubleshooting

**Cannot connect to MongoDB**
- Ensure MongoDB service is running
- Check MONGODB_URI in .env
- Try: `mongo --version` to verify installation

**Port already in use**
- Backend: Change PORT in .env
- Frontend: Kill process on port 3000 or change in package.json

**CORS errors**
- Ensure backend is running on port 5000
- Check FRONTEND_URL in backend .env

**Module not found**
- Run `npm install` in both directories
- Delete node_modules and npm install again

## 📚 Next Steps

1. **Add Sample Products**: Use admin panel or POST to /api/products
2. **Test Payment Integration**: Set up Stripe API keys
3. **Configure Email**: Add Gmail credentials for notifications
4. **Deploy**: Push to Heroku, AWS, or your preferred platform

## 📖 Documentation

- Full documentation: See [README.md](./README.md)
- API Documentation: See [README.md](./README.md#-api-documentation)
- Database Schema: See [README.md](./README.md#-database-schema)

## 🆘 Need Help?

- Check the main [README.md](./README.md) for detailed documentation
- Review error messages in browser console and terminal
- Ensure all environment variables are correctly set
- Verify all services (MongoDB, Backend, Frontend) are running

---

**You're all set! Start shopping medicine online with MedNext+! 🏥💊**
