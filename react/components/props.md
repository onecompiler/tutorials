# Props in React

Props (short for properties) are how components communicate with each other in React. They are read-only data passed from parent components to child components.

## What are Props?

Props are arguments passed into React components, similar to function parameters. They allow you to pass data and event handlers down the component tree.

```jsx
// Parent component passes props
<Greeting name="Alice" age={25} />

// Child component receives props
function Greeting(props) {
  return <h1>Hello, {props.name}! You are {props.age} years old.</h1>;
}
```

## Passing Props

### Basic Props
```jsx
function App() {
  return (
    <div>
      <UserCard 
        name="John Doe"
        email="john@example.com"
        isActive={true}
        age={30}
      />
    </div>
  );
}

function UserCard(props) {
  return (
    <div className="user-card">
      <h2>{props.name}</h2>
      <p>Email: {props.email}</p>
      <p>Age: {props.age}</p>
      <p>Status: {props.isActive ? 'Active' : 'Inactive'}</p>
    </div>
  );
}
```

### Different Types of Props
```jsx
function PropsExample() {
  const user = { name: 'Alice', id: 1 };
  const hobbies = ['reading', 'coding', 'gaming'];
  
  return (
    <Profile
      // String prop
      name="John"
      // Number prop
      age={25}
      // Boolean prop
      isVerified={true}
      // Object prop
      user={user}
      // Array prop
      hobbies={hobbies}
      // Function prop
      onClick={() => console.log('Clicked!')}
      // JSX prop
      icon={<span>👤</span>}
    />
  );
}
```

## Destructuring Props

Make your code cleaner by destructuring props:

```jsx
// Without destructuring
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// With destructuring in function parameter
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// With destructuring in function body
function Greeting(props) {
  const { name, age, city } = props;
  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>Age: {age}, City: {city}</p>
    </div>
  );
}
```

## Default Props

Set default values for props:

```jsx
// Using default parameters
function Button({ text = 'Click me', color = 'blue', size = 'medium' }) {
  return (
    <button className={`btn btn-${color} btn-${size}`}>
      {text}
    </button>
  );
}

// Using defaultProps (older approach)
function Button({ text, color, size }) {
  return (
    <button className={`btn btn-${color} btn-${size}`}>
      {text}
    </button>
  );
}

Button.defaultProps = {
  text: 'Click me',
  color: 'blue',
  size: 'medium'
};
```

## Props.children

Access content passed between component tags:

```jsx
function Card({ title, children }) {
  return (
    <div className="card">
      <div className="card-header">
        <h3>{title}</h3>
      </div>
      <div className="card-body">
        {children}
      </div>
    </div>
  );
}

// Usage
function App() {
  return (
    <Card title="User Profile">
      <p>Name: John Doe</p>
      <p>Email: john@example.com</p>
      <button>Edit Profile</button>
    </Card>
  );
}
```

## Spreading Props

Pass all props or specific props to child components:

```jsx
function Button(props) {
  return <button {...props} />;
}

// More controlled spreading
function CustomButton({ variant, size, ...otherProps }) {
  return (
    <button 
      className={`btn btn-${variant} btn-${size}`}
      {...otherProps}
    />
  );
}

// Usage
<CustomButton 
  variant="primary"
  size="large"
  onClick={handleClick}
  disabled={false}
  type="submit"
/>
```

## Props Validation

Use PropTypes to validate props (in development):

```jsx
import PropTypes from 'prop-types';

function UserProfile({ name, age, email, isAdmin, onUpdate }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Email: {email}</p>
      {isAdmin && <p>Administrator</p>}
      <button onClick={onUpdate}>Update</button>
    </div>
  );
}

UserProfile.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number.isRequired,
  email: PropTypes.string.isRequired,
  isAdmin: PropTypes.bool,
  onUpdate: PropTypes.func.isRequired
};

// More PropTypes examples
MyComponent.propTypes = {
  // Basic types
  optionalString: PropTypes.string,
  optionalNumber: PropTypes.number,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalObject: PropTypes.object,
  optionalArray: PropTypes.array,
  
  // Specific shapes
  user: PropTypes.shape({
    id: PropTypes.number.isRequired,
    name: PropTypes.string.isRequired
  }),
  
  // Array of specific type
  items: PropTypes.arrayOf(PropTypes.string),
  
  // One of specific values
  status: PropTypes.oneOf(['pending', 'approved', 'rejected']),
  
  // One of many types
  value: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number
  ]),
  
  // Custom validator
  customProp: function(props, propName, componentName) {
    if (!/^[0-9]+$/.test(props[propName])) {
      return new Error('Invalid prop');
    }
  }
};
```

## Passing Functions as Props

Handle events in parent components:

```jsx
function TodoApp() {
  const [todos, setTodos] = useState([]);
  
  const addTodo = (text) => {
    setTodos([...todos, { id: Date.now(), text, completed: false }]);
  };
  
  const toggleTodo = (id) => {
    setTodos(todos.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
  };
  
  return (
    <div>
      <TodoForm onAddTodo={addTodo} />
      <TodoList todos={todos} onToggle={toggleTodo} />
    </div>
  );
}

function TodoForm({ onAddTodo }) {
  const [input, setInput] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    if (input.trim()) {
      onAddTodo(input);
      setInput('');
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder="Add todo..."
      />
      <button type="submit">Add</button>
    </form>
  );
}
```

## Props vs State

Understanding the difference:

```jsx
function ParentComponent() {
  const [count, setCount] = useState(0); // State - internal, mutable
  
  return (
    <ChildComponent 
      count={count}              // Props - external, immutable
      onIncrement={() => setCount(count + 1)}
    />
  );
}

function ChildComponent({ count, onIncrement }) {
  // Can't do this - props are read-only!
  // count = 10; // ❌ Error
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={onIncrement}>Increment</button>
    </div>
  );
}
```

## Conditional Props

Pass props conditionally:

```jsx
function ConditionalProps() {
  const isSpecial = true;
  const additionalProps = isSpecial ? { special: true, badge: '⭐' } : {};
  
  return (
    <div>
      <Item 
        name="Regular Item"
        {...additionalProps}
      />
      
      {/* Or inline */}
      <Item
        name="Another Item"
        {...(isSpecial && { special: true, badge: '⭐' })}
      />
    </div>
  );
}
```

## Common Patterns

### Render Props
```jsx
function DataFetcher({ render, url }) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(data => {
        setData(data);
        setLoading(false);
      });
  }, [url]);
  
  return render({ data, loading });
}

// Usage
<DataFetcher 
  url="/api/users"
  render={({ data, loading }) => (
    loading ? <p>Loading...</p> : <UserList users={data} />
  )}
/>
```

### Prop Drilling Problem
```jsx
// Problem: Passing props through multiple levels
function App() {
  const [user, setUser] = useState({ name: 'John', theme: 'dark' });
  
  return <Dashboard user={user} />;
}

function Dashboard({ user }) {
  return <Header user={user} />;
}

function Header({ user }) {
  return <UserMenu user={user} />;
}

function UserMenu({ user }) {
  return <span>Welcome, {user.name}!</span>;
}

// Solution: Use Context API or state management
```

## Best Practices

1. **Keep props minimal** - Only pass what's needed
2. **Use descriptive names** - `isLoading` better than `loading`
3. **Validate props** - Use PropTypes or TypeScript
4. **Avoid passing entire objects** when only few properties are needed
5. **Document complex props** with comments
6. **Use default values** for optional props
7. **Prefer composition** over prop drilling

## Common Mistakes

### Mutating Props
```jsx
// ❌ Never mutate props
function Bad({ user }) {
  user.name = 'New Name'; // Don't do this!
  return <div>{user.name}</div>;
}

// ✅ Create new object
function Good({ user }) {
  const updatedUser = { ...user, name: 'New Name' };
  return <div>{updatedUser.name}</div>;
}
```

### Using Index as Key
```jsx
// ❌ Avoid using index as key for dynamic lists
items.map((item, index) => <Item key={index} {...item} />)

// ✅ Use stable, unique ID
items.map(item => <Item key={item.id} {...item} />)
```

## Summary

Props are fundamental to React's component architecture. They enable:
- Component reusability
- Data flow from parent to child
- Component composition
- Event handling delegation
- Separation of concerns

Master props and you'll be able to build complex, maintainable React applications!