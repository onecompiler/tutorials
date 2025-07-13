# useEffect Hook

The useEffect Hook lets you perform side effects in functional components. It serves the same purpose as componentDidMount, componentDidUpdate, and componentWillUnmount combined.

## Basic Syntax

```jsx
useEffect(() => {
  // Side effect logic here
  
  return () => {
    // Cleanup function (optional)
  };
}, [dependencies]); // Dependency array
```

## Understanding useEffect

### Runs After Every Render
```jsx
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    console.log(`Count is: ${count}`);
    // Runs after every render
  });
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### Runs Once on Mount
```jsx
function Example() {
  useEffect(() => {
    console.log('Component mounted');
    // Runs only once when component mounts
  }, []); // Empty dependency array
  
  return <div>Hello World</div>;
}
```

### Runs When Dependencies Change
```jsx
function Example({ userId }) {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    console.log(`Fetching user ${userId}`);
    fetchUser(userId).then(setUser);
  }, [userId]); // Runs when userId changes
  
  return <div>{user?.name}</div>;
}
```

## Cleanup Functions

Cleanup functions run before the component unmounts and before re-running the effect:

```jsx
function Timer() {
  const [seconds, setSeconds] = useState(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(s => s + 1);
    }, 1000);
    
    // Cleanup function
    return () => {
      console.log('Cleaning up timer');
      clearInterval(interval);
    };
  }, []); // Empty array = setup once, cleanup on unmount
  
  return <div>Seconds: {seconds}</div>;
}
```

## Common Use Cases

### Data Fetching
```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    // Prevent state updates after unmount
    let cancelled = false;
    
    async function fetchUser() {
      try {
        setLoading(true);
        setError(null);
        const response = await fetch(`/api/users/${userId}`);
        
        if (!response.ok) {
          throw new Error('Failed to fetch user');
        }
        
        const data = await response.json();
        
        if (!cancelled) {
          setUser(data);
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
    }
    
    fetchUser();
    
    // Cleanup function
    return () => {
      cancelled = true;
    };
  }, [userId]);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!user) return <div>No user found</div>;
  
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
}
```

### Event Listeners
```jsx
function MousePosition() {
  const [position, setPosition] = useState({ x: 0, y: 0 });
  
  useEffect(() => {
    function handleMouseMove(e) {
      setPosition({ x: e.clientX, y: e.clientY });
    }
    
    window.addEventListener('mousemove', handleMouseMove);
    
    // Cleanup
    return () => {
      window.removeEventListener('mousemove', handleMouseMove);
    };
  }, []); // Empty array = add listener once
  
  return (
    <div>
      Mouse position: {position.x}, {position.y}
    </div>
  );
}
```

### Subscriptions
```jsx
function ChatRoom({ roomId }) {
  const [messages, setMessages] = useState([]);
  
  useEffect(() => {
    const connection = createConnection(roomId);
    
    connection.on('message', (message) => {
      setMessages(prev => [...prev, message]);
    });
    
    connection.connect();
    
    return () => {
      connection.disconnect();
    };
  }, [roomId]);
  
  return (
    <div>
      {messages.map((msg, i) => (
        <div key={i}>{msg}</div>
      ))}
    </div>
  );
}
```

### Local Storage Sync
```jsx
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      return initialValue;
    }
  });
  
  useEffect(() => {
    try {
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error(`Error saving to localStorage:`, error);
    }
  }, [key, value]);
  
  return [value, setValue];
}

// Usage
function Settings() {
  const [theme, setTheme] = useLocalStorage('theme', 'light');
  
  return (
    <div>
      <p>Current theme: {theme}</p>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </div>
  );
}
```

## Dependency Array

### No Dependency Array
```jsx
useEffect(() => {
  console.log('Runs after every render');
});
```

### Empty Dependency Array
```jsx
useEffect(() => {
  console.log('Runs once on mount');
}, []);
```

### With Dependencies
```jsx
useEffect(() => {
  console.log('Runs when count or name changes');
}, [count, name]);
```

### Common Dependency Mistakes
```jsx
function SearchResults({ query }) {
  const [results, setResults] = useState([]);
  
  // ❌ Missing dependency
  useEffect(() => {
    searchAPI(query).then(setResults);
  }, []); // Should include query!
  
  // ✅ Correct
  useEffect(() => {
    searchAPI(query).then(setResults);
  }, [query]);
}
```

## Multiple Effects

Organize effects by concern:

```jsx
function UserDashboard({ userId }) {
  const [user, setUser] = useState(null);
  const [posts, setPosts] = useState([]);
  
  // Effect for user data
  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]);
  
  // Effect for user posts
  useEffect(() => {
    fetchUserPosts(userId).then(setPosts);
  }, [userId]);
  
  // Effect for page title
  useEffect(() => {
    document.title = user ? `${user.name}'s Dashboard` : 'Dashboard';
  }, [user]);
  
  return <div>{/* ... */}</div>;
}
```

## Timing and Order

### useEffect vs useLayoutEffect
```jsx
function Example() {
  const [show, setShow] = useState(false);
  
  // Runs asynchronously after paint
  useEffect(() => {
    console.log('useEffect');
  });
  
  // Runs synchronously before paint
  useLayoutEffect(() => {
    console.log('useLayoutEffect');
  });
  
  return <button onClick={() => setShow(!show)}>Toggle</button>;
}
```

## Advanced Patterns

### Debouncing
```jsx
function SearchInput() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  
  useEffect(() => {
    if (!query) {
      setResults([]);
      return;
    }
    
    const timeoutId = setTimeout(() => {
      console.log('Searching for:', query);
      searchAPI(query).then(setResults);
    }, 500); // 500ms delay
    
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
          <li key={result.id}>{result.title}</li>
        ))}
      </ul>
    </div>
  );
}
```

### Polling
```jsx
function LiveData({ url, interval = 5000 }) {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    let isActive = true;
    
    async function fetchData() {
      try {
        const response = await fetch(url);
        const newData = await response.json();
        
        if (isActive) {
          setData(newData);
        }
      } catch (error) {
        console.error('Fetch error:', error);
      }
    }
    
    fetchData(); // Initial fetch
    
    const intervalId = setInterval(fetchData, interval);
    
    return () => {
      isActive = false;
      clearInterval(intervalId);
    };
  }, [url, interval]);
  
  return <div>{data ? JSON.stringify(data) : 'Loading...'}</div>;
}
```

### Previous Value
```jsx
function usePrevious(value) {
  const ref = useRef();
  
  useEffect(() => {
    ref.current = value;
  });
  
  return ref.current;
}

// Usage
function Counter() {
  const [count, setCount] = useState(0);
  const prevCount = usePrevious(count);
  
  return (
    <div>
      <p>Now: {count}, before: {prevCount}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

## Common Pitfalls

### Infinite Loops
```jsx
// ❌ Infinite loop
function Bad() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    setCount(count + 1); // Causes re-render, which triggers effect again
  }); // No dependency array = runs after every render
}

// ✅ Fixed
function Good() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    setCount(c => c + 1);
  }, []); // Only runs once
}
```

### Stale Closures
```jsx
// ❌ Uses stale count value
function Timer() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      console.log(count); // Always logs 0
    }, 1000);
    
    return () => clearInterval(interval);
  }, []); // Empty deps, so count is captured once
}

// ✅ Fixed with functional update
function Timer() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setCount(c => {
        console.log(c); // Logs current value
        return c;
      });
    }, 1000);
    
    return () => clearInterval(interval);
  }, []);
}
```

## Best Practices

1. **Name your effects** - Use multiple effects for different concerns
2. **Always handle cleanup** - Prevent memory leaks
3. **Check if mounted** - Prevent state updates after unmount
4. **Use the dependency array correctly** - Include all dependencies
5. **Keep effects focused** - One effect per side effect
6. **Consider custom hooks** - Extract complex effect logic
7. **Use ESLint rules** - exhaustive-deps rule helps catch mistakes

## Custom Hooks with Effects

```jsx
function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(navigator.onLine);
  
  useEffect(() => {
    function handleOnline() {
      setIsOnline(true);
    }
    
    function handleOffline() {
      setIsOnline(false);
    }
    
    window.addEventListener('online', handleOnline);
    window.addEventListener('offline', handleOffline);
    
    return () => {
      window.removeEventListener('online', handleOnline);
      window.removeEventListener('offline', handleOffline);
    };
  }, []);
  
  return isOnline;
}

// Usage
function App() {
  const isOnline = useOnlineStatus();
  
  return (
    <div>
      {isOnline ? '✅ Online' : '❌ Offline'}
    </div>
  );
}
```

The useEffect Hook is essential for handling side effects in React. Master it to build applications that interact with the outside world!