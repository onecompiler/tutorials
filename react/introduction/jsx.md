# JSX Basics

JSX (JavaScript XML) is a syntax extension for JavaScript that allows you to write HTML-like code in your JavaScript files. It makes writing React components more intuitive and readable.

## What is JSX?

JSX allows you to write HTML-like syntax directly in JavaScript:

```jsx
const element = <h1>Hello, world!</h1>;
```

This gets compiled to:
```javascript
const element = React.createElement('h1', null, 'Hello, world!');
```

## JSX Rules

### 1. Single Parent Element
JSX expressions must have one parent element:

```jsx
// ❌ Wrong - multiple elements
return (
  <h1>Title</h1>
  <p>Description</p>
);

// ✅ Correct - wrapped in parent
return (
  <div>
    <h1>Title</h1>
    <p>Description</p>
  </div>
);

// ✅ Using Fragment
return (
  <>
    <h1>Title</h1>
    <p>Description</p>
  </>
);
```

### 2. Close All Tags
All tags must be closed, including self-closing tags:

```jsx
// ❌ Wrong
<img src="image.jpg">
<br>

// ✅ Correct
<img src="image.jpg" />
<br />
```

### 3. camelCase Properties
HTML attributes become camelCase in JSX:

```jsx
// HTML attributes → JSX properties
// class → className
// for → htmlFor
// onclick → onClick
// tabindex → tabIndex

<div className="container">
  <label htmlFor="name">Name:</label>
  <input id="name" tabIndex="1" />
  <button onClick={handleClick}>Click me</button>
</div>
```

## Embedding Expressions

Use curly braces `{}` to embed JavaScript expressions:

```jsx
const name = 'John';
const age = 25;

const element = (
  <div>
    <h1>Hello, {name}!</h1>
    <p>Age: {age}</p>
    <p>Next year: {age + 1}</p>
    <p>Status: {age >= 18 ? 'Adult' : 'Minor'}</p>
  </div>
);
```

### Valid Expressions
```jsx
function App() {
  const user = {
    firstName: 'Jane',
    lastName: 'Doe'
  };
  
  const numbers = [1, 2, 3, 4, 5];
  
  return (
    <div>
      {/* Variables */}
      <h1>{user.firstName}</h1>
      
      {/* Function calls */}
      <p>{user.firstName.toUpperCase()}</p>
      
      {/* Arithmetic */}
      <p>{2 + 2}</p>
      
      {/* Array methods */}
      <p>{numbers.map(n => n * 2).join(', ')}</p>
      
      {/* Ternary operators */}
      <p>{numbers.length > 0 ? 'Has numbers' : 'Empty'}</p>
    </div>
  );
}
```

## Conditional Rendering

### Using Ternary Operator
```jsx
function Greeting({ isLoggedIn }) {
  return (
    <div>
      {isLoggedIn ? (
        <h1>Welcome back!</h1>
      ) : (
        <h1>Please sign in.</h1>
      )}
    </div>
  );
}
```

### Using Logical AND
```jsx
function Notification({ message, show }) {
  return (
    <div>
      {show && <p>{message}</p>}
    </div>
  );
}
```

### Using Functions
```jsx
function Status({ status }) {
  const renderStatus = () => {
    switch(status) {
      case 'loading':
        return <p>Loading...</p>;
      case 'error':
        return <p>Error occurred!</p>;
      case 'success':
        return <p>Success!</p>;
      default:
        return null;
    }
  };
  
  return <div>{renderStatus()}</div>;
}
```

## Lists in JSX

Render lists using `map()`:

```jsx
function TodoList({ items }) {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={item.id}>
          {item.text}
        </li>
      ))}
    </ul>
  );
}
```

## Styling in JSX

### Inline Styles
Use objects for inline styles:

```jsx
const styles = {
  color: 'blue',
  fontSize: '20px',
  backgroundColor: '#f0f0f0'
};

function StyledComponent() {
  return (
    <div style={styles}>
      Styled text
    </div>
  );
}

// Or directly inline
<div style={{ color: 'red', marginTop: '10px' }}>
  Inline styled
</div>
```

### CSS Classes
```jsx
import './App.css';

function App() {
  const isActive = true;
  
  return (
    <div className={`container ${isActive ? 'active' : ''}`}>
      <h1 className="title">Hello</h1>
    </div>
  );
}
```

## JSX Comments

```jsx
function App() {
  return (
    <div>
      {/* This is a JSX comment */}
      <h1>Hello</h1>
      {/* 
        Multi-line
        comment
      */}
    </div>
  );
}
```

## JSX Children

Components can receive children:

```jsx
function Card({ children, title }) {
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
  <button>Click me</button>
</Card>
```

## JSX Spread Attributes

Spread object properties as props:

```jsx
function Button(props) {
  return <button {...props} />;
}

// Usage
const buttonProps = {
  onClick: handleClick,
  className: 'primary-button',
  disabled: false
};

<Button {...buttonProps}>Click me</Button>
```

## Common JSX Patterns

### Fragment Shorthand
```jsx
// Long form
<React.Fragment>
  <h1>Title</h1>
  <p>Content</p>
</React.Fragment>

// Short form
<>
  <h1>Title</h1>
  <p>Content</p>
</>
```

### Conditional Attributes
```jsx
<input
  type="text"
  disabled={isDisabled}
  className={error ? 'error' : ''}
  placeholder={showPlaceholder ? 'Enter text' : undefined}
/>
```

### Dynamic Component Types
```jsx
function DynamicComponent({ componentType, children }) {
  const Component = componentType || 'div';
  
  return <Component>{children}</Component>;
}

// Usage
<DynamicComponent componentType="section">
  Content
</DynamicComponent>
```

## JSX Gotchas

### 1. Reserved Words
Some words are reserved in JavaScript:

```jsx
// ❌ Wrong
<div class="container">
<label for="email">

// ✅ Correct
<div className="container">
<label htmlFor="email">
```

### 2. Boolean Attributes
```jsx
// These are equivalent
<button disabled={true}>Click</button>
<button disabled>Click</button>

// For false, omit the attribute
<button disabled={false}>Click</button>
// Same as
<button>Click</button>
```

### 3. Whitespace
JSX removes whitespace at the beginning and end of lines:

```jsx
// This renders as "HelloWorld"
<div>
  Hello
  World
</div>

// Add explicit space
<div>
  Hello{' '}
  World
</div>
```

## Best Practices

1. **Keep JSX readable** - Break long elements into multiple lines
2. **Extract complex logic** - Move complex conditions to functions
3. **Use semantic HTML** - Use proper HTML elements
4. **Avoid inline styles** for complex styling
5. **Always add keys** to list items
6. **Close all tags** including self-closing ones
7. **Use fragments** to avoid unnecessary wrapper divs

## Next Steps

Now that you understand JSX, you're ready to learn about:
- React components and props
- State management
- Event handling
- Component lifecycle