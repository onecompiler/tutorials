# Component Lifecycle

React components go through a lifecycle of events from creation to destruction. Understanding these lifecycle phases helps you control component behavior and optimize performance.

## Lifecycle in Class Components

Class components have explicit lifecycle methods divided into three phases:

### Mounting Phase
When a component is being created and inserted into the DOM:

```jsx
class LifecycleDemo extends React.Component {
  constructor(props) {
    super(props);
    console.log('1. Constructor called');
    this.state = { count: 0 };
  }
  
  static getDerivedStateFromProps(props, state) {
    console.log('2. getDerivedStateFromProps called');
    // Return new state or null
    return null;
  }
  
  componentDidMount() {
    console.log('4. componentDidMount called');
    // Good place for:
    // - API calls
    // - Subscriptions
    // - Timers
  }
  
  render() {
    console.log('3. Render called');
    return <div>Count: {this.state.count}</div>;
  }
}
```

### Updating Phase
When props or state changes:

```jsx
class UpdateDemo extends React.Component {
  static getDerivedStateFromProps(props, state) {
    console.log('getDerivedStateFromProps (update)');
    return null;
  }
  
  shouldComponentUpdate(nextProps, nextState) {
    console.log('shouldComponentUpdate');
    // Return true to update, false to prevent update
    return true;
  }
  
  render() {
    console.log('Render (update)');
    return <div>{this.props.value}</div>;
  }
  
  getSnapshotBeforeUpdate(prevProps, prevState) {
    console.log('getSnapshotBeforeUpdate');
    // Capture information from DOM before update
    return null;
  }
  
  componentDidUpdate(prevProps, prevState, snapshot) {
    console.log('componentDidUpdate');
    // Good place for:
    // - DOM operations
    // - Network requests based on prop changes
    
    if (prevProps.userId !== this.props.userId) {
      this.fetchUserData(this.props.userId);
    }
  }
}
```

### Unmounting Phase
When a component is being removed from the DOM:

```jsx
class UnmountDemo extends React.Component {
  componentDidMount() {
    this.timer = setInterval(() => {
      console.log('Timer running');
    }, 1000);
  }
  
  componentWillUnmount() {
    console.log('componentWillUnmount called');
    // Clean up:
    // - Timers
    // - Subscriptions
    // - Pending requests
    clearInterval(this.timer);
  }
  
  render() {
    return <div>Component with timer</div>;
  }
}
```

## Lifecycle in Functional Components (Hooks)

Functional components use hooks to replicate lifecycle behavior:

### useEffect Hook
Combines componentDidMount, componentDidUpdate, and componentWillUnmount:

```jsx
import React, { useState, useEffect } from 'react';

function LifecycleWithHooks() {
  const [count, setCount] = useState(0);
  
  // componentDidMount & componentDidUpdate
  useEffect(() => {
    console.log('Effect ran');
    
    // componentWillUnmount
    return () => {
      console.log('Cleanup');
    };
  }); // Runs after every render
  
  // componentDidMount only
  useEffect(() => {
    console.log('Mounted');
    
    return () => {
      console.log('Unmounting');
    };
  }, []); // Empty dependency array
  
  // componentDidUpdate for specific values
  useEffect(() => {
    console.log('Count changed:', count);
  }, [count]); // Only when count changes
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

## Common Lifecycle Patterns

### Data Fetching
```jsx
// Class Component
class UserProfile extends React.Component {
  state = { user: null, loading: true };
  
  componentDidMount() {
    this.fetchUser();
  }
  
  componentDidUpdate(prevProps) {
    if (prevProps.userId !== this.props.userId) {
      this.fetchUser();
    }
  }
  
  fetchUser = async () => {
    this.setState({ loading: true });
    try {
      const response = await fetch(`/api/users/${this.props.userId}`);
      const user = await response.json();
      this.setState({ user, loading: false });
    } catch (error) {
      this.setState({ loading: false, error });
    }
  }
  
  render() {
    const { user, loading } = this.state;
    if (loading) return <div>Loading...</div>;
    return <div>{user?.name}</div>;
  }
}

// Functional Component
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    let cancelled = false;
    
    async function fetchUser() {
      try {
        setLoading(true);
        const response = await fetch(`/api/users/${userId}`);
        const data = await response.json();
        if (!cancelled) {
          setUser(data);
          setLoading(false);
        }
      } catch (error) {
        if (!cancelled) {
          setLoading(false);
        }
      }
    }
    
    fetchUser();
    
    // Cleanup function
    return () => {
      cancelled = true;
    };
  }, [userId]);
  
  if (loading) return <div>Loading...</div>;
  return <div>{user?.name}</div>;
}
```

### Subscriptions
```jsx
// Class Component
class MouseTracker extends React.Component {
  state = { x: 0, y: 0 };
  
  componentDidMount() {
    window.addEventListener('mousemove', this.handleMouseMove);
  }
  
  componentWillUnmount() {
    window.removeEventListener('mousemove', this.handleMouseMove);
  }
  
  handleMouseMove = (e) => {
    this.setState({ x: e.clientX, y: e.clientY });
  }
  
  render() {
    return <div>Mouse: {this.state.x}, {this.state.y}</div>;
  }
}

// Functional Component
function MouseTracker() {
  const [position, setPosition] = useState({ x: 0, y: 0 });
  
  useEffect(() => {
    const handleMouseMove = (e) => {
      setPosition({ x: e.clientX, y: e.clientY });
    };
    
    window.addEventListener('mousemove', handleMouseMove);
    
    return () => {
      window.removeEventListener('mousemove', handleMouseMove);
    };
  }, []); // Empty array = only on mount/unmount
  
  return <div>Mouse: {position.x}, {position.y}</div>;
}
```

### Timers
```jsx
// Class Component
class Timer extends React.Component {
  state = { seconds: 0 };
  
  componentDidMount() {
    this.interval = setInterval(() => {
      this.setState(state => ({ seconds: state.seconds + 1 }));
    }, 1000);
  }
  
  componentWillUnmount() {
    clearInterval(this.interval);
  }
  
  render() {
    return <div>Seconds: {this.state.seconds}</div>;
  }
}

// Functional Component
function Timer() {
  const [seconds, setSeconds] = useState(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(s => s + 1);
    }, 1000);
    
    return () => clearInterval(interval);
  }, []);
  
  return <div>Seconds: {seconds}</div>;
}
```

## Advanced Patterns

### Previous Value Hook
```jsx
function usePrevious(value) {
  const ref = useRef();
  
  useEffect(() => {
    ref.current = value;
  });
  
  return ref.current;
}

function Counter() {
  const [count, setCount] = useState(0);
  const prevCount = usePrevious(count);
  
  return (
    <div>
      <p>Current: {count}, Previous: {prevCount}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### Debounced Effect
```jsx
function SearchInput() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  
  useEffect(() => {
    const timeoutId = setTimeout(() => {
      if (query) {
        // Perform search
        performSearch(query).then(setResults);
      }
    }, 500); // Debounce delay
    
    return () => clearTimeout(timeoutId);
  }, [query]);
  
  return (
    <div>
      <input
        value={query}
        onChange={(e) => setQuery(e.target.value)}
        placeholder="Search..."
      />
      <ul>
        {results.map(result => (
          <li key={result.id}>{result.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

### Layout Effect
```jsx
function Tooltip({ text, targetRef }) {
  const [position, setPosition] = useState({ top: 0, left: 0 });
  
  useLayoutEffect(() => {
    if (targetRef.current) {
      const rect = targetRef.current.getBoundingClientRect();
      setPosition({
        top: rect.bottom + 5,
        left: rect.left
      });
    }
  }, [targetRef]);
  
  return (
    <div
      style={{
        position: 'absolute',
        top: position.top,
        left: position.left
      }}
    >
      {text}
    </div>
  );
}
```

## Error Boundaries

Handle errors in component lifecycle:

```jsx
class ErrorBoundary extends React.Component {
  state = { hasError: false, error: null };
  
  static getDerivedStateFromError(error) {
    // Update state to show error UI
    return { hasError: true, error };
  }
  
  componentDidCatch(error, errorInfo) {
    // Log error to error reporting service
    console.error('Error caught:', error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h2>Something went wrong</h2>
          <details>
            <summary>Error details</summary>
            <pre>{this.state.error?.toString()}</pre>
          </details>
        </div>
      );
    }
    
    return this.props.children;
  }
}
```

## Performance Optimization

### shouldComponentUpdate
```jsx
class OptimizedComponent extends React.Component {
  shouldComponentUpdate(nextProps, nextState) {
    // Only update if specific props change
    return (
      this.props.value !== nextProps.value ||
      this.state.count !== nextState.count
    );
  }
  
  render() {
    return <div>{this.props.value}</div>;
  }
}
```

### React.memo for Functional Components
```jsx
const OptimizedComponent = React.memo(function Component({ value }) {
  console.log('Rendering');
  return <div>{value}</div>;
}, (prevProps, nextProps) => {
  // Return true if props are equal (skip re-render)
  return prevProps.value === nextProps.value;
});
```

## Best Practices

1. **Clean up side effects** - Always return cleanup functions
2. **Handle race conditions** - Cancel outdated requests
3. **Avoid memory leaks** - Clear timers and subscriptions
4. **Use dependency arrays correctly** - Include all dependencies
5. **Prefer hooks over class components** for new code
6. **Don't overuse effects** - Consider if you really need them
7. **Keep effects focused** - One effect per concern

## Common Pitfalls

### Missing Dependencies
```jsx
// ❌ Missing dependency
useEffect(() => {
  setCount(count + 1);
}, []); // Should include 'count'

// ✅ Correct
useEffect(() => {
  setCount(c => c + 1);
}, []);
```

### Not Cleaning Up
```jsx
// ❌ Memory leak
useEffect(() => {
  const timer = setInterval(() => {
    console.log('Running');
  }, 1000);
  // Missing cleanup!
});

// ✅ Proper cleanup
useEffect(() => {
  const timer = setInterval(() => {
    console.log('Running');
  }, 1000);
  
  return () => clearInterval(timer);
}, []);
```

Understanding component lifecycle is crucial for building robust React applications. Master these concepts to handle side effects properly and optimize performance!