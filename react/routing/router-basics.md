# React Router Basics

React Router is the standard routing library for React applications. It enables navigation between different components, changing the browser URL while keeping the UI in sync.

## Installation

To use React Router in your project, install it via npm:

```bash
npm install react-router-dom
```

## Basic Setup

### BrowserRouter
```jsx
import React from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';

// Components
function Home() {
  return <h1>Welcome to Home Page</h1>;
}

function About() {
  return <h1>About Us</h1>;
}

function Contact() {
  return <h1>Contact Us</h1>;
}

// App with routing
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </BrowserRouter>
  );
}
```

## Navigation with Link

### Basic Navigation
```jsx
import { Link } from 'react-router-dom';

function Navigation() {
  return (
    <nav>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/about">About</Link></li>
        <li><Link to="/contact">Contact</Link></li>
      </ul>
    </nav>
  );
}

// App with navigation
function App() {
  return (
    <BrowserRouter>
      <Navigation />
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </BrowserRouter>
  );
}
```

## NavLink for Active Styling

```jsx
import { NavLink } from 'react-router-dom';

function Navigation() {
  return (
    <nav>
      <NavLink 
        to="/" 
        className={({ isActive }) => isActive ? "active" : ""}
      >
        Home
      </NavLink>
      <NavLink 
        to="/about"
        className={({ isActive }) => isActive ? "active" : ""}
      >
        About
      </NavLink>
      <NavLink 
        to="/contact"
        style={({ isActive }) => ({
          color: isActive ? 'red' : 'black',
          fontWeight: isActive ? 'bold' : 'normal'
        })}
      >
        Contact
      </NavLink>
    </nav>
  );
}
```

## Route Parameters

### Dynamic Routes
```jsx
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/users" element={<Users />} />
        <Route path="/users/:userId" element={<UserDetail />} />
        <Route path="/products/:category/:id" element={<Product />} />
      </Routes>
    </BrowserRouter>
  );
}

// Accessing route parameters
import { useParams } from 'react-router-dom';

function UserDetail() {
  const { userId } = useParams();
  
  return (
    <div>
      <h1>User Details</h1>
      <p>User ID: {userId}</p>
    </div>
  );
}

function Product() {
  const { category, id } = useParams();
  
  return (
    <div>
      <h1>Product Details</h1>
      <p>Category: {category}</p>
      <p>Product ID: {id}</p>
    </div>
  );
}
```

## Nested Routes

```jsx
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Layout />}>
          <Route index element={<Home />} />
          <Route path="about" element={<About />} />
          <Route path="dashboard/*" element={<Dashboard />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

// Layout component with Outlet
import { Outlet, Link } from 'react-router-dom';

function Layout() {
  return (
    <div>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/dashboard">Dashboard</Link>
      </nav>
      
      <main>
        <Outlet /> {/* Child routes render here */}
      </main>
      
      <footer>© 2024 My App</footer>
    </div>
  );
}

// Dashboard with nested routes
function Dashboard() {
  return (
    <div>
      <h1>Dashboard</h1>
      <nav>
        <Link to="profile">Profile</Link>
        <Link to="settings">Settings</Link>
      </nav>
      
      <Routes>
        <Route index element={<DashboardHome />} />
        <Route path="profile" element={<Profile />} />
        <Route path="settings" element={<Settings />} />
      </Routes>
    </div>
  );
}
```

## 404 Not Found Route

```jsx
function NotFound() {
  return (
    <div>
      <h1>404 - Page Not Found</h1>
      <p>The page you're looking for doesn't exist.</p>
      <Link to="/">Go to Home</Link>
    </div>
  );
}

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}
```

## Programmatic Navigation

### Using useNavigate Hook
```jsx
import { useNavigate } from 'react-router-dom';

function LoginForm() {
  const navigate = useNavigate();
  const [username, setUsername] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    
    // Perform login logic
    if (username === 'admin') {
      // Navigate to dashboard
      navigate('/dashboard');
      
      // Navigate with replace (no back button)
      // navigate('/dashboard', { replace: true });
      
      // Navigate with state
      // navigate('/dashboard', { state: { from: 'login' } });
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        value={username}
        onChange={(e) => setUsername(e.target.value)}
        placeholder="Username"
      />
      <button type="submit">Login</button>
      <button type="button" onClick={() => navigate(-1)}>
        Go Back
      </button>
    </form>
  );
}
```

## Query Parameters

```jsx
import { useSearchParams } from 'react-router-dom';

function SearchPage() {
  const [searchParams, setSearchParams] = useSearchParams();
  
  const query = searchParams.get('q') || '';
  const category = searchParams.get('category') || 'all';
  const page = parseInt(searchParams.get('page') || '1');
  
  const updateSearch = (newQuery) => {
    setSearchParams({
      q: newQuery,
      category,
      page: 1 // Reset to page 1 on new search
    });
  };
  
  const changePage = (newPage) => {
    setSearchParams({
      q: query,
      category,
      page: newPage
    });
  };
  
  return (
    <div>
      <h1>Search Results</h1>
      <input
        value={query}
        onChange={(e) => updateSearch(e.target.value)}
        placeholder="Search..."
      />
      
      <select
        value={category}
        onChange={(e) => setSearchParams({ q: query, category: e.target.value, page: 1 })}
      >
        <option value="all">All Categories</option>
        <option value="books">Books</option>
        <option value="movies">Movies</option>
      </select>
      
      <p>Searching for: {query} in {category}</p>
      <p>Page: {page}</p>
      
      <button onClick={() => changePage(page - 1)} disabled={page === 1}>
        Previous
      </button>
      <button onClick={() => changePage(page + 1)}>
        Next
      </button>
    </div>
  );
}
```

## Route Configuration

### Centralized Route Config
```jsx
const routes = [
  {
    path: '/',
    element: <Layout />,
    children: [
      { index: true, element: <Home /> },
      { path: 'about', element: <About /> },
      {
        path: 'products',
        element: <Products />,
        children: [
          { index: true, element: <ProductList /> },
          { path: ':id', element: <ProductDetail /> }
        ]
      },
      { path: 'contact', element: <Contact /> },
      { path: '*', element: <NotFound /> }
    ]
  }
];

function App() {
  return (
    <BrowserRouter>
      <Routes>
        {routes.map((route, index) => (
          <Route key={index} path={route.path} element={route.element}>
            {route.children?.map((child, childIndex) => (
              <Route
                key={childIndex}
                index={child.index}
                path={child.path}
                element={child.element}
              />
            ))}
          </Route>
        ))}
      </Routes>
    </BrowserRouter>
  );
}
```

## Lazy Loading Routes

```jsx
import { lazy, Suspense } from 'react';

// Lazy load components
const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
const Dashboard = lazy(() => import('./pages/Dashboard'));

function App() {
  return (
    <BrowserRouter>
      <Suspense fallback={<div>Loading...</div>}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
          <Route path="/dashboard/*" element={<Dashboard />} />
        </Routes>
      </Suspense>
    </BrowserRouter>
  );
}
```

## Scroll Restoration

```jsx
import { useEffect } from 'react';
import { useLocation } from 'react-router-dom';

function ScrollToTop() {
  const { pathname } = useLocation();
  
  useEffect(() => {
    window.scrollTo(0, 0);
  }, [pathname]);
  
  return null;
}

function App() {
  return (
    <BrowserRouter>
      <ScrollToTop />
      <Routes>
        {/* Your routes */}
      </Routes>
    </BrowserRouter>
  );
}
```

## Route Guards Example

```jsx
function PrivateRoute({ children }) {
  const isAuthenticated = checkAuth(); // Your auth check
  const location = useLocation();
  
  if (!isAuthenticated) {
    // Redirect to login page but save the attempted location
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  
  return children;
}

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/login" element={<Login />} />
        <Route
          path="/dashboard"
          element={
            <PrivateRoute>
              <Dashboard />
            </PrivateRoute>
          }
        />
      </Routes>
    </BrowserRouter>
  );
}
```

## Custom Router Hook

```jsx
function useRouter() {
  const navigate = useNavigate();
  const location = useLocation();
  const params = useParams();
  const [searchParams, setSearchParams] = useSearchParams();
  
  return {
    navigate,
    location,
    params,
    searchParams,
    setSearchParams,
    
    // Helper methods
    goBack: () => navigate(-1),
    goForward: () => navigate(1),
    push: (path, state) => navigate(path, { state }),
    replace: (path, state) => navigate(path, { replace: true, state })
  };
}

// Usage
function MyComponent() {
  const router = useRouter();
  
  const handleClick = () => {
    router.push('/dashboard', { from: 'home' });
  };
  
  return (
    <div>
      <p>Current path: {router.location.pathname}</p>
      <button onClick={router.goBack}>Go Back</button>
      <button onClick={handleClick}>Go to Dashboard</button>
    </div>
  );
}
```

## Best Practices

1. **Use BrowserRouter** for web apps (not HashRouter)
2. **Organize routes** in a centralized configuration
3. **Implement lazy loading** for better performance
4. **Handle 404s** with a catch-all route
5. **Use NavLink** for navigation menus
6. **Protect private routes** with route guards
7. **Handle loading states** during navigation
8. **Restore scroll position** on route changes

React Router is essential for building single-page applications with multiple views. Master these concepts to create seamless navigation experiences!