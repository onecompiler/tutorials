# useContext Hook

The useContext Hook provides a way to pass data through the component tree without having to pass props down manually at every level. It's React's solution to "prop drilling."

## Understanding Context

Context provides a way to share values between components without explicitly passing props through every level of the tree.

### Creating Context
```jsx
import React, { createContext, useContext } from 'react';

// Create a context
const ThemeContext = createContext();

// Create a provider component
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}
```

### Using Context
```jsx
// Using useContext Hook
function ThemedButton() {
  const { theme, setTheme } = useContext(ThemeContext);
  
  return (
    <button
      style={{
        background: theme === 'light' ? '#fff' : '#333',
        color: theme === 'light' ? '#333' : '#fff'
      }}
      onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}
    >
      Current theme: {theme}
    </button>
  );
}

// App component
function App() {
  return (
    <ThemeProvider>
      <ThemedButton />
    </ThemeProvider>
  );
}
```

## Basic Example

### Without Context (Prop Drilling)
```jsx
// Problem: Passing props through multiple levels
function App() {
  const [user, setUser] = useState({ name: 'John', role: 'admin' });
  
  return <Dashboard user={user} setUser={setUser} />;
}

function Dashboard({ user, setUser }) {
  return <Profile user={user} setUser={setUser} />;
}

function Profile({ user, setUser }) {
  return <ProfileEditor user={user} setUser={setUser} />;
}

function ProfileEditor({ user, setUser }) {
  return <div>Editing {user.name}</div>;
}
```

### With Context
```jsx
// Solution: Using Context
const UserContext = createContext();

function App() {
  const [user, setUser] = useState({ name: 'John', role: 'admin' });
  
  return (
    <UserContext.Provider value={{ user, setUser }}>
      <Dashboard />
    </UserContext.Provider>
  );
}

function Dashboard() {
  return <Profile />;
}

function Profile() {
  return <ProfileEditor />;
}

function ProfileEditor() {
  const { user, setUser } = useContext(UserContext);
  return <div>Editing {user.name}</div>;
}
```

## Complete Authentication Example

```jsx
// AuthContext.js
import React, { createContext, useContext, useState, useEffect } from 'react';

const AuthContext = createContext(null);

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    // Check if user is logged in
    const token = localStorage.getItem('token');
    if (token) {
      fetchUser(token)
        .then(setUser)
        .catch(() => localStorage.removeItem('token'))
        .finally(() => setLoading(false));
    } else {
      setLoading(false);
    }
  }, []);
  
  const login = async (email, password) => {
    try {
      const response = await fetch('/api/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ email, password })
      });
      
      const data = await response.json();
      localStorage.setItem('token', data.token);
      setUser(data.user);
      return { success: true };
    } catch (error) {
      return { success: false, error: error.message };
    }
  };
  
  const logout = () => {
    localStorage.removeItem('token');
    setUser(null);
  };
  
  const value = {
    user,
    login,
    logout,
    loading,
    isAuthenticated: !!user
  };
  
  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
}

// Custom hook for using auth context
export function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
}

// Components using auth
function LoginForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const { login } = useAuth();
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    const result = await login(email, password);
    if (!result.success) {
      alert(result.error);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
      />
      <input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        placeholder="Password"
      />
      <button type="submit">Login</button>
    </form>
  );
}

function UserProfile() {
  const { user, logout, isAuthenticated } = useAuth();
  
  if (!isAuthenticated) {
    return <div>Please log in</div>;
  }
  
  return (
    <div>
      <h1>Welcome, {user.name}!</h1>
      <p>Email: {user.email}</p>
      <button onClick={logout}>Logout</button>
    </div>
  );
}

function App() {
  const { loading } = useAuth();
  
  if (loading) {
    return <div>Loading...</div>;
  }
  
  return (
    <div>
      <UserProfile />
      <LoginForm />
    </div>
  );
}

// Root component
function Root() {
  return (
    <AuthProvider>
      <App />
    </AuthProvider>
  );
}
```

## Multiple Contexts

You can use multiple contexts in your application:

```jsx
// Different contexts for different concerns
const ThemeContext = createContext();
const LanguageContext = createContext();
const UserContext = createContext();

function App() {
  return (
    <ThemeProvider>
      <LanguageProvider>
        <UserProvider>
          <MainContent />
        </UserProvider>
      </LanguageProvider>
    </ThemeProvider>
  );
}

// Component using multiple contexts
function Header() {
  const { theme } = useContext(ThemeContext);
  const { language } = useContext(LanguageContext);
  const { user } = useContext(UserContext);
  
  return (
    <header className={theme}>
      <h1>{translations[language].welcome}, {user.name}!</h1>
    </header>
  );
}
```

## Shopping Cart Context

```jsx
const CartContext = createContext();

function CartProvider({ children }) {
  const [items, setItems] = useState([]);
  
  const addItem = (product) => {
    setItems(prevItems => {
      const existingItem = prevItems.find(item => item.id === product.id);
      
      if (existingItem) {
        return prevItems.map(item =>
          item.id === product.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      
      return [...prevItems, { ...product, quantity: 1 }];
    });
  };
  
  const removeItem = (productId) => {
    setItems(prevItems => prevItems.filter(item => item.id !== productId));
  };
  
  const updateQuantity = (productId, quantity) => {
    if (quantity <= 0) {
      removeItem(productId);
      return;
    }
    
    setItems(prevItems =>
      prevItems.map(item =>
        item.id === productId ? { ...item, quantity } : item
      )
    );
  };
  
  const clearCart = () => setItems([]);
  
  const totalItems = items.reduce((sum, item) => sum + item.quantity, 0);
  const totalPrice = items.reduce(
    (sum, item) => sum + item.price * item.quantity,
    0
  );
  
  const value = {
    items,
    addItem,
    removeItem,
    updateQuantity,
    clearCart,
    totalItems,
    totalPrice
  };
  
  return (
    <CartContext.Provider value={value}>
      {children}
    </CartContext.Provider>
  );
}

function useCart() {
  const context = useContext(CartContext);
  if (!context) {
    throw new Error('useCart must be used within CartProvider');
  }
  return context;
}

// Using the cart
function ProductCard({ product }) {
  const { addItem } = useCart();
  
  return (
    <div>
      <h3>{product.name}</h3>
      <p>${product.price}</p>
      <button onClick={() => addItem(product)}>Add to Cart</button>
    </div>
  );
}

function CartSummary() {
  const { items, totalItems, totalPrice, removeItem } = useCart();
  
  return (
    <div>
      <h2>Cart ({totalItems} items)</h2>
      {items.map(item => (
        <div key={item.id}>
          <span>{item.name} x {item.quantity}</span>
          <span>${item.price * item.quantity}</span>
          <button onClick={() => removeItem(item.id)}>Remove</button>
        </div>
      ))}
      <h3>Total: ${totalPrice.toFixed(2)}</h3>
    </div>
  );
}
```

## Performance Optimization

### Context Value Optimization
```jsx
// ❌ Bad - Creates new object on every render
function BadProvider({ children }) {
  const [state, setState] = useState();
  
  return (
    <Context.Provider value={{ state, setState }}>
      {children}
    </Context.Provider>
  );
}

// ✅ Good - Memoize the value
function GoodProvider({ children }) {
  const [state, setState] = useState();
  
  const value = useMemo(
    () => ({ state, setState }),
    [state]
  );
  
  return (
    <Context.Provider value={value}>
      {children}
    </Context.Provider>
  );
}
```

### Splitting Contexts
```jsx
// Instead of one large context
const AppContext = createContext();

// Split into multiple focused contexts
const UserContext = createContext();
const ThemeContext = createContext();
const SettingsContext = createContext();

// This way, components only re-render when their specific context changes
```

## Context with Reducer

Combining useContext with useReducer for complex state:

```jsx
const StateContext = createContext();
const DispatchContext = createContext();

function reducer(state, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    case 'DECREMENT':
      return { ...state, count: state.count - 1 };
    case 'SET_USER':
      return { ...state, user: action.payload };
    default:
      throw new Error(`Unknown action: ${action.type}`);
  }
}

function AppProvider({ children }) {
  const [state, dispatch] = useReducer(reducer, {
    count: 0,
    user: null
  });
  
  return (
    <StateContext.Provider value={state}>
      <DispatchContext.Provider value={dispatch}>
        {children}
      </DispatchContext.Provider>
    </StateContext.Provider>
  );
}

function useAppState() {
  const context = useContext(StateContext);
  if (!context) {
    throw new Error('useAppState must be used within AppProvider');
  }
  return context;
}

function useAppDispatch() {
  const context = useContext(DispatchContext);
  if (!context) {
    throw new Error('useAppDispatch must be used within AppProvider');
  }
  return context;
}

// Using the context
function Counter() {
  const { count } = useAppState();
  const dispatch = useAppDispatch();
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>+</button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>-</button>
    </div>
  );
}
```

## Best Practices

1. **Don't overuse context** - Not everything needs to be in context
2. **Split contexts** by concern to minimize re-renders
3. **Memoize context values** to prevent unnecessary re-renders
4. **Create custom hooks** for consuming context
5. **Provide good default values** or handle null cases
6. **Keep context close** to where it's used
7. **Document context shape** with TypeScript or comments

## When to Use Context

### Good Use Cases:
- Theme/appearance settings
- User authentication state
- Language/locale preferences
- Shopping cart data
- App-wide configuration

### When NOT to Use Context:
- Frequently changing values (causes many re-renders)
- Very local state (just lift state up instead)
- Complex state with many consumers (consider state management library)

## Common Patterns

### Protected Routes
```jsx
function ProtectedRoute({ children }) {
  const { isAuthenticated } = useAuth();
  const navigate = useNavigate();
  
  useEffect(() => {
    if (!isAuthenticated) {
      navigate('/login');
    }
  }, [isAuthenticated, navigate]);
  
  return isAuthenticated ? children : null;
}
```

### Feature Flags
```jsx
const FeatureFlagsContext = createContext();

function FeatureFlagsProvider({ children }) {
  const [flags, setFlags] = useState({
    newUI: false,
    betaFeatures: false,
    darkMode: true
  });
  
  return (
    <FeatureFlagsContext.Provider value={flags}>
      {children}
    </FeatureFlagsContext.Provider>
  );
}

function useFeatureFlag(flagName) {
  const flags = useContext(FeatureFlagsContext);
  return flags[flagName] ?? false;
}

// Usage
function NewFeature() {
  const isEnabled = useFeatureFlag('betaFeatures');
  
  if (!isEnabled) return null;
  
  return <div>Beta Feature!</div>;
}
```

useContext is a powerful tool for avoiding prop drilling and sharing state across your React application. Use it wisely to keep your code clean and maintainable!