# Introduction to Hooks

Hooks are functions that let you use state and other React features in functional components. They were introduced in React 16.8 and have revolutionized how we write React components.

## What are Hooks?

Hooks are JavaScript functions that:
- Start with "use" (e.g., useState, useEffect)
- Let you "hook into" React features
- Can only be called at the top level of functions
- Enable functional components to have state and lifecycle methods

## Why Hooks?

### Before Hooks
```jsx
// Had to use class components for state
class Counter extends React.Component {
  state = { count: 0 };
  
  increment = () => {
    this.setState({ count: this.state.count + 1 });
  }
  
  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.increment}>+</button>
      </div>
    );
  }
}
```

### With Hooks
```jsx
// Same functionality with less code
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  );
}
```

## Rules of Hooks

### 1. Only Call Hooks at the Top Level
```jsx
// ❌ Don't call Hooks inside conditions
function BadExample({ someCondition }) {
  if (someCondition) {
    const [value, setValue] = useState(0); // Wrong!
  }
}

// ✅ Call Hooks at the top level
function GoodExample({ someCondition }) {
  const [value, setValue] = useState(0); // Correct
  
  if (someCondition) {
    // Use the state here
  }
}
```

### 2. Only Call Hooks from React Functions
```jsx
// ✅ Call from React functional components
function MyComponent() {
  const [state, setState] = useState(0); // Good
  return <div>{state}</div>;
}

// ✅ Call from custom Hooks
function useMyCustomHook() {
  const [state, setState] = useState(0); // Good
  return state;
}

// ❌ Don't call from regular JavaScript functions
function regularFunction() {
  const [state, setState] = useState(0); // Wrong!
}
```

## Built-in Hooks Overview

### Basic Hooks
- **useState**: Manage local state
- **useEffect**: Perform side effects
- **useContext**: Subscribe to React context

### Additional Hooks
- **useReducer**: Complex state management
- **useCallback**: Memoize callbacks
- **useMemo**: Memoize expensive computations
- **useRef**: Persist values/access DOM
- **useImperativeHandle**: Customize instance value
- **useLayoutEffect**: Synchronous effects
- **useDebugValue**: Custom hook debugging

## Your First Hook: useState

```jsx
import React, { useState } from 'react';

function Example() {
  // Declare a state variable
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

### What's happening here?
1. `useState(0)` declares a state variable with initial value 0
2. Returns an array: `[currentValue, setterFunction]`
3. We destructure it to `count` and `setCount`
4. `count` holds the current value
5. `setCount` updates the value and triggers re-render

## Multiple State Variables

```jsx
function UserForm() {
  const [name, setName] = useState('');
  const [age, setAge] = useState(0);
  const [email, setEmail] = useState('');
  
  return (
    <form>
      <input
        value={name}
        onChange={e => setName(e.target.value)}
        placeholder="Name"
      />
      <input
        type="number"
        value={age}
        onChange={e => setAge(parseInt(e.target.value))}
        placeholder="Age"
      />
      <input
        value={email}
        onChange={e => setEmail(e.target.value)}
        placeholder="Email"
      />
    </form>
  );
}
```

## Hooks vs Class Components

### State Management
```jsx
// Class Component
class ClassExample extends React.Component {
  state = {
    name: '',
    age: 0
  };
  
  updateName = (name) => {
    this.setState({ name });
  }
}

// Hooks
function HooksExample() {
  const [name, setName] = useState('');
  const [age, setAge] = useState(0);
}
```

### Lifecycle Methods
```jsx
// Class Component
class ClassExample extends React.Component {
  componentDidMount() {
    console.log('Mounted');
  }
  
  componentDidUpdate() {
    console.log('Updated');
  }
  
  componentWillUnmount() {
    console.log('Unmounting');
  }
}

// Hooks
function HooksExample() {
  useEffect(() => {
    console.log('Mounted or Updated');
    
    return () => {
      console.log('Unmounting');
    };
  });
}
```

## Benefits of Hooks

### 1. Simpler Code
```jsx
// Class: ~20 lines for a counter
// Hooks: ~10 lines for the same functionality
```

### 2. Better Code Reuse
```jsx
// Custom hook for window size
function useWindowSize() {
  const [size, setSize] = useState({ width: 0, height: 0 });
  
  useEffect(() => {
    function handleResize() {
      setSize({
        width: window.innerWidth,
        height: window.innerHeight
      });
    }
    
    window.addEventListener('resize', handleResize);
    handleResize();
    
    return () => window.removeEventListener('resize', handleResize);
  }, []);
  
  return size;
}

// Use in any component
function MyComponent() {
  const { width, height } = useWindowSize();
  return <div>Window: {width} x {height}</div>;
}
```

### 3. Better Testing
- Easier to test pure functions
- Less mocking required
- More predictable behavior

### 4. Better TypeScript Support
```typescript
// Easy to type with TypeScript
function useCounter(initial: number = 0) {
  const [count, setCount] = useState<number>(initial);
  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  
  return { count, increment, decrement };
}
```

## Common Hook Patterns

### Toggle State
```jsx
function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);
  const toggle = () => setValue(!value);
  return [value, toggle];
}

// Usage
function Component() {
  const [isOpen, toggleOpen] = useToggle();
  
  return (
    <div>
      <button onClick={toggleOpen}>Toggle</button>
      {isOpen && <div>Content</div>}
    </div>
  );
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
function Component({ count }) {
  const prevCount = usePrevious(count);
  return <div>Now: {count}, Before: {prevCount}</div>;
}
```

## Migrating to Hooks

### Step-by-Step Migration
1. Start with new components
2. Identify stateful class components
3. Convert one at a time
4. Extract custom hooks for shared logic
5. Keep both versions during transition

### Example Migration
```jsx
// Before: Class Component
class Timer extends React.Component {
  state = { seconds: 0 };
  
  componentDidMount() {
    this.interval = setInterval(() => {
      this.setState({ seconds: this.state.seconds + 1 });
    }, 1000);
  }
  
  componentWillUnmount() {
    clearInterval(this.interval);
  }
  
  render() {
    return <div>Seconds: {this.state.seconds}</div>;
  }
}

// After: Hooks
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

## Best Practices

1. **Follow the rules** - Use ESLint plugin
2. **Start simple** - Don't over-engineer
3. **Extract custom hooks** - For reusable logic
4. **Keep hooks pure** - Avoid side effects in render
5. **Use the right hook** - Don't useState for everything

## Next Steps

Ready to dive deeper? Continue with:
- useState Hook - State management
- useEffect Hook - Side effects
- Custom Hooks - Creating your own
- Advanced Hooks - useReducer, useContext, etc.

Hooks have transformed React development, making it more functional and easier to understand. Start using them today!