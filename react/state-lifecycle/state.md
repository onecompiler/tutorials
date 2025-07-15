# State in React Components

State is data that changes over time in your React application. When state changes, React re-renders the component to reflect the new data.

## Understanding State

State is:
- Private to a component
- Mutable (can be changed)
- Triggers re-renders when updated
- Preserved between re-renders

```jsx
import React, { useState } from 'react';

function Counter() {
  // Declare state variable
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

## State in Functional Components (useState)

### Basic Usage
```jsx
function Example() {
  // Declare a state variable
  const [value, setValue] = useState(initialValue);
  
  // value: current state value
  // setValue: function to update state
  // initialValue: initial state value
}
```

### Multiple State Variables
```jsx
function UserProfile() {
  const [name, setName] = useState('');
  const [age, setAge] = useState(0);
  const [email, setEmail] = useState('');
  
  return (
    <form>
      <input
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Name"
      />
      <input
        type="number"
        value={age}
        onChange={(e) => setAge(parseInt(e.target.value))}
        placeholder="Age"
      />
      <input
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
      />
    </form>
  );
}
```

### Object State
```jsx
function UserForm() {
  const [user, setUser] = useState({
    name: '',
    email: '',
    age: 0
  });
  
  const updateUser = (field, value) => {
    setUser(prevUser => ({
      ...prevUser,
      [field]: value
    }));
  };
  
  return (
    <form>
      <input
        value={user.name}
        onChange={(e) => updateUser('name', e.target.value)}
        placeholder="Name"
      />
      <input
        value={user.email}
        onChange={(e) => updateUser('email', e.target.value)}
        placeholder="Email"
      />
    </form>
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
  
  const toggleTodo = (id) => {
    setTodos(todos.map(todo =>
      todo.id === id
        ? { ...todo, completed: !todo.completed }
        : todo
    ));
  };
  
  const deleteTodo = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };
  
  return (
    <div>
      <input
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
        onKeyPress={(e) => e.key === 'Enter' && addTodo()}
      />
      <button onClick={addTodo}>Add Todo</button>
      
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => toggleTodo(todo.id)}
            />
            <span className={todo.completed ? 'completed' : ''}>
              {todo.text}
            </span>
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

## State in Class Components

### Basic State
```jsx
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
  
  increment = () => {
    this.setState({ count: this.state.count + 1 });
  }
  
  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.increment}>Increment</button>
      </div>
    );
  }
}
```

### Updating State Correctly
```jsx
class Example extends React.Component {
  state = {
    count: 0,
    name: 'React'
  };
  
  // ❌ Wrong - Don't mutate state directly
  wrongUpdate = () => {
    this.state.count = 5; // Never do this!
  }
  
  // ✅ Correct - Use setState
  correctUpdate = () => {
    this.setState({ count: 5 });
  }
  
  // ✅ Update based on previous state
  incrementCount = () => {
    this.setState(prevState => ({
      count: prevState.count + 1
    }));
  }
  
  // ✅ Update multiple properties
  updateMultiple = () => {
    this.setState({
      count: 10,
      name: 'Updated React'
    });
  }
}
```

## Functional Updates

When new state depends on previous state, use functional updates:

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  
  // ❌ Might not work as expected with multiple rapid clicks
  const incrementWrong = () => {
    setCount(count + 1);
    setCount(count + 1);
    // Still increments by 1, not 2!
  };
  
  // ✅ Correct way using functional update
  const incrementCorrect = () => {
    setCount(prevCount => prevCount + 1);
    setCount(prevCount => prevCount + 1);
    // Increments by 2 as expected
  };
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={incrementCorrect}>+2</button>
    </div>
  );
}
```

## Lazy Initial State

For expensive computations, use lazy initialization:

```jsx
function ExpensiveComponent() {
  // ❌ Runs on every render
  const [data, setData] = useState(expensiveComputation());
  
  // ✅ Runs only once
  const [data, setData] = useState(() => expensiveComputation());
  
  return <div>{/* Use data */}</div>;
}

// Example with localStorage
function Settings() {
  const [settings, setSettings] = useState(() => {
    const saved = localStorage.getItem('settings');
    return saved ? JSON.parse(saved) : defaultSettings;
  });
  
  const updateSettings = (newSettings) => {
    setSettings(newSettings);
    localStorage.setItem('settings', JSON.stringify(newSettings));
  };
  
  return <div>{/* Settings UI */}</div>;
}
```

## State Management Patterns

### Lifting State Up
```jsx
function TemperatureInput({ scale, temperature, onTemperatureChange }) {
  return (
    <fieldset>
      <legend>Enter temperature in {scale}:</legend>
      <input
        value={temperature}
        onChange={e => onTemperatureChange(e.target.value)}
      />
    </fieldset>
  );
}

function Calculator() {
  const [temperature, setTemperature] = useState('');
  const [scale, setScale] = useState('celsius');
  
  const handleCelsiusChange = (temp) => {
    setScale('celsius');
    setTemperature(temp);
  };
  
  const handleFahrenheitChange = (temp) => {
    setScale('fahrenheit');
    setTemperature(temp);
  };
  
  const celsius = scale === 'fahrenheit' 
    ? tryConvert(temperature, toCelsius) 
    : temperature;
    
  const fahrenheit = scale === 'celsius' 
    ? tryConvert(temperature, toFahrenheit) 
    : temperature;
  
  return (
    <div>
      <TemperatureInput
        scale="celsius"
        temperature={celsius}
        onTemperatureChange={handleCelsiusChange}
      />
      <TemperatureInput
        scale="fahrenheit"
        temperature={fahrenheit}
        onTemperatureChange={handleFahrenheitChange}
      />
    </div>
  );
}
```

### Derived State
```jsx
function SearchableList({ items }) {
  const [searchTerm, setSearchTerm] = useState('');
  
  // Derived state - computed from other state
  const filteredItems = items.filter(item =>
    item.name.toLowerCase().includes(searchTerm.toLowerCase())
  );
  
  return (
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search..."
      />
      <ul>
        {filteredItems.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

## State vs Props

```jsx
function ParentComponent() {
  const [parentState, setParentState] = useState('Hello');
  
  return (
    <ChildComponent 
      propValue={parentState}  // Passed as prop
      onUpdate={setParentState}
    />
  );
}

function ChildComponent({ propValue, onUpdate }) {
  const [localState, setLocalState] = useState('World'); // Component's own state
  
  return (
    <div>
      <p>Prop from parent: {propValue}</p>
      <p>Local state: {localState}</p>
      <button onClick={() => onUpdate('Updated!')}>
        Update Parent State
      </button>
      <button onClick={() => setLocalState('Changed!')}>
        Update Local State
      </button>
    </div>
  );
}
```

## Common State Patterns

### Toggle State
```jsx
function Toggle() {
  const [isOn, setIsOn] = useState(false);
  
  return (
    <button onClick={() => setIsOn(!isOn)}>
      {isOn ? 'ON' : 'OFF'}
    </button>
  );
}
```

### Form State
```jsx
function ContactForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Form submitted:', formData);
    // Reset form
    setFormData({
      name: '',
      email: '',
      message: ''
    });
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        name="name"
        value={formData.name}
        onChange={handleChange}
        placeholder="Name"
      />
      <input
        name="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <textarea
        name="message"
        value={formData.message}
        onChange={handleChange}
        placeholder="Message"
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Loading State
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
  if (!data) return <button onClick={fetchData}>Load Data</button>;
  
  return <div>{/* Display data */}</div>;
}
```

## Best Practices

1. **Keep state as local as possible**
2. **Don't duplicate props in state**
3. **Use functional updates for state that depends on previous state**
4. **Group related state variables**
5. **Consider using useReducer for complex state logic**
6. **Avoid deeply nested state objects**
7. **Use lazy initialization for expensive computations**

## Common Mistakes

### Mutating State
```jsx
// ❌ Never mutate state directly
const [items, setItems] = useState([1, 2, 3]);

// Wrong
items.push(4);
setItems(items);

// ✅ Create a new array
setItems([...items, 4]);
```

### Stale Closures
```jsx
function Timer() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    const id = setInterval(() => {
      // ❌ This will always log 0
      console.log(count);
      
      // ✅ Use functional update
      setCount(c => c + 1);
    }, 1000);
    
    return () => clearInterval(id);
  }, []); // Empty dependency array
  
  return <div>Count: {count}</div>;
}
```

State is fundamental to React applications. Master state management and you'll be able to build dynamic, interactive user interfaces!