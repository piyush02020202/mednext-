# Frontend Configuration

## Project Setup

This is a React application built with Create React App, Tailwind CSS, and Axios.

## Environment Setup

### Prerequisites
- Node.js 14+ installed
- npm or yarn package manager

### Installation

```bash
cd frontend
npm install
```

### Development Server

```bash
npm start
```

- Opens automatically at `http://localhost:3000`
- Hot-reload enabled on file changes
- Proxy configured to backend on `http://localhost:5000`

## API Configuration

### Base URL
The frontend is configured to use `http://localhost:5000` as the API base URL.

Update in components:
```javascript
const API_BASE_URL = 'http://localhost:5000/api';

// Or use axios instance
const api = axios.create({
  baseURL: API_BASE_URL,
  headers: {
    'Content-Type': 'application/json'
  }
});
```

### Adding Authorization Header

All authenticated requests include JWT token:
```javascript
const token = localStorage.getItem('token');
const headers = {
  Authorization: `Bearer ${token}`
};

axios.get('/api/orders', { headers });
```

## Styling

### Tailwind CSS

Configured with:
- `tailwind.config.js` - Configuration file
- `postcss.config.js` - PostCSS configuration
- `src/index.css` - Global styles

### Custom Classes

```css
/* src/index.css */
@import 'tailwindcss/base';
@import 'tailwindcss/components';
@import 'tailwindcss/utilities';
```

### Using Tailwind in Components

```jsx
<div className="max-w-7xl mx-auto px-4 py-8">
  <h1 className="text-3xl font-bold text-gray-900">
    Welcome to MedNext+
  </h1>
</div>
```

## State Management

### Context API

Two main contexts:

#### AuthContext
```javascript
import { useAuth } from './context/AuthContext';

const { user, token, login, logout } = useAuth();
```

#### CartContext
```javascript
import { useCart } from './context/CartContext';

const { cart, addToCart, removeFromCart, getTotalPrice } = useCart();
```

## Component Structure

### Pages
Located in `src/pages/`:
- `Home.js` - Product listing and filtering
- `Login.js` - User login page
- `Register.js` - User registration
- `Cart.js` - Shopping cart page
- `Checkout.js` - Order checkout
- `Orders.js` - Order history

### Components
Located in `src/components/`:
- `Header.js` - Navigation header
- `ProductCard.js` - Product display component

### Adding New Pages

1. Create file in `src/pages/NewPage.js`
2. Add route in `src/App.js`
3. Add navigation link in `Header.js`

```javascript
// App.js
<Route path="/newpage" element={<NewPage />} />

// Header.js
<Link to="/newpage">New Page</Link>
```

## Icons

Using React Icons library:

```javascript
import { FiShoppingCart, FiLogOut } from 'react-icons/fi';

<FiShoppingCart size={24} />
```

Available icon sets:
- `fi` - Feather Icons
- `bi` - Bootstrap Icons
- `ai` - Ant Design Icons

## Form Handling

Example form with state:

```javascript
const [formData, setFormData] = useState({
  email: '',
  password: ''
});

const handleChange = (e) => {
  setFormData({
    ...formData,
    [e.target.name]: e.target.value
  });
};

const handleSubmit = async (e) => {
  e.preventDefault();
  // Send to API
};
```

## API Integration

### Making Requests

```javascript
import axios from 'axios';

// GET
const response = await axios.get('http://localhost:5000/api/products');

// POST with auth
const token = localStorage.getItem('token');
const response = await axios.post(
  'http://localhost:5000/api/orders',
  orderData,
  {
    headers: { Authorization: `Bearer ${token}` }
  }
);

// Error handling
try {
  // request
} catch (error) {
  console.error(error.response?.data?.message);
}
```

## Building for Production

```bash
npm run build
```

Creates optimized build in `build/` directory.

## Deployment

### Vercel
```bash
npm install -g vercel
vercel
```

### Netlify
```bash
npm install -g netlify-cli
netlify deploy --prod --dir=build
```

### Docker
Create `Dockerfile`:
```dockerfile
FROM node:16-alpine as build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM node:16-alpine
WORKDIR /app
RUN npm install -g serve
COPY --from=build /app/build ./build
EXPOSE 3000
CMD ["serve", "-s", "build", "-l", "3000"]
```

## Troubleshooting

### Cannot connect to backend
- Ensure backend is running on port 5000
- Check `proxy` in package.json
- Verify API_BASE_URL in components

### State not updating
- Check Context Provider wraps components
- Verify useContext hook in component
- Check dependencies in useEffect

### Styling not applied
- Clear browser cache
- Rebuild with `npm run build`
- Check Tailwind class syntax

### Module not found
- Run `npm install` to install dependencies
- Clear node_modules: `rm -rf node_modules && npm install`
- Check import paths

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## Performance Tips

1. **Code Splitting**: Use `React.lazy()` for routes
2. **Image Optimization**: Compress product images
3. **Memoization**: Use `React.memo()` for static components
4. **Bundle Analysis**: Check bundle size

```bash
npm install --save-dev source-map-explorer
npm run build
npx source-map-explorer 'build/static/js/*.js'
```

## Security

- JWT tokens stored in localStorage
- CORS configured for backend
- Sensitive data not exposed in code
- Environment variables for API keys (future)

## Additional Resources

- [React Documentation](https://react.dev/)
- [Tailwind CSS](https://tailwindcss.com/)
- [Axios Documentation](https://axios-http.com/)
- [React Router](https://reactrouter.com/)

---

For backend configuration, see [backend/BACKEND_CONFIG.md](../backend/BACKEND_CONFIG.md)
