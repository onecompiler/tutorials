# Context API for State Management

The Context API is React's built-in solution for managing global state without prop drilling. It's perfect for sharing data across multiple components without passing props through every level.

## Understanding Context API

Context provides a way to share values between components without explicitly passing props through every level of the tree.

### Creating a Basic Context
```jsx
import React, { createContext, useContext, useState } from 'react';

// Create a context
const ThemeContext = createContext();

// Create a provider component
export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(prevTheme => prevTheme === 'light' ? 'dark' : 'light');
  };
  
  const value = {
    theme,
    toggleTheme
  };
  
  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}

// Custom hook to use the context
export function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within a ThemeProvider');
  }
  return context;
}

// Usage in components
function App() {
  return (
    <ThemeProvider>
      <Header />
      <MainContent />
    </ThemeProvider>
  );
}

function Header() {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <header style={{ backgroundColor: theme === 'light' ? '#fff' : '#333' }}>
      <button onClick={toggleTheme}>
        Current theme: {theme}
      </button>
    </header>
  );
}
```

## Advanced State Management

### Complex State with useReducer
```jsx
const AppStateContext = createContext();
const AppDispatchContext = createContext();

// Action types
const ActionTypes = {
  SET_USER: 'SET_USER',
  ADD_NOTIFICATION: 'ADD_NOTIFICATION',
  REMOVE_NOTIFICATION: 'REMOVE_NOTIFICATION',
  UPDATE_SETTINGS: 'UPDATE_SETTINGS',
  TOGGLE_SIDEBAR: 'TOGGLE_SIDEBAR'
};

// Reducer
function appReducer(state, action) {
  switch (action.type) {
    case ActionTypes.SET_USER:
      return {
        ...state,
        user: action.payload
      };
      
    case ActionTypes.ADD_NOTIFICATION:
      return {
        ...state,
        notifications: [...state.notifications, {
          id: Date.now(),
          ...action.payload
        }]
      };
      
    case ActionTypes.REMOVE_NOTIFICATION:
      return {
        ...state,
        notifications: state.notifications.filter(
          n => n.id !== action.payload
        )
      };
      
    case ActionTypes.UPDATE_SETTINGS:
      return {
        ...state,
        settings: {
          ...state.settings,
          ...action.payload
        }
      };
      
    case ActionTypes.TOGGLE_SIDEBAR:
      return {
        ...state,
        ui: {
          ...state.ui,
          sidebarOpen: !state.ui.sidebarOpen
        }
      };
      
    default:
      throw new Error(`Unknown action: ${action.type}`);
  }
}

// Initial state
const initialState = {
  user: null,
  notifications: [],
  settings: {
    theme: 'light',
    language: 'en',
    notifications: true
  },
  ui: {
    sidebarOpen: true
  }
};

// Provider component
export function AppProvider({ children }) {
  const [state, dispatch] = useReducer(appReducer, initialState);
  
  return (
    <AppStateContext.Provider value={state}>
      <AppDispatchContext.Provider value={dispatch}>
        {children}
      </AppDispatchContext.Provider>
    </AppStateContext.Provider>
  );
}

// Custom hooks
export function useAppState() {
  const context = useContext(AppStateContext);
  if (!context) {
    throw new Error('useAppState must be used within AppProvider');
  }
  return context;
}

export function useAppDispatch() {
  const context = useContext(AppDispatchContext);
  if (!context) {
    throw new Error('useAppDispatch must be used within AppProvider');
  }
  return context;
}

// Action creators
export const actions = {
  setUser: (user) => ({
    type: ActionTypes.SET_USER,
    payload: user
  }),
  
  addNotification: (notification) => ({
    type: ActionTypes.ADD_NOTIFICATION,
    payload: notification
  }),
  
  removeNotification: (id) => ({
    type: ActionTypes.REMOVE_NOTIFICATION,
    payload: id
  }),
  
  updateSettings: (settings) => ({
    type: ActionTypes.UPDATE_SETTINGS,
    payload: settings
  }),
  
  toggleSidebar: () => ({
    type: ActionTypes.TOGGLE_SIDEBAR
  })
};

// Usage in components
function UserProfile() {
  const { user } = useAppState();
  const dispatch = useAppDispatch();
  
  const logout = () => {
    dispatch(actions.setUser(null));
    dispatch(actions.addNotification({
      type: 'info',
      message: 'You have been logged out'
    }));
  };
  
  if (!user) {
    return <div>Please log in</div>;
  }
  
  return (
    <div>
      <h1>Welcome, {user.name}!</h1>
      <button onClick={logout}>Logout</button>
    </div>
  );
}
```

## Authentication Context

### Complete Auth Implementation
```jsx
const AuthContext = createContext();

export function AuthProvider({ children }) {
  const [state, setState] = useState({
    user: null,
    isLoading: true,
    error: null
  });
  
  // Check if user is logged in on mount
  useEffect(() => {
    checkAuth();
  }, []);
  
  const checkAuth = async () => {
    try {
      const token = localStorage.getItem('authToken');
      if (!token) {
        setState({ user: null, isLoading: false, error: null });
        return;
      }
      
      const response = await fetch('/api/auth/me', {
        headers: {
          'Authorization': `Bearer ${token}`
        }
      });
      
      if (response.ok) {
        const user = await response.json();
        setState({ user, isLoading: false, error: null });
      } else {
        localStorage.removeItem('authToken');
        setState({ user: null, isLoading: false, error: null });
      }
    } catch (error) {
      setState({ user: null, isLoading: false, error: error.message });
    }
  };
  
  const login = async (credentials) => {
    setState(prev => ({ ...prev, isLoading: true, error: null }));
    
    try {
      const response = await fetch('/api/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(credentials)
      });
      
      const data = await response.json();
      
      if (response.ok) {
        localStorage.setItem('authToken', data.token);
        setState({ user: data.user, isLoading: false, error: null });
        return { success: true };
      } else {
        setState({ user: null, isLoading: false, error: data.message });
        return { success: false, error: data.message };
      }
    } catch (error) {
      setState({ user: null, isLoading: false, error: error.message });
      return { success: false, error: error.message };
    }
  };
  
  const register = async (userData) => {
    setState(prev => ({ ...prev, isLoading: true, error: null }));
    
    try {
      const response = await fetch('/api/auth/register', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(userData)
      });
      
      const data = await response.json();
      
      if (response.ok) {
        localStorage.setItem('authToken', data.token);
        setState({ user: data.user, isLoading: false, error: null });
        return { success: true };
      } else {
        setState({ user: null, isLoading: false, error: data.message });
        return { success: false, error: data.message };
      }
    } catch (error) {
      setState({ user: null, isLoading: false, error: error.message });
      return { success: false, error: error.message };
    }
  };
  
  const logout = () => {
    localStorage.removeItem('authToken');
    setState({ user: null, isLoading: false, error: null });
  };
  
  const updateUser = (updates) => {
    setState(prev => ({
      ...prev,
      user: { ...prev.user, ...updates }
    }));
  };
  
  const value = {
    ...state,
    login,
    register,
    logout,
    updateUser,
    checkAuth
  };
  
  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
}

// Protected route component
export function ProtectedRoute({ children }) {
  const { user, isLoading } = useAuth();
  const location = useLocation();
  
  if (isLoading) {
    return <div>Loading...</div>;
  }
  
  if (!user) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  
  return children;
}
```

## Shopping Cart Context

### E-commerce State Management
```jsx
const CartContext = createContext();

export function CartProvider({ children }) {
  const [cart, setCart] = useState(() => {
    // Load cart from localStorage
    const savedCart = localStorage.getItem('cart');
    return savedCart ? JSON.parse(savedCart) : [];
  });
  
  // Save cart to localStorage whenever it changes
  useEffect(() => {
    localStorage.setItem('cart', JSON.stringify(cart));
  }, [cart]);
  
  const addToCart = (product, quantity = 1) => {
    setCart(prevCart => {
      const existingItem = prevCart.find(item => item.id === product.id);
      
      if (existingItem) {
        return prevCart.map(item =>
          item.id === product.id
            ? { ...item, quantity: item.quantity + quantity }
            : item
        );
      }
      
      return [...prevCart, { ...product, quantity }];
    });
  };
  
  const removeFromCart = (productId) => {
    setCart(prevCart => prevCart.filter(item => item.id !== productId));
  };
  
  const updateQuantity = (productId, quantity) => {
    if (quantity <= 0) {
      removeFromCart(productId);
      return;
    }
    
    setCart(prevCart =>
      prevCart.map(item =>
        item.id === productId
          ? { ...item, quantity }
          : item
      )
    );
  };
  
  const clearCart = () => {
    setCart([]);
  };
  
  const getItemsCount = () => {
    return cart.reduce((total, item) => total + item.quantity, 0);
  };
  
  const getCartTotal = () => {
    return cart.reduce(
      (total, item) => total + (item.price * item.quantity),
      0
    ).toFixed(2);
  };
  
  const isInCart = (productId) => {
    return cart.some(item => item.id === productId);
  };
  
  const value = {
    cart,
    addToCart,
    removeFromCart,
    updateQuantity,
    clearCart,
    getItemsCount,
    getCartTotal,
    isInCart
  };
  
  return (
    <CartContext.Provider value={value}>
      {children}
    </CartContext.Provider>
  );
}

export function useCart() {
  const context = useContext(CartContext);
  if (!context) {
    throw new Error('useCart must be used within CartProvider');
  }
  return context;
}

// Cart components
function CartIcon() {
  const { getItemsCount } = useCart();
  const itemCount = getItemsCount();
  
  return (
    <div className="cart-icon">
      🛒
      {itemCount > 0 && (
        <span className="badge">{itemCount}</span>
      )}
    </div>
  );
}

function ProductCard({ product }) {
  const { addToCart, isInCart } = useCart();
  const inCart = isInCart(product.id);
  
  return (
    <div className="product-card">
      <img src={product.image} alt={product.name} />
      <h3>{product.name}</h3>
      <p>${product.price}</p>
      <button
        onClick={() => addToCart(product)}
        disabled={inCart}
      >
        {inCart ? 'In Cart' : 'Add to Cart'}
      </button>
    </div>
  );
}
```

## Multi-Provider Pattern

### Combining Multiple Contexts
```jsx
// Combine multiple providers
function AppProviders({ children }) {
  return (
    <AuthProvider>
      <ThemeProvider>
        <CartProvider>
          <NotificationProvider>
            {children}
          </NotificationProvider>
        </CartProvider>
      </ThemeProvider>
    </AuthProvider>
  );
}

// Or create a provider composer
function ProviderComposer({ contexts, children }) {
  return contexts.reduceRight(
    (kids, parent) =>
      React.cloneElement(parent, {
        children: kids,
      }),
    children
  );
}

// Usage
function App() {
  return (
    <ProviderComposer
      contexts={[
        <AuthProvider />,
        <ThemeProvider />,
        <CartProvider />,
        <NotificationProvider />
      ]}
    >
      <Router>
        {/* Your app */}
      </Router>
    </ProviderComposer>
  );
}
```

## Performance Optimization

### Context Splitting
```jsx
// Split contexts to avoid unnecessary re-renders
const UserContext = createContext();
const UserSettingsContext = createContext();
const UIContext = createContext();

// Instead of one large context
const AppContext = createContext(); // Avoid this

// Use multiple focused contexts
function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  
  // Memoize the value to prevent unnecessary re-renders
  const value = useMemo(() => ({ user, setUser }), [user]);
  
  return (
    <UserContext.Provider value={value}>
      {children}
    </UserContext.Provider>
  );
}
```

### Memoization Strategies
```jsx
function OptimizedProvider({ children }) {
  const [state, dispatch] = useReducer(reducer, initialState);
  
  // Memoize dispatch actions
  const actions = useMemo(() => ({
    updateUser: (user) => dispatch({ type: 'UPDATE_USER', payload: user }),
    updateSettings: (settings) => dispatch({ type: 'UPDATE_SETTINGS', payload: settings })
  }), []);
  
  // Separate static and dynamic values
  const staticValue = useMemo(() => ({ dispatch, actions }), [actions]);
  const dynamicValue = state;
  
  return (
    <StaticContext.Provider value={staticValue}>
      <DynamicContext.Provider value={dynamicValue}>
        {children}
      </DynamicContext.Provider>
    </StaticContext.Provider>
  );
}
```

## TypeScript Support

### Typed Context
```typescript
interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'user';
}

interface AuthContextType {
  user: User | null;
  isLoading: boolean;
  error: string | null;
  login: (credentials: LoginCredentials) => Promise<LoginResult>;
  logout: () => void;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export function useAuth(): AuthContextType {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
}
```

## Error Boundaries with Context

### Error Handling
```jsx
const ErrorContext = createContext();

export function ErrorProvider({ children }) {
  const [errors, setErrors] = useState([]);
  
  const addError = (error) => {
    const id = Date.now();
    setErrors(prev => [...prev, { id, ...error }]);
    
    // Auto-dismiss after 5 seconds
    setTimeout(() => {
      removeError(id);
    }, 5000);
  };
  
  const removeError = (id) => {
    setErrors(prev => prev.filter(error => error.id !== id));
  };
  
  const value = {
    errors,
    addError,
    removeError
  };
  
  return (
    <ErrorContext.Provider value={value}>
      {children}
      <ErrorDisplay />
    </ErrorContext.Provider>
  );
}

function ErrorDisplay() {
  const { errors, removeError } = useContext(ErrorContext);
  
  return (
    <div className="error-container">
      {errors.map(error => (
        <div key={error.id} className="error-message">
          {error.message}
          <button onClick={() => removeError(error.id)}>×</button>
        </div>
      ))}
    </div>
  );
}
```

## Best Practices

1. **Split contexts** - Don't put everything in one context
2. **Memoize values** - Prevent unnecessary re-renders
3. **Custom hooks** - Always create custom hooks for contexts
4. **Error handling** - Handle missing providers gracefully
5. **Default values** - Provide sensible defaults
6. **TypeScript** - Type your contexts for better DX
7. **Performance** - Use React.memo and useMemo appropriately
8. **Testing** - Create test providers for easier testing

The Context API is powerful for managing global state in React applications. Use it wisely for clean, maintainable code!