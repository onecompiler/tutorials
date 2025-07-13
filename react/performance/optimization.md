# Performance Optimization in React

React is fast by default, but as applications grow, performance can degrade. This tutorial covers various optimization techniques to keep your React apps running smoothly.

## Understanding React Rendering

### How React Updates
```jsx
// React re-renders when:
// 1. State changes
// 2. Props change
// 3. Parent re-renders
// 4. Context changes

function ExpensiveComponent() {
  console.log('ExpensiveComponent rendered');
  
  // Expensive calculation
  const result = Array.from({ length: 1000000 }, (_, i) => i)
    .reduce((sum, num) => sum + num, 0);
  
  return <div>Sum: {result}</div>;
}

function Parent() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>
        Count: {count}
      </button>
      <ExpensiveComponent /> {/* Re-renders on every count change! */}
    </div>
  );
}
```

## React.memo

### Basic Memoization
```jsx
// Prevent unnecessary re-renders
const ExpensiveComponent = React.memo(function ExpensiveComponent({ data }) {
  console.log('ExpensiveComponent rendered');
  
  const result = data.reduce((sum, num) => sum + num, 0);
  
  return <div>Sum: {result}</div>;
});

// Component only re-renders when props change
function Parent() {
  const [count, setCount] = useState(0);
  const data = [1, 2, 3, 4, 5]; // Same reference each render
  
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>
        Count: {count}
      </button>
      <ExpensiveComponent data={data} /> {/* No re-render! */}
    </div>
  );
}
```

### Custom Comparison
```jsx
const TodoItem = React.memo(
  function TodoItem({ todo, onToggle }) {
    console.log('TodoItem rendered:', todo.id);
    
    return (
      <li>
        <input
          type="checkbox"
          checked={todo.completed}
          onChange={() => onToggle(todo.id)}
        />
        <span>{todo.text}</span>
      </li>
    );
  },
  // Custom comparison function
  (prevProps, nextProps) => {
    return (
      prevProps.todo.id === nextProps.todo.id &&
      prevProps.todo.completed === nextProps.todo.completed &&
      prevProps.todo.text === nextProps.todo.text
    );
  }
);
```

## useMemo Hook

### Expensive Calculations
```jsx
function DataProcessor({ items, filter }) {
  // Memoize expensive calculation
  const processedData = useMemo(() => {
    console.log('Processing data...');
    return items
      .filter(item => item.category === filter)
      .sort((a, b) => b.value - a.value)
      .map(item => ({
        ...item,
        formattedValue: `$${item.value.toFixed(2)}`
      }));
  }, [items, filter]); // Only recalculate when items or filter change
  
  return (
    <ul>
      {processedData.map(item => (
        <li key={item.id}>{item.name}: {item.formattedValue}</li>
      ))}
    </ul>
  );
}
```

### Object and Array Creation
```jsx
function SearchResults({ query }) {
  // Prevent creating new objects on every render
  const defaultOptions = useMemo(() => ({
    caseSensitive: false,
    wholeWord: false,
    regex: false
  }), []); // Empty deps = created once
  
  const searchParams = useMemo(() => ({
    query,
    options: defaultOptions
  }), [query, defaultOptions]);
  
  return <ResultsList params={searchParams} />;
}
```

## useCallback Hook

### Stable Function References
```jsx
function TodoList({ todos }) {
  const [filter, setFilter] = useState('all');
  
  // Without useCallback, new function every render
  const handleToggle = useCallback((id) => {
    // Toggle todo logic
    console.log('Toggling todo:', id);
  }, []); // Empty deps = stable reference
  
  const filteredTodos = useMemo(() => {
    switch (filter) {
      case 'active':
        return todos.filter(t => !t.completed);
      case 'completed':
        return todos.filter(t => t.completed);
      default:
        return todos;
    }
  }, [todos, filter]);
  
  return (
    <div>
      <FilterButtons currentFilter={filter} onFilterChange={setFilter} />
      {filteredTodos.map(todo => (
        <TodoItem
          key={todo.id}
          todo={todo}
          onToggle={handleToggle} // Stable reference
        />
      ))}
    </div>
  );
}

// Child component with React.memo
const TodoItem = React.memo(({ todo, onToggle }) => {
  return (
    <li onClick={() => onToggle(todo.id)}>
      {todo.text}
    </li>
  );
});
```

### Event Handler Optimization
```jsx
function Form() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });
  
  // Memoize handlers that depend on state
  const handleChange = useCallback((field) => (e) => {
    setFormData(prev => ({
      ...prev,
      [field]: e.target.value
    }));
  }, []);
  
  const handleSubmit = useCallback((e) => {
    e.preventDefault();
    console.log('Submitting:', formData);
  }, [formData]);
  
  return (
    <form onSubmit={handleSubmit}>
      <Input
        name="name"
        value={formData.name}
        onChange={handleChange('name')}
      />
      <Input
        name="email"
        value={formData.email}
        onChange={handleChange('email')}
      />
      <TextArea
        name="message"
        value={formData.message}
        onChange={handleChange('message')}
      />
      <button type="submit">Submit</button>
    </form>
  );
}

// Memoized input components
const Input = React.memo(({ name, value, onChange }) => {
  console.log('Input rendered:', name);
  return <input name={name} value={value} onChange={onChange} />;
});

const TextArea = React.memo(({ name, value, onChange }) => {
  console.log('TextArea rendered:', name);
  return <textarea name={name} value={value} onChange={onChange} />;
});
```

## Lazy Loading

### Code Splitting with lazy
```jsx
import React, { lazy, Suspense } from 'react';

// Lazy load components
const Dashboard = lazy(() => import('./pages/Dashboard'));
const Profile = lazy(() => import('./pages/Profile'));
const Settings = lazy(() => import('./pages/Settings'));

function App() {
  return (
    <Router>
      <Suspense fallback={<LoadingSpinner />}>
        <Routes>
          <Route path="/dashboard" element={<Dashboard />} />
          <Route path="/profile" element={<Profile />} />
          <Route path="/settings" element={<Settings />} />
        </Routes>
      </Suspense>
    </Router>
  );
}

// Loading component
function LoadingSpinner() {
  return (
    <div className="spinner-container">
      <div className="spinner" />
      <p>Loading...</p>
    </div>
  );
}
```

### Conditional Lazy Loading
```jsx
function FeatureToggle({ feature, children }) {
  const [Component, setComponent] = useState(null);
  
  useEffect(() => {
    if (feature === 'advanced') {
      import('./AdvancedFeature').then(module => {
        setComponent(() => module.default);
      });
    }
  }, [feature]);
  
  if (feature === 'basic') {
    return children;
  }
  
  if (Component) {
    return <Component />;
  }
  
  return <LoadingSpinner />;
}
```

## List Optimization

### Virtualization
```jsx
// Using react-window for large lists
import { FixedSizeList } from 'react-window';

function VirtualizedList({ items }) {
  const Row = ({ index, style }) => (
    <div style={style}>
      {items[index].name}
    </div>
  );
  
  return (
    <FixedSizeList
      height={600}
      itemCount={items.length}
      itemSize={50}
      width="100%"
    >
      {Row}
    </FixedSizeList>
  );
}

// Manual virtualization
function ManualVirtualList({ items, itemHeight = 50, containerHeight = 600 }) {
  const [scrollTop, setScrollTop] = useState(0);
  
  const startIndex = Math.floor(scrollTop / itemHeight);
  const endIndex = Math.min(
    items.length - 1,
    Math.floor((scrollTop + containerHeight) / itemHeight)
  );
  
  const visibleItems = items.slice(startIndex, endIndex + 1);
  const invisibleItemsHeight = startIndex * itemHeight;
  
  return (
    <div
      style={{ height: containerHeight, overflow: 'auto' }}
      onScroll={(e) => setScrollTop(e.target.scrollTop)}
    >
      <div style={{ height: items.length * itemHeight }}>
        <div style={{ transform: `translateY(${invisibleItemsHeight}px)` }}>
          {visibleItems.map((item, index) => (
            <div
              key={startIndex + index}
              style={{ height: itemHeight }}
            >
              {item.name}
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}
```

### Key Optimization
```jsx
// Bad - using index as key with dynamic list
function BadList({ items }) {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{item.name}</li> // Bad for dynamic lists
      ))}
    </ul>
  );
}

// Good - using stable unique ID
function GoodList({ items }) {
  return (
    <ul>
      {items.map(item => (
        <li key={item.id}>{item.name}</li> // Stable key
      ))}
    </ul>
  );
}

// Generate stable keys for items without IDs
function ListWithGeneratedKeys({ items }) {
  const stableItems = useMemo(() => 
    items.map(item => ({
      ...item,
      key: `${item.type}-${item.name}-${item.timestamp}`
    })),
    [items]
  );
  
  return (
    <ul>
      {stableItems.map(item => (
        <li key={item.key}>{item.name}</li>
      ))}
    </ul>
  );
}
```

## State Management Optimization

### State Colocation
```jsx
// Bad - lifting state too high
function App() {
  const [inputValue, setInputValue] = useState('');
  
  return (
    <div>
      <Header />
      <MainContent />
      <SearchBar value={inputValue} onChange={setInputValue} />
    </div>
  );
}

// Good - colocate state where it's used
function App() {
  return (
    <div>
      <Header />
      <MainContent />
      <SearchBar /> {/* Manages its own state */}
    </div>
  );
}

function SearchBar() {
  const [value, setValue] = useState('');
  
  return (
    <input
      value={value}
      onChange={(e) => setValue(e.target.value)}
    />
  );
}
```

### State Splitting
```jsx
// Bad - single state object
function BadComponent() {
  const [state, setState] = useState({
    user: null,
    posts: [],
    comments: [],
    ui: { theme: 'light', sidebarOpen: true }
  });
  
  // Any update re-renders everything
  const toggleSidebar = () => {
    setState(prev => ({
      ...prev,
      ui: { ...prev.ui, sidebarOpen: !prev.ui.sidebarOpen }
    }));
  };
}

// Good - split state
function GoodComponent() {
  const [user, setUser] = useState(null);
  const [posts, setPosts] = useState([]);
  const [comments, setComments] = useState([]);
  const [theme, setTheme] = useState('light');
  const [sidebarOpen, setSidebarOpen] = useState(true);
  
  // Only affected components re-render
  const toggleSidebar = () => {
    setSidebarOpen(prev => !prev);
  };
}
```

## Context Optimization

### Split Contexts
```jsx
// Bad - single context
const AppContext = createContext();

function AppProvider({ children }) {
  const [user, setUser] = useState(null);
  const [theme, setTheme] = useState('light');
  const [cart, setCart] = useState([]);
  
  return (
    <AppContext.Provider value={{ user, theme, cart, setUser, setTheme, setCart }}>
      {children}
    </AppContext.Provider>
  );
}

// Good - split contexts
const UserContext = createContext();
const ThemeContext = createContext();
const CartContext = createContext();

function Providers({ children }) {
  return (
    <UserProvider>
      <ThemeProvider>
        <CartProvider>
          {children}
        </CartProvider>
      </ThemeProvider>
    </UserProvider>
  );
}
```

### Stable Context Values
```jsx
function OptimizedProvider({ children }) {
  const [state, dispatch] = useReducer(reducer, initialState);
  
  // Memoize context value
  const contextValue = useMemo(() => ({
    state,
    dispatch
  }), [state]);
  
  // Or split state and dispatch
  return (
    <StateContext.Provider value={state}>
      <DispatchContext.Provider value={dispatch}>
        {children}
      </DispatchContext.Provider>
    </StateContext.Provider>
  );
}
```

## Image Optimization

### Lazy Loading Images
```jsx
function LazyImage({ src, alt, placeholder }) {
  const [imageSrc, setImageSrc] = useState(placeholder);
  const [imageRef, setImageRef] = useState();
  
  useEffect(() => {
    let observer;
    
    if (imageRef && imageSrc === placeholder) {
      observer = new IntersectionObserver(
        entries => {
          entries.forEach(entry => {
            if (entry.isIntersecting) {
              setImageSrc(src);
              observer.unobserve(imageRef);
            }
          });
        },
        { threshold: 0.1 }
      );
      observer.observe(imageRef);
    }
    
    return () => {
      if (observer && observer.unobserve) {
        observer.unobserve(imageRef);
      }
    };
  }, [imageRef, imageSrc, placeholder, src]);
  
  return (
    <img
      ref={setImageRef}
      src={imageSrc}
      alt={alt}
      loading="lazy"
    />
  );
}
```

### Progressive Image Loading
```jsx
function ProgressiveImage({ lowQualitySrc, highQualitySrc, alt }) {
  const [src, setSrc] = useState(lowQualitySrc);
  const [isLoading, setIsLoading] = useState(true);
  
  useEffect(() => {
    const img = new Image();
    img.src = highQualitySrc;
    img.onload = () => {
      setSrc(highQualitySrc);
      setIsLoading(false);
    };
  }, [highQualitySrc]);
  
  return (
    <div className={`image-container ${isLoading ? 'loading' : ''}`}>
      <img
        src={src}
        alt={alt}
        style={{
          filter: isLoading ? 'blur(5px)' : 'none',
          transition: 'filter 0.3s ease-out'
        }}
      />
    </div>
  );
}
```

## Bundle Size Optimization

### Tree Shaking
```jsx
// Bad - importing entire library
import _ from 'lodash';

function Component() {
  const debounced = _.debounce(handleSearch, 300);
}

// Good - import only what you need
import debounce from 'lodash/debounce';

function Component() {
  const debounced = debounce(handleSearch, 300);
}

// Even better - use native alternatives
function Component() {
  const debounced = useCallback(
    debounce(handleSearch, 300),
    []
  );
}

function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}
```

## Performance Monitoring

### React Profiler
```jsx
import { Profiler } from 'react';

function onRenderCallback(
  id, // the "id" prop of the Profiler tree that has just committed
  phase, // either "mount" (if the tree just mounted) or "update"
  actualDuration, // time spent rendering the committed update
  baseDuration, // estimated time to render the entire subtree without memoization
  startTime, // when React began rendering this update
  commitTime, // when React committed this update
  interactions // the Set of interactions belonging to this update
) {
  console.log(`${id} (${phase}) took ${actualDuration}ms`);
}

function App() {
  return (
    <Profiler id="App" onRender={onRenderCallback}>
      <Header />
      <Profiler id="MainContent" onRender={onRenderCallback}>
        <MainContent />
      </Profiler>
      <Footer />
    </Profiler>
  );
}
```

### Custom Performance Hooks
```jsx
function useRenderCount(componentName) {
  const renderCount = useRef(0);
  
  useEffect(() => {
    renderCount.current += 1;
    console.log(`${componentName} rendered ${renderCount.current} times`);
  });
}

function useWhyDidYouUpdate(name, props) {
  const previousProps = useRef();
  
  useEffect(() => {
    if (previousProps.current) {
      const allKeys = Object.keys({ ...previousProps.current, ...props });
      const changedProps = {};
      
      allKeys.forEach(key => {
        if (previousProps.current[key] !== props[key]) {
          changedProps[key] = {
            from: previousProps.current[key],
            to: props[key]
          };
        }
      });
      
      if (Object.keys(changedProps).length) {
        console.log('[why-did-you-update]', name, changedProps);
      }
    }
    
    previousProps.current = props;
  });
}
```

## Best Practices

1. **Measure first** - Use React DevTools Profiler
2. **Optimize when needed** - Don't optimize prematurely
3. **Start with React.memo** - For expensive components
4. **Use production builds** - For accurate performance testing
5. **Watch bundle size** - Use tools like Bundle Analyzer
6. **Lazy load routes** - Split code at route level
7. **Virtualize long lists** - For 100+ items
8. **Optimize images** - Use appropriate formats and sizes

Performance optimization is crucial for maintaining a smooth user experience as your app grows!