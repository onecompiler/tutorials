# Functional Components

Functional components are the modern way to write React components. They are JavaScript functions that return JSX and can use hooks for state and lifecycle features.

## Basic Functional Component

A functional component is just a JavaScript function that returns JSX:

```jsx
function Welcome() {
  return <h1>Hello, World!</h1>;
}

// Arrow function syntax
const Welcome = () => {
  return <h1>Hello, World!</h1>;
};

// Implicit return for single expressions
const Welcome = () => <h1>Hello, World!</h1>;
```

## Components with Props

Functional components receive props as their first argument:

```jsx
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// With destructuring
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// Usage
<Greeting name="Alice" />
```

## Multiple Props

```jsx
function UserCard({ name, age, email, avatar }) {
  return (
    <div className="user-card">
      <img src={avatar} alt={name} />
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Email: {email}</p>
    </div>
  );
}

// Usage
<UserCard 
  name="John Doe"
  age={25}
  email="john@example.com"
  avatar="/avatar.jpg"
/>
```

## Default Props

Set default values for props:

```jsx
function Button({ text = 'Click me', color = 'blue', onClick }) {
  return (
    <button 
      style={{ backgroundColor: color }}
      onClick={onClick}
    >
      {text}
    </button>
  );
}

// Or using defaultProps (being phased out)
Button.defaultProps = {
  text: 'Click me',
  color: 'blue'
};
```

## Children Props

Components can accept and render children:

```jsx
function Card({ title, children }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div className="card-content">
        {children}
      </div>
    </div>
  );
}

// Usage
<Card title="My Card">
  <p>This is the card content</p>
  <button>Action</button>
</Card>
```

## Component Composition

Build complex UIs by composing smaller components:

```jsx
function Avatar({ src, alt }) {
  return <img className="avatar" src={src} alt={alt} />;
}

function UserInfo({ name, title }) {
  return (
    <div className="user-info">
      <h3>{name}</h3>
      <p>{title}</p>
    </div>
  );
}

function UserProfile({ user }) {
  return (
    <div className="user-profile">
      <Avatar src={user.avatar} alt={user.name} />
      <UserInfo name={user.name} title={user.title} />
    </div>
  );
}
```

## Conditional Rendering

```jsx
function Notification({ type, message }) {
  const getIcon = () => {
    switch(type) {
      case 'success': return '✅';
      case 'error': return '❌';
      case 'warning': return '⚠️';
      default: return 'ℹ️';
    }
  };
  
  return (
    <div className={`notification ${type}`}>
      <span>{getIcon()}</span>
      <span>{message}</span>
    </div>
  );
}
```

## Lists and Keys

```jsx
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map(todo => (
        <TodoItem key={todo.id} todo={todo} />
      ))}
    </ul>
  );
}

function TodoItem({ todo }) {
  return (
    <li className={todo.completed ? 'completed' : ''}>
      {todo.text}
    </li>
  );
}
```

## Event Handling

```jsx
function Counter() {
  const [count, setCount] = React.useState(0);
  
  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  const reset = () => setCount(0);
  
  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}
```

## Component with State

Using the useState hook:

```jsx
function Toggle() {
  const [isOn, setIsOn] = React.useState(false);
  
  return (
    <div>
      <p>The switch is {isOn ? 'ON' : 'OFF'}</p>
      <button onClick={() => setIsOn(!isOn)}>
        Toggle
      </button>
    </div>
  );
}
```

## Component with Effects

Using the useEffect hook:

```jsx
function Timer() {
  const [seconds, setSeconds] = React.useState(0);
  
  React.useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(s => s + 1);
    }, 1000);
    
    // Cleanup function
    return () => clearInterval(interval);
  }, []); // Empty dependency array = run once
  
  return <div>Seconds: {seconds}</div>;
}
```

## Props Validation with PropTypes

```jsx
import PropTypes from 'prop-types';

function UserProfile({ name, age, email, isActive }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Email: {email}</p>
      <p>Status: {isActive ? 'Active' : 'Inactive'}</p>
    </div>
  );
}

UserProfile.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number.isRequired,
  email: PropTypes.string.isRequired,
  isActive: PropTypes.bool
};
```

## Pure Components

Functional components are "pure" when they always render the same output for the same props:

```jsx
// Pure component
function PureGreeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// Not pure - uses external data
let globalName = 'World';
function ImpureGreeting() {
  return <h1>Hello, {globalName}!</h1>;
}
```

## Memoization

Optimize performance with React.memo:

```jsx
const ExpensiveComponent = React.memo(function ExpensiveComponent({ data }) {
  console.log('Rendering ExpensiveComponent');
  // Complex calculations
  return <div>{/* Render data */}</div>;
});

// With custom comparison
const MyComponent = React.memo(
  function MyComponent({ value, label }) {
    return <div>{label}: {value}</div>;
  },
  (prevProps, nextProps) => {
    // Return true if props are equal (skip re-render)
    return prevProps.value === nextProps.value;
  }
);
```

## Best Practices

### 1. Keep Components Small and Focused
```jsx
// Good - Single responsibility
function UserAvatar({ src, alt }) {
  return <img className="avatar" src={src} alt={alt} />;
}

// Bad - Doing too much
function UserSection({ user, posts, comments }) {
  // Too much logic in one component
}
```

### 2. Use Descriptive Names
```jsx
// Good
function ProductCard({ product }) { }
function NavigationMenu({ items }) { }

// Bad
function Component1({ data }) { }
function MyComp({ stuff }) { }
```

### 3. Destructure Props
```jsx
// Good
function Profile({ name, age, location }) {
  return <div>{name}, {age}, {location}</div>;
}

// Less readable
function Profile(props) {
  return <div>{props.name}, {props.age}, {props.location}</div>;
}
```

### 4. Handle Loading and Error States
```jsx
function DataDisplay({ loading, error, data }) {
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  
  return (
    <div>
      {data.map(item => (
        <div key={item.id}>{item.name}</div>
      ))}
    </div>
  );
}
```

## Common Patterns

### Container and Presentational Components
```jsx
// Container (handles logic)
function UserListContainer() {
  const [users, setUsers] = React.useState([]);
  const [loading, setLoading] = React.useState(true);
  
  React.useEffect(() => {
    fetchUsers().then(data => {
      setUsers(data);
      setLoading(false);
    });
  }, []);
  
  return <UserList users={users} loading={loading} />;
}

// Presentational (handles display)
function UserList({ users, loading }) {
  if (loading) return <div>Loading...</div>;
  
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

### Compound Components
```jsx
function Tabs({ children }) {
  const [activeTab, setActiveTab] = React.useState(0);
  
  return (
    <div className="tabs">
      {React.Children.map(children, (child, index) =>
        React.cloneElement(child, { activeTab, setActiveTab, index })
      )}
    </div>
  );
}

function TabList({ children, activeTab, setActiveTab }) {
  return (
    <div className="tab-list">
      {React.Children.map(children, (child, index) =>
        React.cloneElement(child, { 
          isActive: activeTab === index,
          onClick: () => setActiveTab(index)
        })
      )}
    </div>
  );
}

function Tab({ children, isActive, onClick }) {
  return (
    <button className={isActive ? 'active' : ''} onClick={onClick}>
      {children}
    </button>
  );
}
```

## Next Steps

Now that you understand functional components, you can:
- Learn about class components (legacy)
- Dive deeper into React Hooks
- Explore advanced patterns
- Build complex applications with functional components