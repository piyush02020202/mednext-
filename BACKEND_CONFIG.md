# Backend Configuration

## Environment Variables

Create a `.env` file in the backend directory with the following variables:

### Required Variables

```env
# Server Configuration
NODE_ENV=development
PORT=5000

# Database
MONGODB_URI=mongodb://localhost:27017/mednext

# JWT
JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
JWT_EXPIRE=7d
```

### Optional Variables

```env
# Stripe Payment Gateway
STRIPE_SECRET_KEY=sk_test_your_stripe_key

# Email Service
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_app_password

# Cloudinary (Image Upload)
CLOUDINARY_NAME=your_cloudinary_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

# Frontend URL (for CORS)
FRONTEND_URL=http://localhost:3000
```

## Database Setup

### Using Local MongoDB

1. Install MongoDB Community Server from https://www.mongodb.com/try/download/community
2. Run MongoDB:
   ```bash
   mongod
   ```
3. Default connection string: `mongodb://localhost:27017/mednext`

### Using MongoDB Atlas (Cloud)

1. Create account at https://www.mongodb.com/cloud/atlas
2. Create cluster and database
3. Get connection string:
   ```
   mongodb+srv://username:password@cluster.mongodb.net/mednext
   ```
4. Update MONGODB_URI in .env

## Running Backend

### Development Mode
```bash
npm run dev
```
Uses nodemon for hot-reload

### Production Mode
```bash
npm start
```

### Testing
```bash
npm test
```

## API Server

Default URL: `http://localhost:5000`

Health Check:
```bash
curl http://localhost:5000/api/health
```

## Troubleshooting

### Cannot connect to MongoDB
- Ensure MongoDB is running
- Check MONGODB_URI format
- Verify database name in connection string

### Port already in use
- Change PORT in .env
- Or kill process: `lsof -ti :5000 | xargs kill -9`

### JWT errors
- Update JWT_SECRET in .env
- Clear any existing tokens
- Regenerate new JWT

### CORS issues
- Ensure FRONTEND_URL matches your frontend address
- Check Origin header from browser
