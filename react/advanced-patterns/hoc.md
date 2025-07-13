# Higher-Order Components (HOCs)

Higher-Order Components are a pattern for reusing component logic. A HOC is a function that takes a component and returns a new component with additional functionality.

## Basic HOC Pattern

### Simple HOC Example
```jsx
// Basic HOC that adds loading functionality
function withLoading(WrappedComponent) {
  return function WithLoadingComponent(props) {
    if (props.isLoading) {
      return <div>Loading...</div>;
    }
    
    return <WrappedComponent {...props} />;
  };
}

// Component to enhance
function UserProfile({ user }) {
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
}

// Enhanced component
const UserProfileWithLoading = withLoading(UserProfile);

// Usage
function App() {
  const [user, setUser] = useState(null);
  const [isLoading, setIsLoading] = useState(true);
  
  useEffect(() => {
    fetchUser().then(userData => {
      setUser(userData);
      setIsLoading(false);
    });
  }, []);
  
  return (
    <UserProfileWithLoading 
      user={user} 
      isLoading={isLoading} 
    />
  );
}
```

## Authentication HOC

### Protecting Routes with HOC
```jsx
function withAuth(WrappedComponent, allowedRoles = []) {
  return function AuthenticatedComponent(props) {
    const [user, setUser] = useState(null);
    const [loading, setLoading] = useState(true);
    const navigate = useNavigate();
    
    useEffect(() => {
      const checkAuth = async () => {
        try {
          const token = localStorage.getItem('authToken');
          if (!token) {
            navigate('/login');
            return;
          }
          
          const userData = await verifyToken(token);
          
          // Check role-based access
          if (allowedRoles.length > 0 && !allowedRoles.includes(userData.role)) {
            navigate('/unauthorized');
            return;
          }
          
          setUser(userData);
        } catch (error) {
          navigate('/login');
        } finally {
          setLoading(false);
        }
      };
      
      checkAuth();
    }, [navigate]);
    
    if (loading) {
      return <div>Checking authentication...</div>;
    }
    
    if (!user) {
      return null; // Will redirect
    }
    
    return <WrappedComponent {...props} user={user} />;
  };
}

// Protected components
const AdminDashboard = withAuth(function AdminDashboard({ user }) {
  return (
    <div>
      <h1>Admin Dashboard</h1>
      <p>Welcome, {user.name}</p>
    </div>
  );
}, ['admin']);

const UserDashboard = withAuth(function UserDashboard({ user }) {
  return (
    <div>
      <h1>User Dashboard</h1>
      <p>Welcome, {user.name}</p>
    </div>
  );
}, ['user', 'admin']);
```

## Data Fetching HOC

### Generic Data Fetcher
```jsx
function withData(WrappedComponent, dataSource) {
  return function WithDataComponent(props) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);
    
    useEffect(() => {
      let cancelled = false;
      
      const fetchData = async () => {
        try {
          setLoading(true);
          setError(null);
          
          // dataSource can be a function or string
          const url = typeof dataSource === 'function' 
            ? dataSource(props) 
            : dataSource;
            
          const response = await fetch(url);
          if (!response.ok) throw new Error('Failed to fetch');
          
          const result = await response.json();
          
          if (!cancelled) {
            setData(result);
          }
        } catch (err) {
          if (!cancelled) {
            setError(err.message);
          }
        } finally {
          if (!cancelled) {
            setLoading(false);
          }
        }
      };
      
      fetchData();
      
      return () => {
        cancelled = true;
      };
    }, [props.userId, props.refresh]); // Dependencies based on props
    
    return (
      <WrappedComponent
        {...props}
        data={data}
        loading={loading}
        error={error}
        refetch={() => setData(null)} // Trigger refetch
      />
    );
  };
}

// Usage with different data sources
const UserProfileWithData = withData(
  function UserProfile({ data: user, loading, error }) {
    if (loading) return <div>Loading user...</div>;
    if (error) return <div>Error: {error}</div>;
    if (!user) return <div>No user found</div>;
    
    return (
      <div>
        <h1>{user.name}</h1>
        <p>{user.email}</p>
      </div>
    );
  },
  (props) => `/api/users/${props.userId}`
);

const PostListWithData = withData(
  function PostList({ data: posts, loading, error, refetch }) {
    if (loading) return <div>Loading posts...</div>;
    if (error) return <div>Error: {error}</div>;
    
    return (
      <div>
        <button onClick={refetch}>Refresh</button>
        {posts.map(post => (
          <article key={post.id}>
            <h2>{post.title}</h2>
            <p>{post.content}</p>
          </article>
        ))}
      </div>
    );
  },
  '/api/posts'
);
```

## Theme HOC

### Theming with HOC
```jsx
const ThemeContext = createContext();

function withTheme(WrappedComponent) {
  return function ThemedComponent(props) {
    const theme = useContext(ThemeContext);
    
    if (!theme) {
      throw new Error('withTheme must be used within ThemeProvider');
    }
    
    return <WrappedComponent {...props} theme={theme} />;
  };
}

// Theme provider
function ThemeProvider({ children }) {
  const [currentTheme, setCurrentTheme] = useState('light');
  
  const themes = {
    light: {
      background: '#ffffff',
      text: '#333333',
      primary: '#007bff'
    },
    dark: {
      background: '#333333',
      text: '#ffffff',
      primary: '#0d6efd'
    }
  };
  
  const value = {
    ...themes[currentTheme],
    themeName: currentTheme,
    toggleTheme: () => setCurrentTheme(prev => prev === 'light' ? 'dark' : 'light')
  };
  
  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}

// Themed components
const ThemedButton = withTheme(function Button({ theme, children, ...props }) {
  const style = {
    backgroundColor: theme.primary,
    color: theme.background,
    border: 'none',
    padding: '10px 20px',
    borderRadius: '4px',
    cursor: 'pointer'
  };
  
  return (
    <button style={style} {...props}>
      {children}
    </button>
  );
});

const ThemedCard = withTheme(function Card({ theme, children }) {
  const style = {
    backgroundColor: theme.background,
    color: theme.text,
    padding: '20px',
    borderRadius: '8px',
    boxShadow: '0 2px 4px rgba(0,0,0,0.1)'
  };
  
  return <div style={style}>{children}</div>;
});
```

## Performance HOC

### Memoization HOC
```jsx
function withMemo(WrappedComponent, areEqual) {
  const MemoizedComponent = React.memo(WrappedComponent, areEqual);
  
  // Preserve display name for debugging
  MemoizedComponent.displayName = `withMemo(${
    WrappedComponent.displayName || WrappedComponent.name || 'Component'
  })`;
  
  return MemoizedComponent;
}

// Usage
const ExpensiveComponent = withMemo(
  function ExpensiveComponent({ data, filter }) {
    console.log('ExpensiveComponent rendered');
    
    const processedData = useMemo(() => {
      return data
        .filter(item => item.category === filter)
        .sort((a, b) => b.score - a.score);
    }, [data, filter]);
    
    return (
      <div>
        {processedData.map(item => (
          <div key={item.id}>{item.name}</div>
        ))}
      </div>
    );
  },
  // Custom comparison function
  (prevProps, nextProps) => {
    return (
      prevProps.data === nextProps.data &&
      prevProps.filter === nextProps.filter
    );
  }
);
```

## Error Boundary HOC

### Error Handling with HOC
```jsx
function withErrorBoundary(WrappedComponent, errorFallback) {
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.state = { hasError: false, error: null };
    }
    
    static getDerivedStateFromError(error) {
      return { hasError: true, error };
    }
    
    componentDidCatch(error, errorInfo) {
      console.error('Error caught by HOC:', error, errorInfo);
      
      // Send to error reporting service
      if (window.Sentry) {
        window.Sentry.captureException(error);
      }
    }
    
    render() {
      if (this.state.hasError) {
        if (errorFallback) {
          return errorFallback(this.state.error, this.props);
        }
        
        return (
          <div style={{ padding: '20px', textAlign: 'center' }}>
            <h2>Something went wrong</h2>
            <details style={{ marginTop: '10px' }}>
              {this.state.error && this.state.error.toString()}
            </details>
            <button 
              onClick={() => this.setState({ hasError: false, error: null })}
              style={{ marginTop: '10px' }}
            >
              Try Again
            </button>
          </div>
        );
      }
      
      return <WrappedComponent {...this.props} />;
    }
  };
}

// Custom error fallback
const customErrorFallback = (error, props) => (
  <div style={{ background: '#ffe6e6', padding: '20px', border: '1px solid #ff0000' }}>
    <h3>Oops! Something went wrong</h3>
    <p>Error in component: {props.componentName || 'Unknown'}</p>
    <button onClick={() => window.location.reload()}>
      Reload Page
    </button>
  </div>
);

// Protected components
const SafeUserProfile = withErrorBoundary(
  function UserProfile({ user }) {
    // This might throw an error
    if (!user) throw new Error('User data is required');
    
    return <div>{user.name}</div>;
  },
  customErrorFallback
);
```

## Composing HOCs

### Combining Multiple HOCs
```jsx
// Helper function to compose HOCs
function compose(...hocs) {
  return (WrappedComponent) => {
    return hocs.reduceRight((acc, hoc) => hoc(acc), WrappedComponent);
  };
}

// Or using a library like recompose
// import { compose } from 'recompose';

// Individual HOCs
function withLoading(WrappedComponent) {
  return function WithLoadingComponent(props) {
    if (props.isLoading) return <div>Loading...</div>;
    return <WrappedComponent {...props} />;
  };
}

function withError(WrappedComponent) {
  return function WithErrorComponent(props) {
    if (props.error) return <div>Error: {props.error}</div>;
    return <WrappedComponent {...props} />;
  };
}

function withData(apiEndpoint) {
  return function(WrappedComponent) {
    return function WithDataComponent(props) {
      const [data, setData] = useState(null);
      const [loading, setLoading] = useState(true);
      const [error, setError] = useState(null);
      
      useEffect(() => {
        fetch(apiEndpoint)
          .then(response => response.json())
          .then(setData)
          .catch(setError)
          .finally(() => setLoading(false));
      }, []);
      
      return (
        <WrappedComponent
          {...props}
          data={data}
          isLoading={loading}
          error={error}
        />
      );
    };
  };
}

// Compose multiple HOCs
const enhance = compose(
  withData('/api/users'),
  withLoading,
  withError,
  withAuth(['user', 'admin'])
);

const EnhancedUserList = enhance(function UserList({ data: users }) {
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
});
```

## Advanced HOC Patterns

### HOC with Configuration
```jsx
function withAnalytics(config = {}) {
  const {
    trackClicks = true,
    trackViews = true,
    customEvents = []
  } = config;
  
  return function(WrappedComponent) {
    return function AnalyticsComponent(props) {
      const componentRef = useRef();
      
      useEffect(() => {
        if (trackViews) {
          // Track component view
          analytics.track('component_viewed', {
            component: WrappedComponent.name,
            props: Object.keys(props)
          });
        }
      }, []);
      
      const handleClick = useCallback((event) => {
        if (trackClicks) {
          analytics.track('component_clicked', {
            component: WrappedComponent.name,
            target: event.target.tagName
          });
        }
        
        // Call original onClick if it exists
        if (props.onClick) {
          props.onClick(event);
        }
      }, [props.onClick]);
      
      // Track custom events
      const customEventHandlers = customEvents.reduce((handlers, eventName) => {
        handlers[eventName] = (data) => {
          analytics.track(eventName, {
            component: WrappedComponent.name,
            ...data
          });
        };
        return handlers;
      }, {});
      
      return (
        <WrappedComponent
          ref={componentRef}
          {...props}
          onClick={handleClick}
          analytics={customEventHandlers}
        />
      );
    };
  };
}

// Usage with configuration
const TrackedButton = withAnalytics({
  trackClicks: true,
  trackViews: false,
  customEvents: ['button_hover', 'button_focus']
})(function Button({ children, analytics, ...props }) {
  return (
    <button
      {...props}
      onMouseEnter={() => analytics.button_hover?.()}
      onFocus={() => analytics.button_focus?.()}
    >
      {children}
    </button>
  );
});
```

### Dynamic HOC
```jsx
function withFeatureToggle(featureName, fallbackComponent = null) {
  return function(WrappedComponent) {
    return function FeatureToggleComponent(props) {
      const [isEnabled, setIsEnabled] = useState(false);
      const [loading, setLoading] = useState(true);
      
      useEffect(() => {
        // Check if feature is enabled
        checkFeatureFlag(featureName)
          .then(setIsEnabled)
          .finally(() => setLoading(false));
      }, []);
      
      if (loading) {
        return <div>Loading feature...</div>;
      }
      
      if (!isEnabled) {
        if (fallbackComponent) {
          return React.createElement(fallbackComponent, props);
        }
        return null;
      }
      
      return <WrappedComponent {...props} />;
    };
  };
}

// Usage
const FallbackComponent = () => <div>This feature is not available</div>;

const NewFeature = withFeatureToggle(
  'new_dashboard',
  FallbackComponent
)(function NewDashboard() {
  return <div>New Dashboard Feature!</div>;
});
```

## HOC Best Practices

### Forwarding Refs
```jsx
function withLogging(WrappedComponent) {
  const WithLoggingComponent = React.forwardRef((props, ref) => {
    useEffect(() => {
      console.log(`${WrappedComponent.name} mounted`);
      return () => console.log(`${WrappedComponent.name} unmounted`);
    }, []);
    
    return <WrappedComponent {...props} ref={ref} />;
  });
  
  // Set display name for debugging
  WithLoggingComponent.displayName = `withLogging(${
    WrappedComponent.displayName || WrappedComponent.name || 'Component'
  })`;
  
  return WithLoggingComponent;
}
```

### Static Hoisting
```jsx
import hoistNonReactStatics from 'hoist-non-react-statics';

function withEnhancement(WrappedComponent) {
  function EnhancedComponent(props) {
    return <WrappedComponent {...props} enhanced />;
  }
  
  // Copy static methods from WrappedComponent to EnhancedComponent
  hoistNonReactStatics(EnhancedComponent, WrappedComponent);
  
  return EnhancedComponent;
}
```

## When to Use HOCs

### Good Use Cases
- Cross-cutting concerns (auth, logging, analytics)
- Code reuse across multiple components
- Conditional rendering based on external state
- Adding lifecycle methods to functional components (before hooks)

### When to Avoid
- Simple prop manipulation (use regular components)
- When hooks can solve the problem more elegantly
- Deep nesting of HOCs (prefer composition)
- Performance-critical components (hooks are often better)

### HOCs vs Hooks
```jsx
// HOC approach
const withCounter = (WrappedComponent) => {
  return function WithCounterComponent(props) {
    const [count, setCount] = useState(0);
    return <WrappedComponent {...props} count={count} setCount={setCount} />;
  };
};

// Hook approach (often preferred)
function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);
  return [count, setCount];
}
```

HOCs are a powerful pattern for sharing logic between components. While hooks have replaced many HOC use cases, HOCs are still valuable for certain scenarios!