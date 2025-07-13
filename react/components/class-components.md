# Class Components

Class components are the traditional way of writing React components. While functional components with hooks are now preferred, understanding class components is important for maintaining older codebases.

## Basic Class Component

A class component is a JavaScript class that extends React.Component:

```jsx
import React, { Component } from 'react';

class Welcome extends Component {
  render() {
    return <h1>Hello, World!</h1>;
  }
}

// Or with React.Component
class Welcome extends React.Component {
  render() {
    return <h1>Hello, World!</h1>;
  }
}
```

## Components with Props

Props are accessed via `this.props`:

```jsx
class Greeting extends Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}

// Usage
<Greeting name="Alice" />
```

## State in Class Components

State is managed using `this.state` and `this.setState()`:

```jsx
class Counter extends Component {
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

## Alternative State Initialization

Using class properties (modern syntax):

```jsx
class Counter extends Component {
  state = {
    count: 0
  };
  
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

## Component Lifecycle Methods

### Mounting Phase
```jsx
class LifecycleDemo extends Component {
  constructor(props) {
    super(props);
    console.log('1. Constructor');
    this.state = { data: null };
  }
  
  static getDerivedStateFromProps(props, state) {
    console.log('2. getDerivedStateFromProps');
    return null;
  }
  
  componentDidMount() {
    console.log('4. componentDidMount');
    // Good place for API calls
    fetch('/api/data')
      .then(res => res.json())
      .then(data => this.setState({ data }));
  }
  
  render() {
    console.log('3. Render');
    return <div>Lifecycle Demo</div>;
  }
}
```

### Updating Phase
```jsx
class UpdateDemo extends Component {
  shouldComponentUpdate(nextProps, nextState) {
    console.log('shouldComponentUpdate');
    // Return false to prevent re-render
    return true;
  }
  
  getSnapshotBeforeUpdate(prevProps, prevState) {
    console.log('getSnapshotBeforeUpdate');
    return null;
  }
  
  componentDidUpdate(prevProps, prevState, snapshot) {
    console.log('componentDidUpdate');
    // Good place to update DOM or make API calls
    if (prevProps.userId !== this.props.userId) {
      this.fetchUserData(this.props.userId);
    }
  }
  
  render() {
    return <div>Update Demo</div>;
  }
}
```

### Unmounting Phase
```jsx
class CleanupDemo extends Component {
  componentDidMount() {
    this.timer = setInterval(() => {
      console.log('Timer tick');
    }, 1000);
  }
  
  componentWillUnmount() {
    console.log('componentWillUnmount');
    // Cleanup timers, subscriptions, etc.
    clearInterval(this.timer);
  }
  
  render() {
    return <div>Cleanup Demo</div>;
  }
}
```

## Event Handling

### Method Binding
```jsx
class EventDemo extends Component {
  constructor(props) {
    super(props);
    this.state = { message: '' };
    
    // Method 1: Bind in constructor
    this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick() {
    this.setState({ message: 'Button clicked!' });
  }
  
  // Method 2: Arrow function property
  handleReset = () => {
    this.setState({ message: '' });
  }
  
  render() {
    return (
      <div>
        <p>{this.state.message}</p>
        <button onClick={this.handleClick}>Click me</button>
        <button onClick={this.handleReset}>Reset</button>
        {/* Method 3: Arrow function in render (not recommended) */}
        <button onClick={() => this.setState({ message: 'Inline!' })}>
          Inline Handler
        </button>
      </div>
    );
  }
}
```

## Updating State Correctly

### State Updates May Be Asynchronous
```jsx
class StateUpdate extends Component {
  state = { count: 0 };
  
  // Wrong - might not work as expected
  incrementWrong = () => {
    this.setState({ count: this.state.count + 1 });
    this.setState({ count: this.state.count + 1 });
    this.setState({ count: this.state.count + 1 });
    // May only increment by 1!
  }
  
  // Correct - use function form
  incrementCorrect = () => {
    this.setState(prevState => ({ count: prevState.count + 1 }));
    this.setState(prevState => ({ count: prevState.count + 1 }));
    this.setState(prevState => ({ count: prevState.count + 1 }));
    // Will increment by 3
  }
  
  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.incrementCorrect}>Increment</button>
      </div>
    );
  }
}
```

### State Updates Are Merged
```jsx
class UserProfile extends Component {
  state = {
    name: 'John',
    age: 25,
    email: 'john@example.com'
  };
  
  updateName = () => {
    // Only updates name, preserves age and email
    this.setState({ name: 'Jane' });
  }
  
  updateMultiple = () => {
    // Updates multiple properties
    this.setState({
      name: 'Bob',
      age: 30
    });
  }
}
```

## Complex Example

```jsx
class TodoList extends Component {
  state = {
    todos: [],
    inputValue: '',
    filter: 'all' // all, active, completed
  };
  
  componentDidMount() {
    const savedTodos = localStorage.getItem('todos');
    if (savedTodos) {
      this.setState({ todos: JSON.parse(savedTodos) });
    }
  }
  
  componentDidUpdate(prevProps, prevState) {
    if (prevState.todos !== this.state.todos) {
      localStorage.setItem('todos', JSON.stringify(this.state.todos));
    }
  }
  
  handleInputChange = (e) => {
    this.setState({ inputValue: e.target.value });
  }
  
  addTodo = (e) => {
    e.preventDefault();
    if (!this.state.inputValue.trim()) return;
    
    const newTodo = {
      id: Date.now(),
      text: this.state.inputValue,
      completed: false
    };
    
    this.setState(prevState => ({
      todos: [...prevState.todos, newTodo],
      inputValue: ''
    }));
  }
  
  toggleTodo = (id) => {
    this.setState(prevState => ({
      todos: prevState.todos.map(todo =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    }));
  }
  
  deleteTodo = (id) => {
    this.setState(prevState => ({
      todos: prevState.todos.filter(todo => todo.id !== id)
    }));
  }
  
  getFilteredTodos = () => {
    const { todos, filter } = this.state;
    switch (filter) {
      case 'active':
        return todos.filter(todo => !todo.completed);
      case 'completed':
        return todos.filter(todo => todo.completed);
      default:
        return todos;
    }
  }
  
  render() {
    const filteredTodos = this.getFilteredTodos();
    
    return (
      <div className="todo-app">
        <h1>Todo List</h1>
        
        <form onSubmit={this.addTodo}>
          <input
            type="text"
            value={this.state.inputValue}
            onChange={this.handleInputChange}
            placeholder="Add a todo..."
          />
          <button type="submit">Add</button>
        </form>
        
        <div className="filters">
          <button onClick={() => this.setState({ filter: 'all' })}>
            All
          </button>
          <button onClick={() => this.setState({ filter: 'active' })}>
            Active
          </button>
          <button onClick={() => this.setState({ filter: 'completed' })}>
            Completed
          </button>
        </div>
        
        <ul>
          {filteredTodos.map(todo => (
            <li key={todo.id}>
              <input
                type="checkbox"
                checked={todo.completed}
                onChange={() => this.toggleTodo(todo.id)}
              />
              <span className={todo.completed ? 'completed' : ''}>
                {todo.text}
              </span>
              <button onClick={() => this.deleteTodo(todo.id)}>
                Delete
              </button>
            </li>
          ))}
        </ul>
      </div>
    );
  }
}
```

## Error Boundaries

Class components can catch JavaScript errors:

```jsx
class ErrorBoundary extends Component {
  state = { hasError: false, error: null };
  
  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('Error caught:', error, errorInfo);
    // Log to error reporting service
  }
  
  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h2>Something went wrong.</h2>
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

// Usage
<ErrorBoundary>
  <App />
</ErrorBoundary>
```

## Converting to Functional Components

Class component:
```jsx
class Timer extends Component {
  state = { seconds: 0 };
  
  componentDidMount() {
    this.interval = setInterval(() => {
      this.setState(prevState => ({
        seconds: prevState.seconds + 1
      }));
    }, 1000);
  }
  
  componentWillUnmount() {
    clearInterval(this.interval);
  }
  
  render() {
    return <div>Seconds: {this.state.seconds}</div>;
  }
}
```

Equivalent functional component:
```jsx
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

## When to Use Class Components

While functional components are preferred, class components are still used for:
1. Legacy codebases
2. Error boundaries (currently only available in class components)
3. When working with older libraries that expect class components

## Best Practices

1. **Bind methods properly** - Use arrow functions or bind in constructor
2. **Don't mutate state** - Always use setState
3. **Clean up in componentWillUnmount** - Remove timers, subscriptions
4. **Use PureComponent** for performance when appropriate
5. **Keep render method pure** - No side effects
6. **Initialize state properly** - In constructor or class properties