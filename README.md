# MedNext+ - E-Commerce Platform for Online Medicine Shopping

A full-stack e-commerce application for buying medicines online with features like product catalog, shopping cart, secure checkout, order management, and admin dashboard.

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [API Documentation](#api-documentation)
- [Database Schema](#database-schema)
- [Contributing](#contributing)

## 🎯 Project Overview

MedNext+ is a modern e-commerce platform designed specifically for online medicine shopping. It provides a seamless experience for customers to browse medicines, manage shopping carts, place orders, and track deliveries. Administrators can manage products, inventory, and orders through an admin panel.

## ✨ Features

### Customer Features
- **User Authentication**: Secure registration and login with JWT
- **Product Catalog**: Browse medicines by category with search functionality
- **Product Details**: View detailed information, ratings, and reviews
- **Shopping Cart**: Add/remove items, update quantities
- **Checkout**: Secure checkout with multiple payment methods
- **Order Management**: Track orders and view order history
- **User Profile**: Manage personal information and addresses
- **Product Reviews**: Rate and review medicines

### Admin Features
- **Product Management**: Create, update, and delete products
- **Inventory Management**: Manage stock levels
- **Order Management**: View and update order status
- **Sales Analytics**: Dashboard with sales metrics
- **User Management**: View customer information

## 🛠️ Tech Stack

### Backend
- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB
- **Authentication**: JWT (JSON Web Tokens)
- **Password Hashing**: bcryptjs
- **Payment Integration**: Stripe (Ready for integration)
- **Email**: Nodemailer
- **File Upload**: Cloudinary

### Frontend
- **Library**: React 18
- **Routing**: React Router v6
- **HTTP Client**: Axios
- **Styling**: Tailwind CSS
- **Icons**: React Icons
- **State Management**: React Context API

## 📁 Project Structure

```
mednext+/
├── backend/
│   ├── src/
│   │   ├── config/
│   │   │   └── database.js          # MongoDB connection
│   │   ├── controllers/
│   │   │   ├── authController.js    # Auth logic
│   │   │   ├── productController.js # Product operations
│   │   │   ├── cartController.js    # Cart operations
│   │   │   └── orderController.js   # Order operations
│   │   ├── middleware/
│   │   │   └── auth.js              # Authentication & authorization
│   │   ├── models/
│   │   │   ├── User.js              # User schema
│   │   │   ├── Product.js           # Product schema
│   │   │   ├── Order.js             # Order schema
│   │   │   └── Cart.js              # Cart schema
│   │   ├── routes/
│   │   │   ├── authRoutes.js        # Auth endpoints
│   │   │   ├── productRoutes.js     # Product endpoints
│   │   │   ├── cartRoutes.js        # Cart endpoints
│   │   │   └── orderRoutes.js       # Order endpoints
│   │   └── server.js                # Main application file
│   ├── .env.example                 # Environment variables template
│   └── package.json                 # Dependencies
│
├── frontend/
│   ├── public/
│   │   ├── index.html               # Main HTML file
│   │   └── manifest.json            # PWA manifest
│   ├── src/
│   │   ├── components/
│   │   │   ├── Header.js            # Navigation header
│   │   │   └── ProductCard.js       # Product card component
│   │   ├── context/
│   │   │   ├── AuthContext.js       # Auth state management
│   │   │   └── CartContext.js       # Cart state management
│   │   ├── pages/
│   │   │   ├── Home.js              # Product listing
│   │   │   ├── Login.js             # Login page
│   │   │   ├── Register.js          # Registration page
│   │   │   ├── Cart.js              # Shopping cart
│   │   │   ├── Checkout.js          # Checkout page
│   │   │   └── Orders.js            # Order history
│   │   ├── App.js                   # Main app component
│   │   ├── index.js                 # React entry point
│   │   └── index.css                # Global styles
│   ├── tailwind.config.js           # Tailwind configuration
│   ├── postcss.config.js            # PostCSS configuration
│   └── package.json                 # Dependencies
│
└── README.md                        # This file
```

## 🚀 Installation

### Prerequisites
- Node.js (v14 or higher)
- npm or yarn
- MongoDB (local or Atlas)

### Backend Setup

1. **Navigate to backend directory**
   ```bash
   cd backend
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Create .env file**
   ```bash
   cp .env.example .env
   ```

4. **Configure environment variables** (see Configuration section)

### Frontend Setup

1. **Navigate to frontend directory**
   ```bash
   cd frontend
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

## ⚙️ Configuration

### Backend .env Configuration

```env
# Server
NODE_ENV=development
PORT=5000

# Database
MONGODB_URI=mongodb://localhost:27017/mednext
# Or for MongoDB Atlas:
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/mednext

# JWT
JWT_SECRET=your_super_secret_jwt_key_here
JWT_EXPIRE=7d

# Stripe Payment
STRIPE_SECRET_KEY=your_stripe_secret_key

# Email Service
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_app_password

# Cloudinary (Image upload)
CLOUDINARY_NAME=your_cloudinary_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

# Frontend URL
FRONTEND_URL=http://localhost:3000
```

### Frontend Configuration

Update the API base URL in your axios calls (currently set to `http://localhost:5000`).

## 📌 Running the Application

### Start MongoDB
```bash
# If using local MongoDB
mongod
```

### Start Backend Server
```bash
cd backend
npm run dev    # Development mode with nodemon
npm start      # Production mode
```

The backend will run on `http://localhost:5000`

### Start Frontend
```bash
cd frontend
npm start
```

The frontend will open at `http://localhost:3000`

## 📚 API Documentation

### Authentication Endpoints

#### Register
```http
POST /api/auth/register
Content-Type: application/json

{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "password": "securepassword",
  "phone": "9876543210"
}
```

#### Login
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "securepassword"
}
```

#### Get Profile
```http
GET /api/auth/profile
Authorization: Bearer <JWT_TOKEN>
```

### Product Endpoints

#### Get All Products
```http
GET /api/products?category=Antibiotics&page=1&limit=10&search=amoxicillin
```

#### Get Product by ID
```http
GET /api/products/:id
```

#### Create Product (Admin)
```http
POST /api/products
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json

{
  "name": "Aspirin 500mg",
  "description": "Pain relief tablet",
  "price": 50,
  "category": "Pain Relief",
  "manufacturer": "Bayer",
  "stock": 100,
  "requiresPrescription": false,
  "image": "image_url"
}
```

### Cart Endpoints

#### Get Cart
```http
GET /api/cart
Authorization: Bearer <JWT_TOKEN>
```

#### Add to Cart
```http
POST /api/cart/add
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json

{
  "productId": "60d5ec49c1234567890abcd1",
  "quantity": 2
}
```

#### Remove from Cart
```http
DELETE /api/cart/:productId
Authorization: Bearer <JWT_TOKEN>
```

### Order Endpoints

#### Create Order
```http
POST /api/orders
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json

{
  "shippingAddress": {
    "street": "123 Main St",
    "city": "Delhi",
    "state": "DL",
    "zipCode": "110001",
    "country": "India"
  },
  "paymentMethod": "card"
}
```

#### Get Orders
```http
GET /api/orders
Authorization: Bearer <JWT_TOKEN>
```

## 💾 Database Schema

### User Schema
```javascript
{
  firstName: String,
  lastName: String,
  email: String (unique),
  password: String (hashed),
  phone: String,
  address: {
    street: String,
    city: String,
    state: String,
    zipCode: String,
    country: String
  },
  role: String (customer, admin),
  isVerified: Boolean,
  createdAt: Date,
  updatedAt: Date
}
```

### Product Schema
```javascript
{
  name: String,
  description: String,
  price: Number,
  discount: Number,
  category: String,
  manufacturer: String,
  stock: Number,
  requiresPrescription: Boolean,
  image: String,
  rating: Number,
  reviews: [{
    userId: ObjectId,
    userName: String,
    rating: Number,
    comment: String,
    createdAt: Date
  }],
  createdAt: Date,
  updatedAt: Date
}
```

### Order Schema
```javascript
{
  orderNumber: String (unique),
  userId: ObjectId,
  items: [{
    productId: ObjectId,
    name: String,
    price: Number,
    quantity: Number
  }],
  shippingAddress: Object,
  totalAmount: Number,
  taxAmount: Number,
  shippingCost: Number,
  discount: Number,
  paymentMethod: String,
  paymentStatus: String,
  orderStatus: String,
  trackingNumber: String,
  createdAt: Date,
  updatedAt: Date
}
```

## 🔒 Security Features

- **Password Hashing**: bcryptjs with salt rounds
- **JWT Authentication**: Secure token-based authentication
- **Authorization**: Role-based access control (admin/customer)
- **Input Validation**: Express validator for all inputs
- **CORS**: Configured for frontend domain
- **Environment Variables**: Sensitive data stored securely

## 🚦 Common Issues & Solutions

### MongoDB Connection Error
- Ensure MongoDB is running
- Check MONGODB_URI in .env file
- Verify database credentials

### JWT Token Errors
- Clear browser localStorage and re-login
- Check JWT_SECRET in .env
- Verify token format in Authorization header

### CORS Errors
- Update FRONTEND_URL in backend .env
- Ensure frontend is running on correct port
- Check CORS middleware configuration

## 📝 Future Enhancements

- [ ] Payment gateway integration (Stripe, Razorpay)
- [ ] Email notifications
- [ ] SMS notifications
- [ ] Advanced search and filters
- [ ] Prescription verification system
- [ ] Medicine interactions checker
- [ ] Loyalty program
- [ ] Mobile app (React Native)
- [ ] Real-time order tracking
- [ ] Video consultation feature

## 📄 License

This project is licensed under the MIT License.

## 👥 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📞 Support

For support, please contact or create an issue in the repository.

---

**Happy Coding! 🚀**
