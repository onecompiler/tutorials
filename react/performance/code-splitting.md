# Code Splitting in React

Code splitting is a technique that allows you to split your JavaScript bundle into smaller chunks, loading them on demand. This significantly improves initial load time and overall application performance.

## Why Code Splitting?

Without code splitting, your entire application is bundled into a single JavaScript file. As your app grows, this file becomes larger, leading to:
- Slower initial page load
- Unnecessary code being downloaded
- Poor performance on slower networks

## React.lazy and Suspense

### Basic Code Splitting
```jsx
import React, { lazy, Suspense } from 'react';

// Before: Regular import
// import About from './pages/About';

// After: Lazy import
const About = lazy(() => import('./pages/About'));

function App() {
  return (
    <div>
      <h1>My App</h1>
      <Suspense fallback={<div>Loading...</div>}>
        <About />
      </Suspense>
    </div>
  );
}
```

### Route-Based Code Splitting
```jsx
import { lazy, Suspense } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';

// Lazy load route components
const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
const Products = lazy(() => import('./pages/Products'));
const Contact = lazy(() => import('./pages/Contact'));
const Admin = lazy(() => import('./pages/Admin'));

// Loading component
function PageLoader() {
  return (
    <div className="page-loader">
      <div className="spinner"></div>
      <p>Loading page...</p>
    </div>
  );
}

function App() {
  return (
    <Router>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/products">Products</Link>
        <Link to="/contact">Contact</Link>
      </nav>
      
      <Suspense fallback={<PageLoader />}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
          <Route path="/products" element={<Products />} />
          <Route path="/contact" element={<Contact />} />
          <Route path="/admin/*" element={<Admin />} />
        </Routes>
      </Suspense>
    </Router>
  );
}
```

## Component-Based Code Splitting

### Heavy Components
```jsx
// Modal that contains heavy dependencies
const HeavyModal = lazy(() => import('./components/HeavyModal'));

function App() {
  const [showModal, setShowModal] = useState(false);
  
  return (
    <div>
      <button onClick={() => setShowModal(true)}>
        Open Heavy Modal
      </button>
      
      {showModal && (
        <Suspense fallback={<div>Loading modal...</div>}>
          <HeavyModal onClose={() => setShowModal(false)} />
        </Suspense>
      )}
    </div>
  );
}

// HeavyModal.js
import Chart from 'chart.js'; // Heavy dependency
import DataTable from 'react-data-table'; // Another heavy dependency

export default function HeavyModal({ onClose }) {
  return (
    <div className="modal">
      <Chart data={data} />
      <DataTable columns={columns} data={rows} />
      <button onClick={onClose}>Close</button>
    </div>
  );
}
```

### Conditional Loading
```jsx
function Dashboard({ userRole }) {
  // Load components based on user role
  const AdminPanel = userRole === 'admin' 
    ? lazy(() => import('./components/AdminPanel'))
    : null;
    
  const Analytics = userRole !== 'guest'
    ? lazy(() => import('./components/Analytics'))
    : null;
  
  return (
    <div>
      <h1>Dashboard</h1>
      
      {AdminPanel && (
        <Suspense fallback={<div>Loading admin panel...</div>}>
          <AdminPanel />
        </Suspense>
      )}
      
      {Analytics && (
        <Suspense fallback={<div>Loading analytics...</div>}>
          <Analytics />
        </Suspense>
      )}
    </div>
  );
}
```

## Advanced Patterns

### Error Boundaries with Code Splitting
```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return (
        <div className="error-fallback">
          <h2>Failed to load component</h2>
          <button onClick={() => window.location.reload()}>
            Reload Page
          </button>
        </div>
      );
    }
    
    return this.props.children;
  }
}

// Usage with lazy components
function App() {
  return (
    <ErrorBoundary>
      <Suspense fallback={<PageLoader />}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/dashboard" element={<Dashboard />} />
        </Routes>
      </Suspense>
    </ErrorBoundary>
  );
}
```

### Preloading Components
```jsx
// Preload component before it's needed
const ProductDetails = lazy(() => import('./pages/ProductDetails'));

// Preload function
const preloadProductDetails = () => {
  import('./pages/ProductDetails');
};

function ProductList({ products }) {
  return (
    <div>
      {products.map(product => (
        <Link
          key={product.id}
          to={`/products/${product.id}`}
          onMouseEnter={preloadProductDetails} // Preload on hover
        >
          {product.name}
        </Link>
      ))}
    </div>
  );
}

// Alternative: Preload after initial render
function App() {
  useEffect(() => {
    // Preload critical routes after app loads
    const timer = setTimeout(() => {
      import('./pages/Dashboard');
      import('./pages/Profile');
    }, 2000);
    
    return () => clearTimeout(timer);
  }, []);
  
  return <Routes>{/* ... */}</Routes>;
}
```

### Named Exports
```jsx
// When the module doesn't have a default export
const { NamedComponent } = lazy(() =>
  import('./components/NamedComponents').then(module => ({
    default: module.NamedComponent
  }))
);

// Helper function for named exports
function lazyImport(factory, name) {
  return lazy(() =>
    factory().then(module => ({
      default: module[name]
    }))
  );
}

// Usage
const Header = lazyImport(() => import('./components/Layout'), 'Header');
const Footer = lazyImport(() => import('./components/Layout'), 'Footer');
```

## Library Code Splitting

### Dynamic Imports for Libraries
```jsx
function ChartComponent({ data, type }) {
  const [Chart, setChart] = useState(null);
  
  useEffect(() => {
    // Dynamically import heavy charting library
    import('chart.js').then(module => {
      setChart(() => module.default);
    });
  }, []);
  
  if (!Chart) {
    return <div>Loading chart...</div>;
  }
  
  return <Chart data={data} type={type} />;
}

// Or with async/await
function DatePicker({ value, onChange }) {
  const [PickerComponent, setPickerComponent] = useState(null);
  
  useEffect(() => {
    async function loadDatePicker() {
      const { DatePicker } = await import('react-datepicker');
      await import('react-datepicker/dist/react-datepicker.css');
      setPickerComponent(() => DatePicker);
    }
    
    loadDatePicker();
  }, []);
  
  if (!PickerComponent) {
    return <input type="date" value={value} onChange={onChange} />;
  }
  
  return <PickerComponent selected={value} onChange={onChange} />;
}
```

### Vendor Bundle Splitting
```jsx
// webpack.config.js
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          priority: 10
        },
        common: {
          minChunks: 2,
          priority: 5,
          reuseExistingChunk: true
        }
      }
    }
  }
};

// Vite config
export default {
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          'react-vendor': ['react', 'react-dom', 'react-router-dom'],
          'ui-vendor': ['@mui/material', '@emotion/react', '@emotion/styled'],
          'utils': ['lodash', 'date-fns', 'axios']
        }
      }
    }
  }
};
```

## Progressive Enhancement

### Feature Detection
```jsx
function VideoPlayer({ src }) {
  const [Player, setPlayer] = useState(null);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    // Check if browser supports certain features
    if ('IntersectionObserver' in window && 'requestIdleCallback' in window) {
      import('./components/AdvancedVideoPlayer')
        .then(module => setPlayer(() => module.default))
        .catch(err => {
          setError(err);
          // Fallback to basic player
          import('./components/BasicVideoPlayer')
            .then(module => setPlayer(() => module.default));
        });
    } else {
      // Load basic player for older browsers
      import('./components/BasicVideoPlayer')
        .then(module => setPlayer(() => module.default));
    }
  }, []);
  
  if (!Player) {
    return <div>Loading player...</div>;
  }
  
  return <Player src={src} />;
}
```

### Progressive Loading
```jsx
function ImageGallery({ images }) {
  const [loadedFeatures, setLoadedFeatures] = useState({
    zoom: false,
    slideshow: false,
    edit: false
  });
  
  // Load features progressively
  const loadZoom = async () => {
    const { ZoomFeature } = await import('./features/ZoomFeature');
    setLoadedFeatures(prev => ({ ...prev, zoom: ZoomFeature }));
  };
  
  const loadSlideshow = async () => {
    const { SlideshowFeature } = await import('./features/SlideshowFeature');
    setLoadedFeatures(prev => ({ ...prev, slideshow: SlideshowFeature }));
  };
  
  const loadEdit = async () => {
    const { EditFeature } = await import('./features/EditFeature');
    setLoadedFeatures(prev => ({ ...prev, edit: EditFeature }));
  };
  
  return (
    <div>
      <div className="gallery">
        {images.map(image => (
          <img key={image.id} src={image.url} alt={image.alt} />
        ))}
      </div>
      
      <div className="features">
        {!loadedFeatures.zoom ? (
          <button onClick={loadZoom}>Enable Zoom</button>
        ) : (
          <loadedFeatures.zoom images={images} />
        )}
        
        {!loadedFeatures.slideshow ? (
          <button onClick={loadSlideshow}>Enable Slideshow</button>
        ) : (
          <loadedFeatures.slideshow images={images} />
        )}
        
        {!loadedFeatures.edit ? (
          <button onClick={loadEdit}>Enable Edit</button>
        ) : (
          <loadedFeatures.edit images={images} />
        )}
      </div>
    </div>
  );
}
```

## Webpack Magic Comments

### Prefetch and Preload
```jsx
// Prefetch: Load during idle time
const AdminDashboard = lazy(() => 
  import(/* webpackPrefetch: true */ './pages/AdminDashboard')
);

// Preload: Load in parallel with parent
const CriticalComponent = lazy(() => 
  import(/* webpackPreload: true */ './components/CriticalComponent')
);

// Custom chunk names
const Analytics = lazy(() => 
  import(/* webpackChunkName: "analytics" */ './pages/Analytics')
);

// Combine multiple hints
const Reports = lazy(() => 
  import(
    /* webpackChunkName: "reports" */
    /* webpackPrefetch: true */
    './pages/Reports'
  )
);
```

## Measuring Impact

### Performance Monitoring
```jsx
function LazyBoundary({ children, name }) {
  const [loadTime, setLoadTime] = useState(null);
  
  useEffect(() => {
    const startTime = performance.now();
    
    return () => {
      const endTime = performance.now();
      const duration = endTime - startTime;
      setLoadTime(duration);
      
      // Send to analytics
      if (window.gtag) {
        window.gtag('event', 'timing_complete', {
          name: 'lazy_load',
          value: Math.round(duration),
          event_category: 'JS Dependencies',
          event_label: name
        });
      }
    };
  }, [name]);
  
  return (
    <>
      {children}
      {loadTime && (
        <div className="debug-info">
          Loaded {name} in {loadTime.toFixed(2)}ms
        </div>
      )}
    </>
  );
}

// Usage
<LazyBoundary name="Dashboard">
  <Suspense fallback={<Loading />}>
    <Dashboard />
  </Suspense>
</LazyBoundary>
```

### Bundle Analysis
```jsx
// Create a visual bundle report
// package.json
{
  "scripts": {
    "analyze": "source-map-explorer 'build/static/js/*.js'",
    "analyze:bundle": "webpack-bundle-analyzer build/stats.json"
  }
}

// webpack.config.js
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

module.exports = {
  plugins: [
    new BundleAnalyzerPlugin({
      analyzerMode: process.env.ANALYZE ? 'server' : 'disabled'
    })
  ]
};
```

## Best Practices

1. **Split at route level first** - Biggest impact with least effort
2. **Avoid over-splitting** - Too many chunks can hurt performance
3. **Consider network latency** - Balance chunk size vs. request count
4. **Preload critical paths** - Use prefetch/preload hints
5. **Handle loading states** - Provide meaningful feedback
6. **Implement error boundaries** - Handle chunk loading failures
7. **Monitor bundle sizes** - Use bundle analyzers regularly
8. **Test on slow networks** - Ensure good experience for all users

Code splitting is essential for scaling React applications while maintaining good performance!