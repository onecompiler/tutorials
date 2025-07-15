# Custom Hooks

Custom hooks are a powerful feature in React that allow you to extract component logic into reusable functions. They enable you to share stateful logic between multiple components without changing your component hierarchy.

## What are Custom Hooks?

Custom hooks are JavaScript functions whose names start with "use" and that may call other hooks. They follow the same rules as React's built-in hooks.

## Why Use Custom Hooks?

- **Reusability**: Share logic between multiple components
- **Separation of Concerns**: Keep components clean and focused on rendering
- **Testing**: Easier to test logic in isolation
- **Composition**: Build complex functionality by combining simpler hooks

## Creating Your First Custom Hook

Let's create a simple custom hook that manages a counter:

```javascript
// useCounter.js
import { useState } from 'react';

function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);
  
  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  const reset = () => setCount(initialValue);
  
  return { count, increment, decrement, reset };
}

export default useCounter;
```

Using the custom hook in a component:

```javascript
import React from 'react';
import useCounter from './useCounter';

function Counter() {
  const { count, increment, decrement, reset } = useCounter(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}
```

## Common Custom Hook Patterns

### 1. useLocalStorage

A hook that syncs state with localStorage:

```javascript
import { useState, useEffect } from 'react';

function useLocalStorage(key, initialValue) {
  // Get from local storage then parse stored json or return initialValue
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(error);
      return initialValue;
    }
  });
  
  // Return a wrapped version of useState's setter function that persists the new value to localStorage
  const setValue = (value) => {
    try {
      setStoredValue(value);
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error(error);
    }
  };
  
  return [storedValue, setValue];
}
```

### 2. useFetch

A hook for data fetching:

```javascript
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        
        if (!response.ok) {
          throw new Error(`Error: ${response.status}`);
        }
        
        const jsonData = await response.json();
        setData(jsonData);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };
    
    fetchData();
  }, [url]);
  
  return { data, loading, error };
}
```

### 3. useToggle

A simple toggle hook:

```javascript
import { useState, useCallback } from 'react';

function useToggle(initialState = false) {
  const [state, setState] = useState(initialState);
  
  const toggle = useCallback(() => setState(state => !state), []);
  
  return [state, toggle];
}
```

### 4. useWindowSize

A hook that tracks window dimensions:

```javascript
import { useState, useEffect } from 'react';

function useWindowSize() {
  const [windowSize, setWindowSize] = useState({
    width: undefined,
    height: undefined,
  });
  
  useEffect(() => {
    function handleResize() {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight,
      });
    }
    
    window.addEventListener('resize', handleResize);
    handleResize();
    
    return () => window.removeEventListener('resize', handleResize);
  }, []);
  
  return windowSize;
}
```

## Best Practices

### 1. Naming Convention
Always start custom hook names with "use":
```javascript
// Good
useAuth, useLocalStorage, useWindowSize

// Bad
getAuth, authHook, storageHook
```

### 2. Keep Hooks Pure
Custom hooks should be pure functions that don't cause side effects outside of React's lifecycle:

```javascript
// Good
function useTimer(initialTime) {
  const [time, setTime] = useState(initialTime);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setTime(t => t - 1);
    }, 1000);
    
    return () => clearInterval(interval);
  }, []);
  
  return time;
}

// Bad - modifying external state
let globalTimer;
function useBadTimer() {
  globalTimer = setInterval(() => {}, 1000); // Don't do this!
}
```

### 3. Return Consistent Values
Return an object or array with consistent structure:

```javascript
// Good - consistent return structure
function useApi(url) {
  const [state, setState] = useState({
    data: null,
    loading: true,
    error: null
  });
  
  // ... fetch logic
  
  return { data: state.data, loading: state.loading, error: state.error };
}

// Also good - using array for simple cases
function useToggle(initial) {
  const [value, setValue] = useState(initial);
  const toggle = () => setValue(!value);
  return [value, toggle];
}
```

## Advanced Custom Hooks

### useDebounce

Delays updating a value until after a specified delay:

```javascript
import { useState, useEffect } from 'react';

function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);
  
  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);
    
    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);
  
  return debouncedValue;
}

// Usage
function SearchComponent() {
  const [searchTerm, setSearchTerm] = useState('');
  const debouncedSearchTerm = useDebounce(searchTerm, 500);
  
  useEffect(() => {
    if (debouncedSearchTerm) {
      // Perform search with debouncedSearchTerm
    }
  }, [debouncedSearchTerm]);
  
  return (
    <input
      type="text"
      value={searchTerm}
      onChange={(e) => setSearchTerm(e.target.value)}
      placeholder="Search..."
    />
  );
}
```

### usePrevious

Stores the previous value of a prop or state:

```javascript
import { useRef, useEffect } from 'react';

function usePrevious(value) {
  const ref = useRef();
  
  useEffect(() => {
    ref.current = value;
  }, [value]);
  
  return ref.current;
}

// Usage
function Component({ count }) {
  const prevCount = usePrevious(count);
  
  return (
    <div>
      <p>Current: {count}</p>
      <p>Previous: {prevCount}</p>
    </div>
  );
}
```

## Testing Custom Hooks

Custom hooks can be tested using the React Testing Library's `renderHook` utility:

```javascript
import { renderHook, act } from '@testing-library/react';
import useCounter from './useCounter';

test('useCounter increments count', () => {
  const { result } = renderHook(() => useCounter(0));
  
  act(() => {
    result.current.increment();
  });
  
  expect(result.current.count).toBe(1);
});
```

## Conclusion

Custom hooks are a powerful pattern in React that promote code reuse and better organization. They allow you to:

- Extract component logic into reusable functions
- Share stateful logic between components
- Keep components focused on rendering
- Create cleaner, more maintainable code

Start simple and build more complex hooks as you become comfortable with the pattern. Remember to follow React's rules of hooks and naming conventions to ensure your custom hooks work correctly.