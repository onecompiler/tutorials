# Protected Routes in React Router

Protected routes are routes that require authentication or specific permissions to access. This pattern is essential for building secure applications with private areas like user dashboards, admin panels, or premium content.

## Basic Protected Route

### Simple Authentication Check
```jsx
import { Navigate } from 'react-router-dom';

function ProtectedRoute({ children }) {
  const isAuthenticated = localStorage.getItem('authToken');
  
  if (!isAuthenticated) {
    // Redirect to login if not authenticated
    return <Navigate to="/login" replace />;
  }
  
  return children;
}

// Usage
function App() {
  return (
    <Routes>
      <Route path="/login" element={<Login />} />
      <Route
        path="/dashboard"
        element={
          <ProtectedRoute>
            <Dashboard />
          </ProtectedRoute>
        }
      />
    </Routes>
  );
}
```

### With Authentication Context
```jsx
// AuthContext.js
const AuthContext = createContext(null);

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    // Check if user is logged in
    const token = localStorage.getItem('authToken');
    if (token) {
      fetchUser(token)
        .then(setUser)
        .catch(() => localStorage.removeItem('authToken'))
        .finally(() => setLoading(false));
    } else {
      setLoading(false);
    }
  }, []);
  
  const login = async (credentials) => {
    const response = await fetch('/api/login', {
      method: 'POST',
      body: JSON.stringify(credentials)
    });
    
    if (response.ok) {
      const { user, token } = await response.json();
      localStorage.setItem('authToken', token);
      setUser(user);
      return { success: true };
    }
    
    return { success: false, error: 'Invalid credentials' };
  };
  
  const logout = () => {
    localStorage.removeItem('authToken');
    setUser(null);
  };
  
  return (
    <AuthContext.Provider value={{ user, login, logout, loading }}>
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

// ProtectedRoute component
function ProtectedRoute({ children }) {
  const { user, loading } = useAuth();
  const location = useLocation();
  
  if (loading) {
    return <div>Loading...</div>;
  }
  
  if (!user) {
    // Save the location they were trying to go to
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  
  return children;
}
```

## Role-Based Protection

### Simple Role Check
```jsx
function RequireRole({ children, allowedRoles }) {
  const { user } = useAuth();
  const location = useLocation();
  
  if (!user) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  
  if (!allowedRoles.includes(user.role)) {
    return <Navigate to="/unauthorized" replace />;
  }
  
  return children;
}

// Usage
<Route
  path="/admin"
  element={
    <RequireRole allowedRoles={['admin']}>
      <AdminDashboard />
    </RequireRole>
  }
/>

<Route
  path="/moderator"
  element={
    <RequireRole allowedRoles={['admin', 'moderator']}>
      <ModeratorPanel />
    </RequireRole>
  }
/>
```

### Permission-Based Protection
```jsx
function RequirePermission({ children, permissions }) {
  const { user } = useAuth();
  
  if (!user) {
    return <Navigate to="/login" replace />;
  }
  
  const hasPermission = permissions.every(permission =>
    user.permissions?.includes(permission)
  );
  
  if (!hasPermission) {
    return (
      <div>
        <h1>Access Denied</h1>
        <p>You don't have permission to view this page.</p>
        <Link to="/">Go to Home</Link>
      </div>
    );
  }
  
  return children;
}

// Usage
<Route
  path="/users/manage"
  element={
    <RequirePermission permissions={['users.read', 'users.write']}>
      <UserManagement />
    </RequirePermission>
  }
/>
```

## Advanced Protection Patterns

### Outlet-Based Protection
```jsx
function ProtectedLayout() {
  const { user, loading } = useAuth();
  
  if (loading) {
    return <LoadingSpinner />;
  }
  
  if (!user) {
    return <Navigate to="/login" replace />;
  }
  
  return (
    <div className="protected-layout">
      <Header user={user} />
      <Sidebar />
      <main>
        <Outlet />
      </main>
    </div>
  );
}

// Routes configuration
<Routes>
  <Route path="/login" element={<Login />} />
  <Route element={<ProtectedLayout />}>
    <Route path="/dashboard" element={<Dashboard />} />
    <Route path="/profile" element={<Profile />} />
    <Route path="/settings" element={<Settings />} />
  </Route>
</Routes>
```

### Multi-Level Protection
```jsx
function App() {
  return (
    <Routes>
      {/* Public routes */}
      <Route path="/" element={<Home />} />
      <Route path="/login" element={<Login />} />
      <Route path="/register" element={<Register />} />
      
      {/* Protected routes - requires authentication */}
      <Route element={<RequireAuth />}>
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/profile" element={<Profile />} />
        
        {/* Admin routes - requires admin role */}
        <Route element={<RequireRole allowedRoles={['admin']} />}>
          <Route path="/admin" element={<AdminDashboard />} />
          <Route path="/admin/users" element={<UserManagement />} />
          <Route path="/admin/settings" element={<AdminSettings />} />
        </Route>
        
        {/* Moderator routes */}
        <Route element={<RequireRole allowedRoles={['admin', 'moderator']} />}>
          <Route path="/moderate" element={<ModerationPanel />} />
          <Route path="/moderate/reports" element={<Reports />} />
        </Route>
      </Route>
      
      {/* Catch all */}
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
}
```

## Redirect After Login

### Preserving Intended Destination
```jsx
function Login() {
  const navigate = useNavigate();
  const location = useLocation();
  const { login } = useAuth();
  
  const [credentials, setCredentials] = useState({ email: '', password: '' });
  const [error, setError] = useState('');
  
  const from = location.state?.from?.pathname || '/dashboard';
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    setError('');
    
    const result = await login(credentials);
    
    if (result.success) {
      // Redirect to the page they tried to visit
      navigate(from, { replace: true });
    } else {
      setError(result.error);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <h1>Login</h1>
      {location.state?.from && (
        <p>You must log in to view {from}</p>
      )}
      
      {error && <div className="error">{error}</div>}
      
      <input
        type="email"
        value={credentials.email}
        onChange={(e) => setCredentials({...credentials, email: e.target.value})}
        placeholder="Email"
        required
      />
      
      <input
        type="password"
        value={credentials.password}
        onChange={(e) => setCredentials({...credentials, password: e.target.value})}
        placeholder="Password"
        required
      />
      
      <button type="submit">Login</button>
    </form>
  );
}
```

## Loading States and Suspense

### Async Authentication Check
```jsx
function ProtectedRoute({ children }) {
  const { user, loading } = useAuth();
  const location = useLocation();
  
  if (loading) {
    return (
      <div className="loading-container">
        <Spinner />
        <p>Checking authentication...</p>
      </div>
    );
  }
  
  if (!user) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  
  return children;
}
```

### With React Suspense
```jsx
const Dashboard = lazy(() => import('./pages/Dashboard'));

function ProtectedRoute({ children }) {
  const { user } = useAuth();
  
  if (!user) {
    return <Navigate to="/login" replace />;
  }
  
  return (
    <Suspense fallback={<LoadingSpinner />}>
      {children}
    </Suspense>
  );
}
```

## Session Management

### Session Timeout
```jsx
function SessionProvider({ children }) {
  const [user, setUser] = useState(null);
  const [lastActivity, setLastActivity] = useState(Date.now());
  const navigate = useNavigate();
  
  const SESSION_TIMEOUT = 30 * 60 * 1000; // 30 minutes
  
  // Reset activity timer on user interaction
  useEffect(() => {
    const updateActivity = () => setLastActivity(Date.now());
    
    window.addEventListener('click', updateActivity);
    window.addEventListener('keypress', updateActivity);
    
    return () => {
      window.removeEventListener('click', updateActivity);
      window.removeEventListener('keypress', updateActivity);
    };
  }, []);
  
  // Check for session timeout
  useEffect(() => {
    const interval = setInterval(() => {
      if (user && Date.now() - lastActivity > SESSION_TIMEOUT) {
        // Session expired
        logout();
        navigate('/login', {
          state: { message: 'Your session has expired. Please login again.' }
        });
      }
    }, 60000); // Check every minute
    
    return () => clearInterval(interval);
  }, [user, lastActivity, navigate]);
  
  const logout = () => {
    setUser(null);
    localStorage.removeItem('authToken');
  };
  
  return (
    <AuthContext.Provider value={{ user, setUser, logout }}>
      {children}
    </AuthContext.Provider>
  );
}
```

## Route Guards

### Custom Route Guard Hook
```jsx
function useRouteGuard(guardFn, redirectTo = '/') {
  const navigate = useNavigate();
  const location = useLocation();
  
  useEffect(() => {
    const checkGuard = async () => {
      const canAccess = await guardFn();
      
      if (!canAccess) {
        navigate(redirectTo, {
          state: { from: location },
          replace: true
        });
      }
    };
    
    checkGuard();
  }, [guardFn, navigate, redirectTo, location]);
}

// Usage
function PremiumContent() {
  useRouteGuard(
    async () => {
      const user = await getCurrentUser();
      return user?.subscription === 'premium';
    },
    '/upgrade'
  );
  
  return <div>Premium content here</div>;
}
```

### Multiple Guards
```jsx
function GuardedRoute({ children, guards = [] }) {
  const [canAccess, setCanAccess] = useState(null);
  const [guardError, setGuardError] = useState(null);
  
  useEffect(() => {
    const checkGuards = async () => {
      try {
        for (const guard of guards) {
          const result = await guard();
          if (!result.success) {
            setGuardError(result);
            setCanAccess(false);
            return;
          }
        }
        setCanAccess(true);
      } catch (error) {
        setGuardError({ message: 'An error occurred' });
        setCanAccess(false);
      }
    };
    
    checkGuards();
  }, [guards]);
  
  if (canAccess === null) {
    return <LoadingSpinner />;
  }
  
  if (!canAccess) {
    return <Navigate to={guardError.redirectTo || '/login'} replace />;
  }
  
  return children;
}

// Guards
const isAuthenticated = async () => {
  const token = localStorage.getItem('authToken');
  return {
    success: !!token,
    redirectTo: '/login'
  };
};

const hasSubscription = async () => {
  const user = await getCurrentUser();
  return {
    success: user?.hasActiveSubscription,
    redirectTo: '/subscribe'
  };
};

const isEmailVerified = async () => {
  const user = await getCurrentUser();
  return {
    success: user?.emailVerified,
    redirectTo: '/verify-email'
  };
};

// Usage
<Route
  path="/premium-features"
  element={
    <GuardedRoute guards={[isAuthenticated, hasSubscription, isEmailVerified]}>
      <PremiumFeatures />
    </GuardedRoute>
  }
/>
```

## Complete Example

### Full Authentication System
```jsx
// AuthContext.js
const AuthContext = createContext(null);

export function AuthProvider({ children }) {
  const [state, setState] = useState({
    user: null,
    loading: true,
    error: null
  });
  
  useEffect(() => {
    checkAuth();
  }, []);
  
  const checkAuth = async () => {
    try {
      const token = localStorage.getItem('authToken');
      if (token) {
        const user = await fetchUser(token);
        setState({ user, loading: false, error: null });
      } else {
        setState({ user: null, loading: false, error: null });
      }
    } catch (error) {
      setState({ user: null, loading: false, error: error.message });
      localStorage.removeItem('authToken');
    }
  };
  
  const value = {
    ...state,
    login: async (credentials) => {
      try {
        const { user, token } = await loginAPI(credentials);
        localStorage.setItem('authToken', token);
        setState({ user, loading: false, error: null });
        return { success: true };
      } catch (error) {
        return { success: false, error: error.message };
      }
    },
    logout: () => {
      localStorage.removeItem('authToken');
      setState({ user: null, loading: false, error: null });
    },
    checkAuth
  };
  
  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
}

// ProtectedRoute.js
export function ProtectedRoute({ 
  children, 
  requireAuth = true,
  requireRoles = [],
  requirePermissions = [],
  fallback = '/login'
}) {
  const { user, loading } = useAuth();
  const location = useLocation();
  
  if (loading) {
    return <LoadingSpinner />;
  }
  
  if (requireAuth && !user) {
    return <Navigate to={fallback} state={{ from: location }} replace />;
  }
  
  if (requireRoles.length > 0 && !requireRoles.includes(user?.role)) {
    return <Navigate to="/unauthorized" replace />;
  }
  
  if (requirePermissions.length > 0) {
    const hasPermissions = requirePermissions.every(
      permission => user?.permissions?.includes(permission)
    );
    
    if (!hasPermissions) {
      return <Navigate to="/forbidden" replace />;
    }
  }
  
  return children;
}

// App.js
function App() {
  return (
    <AuthProvider>
      <BrowserRouter>
        <Routes>
          {/* Public routes */}
          <Route path="/login" element={<Login />} />
          <Route path="/register" element={<Register />} />
          <Route path="/forgot-password" element={<ForgotPassword />} />
          
          {/* Protected routes */}
          <Route
            path="/dashboard"
            element={
              <ProtectedRoute>
                <Dashboard />
              </ProtectedRoute>
            }
          />
          
          {/* Admin routes */}
          <Route
            path="/admin/*"
            element={
              <ProtectedRoute requireRoles={['admin']}>
                <AdminLayout />
              </ProtectedRoute>
            }
          >
            <Route index element={<AdminDashboard />} />
            <Route path="users" element={<UserManagement />} />
            <Route path="settings" element={<AdminSettings />} />
          </Route>
          
          {/* Error pages */}
          <Route path="/unauthorized" element={<Unauthorized />} />
          <Route path="/forbidden" element={<Forbidden />} />
          <Route path="*" element={<NotFound />} />
        </Routes>
      </BrowserRouter>
    </AuthProvider>
  );
}
```

## Best Practices

1. **Always validate on the server** - Client-side protection is not enough
2. **Handle loading states** - Show spinner while checking auth
3. **Preserve navigation intent** - Redirect to intended page after login
4. **Clear error messages** - Explain why access was denied
5. **Implement session timeout** - For security
6. **Use HTTPS** - Always use secure connections
7. **Store tokens securely** - Consider httpOnly cookies
8. **Refresh tokens** - Implement token refresh mechanism

Protected routes are crucial for application security. Implement them carefully to balance security with user experience!