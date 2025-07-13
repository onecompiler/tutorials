# Navigation in React Router

Navigation is a core feature of React Router, enabling users to move between different pages and components in your application. This tutorial covers various navigation techniques and patterns.

## Link Component

The Link component is the primary way to navigate between routes in React Router.

### Basic Link Usage
```jsx
import { Link } from 'react-router-dom';

function Navigation() {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
      <Link to="/contact">Contact</Link>
      
      {/* Link with object */}
      <Link 
        to={{
          pathname: "/products",
          search: "?sort=name",
          hash: "#top"
        }}
      >
        Products
      </Link>
    </nav>
  );
}
```

### Link with State
```jsx
function ProductList() {
  const products = [
    { id: 1, name: 'Laptop', price: 999 },
    { id: 2, name: 'Phone', price: 699 }
  ];
  
  return (
    <div>
      {products.map(product => (
        <Link
          key={product.id}
          to={`/products/${product.id}`}
          state={{ product }}
        >
          {product.name}
        </Link>
      ))}
    </div>
  );
}

// Accessing state in target component
function ProductDetail() {
  const location = useLocation();
  const { product } = location.state || {};
  
  return (
    <div>
      {product ? (
        <>
          <h1>{product.name}</h1>
          <p>Price: ${product.price}</p>
        </>
      ) : (
        <p>Product not found</p>
      )}
    </div>
  );
}
```

## NavLink Component

NavLink provides additional styling capabilities for active links.

### Active Styling
```jsx
import { NavLink } from 'react-router-dom';

function Navigation() {
  return (
    <nav>
      <NavLink
        to="/"
        className={({ isActive }) => 
          isActive ? "nav-link active" : "nav-link"
        }
      >
        Home
      </NavLink>
      
      <NavLink
        to="/about"
        style={({ isActive }) => ({
          color: isActive ? '#007bff' : '#333',
          fontWeight: isActive ? 'bold' : 'normal',
          textDecoration: isActive ? 'underline' : 'none'
        })}
      >
        About
      </NavLink>
      
      {/* With custom active check */}
      <NavLink
        to="/messages"
        className={({ isActive, isPending }) => 
          isPending ? "pending" : isActive ? "active" : ""
        }
      >
        Messages
      </NavLink>
    </nav>
  );
}
```

### NavLink with Children Function
```jsx
function Navigation() {
  return (
    <nav>
      <NavLink to="/dashboard">
        {({ isActive }) => (
          <span className={isActive ? "active-icon" : ""}>
            {isActive ? "📊" : "📈"} Dashboard
          </span>
        )}
      </NavLink>
    </nav>
  );
}
```

## Programmatic Navigation

### useNavigate Hook
```jsx
import { useNavigate } from 'react-router-dom';

function LoginForm() {
  const navigate = useNavigate();
  const [credentials, setCredentials] = useState({ username: '', password: '' });
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    try {
      const user = await login(credentials);
      
      // Navigate to dashboard after successful login
      navigate('/dashboard');
      
      // Navigate with state
      navigate('/dashboard', { 
        state: { message: 'Login successful!' }
      });
      
      // Replace current history entry
      navigate('/dashboard', { replace: true });
      
    } catch (error) {
      console.error('Login failed:', error);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      {/* Form fields */}
      <button type="submit">Login</button>
      <button type="button" onClick={() => navigate(-1)}>
        Cancel
      </button>
    </form>
  );
}
```

### Navigate Component
```jsx
import { Navigate } from 'react-router-dom';

function ProtectedRoute({ isAuthenticated, children }) {
  if (!isAuthenticated) {
    // Redirect to login
    return <Navigate to="/login" replace />;
  }
  
  return children;
}

// Conditional navigation
function Profile({ user }) {
  // Redirect if no user
  if (!user) {
    return <Navigate to="/login" state={{ from: '/profile' }} />;
  }
  
  return <div>Welcome, {user.name}!</div>;
}
```

## Advanced Navigation Patterns

### Breadcrumb Navigation
```jsx
import { Link, useLocation, useMatches } from 'react-router-dom';

function Breadcrumbs() {
  const location = useLocation();
  const matches = useMatches();
  
  const crumbs = matches
    .filter(match => match.handle?.crumb)
    .map(match => ({
      path: match.pathname,
      label: match.handle.crumb(match.data)
    }));
  
  return (
    <nav aria-label="breadcrumb">
      <ol>
        <li>
          <Link to="/">Home</Link>
        </li>
        {crumbs.map((crumb, index) => (
          <li key={crumb.path}>
            {index === crumbs.length - 1 ? (
              <span>{crumb.label}</span>
            ) : (
              <Link to={crumb.path}>{crumb.label}</Link>
            )}
          </li>
        ))}
      </ol>
    </nav>
  );
}
```

### Tab Navigation
```jsx
function TabNavigation() {
  const location = useLocation();
  
  const tabs = [
    { path: '/settings/profile', label: 'Profile' },
    { path: '/settings/security', label: 'Security' },
    { path: '/settings/notifications', label: 'Notifications' }
  ];
  
  return (
    <div>
      <nav className="tabs">
        {tabs.map(tab => (
          <NavLink
            key={tab.path}
            to={tab.path}
            className={({ isActive }) =>
              `tab ${isActive ? 'tab-active' : ''}`
            }
          >
            {tab.label}
          </NavLink>
        ))}
      </nav>
      
      <Outlet />
    </div>
  );
}
```

### Wizard Navigation
```jsx
function WizardNavigation() {
  const navigate = useNavigate();
  const location = useLocation();
  
  const steps = [
    { path: '/wizard/step1', label: 'Personal Info' },
    { path: '/wizard/step2', label: 'Contact Details' },
    { path: '/wizard/step3', label: 'Confirmation' }
  ];
  
  const currentStepIndex = steps.findIndex(
    step => step.path === location.pathname
  );
  
  const goToNextStep = () => {
    if (currentStepIndex < steps.length - 1) {
      navigate(steps[currentStepIndex + 1].path);
    }
  };
  
  const goToPreviousStep = () => {
    if (currentStepIndex > 0) {
      navigate(steps[currentStepIndex - 1].path);
    }
  };
  
  return (
    <div>
      <div className="wizard-steps">
        {steps.map((step, index) => (
          <div
            key={step.path}
            className={`step ${
              index === currentStepIndex ? 'active' :
              index < currentStepIndex ? 'completed' : ''
            }`}
          >
            {index < currentStepIndex ? (
              <Link to={step.path}>{step.label}</Link>
            ) : (
              <span>{step.label}</span>
            )}
          </div>
        ))}
      </div>
      
      <Outlet />
      
      <div className="wizard-navigation">
        <button
          onClick={goToPreviousStep}
          disabled={currentStepIndex === 0}
        >
          Previous
        </button>
        <button
          onClick={goToNextStep}
          disabled={currentStepIndex === steps.length - 1}
        >
          Next
        </button>
      </div>
    </div>
  );
}
```

## Navigation with Animation

### Page Transitions
```jsx
import { useLocation, Outlet } from 'react-router-dom';
import { CSSTransition, TransitionGroup } from 'react-transition-group';

function AnimatedRoutes() {
  const location = useLocation();
  
  return (
    <TransitionGroup>
      <CSSTransition
        key={location.pathname}
        classNames="page"
        timeout={300}
      >
        <div className="page">
          <Outlet />
        </div>
      </CSSTransition>
    </TransitionGroup>
  );
}

// CSS for transitions
/*
.page-enter {
  opacity: 0;
  transform: translateX(100%);
}

.page-enter-active {
  opacity: 1;
  transform: translateX(0);
  transition: all 300ms;
}

.page-exit {
  opacity: 1;
  transform: translateX(0);
}

.page-exit-active {
  opacity: 0;
  transform: translateX(-100%);
  transition: all 300ms;
}
*/
```

## Navigation Hooks

### Custom Navigation Hook
```jsx
function useNavigation() {
  const navigate = useNavigate();
  const location = useLocation();
  
  const navigateWithConfirmation = useCallback((to, message = "Are you sure?") => {
    if (window.confirm(message)) {
      navigate(to);
    }
  }, [navigate]);
  
  const navigateWithDelay = useCallback((to, delay = 1000) => {
    setTimeout(() => navigate(to), delay);
  }, [navigate]);
  
  const navigatePreservingScroll = useCallback((to) => {
    const scrollPosition = window.scrollY;
    navigate(to, { state: { scrollPosition } });
  }, [navigate]);
  
  return {
    navigate,
    location,
    navigateWithConfirmation,
    navigateWithDelay,
    navigatePreservingScroll,
    goBack: () => navigate(-1),
    goForward: () => navigate(1)
  };
}
```

## Navigation Guards

### Before Leave Guard
```jsx
function FormWithGuard() {
  const [hasUnsavedChanges, setHasUnsavedChanges] = useState(false);
  const [isBlocking, setIsBlocking] = useState(false);
  
  useEffect(() => {
    setIsBlocking(hasUnsavedChanges);
  }, [hasUnsavedChanges]);
  
  useEffect(() => {
    const handleBeforeUnload = (e) => {
      if (hasUnsavedChanges) {
        e.preventDefault();
        e.returnValue = '';
      }
    };
    
    window.addEventListener('beforeunload', handleBeforeUnload);
    return () => window.removeEventListener('beforeunload', handleBeforeUnload);
  }, [hasUnsavedChanges]);
  
  return (
    <>
      <Prompt
        when={isBlocking}
        message="You have unsaved changes. Are you sure you want to leave?"
      />
      <form onChange={() => setHasUnsavedChanges(true)}>
        {/* Form fields */}
      </form>
    </>
  );
}
```

## Mobile-Style Navigation

### Bottom Navigation
```jsx
function BottomNavigation() {
  const location = useLocation();
  
  const navItems = [
    { path: '/', icon: '🏠', label: 'Home' },
    { path: '/search', icon: '🔍', label: 'Search' },
    { path: '/profile', icon: '👤', label: 'Profile' },
    { path: '/settings', icon: '⚙️', label: 'Settings' }
  ];
  
  return (
    <nav className="bottom-nav">
      {navItems.map(item => (
        <NavLink
          key={item.path}
          to={item.path}
          className={({ isActive }) =>
            `nav-item ${isActive ? 'active' : ''}`
          }
        >
          <span className="icon">{item.icon}</span>
          <span className="label">{item.label}</span>
        </NavLink>
      ))}
    </nav>
  );
}
```

### Drawer Navigation
```jsx
function DrawerNavigation() {
  const [isOpen, setIsOpen] = useState(false);
  const navigate = useNavigate();
  
  const handleNavigate = (path) => {
    navigate(path);
    setIsOpen(false);
  };
  
  return (
    <>
      <button onClick={() => setIsOpen(true)}>☰ Menu</button>
      
      <div className={`drawer ${isOpen ? 'open' : ''}`}>
        <button onClick={() => setIsOpen(false)}>✕</button>
        
        <nav>
          <button onClick={() => handleNavigate('/')}>Home</button>
          <button onClick={() => handleNavigate('/about')}>About</button>
          <button onClick={() => handleNavigate('/services')}>Services</button>
          <button onClick={() => handleNavigate('/contact')}>Contact</button>
        </nav>
      </div>
      
      {isOpen && (
        <div 
          className="overlay" 
          onClick={() => setIsOpen(false)}
        />
      )}
    </>
  );
}
```

## Smart Navigation Components

### Navigation with History
```jsx
function NavigationHistory() {
  const [history, setHistory] = useState([]);
  const location = useLocation();
  
  useEffect(() => {
    setHistory(prev => [...prev, location.pathname].slice(-10));
  }, [location]);
  
  return (
    <div>
      <h3>Recently Visited:</h3>
      <ul>
        {[...new Set(history)].map((path, index) => (
          <li key={index}>
            <Link to={path}>{path}</Link>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### Contextual Navigation
```jsx
function ContextualNavigation({ userRole }) {
  const adminLinks = [
    { path: '/admin/dashboard', label: 'Dashboard' },
    { path: '/admin/users', label: 'Users' },
    { path: '/admin/settings', label: 'Settings' }
  ];
  
  const userLinks = [
    { path: '/profile', label: 'Profile' },
    { path: '/orders', label: 'Orders' },
    { path: '/support', label: 'Support' }
  ];
  
  const links = userRole === 'admin' ? adminLinks : userLinks;
  
  return (
    <nav>
      {links.map(link => (
        <NavLink
          key={link.path}
          to={link.path}
          className={({ isActive }) => isActive ? 'active' : ''}
        >
          {link.label}
        </NavLink>
      ))}
    </nav>
  );
}
```

## Best Practices

1. **Use semantic HTML** - nav, ul/li for navigation
2. **Provide visual feedback** - Active states, hover effects
3. **Handle loading states** - Show progress during navigation
4. **Implement breadcrumbs** - For deep navigation hierarchies
5. **Consider mobile UX** - Bottom navigation, hamburger menus
6. **Add keyboard support** - Tab navigation, arrow keys
7. **Preserve scroll position** - When navigating back
8. **Use proper ARIA labels** - For accessibility

Navigation is crucial for user experience. Implement it thoughtfully to create intuitive and accessible applications!