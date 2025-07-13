# Render Props Pattern

The Render Props pattern is a technique for sharing code between React components using a prop whose value is a function. It's a way to make components more reusable by allowing the consumer to control what gets rendered.

## Basic Render Props

### Simple Mouse Tracker
```jsx
// Component that tracks mouse position
function MouseTracker({ render }) {
  const [position, setPosition] = useState({ x: 0, y: 0 });
  
  useEffect(() => {
    function handleMouseMove(event) {
      setPosition({
        x: event.clientX,
        y: event.clientY
      });
    }
    
    document.addEventListener('mousemove', handleMouseMove);
    
    return () => {
      document.removeEventListener('mousemove', handleMouseMove);
    };
  }, []);
  
  return render(position);
}

// Usage
function App() {
  return (
    <div>
      <MouseTracker
        render={({ x, y }) => (
          <h1>Mouse position: {x}, {y}</h1>
        )}
      />
      
      <MouseTracker
        render={({ x, y }) => (
          <div
            style={{
              position: 'absolute',
              left: x,
              top: y,
              width: 10,
              height: 10,
              backgroundColor: 'red',
              borderRadius: '50%',
              transform: 'translate(-50%, -50%)'
            }}
          />
        )}
      />
    </div>
  );
}
```

### Children as Function Pattern
```jsx
// Using children prop as render function
function DataFetcher({ url, children }) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    let cancelled = false;
    
    const fetchData = async () => {
      try {
        setLoading(true);
        setError(null);
        
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
  }, [url]);
  
  return children({ data, loading, error });
}

// Usage
function UserList() {
  return (
    <DataFetcher url="/api/users">
      {({ data: users, loading, error }) => {
        if (loading) return <div>Loading users...</div>;
        if (error) return <div>Error: {error}</div>;
        if (!users) return <div>No users found</div>;
        
        return (
          <ul>
            {users.map(user => (
              <li key={user.id}>{user.name}</li>
            ))}
          </ul>
        );
      }}
    </DataFetcher>
  );
}
```

## Form Management with Render Props

### Generic Form Handler
```jsx
function Form({ initialValues = {}, onSubmit, children }) {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);
  
  const handleChange = (name, value) => {
    setValues(prev => ({ ...prev, [name]: value }));
    
    // Clear error when user starts typing
    if (errors[name]) {
      setErrors(prev => ({ ...prev, [name]: '' }));
    }
  };
  
  const handleBlur = (name) => {
    setTouched(prev => ({ ...prev, [name]: true }));
  };
  
  const setFieldError = (name, error) => {
    setErrors(prev => ({ ...prev, [name]: error }));
  };
  
  const handleSubmit = async (event) => {
    event.preventDefault();
    setIsSubmitting(true);
    
    try {
      await onSubmit(values);
    } catch (error) {
      console.error('Form submission error:', error);
    } finally {
      setIsSubmitting(false);
    }
  };
  
  const formProps = {
    values,
    errors,
    touched,
    isSubmitting,
    handleChange,
    handleBlur,
    setFieldError,
    handleSubmit
  };
  
  return (
    <form onSubmit={handleSubmit}>
      {children(formProps)}
    </form>
  );
}

// Usage
function LoginForm() {
  const handleSubmit = async (values) => {
    // Validate
    if (!values.email) throw new Error('Email is required');
    if (!values.password) throw new Error('Password is required');
    
    // Submit
    await loginUser(values);
  };
  
  return (
    <Form
      initialValues={{ email: '', password: '' }}
      onSubmit={handleSubmit}
    >
      {({ values, errors, touched, isSubmitting, handleChange, handleBlur }) => (
        <>
          <div>
            <input
              type="email"
              value={values.email}
              onChange={(e) => handleChange('email', e.target.value)}
              onBlur={() => handleBlur('email')}
              placeholder="Email"
            />
            {touched.email && errors.email && (
              <span className="error">{errors.email}</span>
            )}
          </div>
          
          <div>
            <input
              type="password"
              value={values.password}
              onChange={(e) => handleChange('password', e.target.value)}
              onBlur={() => handleBlur('password')}
              placeholder="Password"
            />
            {touched.password && errors.password && (
              <span className="error">{errors.password}</span>
            )}
          </div>
          
          <button type="submit" disabled={isSubmitting}>
            {isSubmitting ? 'Logging in...' : 'Login'}
          </button>
        </>
      )}
    </Form>
  );
}
```

## Toggle Component

### Reusable Toggle Logic
```jsx
function Toggle({ initialToggled = false, children }) {
  const [toggled, setToggled] = useState(initialToggled);
  
  const toggle = () => setToggled(prev => !prev);
  const setToggle = (value) => setToggled(value);
  
  return children({
    toggled,
    toggle,
    setToggle
  });
}

// Usage examples
function App() {
  return (
    <div>
      {/* Modal Toggle */}
      <Toggle>
        {({ toggled, toggle }) => (
          <>
            <button onClick={toggle}>
              {toggled ? 'Hide' : 'Show'} Modal
            </button>
            {toggled && (
              <div className="modal">
                <div className="modal-content">
                  <h2>Modal Content</h2>
                  <button onClick={toggle}>Close</button>
                </div>
              </div>
            )}
          </>
        )}
      </Toggle>
      
      {/* Theme Toggle */}
      <Toggle initialToggled={false}>
        {({ toggled: isDark, toggle: toggleTheme }) => (
          <div style={{
            backgroundColor: isDark ? '#333' : '#fff',
            color: isDark ? '#fff' : '#333',
            padding: '20px'
          }}>
            <button onClick={toggleTheme}>
              Switch to {isDark ? 'Light' : 'Dark'} Theme
            </button>
            <p>Current theme: {isDark ? 'Dark' : 'Light'}</p>
          </div>
        )}
      </Toggle>
      
      {/* Accordion Toggle */}
      <Toggle>
        {({ toggled: isExpanded, toggle: toggleExpanded }) => (
          <div className="accordion">
            <button onClick={toggleExpanded}>
              {isExpanded ? '▼' : '▶'} Click to expand
            </button>
            {isExpanded && (
              <div className="accordion-content">
                <p>This content is now visible!</p>
              </div>
            )}
          </div>
        )}
      </Toggle>
    </div>
  );
}
```

## Data Loading with Render Props

### Async Data Component
```jsx
function AsyncData({ 
  promise, 
  deps = [], 
  children,
  loadingComponent = <div>Loading...</div>,
  errorComponent = null
}) {
  const [state, setState] = useState({
    data: null,
    loading: true,
    error: null
  });
  
  useEffect(() => {
    let cancelled = false;
    
    setState({ data: null, loading: true, error: null });
    
    Promise.resolve(promise)
      .then(data => {
        if (!cancelled) {
          setState({ data, loading: false, error: null });
        }
      })
      .catch(error => {
        if (!cancelled) {
          setState({ data: null, loading: false, error });
        }
      });
    
    return () => {
      cancelled = true;
    };
  }, deps);
  
  const refetch = () => {
    setState(prev => ({ ...prev, loading: true, error: null }));
    
    Promise.resolve(promise)
      .then(data => setState({ data, loading: false, error: null }))
      .catch(error => setState({ data: null, loading: false, error }));
  };
  
  if (state.loading) {
    return loadingComponent;
  }
  
  if (state.error) {
    if (errorComponent) {
      return errorComponent;
    }
    return (
      <div>
        <p>Error: {state.error.message}</p>
        <button onClick={refetch}>Retry</button>
      </div>
    );
  }
  
  return children({ data: state.data, refetch });
}

// Usage
function UserProfile({ userId }) {
  return (
    <AsyncData
      promise={fetchUser(userId)}
      deps={[userId]}
      loadingComponent={<div>Loading user profile...</div>}
    >
      {({ data: user, refetch }) => (
        <div>
          <h1>{user.name}</h1>
          <p>Email: {user.email}</p>
          <p>Joined: {new Date(user.joinDate).toLocaleDateString()}</p>
          <button onClick={refetch}>Refresh</button>
        </div>
      )}
    </AsyncData>
  );
}

// Multiple async operations
function Dashboard() {
  return (
    <div>
      <AsyncData promise={fetchUser()}>
        {({ data: user }) => (
          <div>
            <h1>Welcome, {user.name}!</h1>
            
            <AsyncData promise={fetchPosts(user.id)} deps={[user.id]}>
              {({ data: posts }) => (
                <div>
                  <h2>Your Posts</h2>
                  {posts.map(post => (
                    <article key={post.id}>
                      <h3>{post.title}</h3>
                      <p>{post.excerpt}</p>
                    </article>
                  ))}
                </div>
              )}
            </AsyncData>
          </div>
        )}
      </AsyncData>
    </div>
  );
}
```

## Intersection Observer

### Visibility Detection
```jsx
function IntersectionObserver({ 
  children, 
  threshold = 0.1, 
  rootMargin = '0px',
  triggerOnce = false 
}) {
  const [isIntersecting, setIsIntersecting] = useState(false);
  const [hasTriggered, setHasTriggered] = useState(false);
  const targetRef = useRef();
  
  useEffect(() => {
    const observer = new window.IntersectionObserver(
      ([entry]) => {
        const isVisible = entry.isIntersecting;
        setIsIntersecting(isVisible);
        
        if (isVisible && triggerOnce && !hasTriggered) {
          setHasTriggered(true);
        }
      },
      { threshold, rootMargin }
    );
    
    if (targetRef.current) {
      observer.observe(targetRef.current);
    }
    
    return () => {
      if (targetRef.current) {
        observer.unobserve(targetRef.current);
      }
    };
  }, [threshold, rootMargin, triggerOnce, hasTriggered]);
  
  const shouldShow = triggerOnce ? (isIntersecting || hasTriggered) : isIntersecting;
  
  return children({
    isIntersecting: shouldShow,
    targetRef
  });
}

// Usage for lazy loading
function LazyImage({ src, alt }) {
  return (
    <IntersectionObserver triggerOnce>
      {({ isIntersecting, targetRef }) => (
        <div ref={targetRef} style={{ minHeight: '200px' }}>
          {isIntersecting ? (
            <img src={src} alt={alt} />
          ) : (
            <div>Loading image...</div>
          )}
        </div>
      )}
    </IntersectionObserver>
  );
}

// Usage for animations
function AnimateOnScroll({ children }) {
  return (
    <IntersectionObserver threshold={0.5}>
      {({ isIntersecting, targetRef }) => (
        <div
          ref={targetRef}
          style={{
            opacity: isIntersecting ? 1 : 0,
            transform: isIntersecting ? 'translateY(0)' : 'translateY(20px)',
            transition: 'opacity 0.5s, transform 0.5s'
          }}
        >
          {children}
        </div>
      )}
    </IntersectionObserver>
  );
}
```

## Window Dimensions

### Responsive Render Props
```jsx
function WindowDimensions({ children }) {
  const [dimensions, setDimensions] = useState({
    width: window.innerWidth,
    height: window.innerHeight
  });
  
  useEffect(() => {
    function handleResize() {
      setDimensions({
        width: window.innerWidth,
        height: window.innerHeight
      });
    }
    
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);
  
  const isMobile = dimensions.width <= 768;
  const isTablet = dimensions.width > 768 && dimensions.width <= 1024;
  const isDesktop = dimensions.width > 1024;
  
  return children({
    ...dimensions,
    isMobile,
    isTablet,
    isDesktop
  });
}

// Usage
function ResponsiveLayout() {
  return (
    <WindowDimensions>
      {({ width, height, isMobile, isTablet, isDesktop }) => (
        <div>
          <p>Screen: {width} x {height}</p>
          
          {isMobile && (
            <MobileNavigation />
          )}
          
          {isTablet && (
            <TabletLayout>
              <SidebarContent />
              <MainContent />
            </TabletLayout>
          )}
          
          {isDesktop && (
            <DesktopLayout>
              <Sidebar />
              <MainContent />
              <RightPanel />
            </DesktopLayout>
          )}
        </div>
      )}
    </WindowDimensions>
  );
}
```

## Geolocation

### Location-Based Render Props
```jsx
function Geolocation({ children, options = {} }) {
  const [location, setLocation] = useState(null);
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    if (!navigator.geolocation) {
      setError(new Error('Geolocation is not supported'));
      setLoading(false);
      return;
    }
    
    const watchId = navigator.geolocation.watchPosition(
      (position) => {
        setLocation({
          latitude: position.coords.latitude,
          longitude: position.coords.longitude,
          accuracy: position.coords.accuracy,
          timestamp: position.timestamp
        });
        setError(null);
        setLoading(false);
      },
      (error) => {
        setError(error);
        setLoading(false);
      },
      {
        enableHighAccuracy: true,
        timeout: 10000,
        maximumAge: 60000,
        ...options
      }
    );
    
    return () => {
      navigator.geolocation.clearWatch(watchId);
    };
  }, []);
  
  return children({ location, error, loading });
}

// Usage
function LocationApp() {
  return (
    <Geolocation>
      {({ location, error, loading }) => {
        if (loading) return <div>Getting your location...</div>;
        if (error) return <div>Error: {error.message}</div>;
        if (!location) return <div>No location available</div>;
        
        return (
          <div>
            <h2>Your Location</h2>
            <p>Latitude: {location.latitude.toFixed(6)}</p>
            <p>Longitude: {location.longitude.toFixed(6)}</p>
            <p>Accuracy: {location.accuracy} meters</p>
            
            <MapComponent
              lat={location.latitude}
              lng={location.longitude}
            />
          </div>
        );
      }}
    </Geolocation>
  );
}
```

## Render Props Best Practices

### Performance Optimization
```jsx
// ❌ Creates new function on every render
function BadParent() {
  return (
    <DataFetcher url="/api/data">
      {({ data, loading }) => {
        if (loading) return <div>Loading...</div>;
        return <div>{data?.name}</div>;
      }}
    </DataFetcher>
  );
}

// ✅ Stable function reference
function GoodParent() {
  const renderData = useCallback(({ data, loading }) => {
    if (loading) return <div>Loading...</div>;
    return <div>{data?.name}</div>;
  }, []);
  
  return (
    <DataFetcher url="/api/data">
      {renderData}
    </DataFetcher>
  );
}

// ✅ Or extract to separate component
function DataRenderer({ data, loading }) {
  if (loading) return <div>Loading...</div>;
  return <div>{data?.name}</div>;
}

function OptimalParent() {
  return (
    <DataFetcher url="/api/data" component={DataRenderer} />
  );
}
```

### Compound Render Props
```jsx
function Modal({ children }) {
  const [isOpen, setIsOpen] = useState(false);
  
  return (
    <>
      {children({
        isOpen,
        open: () => setIsOpen(true),
        close: () => setIsOpen(false),
        toggle: () => setIsOpen(prev => !prev)
      })}
      
      {isOpen && (
        <div className="modal-overlay" onClick={() => setIsOpen(false)}>
          <div className="modal-content" onClick={e => e.stopPropagation()}>
            {children.content?.({ close: () => setIsOpen(false) })}
          </div>
        </div>
      )}
    </>
  );
}

// Usage
function App() {
  return (
    <Modal>
      {({ isOpen, open, close }) => ({
        trigger: <button onClick={open}>Open Modal</button>,
        content: ({ close }) => (
          <div>
            <h2>Modal Content</h2>
            <button onClick={close}>Close</button>
          </div>
        )
      })}
    </Modal>
  );
}
```

## When to Use Render Props

### Good Use Cases
- Sharing stateful logic between components
- Conditional rendering based on complex state
- Providing flexible rendering options
- Creating reusable UI patterns

### Alternatives to Consider
- **Custom Hooks**: Often simpler for logic sharing
- **Component Composition**: For simpler cases
- **Higher-Order Components**: For cross-cutting concerns

### Render Props vs Hooks
```jsx
// Render Props approach
<MouseTracker>
  {({ x, y }) => <div>Mouse: {x}, {y}</div>}
</MouseTracker>

// Hooks approach (often preferred)
function Component() {
  const { x, y } = useMousePosition();
  return <div>Mouse: {x}, {y}</div>;
}
```

Render props provide a powerful way to share logic and create flexible, reusable components in React!