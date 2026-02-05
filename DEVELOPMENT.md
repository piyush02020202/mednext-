# Development Guide - MedNext+ E-Commerce Platform

## 🏗️ Architecture Overview

MedNext+ is a full-stack application with clear separation of concerns:

```
┌─────────────────────────────────────────────────────────────┐
│                    Frontend (React)                         │
│  ├─ Components: Reusable UI elements                        │
│  ├─ Pages: Full page components                             │
│  ├─ Context: State management (Auth, Cart)                  │
│  └─ Styles: Tailwind CSS styling                            │
└────────────┬────────────────────────────────────────────────┘
             │ HTTP/HTTPS (Axios)
             ▼
┌─────────────────────────────────────────────────────────────┐
│                 Backend (Express.js)                        │
│  ├─ Routes: API endpoints                                   │
│  ├─ Controllers: Business logic                             │
│  ├─ Models: MongoDB schemas                                 │
│  ├─ Middleware: Authentication, validation                  │
│  └─ Config: Database connection                             │
└────────────┬────────────────────────────────────────────────┘
             │ Mongoose ODM
             ▼
┌─────────────────────────────────────────────────────────────┐
│                 MongoDB Database                            │
│  ├─ Users Collection                                        │
│  ├─ Products Collection                                     │
│  ├─ Orders Collection                                       │
│  └─ Carts Collection                                        │
└─────────────────────────────────────────────────────────────┘
```

## 📦 Dependencies

### Backend Dependencies

#### Core
- **express**: Web framework
- **mongoose**: MongoDB ODM
- **dotenv**: Environment variable management

#### Authentication & Security
- **jsonwebtoken**: JWT token management
- **bcryptjs**: Password hashing

#### Validation & Middleware
- **express-validator**: Input validation
- **cors**: Cross-Origin Resource Sharing

#### File & Data
- **multer**: File upload handling
- **cloudinary**: Cloud storage for images

#### Payments & Communication
- **stripe**: Payment processing
- **nodemailer**: Email sending

#### Development
- **nodemon**: Auto-restart development server

### Frontend Dependencies

#### Core
- **react**: UI library
- **react-dom**: React rendering
- **react-router-dom**: Client-side routing

#### HTTP & Data
- **axios**: HTTP client

#### UI & Styling
- **tailwindcss**: Utility-first CSS
- **react-icons**: Icon library

## 🔄 Development Workflow

### Setting Up Your Development Environment

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd mednext+
   ```

2. **Install dependencies**
   ```bash
   cd backend && npm install
   cd ../frontend && npm install
   ```

3. **Configure environment**
   - Copy `.env.example` to `.env` in backend folder
   - Update MongoDB URI and other credentials

4. **Start development servers**
   ```bash
   # Terminal 1: Backend
   cd backend
   npm run dev

   # Terminal 2: Frontend
   cd frontend
   npm start
   ```

## 🗂️ File Organization

### Backend Structure

```
backend/src/
├── config/
│   └── database.js              # MongoDB connection setup
├── controllers/
│   ├── authController.js        # User authentication logic
│   ├── productController.js     # Product CRUD operations
│   ├── cartController.js        # Shopping cart logic
│   └── orderController.js       # Order management
├── middleware/
│   └── auth.js                  # JWT validation & role checking
├── models/
│   ├── User.js                  # User schema & methods
│   ├── Product.js               # Product schema
│   ├── Order.js                 # Order schema
│   └── Cart.js                  # Cart schema
├── routes/
│   ├── authRoutes.js            # /api/auth endpoints
│   ├── productRoutes.js         # /api/products endpoints
│   ├── cartRoutes.js            # /api/cart endpoints
│   └── orderRoutes.js           # /api/orders endpoints
├── utils/                       # Helper functions (future)
└── server.js                    # Express app initialization
```

### Frontend Structure

```
frontend/src/
├── components/
│   ├── Header.js                # Navigation header
│   └── ProductCard.js           # Product display card
├── pages/
│   ├── Home.js                  # Product listing
│   ├── Login.js                 # User login
│   ├── Register.js              # User registration
│   ├── Cart.js                  # Shopping cart
│   ├── Checkout.js              # Order checkout
│   └── Orders.js                # Order history
├── context/
│   ├── AuthContext.js           # Authentication state
│   └── CartContext.js           # Shopping cart state
├── styles/
│   └── index.css                # Global styles
├── App.js                       # Main app component
├── index.js                     # React entry point
└── index.css                    # Global styles
```

## 🔐 Authentication Flow

```
1. User Registration
   │
   ├─ Frontend: Submit form
   ├─ Backend: Hash password, save user
   └─ Response: JWT token

2. User Login
   │
   ├─ Frontend: Submit credentials
   ├─ Backend: Verify password, generate JWT
   └─ Response: JWT token + user info

3. Protected Routes
   │
   ├─ Frontend: Include token in header
   ├─ Backend: Verify JWT in middleware
   └─ Allow/Deny access

4. Token Storage
   └─ Frontend: localStorage.setItem('token')
```

## 📡 API Naming Conventions

### Endpoints Pattern
```
GET    /api/resource              # List all
GET    /api/resource/:id          # Get one
POST   /api/resource              # Create
PUT    /api/resource/:id          # Update
DELETE /api/resource/:id          # Delete
```

### Request/Response Format
```javascript
// Request
{
  "headers": {
    "Authorization": "Bearer <JWT_TOKEN>",
    "Content-Type": "application/json"
  }
}

// Success Response
{
  "success": true,
  "message": "Operation successful",
  "data": { /* actual data */ }
}

// Error Response
{
  "success": false,
  "message": "Error description"
}
```

## 🧪 Testing API Endpoints

### Using Postman
1. Import API endpoints
2. Set up collection variables for TOKEN and BASE_URL
3. Use Pre-request Script to set Authorization header

### Using cURL
```bash
# Get auth token
TOKEN=$(curl -s -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@test.com","password":"password"}' \
  | jq -r '.token')

# Use token in requests
curl http://localhost:5000/api/orders \
  -H "Authorization: Bearer $TOKEN"
```

## 🐛 Debugging Tips

### Backend Debugging
```javascript
// Add console logs
console.log('User:', req.user);
console.log('Cart Items:', cart.items);

// Use nodemon for auto-reload
npm run dev

// Check MongoDB
mongo
> use mednext
> db.users.find().pretty()
```

### Frontend Debugging
```javascript
// React DevTools Chrome Extension
// Redux/Context DevTools

// Console debugging
console.log('State:', state);
console.log('Props:', props);

// Network tab in DevTools
// Check API requests and responses
```

## 🚀 Performance Optimization

### Backend
- **Database Indexes**: Add indexes on frequently queried fields
- **Caching**: Implement Redis for frequent queries
- **Pagination**: Implement pagination for large datasets
- **Compression**: Use gzip middleware

### Frontend
- **Code Splitting**: Use React.lazy() for routes
- **Image Optimization**: Compress product images
- **Bundle Analysis**: Check bundle size with source-map-explorer
- **Memoization**: Use React.memo() for expensive components

## 🔄 Common Development Tasks

### Adding a New Product Feature

1. **Backend**
   ```javascript
   // 1. Update Product schema in models/Product.js
   // 2. Add validation in controller
   // 3. Update routes if needed
   // 4. Test with API client
   ```

2. **Frontend**
   ```javascript
   // 1. Update API call in component
   // 2. Handle new data in render
   // 3. Add new UI elements
   // 4. Test in browser
   ```

### Adding Authentication to a Route

**Backend**
```javascript
const { authMiddleware, adminMiddleware } = require('../middleware/auth');

router.post('/admin/product', 
  authMiddleware,    // Verify token
  adminMiddleware,   // Check admin role
  controller
);
```

**Frontend**
```javascript
import { useAuth } from '../context/AuthContext';

const ProtectedComponent = () => {
  const { user, token } = useAuth();
  
  if (!token) {
    return <Navigate to="/login" />;
  }
  
  return <div>Protected Content</div>;
};
```

## 📋 Code Style Guidelines

### JavaScript
- Use const by default, let when reassignment needed
- Use arrow functions
- Use descriptive variable names
- Add comments for complex logic

### React Components
```javascript
const MyComponent = ({ prop1, prop2 }) => {
  const [state, setState] = useState(null);
  
  useEffect(() => {
    // Effect logic
  }, [dependencies]);
  
  return (
    <div>
      {/* JSX */}
    </div>
  );
};

export default MyComponent;
```

### Express Routes
```javascript
router.get('/:id', authMiddleware, (req, res) => {
  // Validation
  if (!req.params.id) {
    return res.status(400).json({ message: 'Invalid ID' });
  }
  
  // Logic
  // Response
});
```

## 🚨 Error Handling

### Backend Error Handling
```javascript
try {
  // Operation
  const result = await Model.findById(id);
  if (!result) {
    return res.status(404).json({ message: 'Not found' });
  }
  res.json(result);
} catch (error) {
  console.error(error);
  res.status(500).json({ message: 'Server error' });
}
```

### Frontend Error Handling
```javascript
try {
  const response = await axios.get('/api/endpoint');
  setData(response.data);
} catch (error) {
  setError(error.response?.data?.message || 'Something went wrong');
}
```

## 📚 Resources & Documentation

- [Express.js Documentation](https://expressjs.com/)
- [MongoDB/Mongoose Docs](https://mongoosejs.com/)
- [React Documentation](https://react.dev/)
- [Tailwind CSS](https://tailwindcss.com/)
- [React Router](https://reactrouter.com/)

## 🎓 Learning Path

1. **Understand the architecture** - Review this guide
2. **Run the application** - Follow QUICKSTART.md
3. **Explore the code** - Read through files
4. **Make small changes** - Try adding a field to a form
5. **Build features** - Implement new functionality
6. **Deploy** - Push to production

## ✅ Commit Message Guidelines

```bash
git commit -m "feat: Add product search functionality"
git commit -m "fix: Fix cart quantity update bug"
git commit -m "docs: Update API documentation"
git commit -m "style: Format code with prettier"
git commit -m "refactor: Extract common logic to utils"
```

---

Happy coding! 🚀 For questions, check the main README.md or create an issue.
