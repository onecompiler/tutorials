# useState Hook

The useState Hook is the most fundamental Hook in React. It allows functional components to have state, which was previously only possible with class components.

## Basic Syntax

```jsx
const [state, setState] = useState(initialState);
```

- `state`: The current state value
- `setState`: Function to update the state
- `initialState`: The initial state value

## Simple Examples

### Counter
```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

### Text Input
```jsx
function TextInput() {
  const [text, setText] = useState('');
  
  return (
    <div>
      <input
        type="text"
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Type something..."
      />
      <p>You typed: {text}</p>
      <p>Character count: {text.length}</p>
    </div>
  );
}
```

### Toggle Boolean
```jsx
function Toggle() {
  const [isOn, setIsOn] = useState(false);
  
  return (
    <div>
      <button onClick={() => setIsOn(!isOn)}>
        {isOn ? 'ON' : 'OFF'}
      </button>
      <p>The switch is {isOn ? 'on' : 'off'}</p>
    </div>
  );
}
```

## Different State Types

### Primitive Values
```jsx
function PrimitiveStates() {
  const [count, setCount] = useState(0);          // number
  const [name, setName] = useState('');           // string
  const [isVisible, setIsVisible] = useState(true); // boolean
  const [data, setData] = useState(null);         // null
  const [id, setId] = useState(undefined);        // undefined
  
  return <div>{/* Use states */}</div>;
}
```

### Object State
```jsx
function UserProfile() {
  const [user, setUser] = useState({
    name: '',
    email: '',
    age: 0
  });
  
  // Update entire object
  const updateUser = (newUser) => {
    setUser(newUser);
  };
  
  // Update specific field
  const updateField = (field, value) => {
    setUser(prevUser => ({
      ...prevUser,
      [field]: value
    }));
  };
  
  return (
    <div>
      <input
        value={user.name}
        onChange={(e) => updateField('name', e.target.value)}
        placeholder="Name"
      />
      <input
        value={user.email}
        onChange={(e) => updateField('email', e.target.value)}
        placeholder="Email"
      />
      <input
        type="number"
        value={user.age}
        onChange={(e) => updateField('age', parseInt(e.target.value))}
        placeholder="Age"
      />
    </div>
  );
}
```

### Array State
```jsx
function TodoList() {
  const [todos, setTodos] = useState([]);
  const [inputValue, setInputValue] = useState('');
  
  const addTodo = () => {
    if (inputValue.trim()) {
      setTodos([...todos, {
        id: Date.now(),
        text: inputValue,
        completed: false
      }]);
      setInputValue('');
    }
  };
  
  const removeTodo = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };
  
  const toggleTodo = (id) => {
    setTodos(todos.map(todo =>
      todo.id === id
        ? { ...todo, completed: !todo.completed }
        : todo
    ));
  };
  
  return (
    <div>
      <input
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
        placeholder="Add todo..."
      />
      <button onClick={addTodo}>Add</button>
      
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => toggleTodo(todo.id)}
            />
            <span style={{
              textDecoration: todo.completed ? 'line-through' : 'none'
            }}>
              {todo.text}
            </span>
            <button onClick={() => removeTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

## Functional Updates

When new state depends on previous state, use the functional form:

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  
  // ❌ May not work correctly
  const incrementTwiceBad = () => {
    setCount(count + 1);
    setCount(count + 1); // Still increments by 1!
  };
  
  // ✅ Correct way
  const incrementTwiceGood = () => {
    setCount(prevCount => prevCount + 1);
    setCount(prevCount => prevCount + 1); // Increments by 2
  };
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={incrementTwiceGood}>+2</button>
    </div>
  );
}
```

### Why Functional Updates?
```jsx
function DelayedCounter() {
  const [count, setCount] = useState(0);
  
  const handleClick = () => {
    setTimeout(() => {
      // ❌ Uses stale count value
      // setCount(count + 1);
      
      // ✅ Always uses current value
      setCount(prevCount => prevCount + 1);
    }, 3000);
  };
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleClick}>Increment after 3s</button>
    </div>
  );
}
```

## Lazy Initial State

For expensive computations, use a function to initialize state:

```jsx
// ❌ Expensive function runs on every render
function ExpensiveComponent() {
  const [data, setData] = useState(expensiveComputation());
  return <div>{/* ... */}</div>;
}

// ✅ Function only runs once
function ExpensiveComponent() {
  const [data, setData] = useState(() => expensiveComputation());
  return <div>{/* ... */}</div>;
}

// Real example
function TodoApp() {
  const [todos, setTodos] = useState(() => {
    // Only runs on first render
    const saved = localStorage.getItem('todos');
    return saved ? JSON.parse(saved) : [];
  });
  
  useEffect(() => {
    localStorage.setItem('todos', JSON.stringify(todos));
  }, [todos]);
  
  return <div>{/* ... */}</div>;
}
```

## Multiple State Variables vs Single Object

### Multiple State Variables (Recommended)
```jsx
function Form() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [age, setAge] = useState(0);
  
  // Easy to update individually
  // Clear what each setter does
  // Can have different types
}
```

### Single Object State
```jsx
function Form() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    age: 0
  });
  
  // Must spread to update
  const updateField = (field, value) => {
    setFormData(prev => ({ ...prev, [field]: value }));
  };
}
```

## State Update Batching

React batches state updates for performance:

```jsx
function BatchingExample() {
  const [count, setCount] = useState(0);
  const [flag, setFlag] = useState(false);
  
  const handleClick = () => {
    // These updates are batched
    setCount(c => c + 1);
    setFlag(f => !f);
    // Only one re-render happens
  };
  
  console.log('Rendered with', { count, flag });
  
  return (
    <div>
      <button onClick={handleClick}>Update Both</button>
      <p>Count: {count}, Flag: {flag.toString()}</p>
    </div>
  );
}
```

## Common Patterns

### Form Handling
```jsx
function ContactForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });
  const [errors, setErrors] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    // Clear error when user types
    if (errors[name]) {
      setErrors(prev => ({ ...prev, [name]: '' }));
    }
  };
  
  const validate = () => {
    const newErrors = {};
    if (!formData.name) newErrors.name = 'Name required';
    if (!formData.email) newErrors.email = 'Email required';
    if (!formData.message) newErrors.message = 'Message required';
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    if (!validate()) return;
    
    setIsSubmitting(true);
    try {
      // Submit form
      await submitForm(formData);
      // Reset form
      setFormData({ name: '', email: '', message: '' });
    } finally {
      setIsSubmitting(false);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        name="name"
        value={formData.name}
        onChange={handleChange}
        placeholder="Name"
      />
      {errors.name && <span>{errors.name}</span>}
      
      <input
        name="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      {errors.email && <span>{errors.email}</span>}
      
      <textarea
        name="message"
        value={formData.message}
        onChange={handleChange}
        placeholder="Message"
      />
      {errors.message && <span>{errors.message}</span>}
      
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Submitting...' : 'Submit'}
      </button>
    </form>
  );
}
```

### Loading States
```jsx
function DataFetcher() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  
  const fetchData = async () => {
    setLoading(true);
    setError(null);
    try {
      const response = await fetch('/api/data');
      const result = await response.json();
      setData(result);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <div>
      <button onClick={fetchData}>Fetch Data</button>
      {data && <pre>{JSON.stringify(data, null, 2)}</pre>}
    </div>
  );
}
```

### Undo/Redo
```jsx
function UndoableCounter() {
  const [count, setCount] = useState(0);
  const [history, setHistory] = useState([0]);
  const [currentIndex, setCurrentIndex] = useState(0);
  
  const updateCount = (newCount) => {
    const newHistory = history.slice(0, currentIndex + 1);
    newHistory.push(newCount);
    
    setHistory(newHistory);
    setCurrentIndex(newHistory.length - 1);
    setCount(newCount);
  };
  
  const undo = () => {
    if (currentIndex > 0) {
      setCurrentIndex(currentIndex - 1);
      setCount(history[currentIndex - 1]);
    }
  };
  
  const redo = () => {
    if (currentIndex < history.length - 1) {
      setCurrentIndex(currentIndex + 1);
      setCount(history[currentIndex + 1]);
    }
  };
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => updateCount(count + 1)}>+</button>
      <button onClick={() => updateCount(count - 1)}>-</button>
      <button onClick={undo} disabled={currentIndex === 0}>Undo</button>
      <button onClick={redo} disabled={currentIndex === history.length - 1}>
        Redo
      </button>
    </div>
  );
}
```

## Best Practices

1. **Keep state minimal** - Only store what you can't compute
2. **Lift state up** when needed by multiple components
3. **Use functional updates** when depending on previous state
4. **Don't mutate state** - Always create new objects/arrays
5. **Group related state** - But don't overdo it
6. **Initialize expensive state lazily**
7. **Clear state** when components unmount if needed

## Common Mistakes

### Mutating State
```jsx
// ❌ Never mutate state directly
const [user, setUser] = useState({ name: 'John', age: 30 });

// Wrong
user.name = 'Jane';
setUser(user);

// ✅ Create new object
setUser({ ...user, name: 'Jane' });
```

### Using State Immediately
```jsx
// ❌ State updates are asynchronous
const [count, setCount] = useState(0);

const handleClick = () => {
  setCount(count + 1);
  console.log(count); // Still logs old value!
};

// ✅ Use the new value directly
const handleClick = () => {
  const newCount = count + 1;
  setCount(newCount);
  console.log(newCount); // Logs new value
};
```

The useState Hook is the foundation of state management in functional components. Master it and you'll be able to build any interactive UI!