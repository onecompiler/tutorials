# useReducer Hook

The useReducer Hook is an alternative to useState for managing complex state logic. It's particularly useful when state updates depend on multiple values or when the next state depends on the previous one.

## Basic Syntax

```jsx
const [state, dispatch] = useReducer(reducer, initialState, init);
```

- `reducer`: Function that determines state changes
- `initialState`: Initial state value
- `init`: Optional function for lazy initialization
- `state`: Current state value
- `dispatch`: Function to trigger state updates

## Simple Example

```jsx
import React, { useReducer } from 'react';

// Reducer function
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return { count: 0 };
    default:
      throw new Error(`Unknown action: ${action.type}`);
  }
}

// Component
function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });
  
  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
}
```

## useState vs useReducer

### When to use useState:
- Simple state logic
- Single values or simple objects
- Independent state updates

```jsx
// Good for useState
const [isOpen, setIsOpen] = useState(false);
const [name, setName] = useState('');
```

### When to use useReducer:
- Complex state logic
- Multiple sub-values
- State updates depend on multiple values
- Next state depends on previous state

```jsx
// Better for useReducer
const [state, dispatch] = useReducer(formReducer, {
  name: '',
  email: '',
  errors: {},
  isSubmitting: false,
  isValid: false
});
```

## Complex State Example

```jsx
const initialState = {
  todos: [],
  filter: 'all', // all, active, completed
  loading: false,
  error: null
};

function todoReducer(state, action) {
  switch (action.type) {
    case 'ADD_TODO':
      return {
        ...state,
        todos: [...state.todos, {
          id: Date.now(),
          text: action.payload,
          completed: false
        }]
      };
      
    case 'TOGGLE_TODO':
      return {
        ...state,
        todos: state.todos.map(todo =>
          todo.id === action.payload
            ? { ...todo, completed: !todo.completed }
            : todo
        )
      };
      
    case 'DELETE_TODO':
      return {
        ...state,
        todos: state.todos.filter(todo => todo.id !== action.payload)
      };
      
    case 'SET_FILTER':
      return {
        ...state,
        filter: action.payload
      };
      
    case 'SET_LOADING':
      return {
        ...state,
        loading: action.payload
      };
      
    case 'SET_ERROR':
      return {
        ...state,
        error: action.payload,
        loading: false
      };
      
    case 'FETCH_TODOS_SUCCESS':
      return {
        ...state,
        todos: action.payload,
        loading: false,
        error: null
      };
      
    default:
      return state;
  }
}

function TodoApp() {
  const [state, dispatch] = useReducer(todoReducer, initialState);
  const [input, setInput] = useState('');
  
  const { todos, filter, loading, error } = state;
  
  // Filter todos based on current filter
  const filteredTodos = todos.filter(todo => {
    if (filter === 'active') return !todo.completed;
    if (filter === 'completed') return todo.completed;
    return true;
  });
  
  const addTodo = (e) => {
    e.preventDefault();
    if (input.trim()) {
      dispatch({ type: 'ADD_TODO', payload: input });
      setInput('');
    }
  };
  
  useEffect(() => {
    dispatch({ type: 'SET_LOADING', payload: true });
    
    fetchTodos()
      .then(todos => {
        dispatch({ type: 'FETCH_TODOS_SUCCESS', payload: todos });
      })
      .catch(error => {
        dispatch({ type: 'SET_ERROR', payload: error.message });
      });
  }, []);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <div>
      <form onSubmit={addTodo}>
        <input
          value={input}
          onChange={(e) => setInput(e.target.value)}
          placeholder="Add todo..."
        />
        <button type="submit">Add</button>
      </form>
      
      <div>
        <button onClick={() => dispatch({ type: 'SET_FILTER', payload: 'all' })}>
          All
        </button>
        <button onClick={() => dispatch({ type: 'SET_FILTER', payload: 'active' })}>
          Active
        </button>
        <button onClick={() => dispatch({ type: 'SET_FILTER', payload: 'completed' })}>
          Completed
        </button>
      </div>
      
      <ul>
        {filteredTodos.map(todo => (
          <li key={todo.id}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => dispatch({ type: 'TOGGLE_TODO', payload: todo.id })}
            />
            <span style={{
              textDecoration: todo.completed ? 'line-through' : 'none'
            }}>
              {todo.text}
            </span>
            <button onClick={() => dispatch({ type: 'DELETE_TODO', payload: todo.id })}>
              Delete
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

## Form Management with useReducer

```jsx
const initialFormState = {
  values: {
    username: '',
    email: '',
    password: '',
    confirmPassword: ''
  },
  errors: {},
  touched: {},
  isSubmitting: false
};

function formReducer(state, action) {
  switch (action.type) {
    case 'SET_FIELD_VALUE':
      return {
        ...state,
        values: {
          ...state.values,
          [action.field]: action.value
        }
      };
      
    case 'SET_FIELD_TOUCHED':
      return {
        ...state,
        touched: {
          ...state.touched,
          [action.field]: true
        }
      };
      
    case 'SET_FIELD_ERROR':
      return {
        ...state,
        errors: {
          ...state.errors,
          [action.field]: action.error
        }
      };
      
    case 'SET_ERRORS':
      return {
        ...state,
        errors: action.errors
      };
      
    case 'SET_SUBMITTING':
      return {
        ...state,
        isSubmitting: action.isSubmitting
      };
      
    case 'RESET_FORM':
      return initialFormState;
      
    default:
      return state;
  }
}

function RegistrationForm() {
  const [state, dispatch] = useReducer(formReducer, initialFormState);
  const { values, errors, touched, isSubmitting } = state;
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    dispatch({
      type: 'SET_FIELD_VALUE',
      field: name,
      value
    });
  };
  
  const handleBlur = (e) => {
    const { name } = e.target;
    dispatch({
      type: 'SET_FIELD_TOUCHED',
      field: name
    });
    validateField(name, values[name]);
  };
  
  const validateField = (name, value) => {
    let error = '';
    
    switch (name) {
      case 'username':
        if (!value) error = 'Username is required';
        else if (value.length < 3) error = 'Username must be at least 3 characters';
        break;
        
      case 'email':
        if (!value) error = 'Email is required';
        else if (!/\S+@\S+\.\S+/.test(value)) error = 'Email is invalid';
        break;
        
      case 'password':
        if (!value) error = 'Password is required';
        else if (value.length < 6) error = 'Password must be at least 6 characters';
        break;
        
      case 'confirmPassword':
        if (!value) error = 'Please confirm password';
        else if (value !== values.password) error = 'Passwords do not match';
        break;
    }
    
    dispatch({
      type: 'SET_FIELD_ERROR',
      field: name,
      error
    });
  };
  
  const validateForm = () => {
    const newErrors = {};
    
    Object.keys(values).forEach(field => {
      validateField(field, values[field]);
    });
    
    return Object.keys(newErrors).length === 0;
  };
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    if (!validateForm()) return;
    
    dispatch({ type: 'SET_SUBMITTING', isSubmitting: true });
    
    try {
      await submitForm(values);
      dispatch({ type: 'RESET_FORM' });
      alert('Registration successful!');
    } catch (error) {
      alert('Registration failed!');
    } finally {
      dispatch({ type: 'SET_SUBMITTING', isSubmitting: false });
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          name="username"
          value={values.username}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Username"
        />
        {touched.username && errors.username && (
          <span className="error">{errors.username}</span>
        )}
      </div>
      
      <div>
        <input
          name="email"
          type="email"
          value={values.email}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Email"
        />
        {touched.email && errors.email && (
          <span className="error">{errors.email}</span>
        )}
      </div>
      
      <div>
        <input
          name="password"
          type="password"
          value={values.password}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Password"
        />
        {touched.password && errors.password && (
          <span className="error">{errors.password}</span>
        )}
      </div>
      
      <div>
        <input
          name="confirmPassword"
          type="password"
          value={values.confirmPassword}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Confirm Password"
        />
        {touched.confirmPassword && errors.confirmPassword && (
          <span className="error">{errors.confirmPassword}</span>
        )}
      </div>
      
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Registering...' : 'Register'}
      </button>
    </form>
  );
}
```

## Async Actions with useReducer

```jsx
function dataFetchReducer(state, action) {
  switch (action.type) {
    case 'FETCH_INIT':
      return {
        ...state,
        isLoading: true,
        isError: false
      };
    case 'FETCH_SUCCESS':
      return {
        ...state,
        isLoading: false,
        isError: false,
        data: action.payload
      };
    case 'FETCH_FAILURE':
      return {
        ...state,
        isLoading: false,
        isError: true,
        error: action.payload
      };
    default:
      throw new Error();
  }
}

function useDataApi(initialUrl, initialData) {
  const [url, setUrl] = useState(initialUrl);
  const [state, dispatch] = useReducer(dataFetchReducer, {
    isLoading: false,
    isError: false,
    data: initialData,
    error: null
  });
  
  useEffect(() => {
    let didCancel = false;
    
    const fetchData = async () => {
      dispatch({ type: 'FETCH_INIT' });
      
      try {
        const response = await fetch(url);
        if (!response.ok) throw new Error('Network response was not ok');
        
        const result = await response.json();
        
        if (!didCancel) {
          dispatch({ type: 'FETCH_SUCCESS', payload: result });
        }
      } catch (error) {
        if (!didCancel) {
          dispatch({ type: 'FETCH_FAILURE', payload: error.message });
        }
      }
    };
    
    fetchData();
    
    return () => {
      didCancel = true;
    };
  }, [url]);
  
  return [state, setUrl];
}

// Usage
function DataFetchingComponent() {
  const [{ data, isLoading, isError, error }, doFetch] = useDataApi(
    '/api/data',
    []
  );
  
  return (
    <div>
      <button onClick={() => doFetch('/api/other-data')}>
        Fetch Other Data
      </button>
      
      {isError && <div>Error: {error}</div>}
      {isLoading ? (
        <div>Loading...</div>
      ) : (
        <ul>
          {data.map(item => (
            <li key={item.id}>{item.title}</li>
          ))}
        </ul>
      )}
    </div>
  );
}
```

## Combining with Context

```jsx
// Create contexts for state and dispatch
const StateContext = createContext();
const DispatchContext = createContext();

// Reducer
function appReducer(state, action) {
  switch (action.type) {
    case 'SET_USER':
      return { ...state, user: action.payload };
    case 'SET_THEME':
      return { ...state, theme: action.payload };
    case 'TOGGLE_SIDEBAR':
      return { ...state, sidebarOpen: !state.sidebarOpen };
    default:
      return state;
  }
}

// Provider component
function AppProvider({ children }) {
  const [state, dispatch] = useReducer(appReducer, {
    user: null,
    theme: 'light',
    sidebarOpen: true
  });
  
  return (
    <StateContext.Provider value={state}>
      <DispatchContext.Provider value={dispatch}>
        {children}
      </DispatchContext.Provider>
    </StateContext.Provider>
  );
}

// Custom hooks to use state and dispatch
function useAppState() {
  const context = useContext(StateContext);
  if (!context) {
    throw new Error('useAppState must be used within AppProvider');
  }
  return context;
}

function useAppDispatch() {
  const context = useContext(DispatchContext);
  if (!context) {
    throw new Error('useAppDispatch must be used within AppProvider');
  }
  return context;
}

// Usage in components
function UserProfile() {
  const { user } = useAppState();
  const dispatch = useAppDispatch();
  
  const logout = () => {
    dispatch({ type: 'SET_USER', payload: null });
  };
  
  return (
    <div>
      <h1>Welcome, {user?.name}!</h1>
      <button onClick={logout}>Logout</button>
    </div>
  );
}
```

## Lazy Initialization

```jsx
function init(initialCount) {
  // Expensive computation
  return {
    count: initialCount,
    history: [initialCount]
  };
}

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      const newCount = state.count + 1;
      return {
        count: newCount,
        history: [...state.history, newCount]
      };
    case 'decrement':
      const newCount = state.count - 1;
      return {
        count: newCount,
        history: [...state.history, newCount]
      };
    case 'reset':
      return init(action.payload);
    default:
      throw new Error();
  }
}

function Counter({ initialCount }) {
  // Lazy initialization
  const [state, dispatch] = useReducer(reducer, initialCount, init);
  
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset', payload: initialCount })}>
        Reset
      </button>
      <div>
        History: {state.history.join(', ')}
      </div>
    </>
  );
}
```

## Best Practices

1. **Keep reducers pure** - No side effects, always return new state
2. **Use descriptive action types** - 'ADD_TODO' not 'ADD'
3. **Handle all action types** - Use default case to handle errors
4. **Structure actions consistently** - Use `{ type, payload }` pattern
5. **Split complex reducers** - Use multiple reducers for different concerns
6. **Avoid deeply nested state** - Keep state structure flat when possible
7. **Use TypeScript** - Type your actions and state for better safety

## Common Patterns

### Action Creators
```jsx
// Define action creators for consistency
const actions = {
  addTodo: (text) => ({ type: 'ADD_TODO', payload: text }),
  toggleTodo: (id) => ({ type: 'TOGGLE_TODO', payload: id }),
  deleteTodo: (id) => ({ type: 'DELETE_TODO', payload: id })
};

// Usage
dispatch(actions.addTodo('Learn useReducer'));
```

### Reducer Composition
```jsx
function todosReducer(state, action) {
  // Handle todo-related actions
}

function filterReducer(state, action) {
  // Handle filter-related actions
}

function rootReducer(state, action) {
  return {
    todos: todosReducer(state.todos, action),
    filter: filterReducer(state.filter, action)
  };
}
```

useReducer is perfect for managing complex state logic in React. Use it when useState becomes unwieldy or when you need more predictable state updates!