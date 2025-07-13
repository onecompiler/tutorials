# CSS Styling in React

React provides several ways to style components using CSS. This tutorial covers various approaches from basic CSS files to inline styles, each with their own benefits and use cases.

## Traditional CSS Files

### Basic CSS Import
```jsx
// styles.css
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.title {
  color: #333;
  font-size: 2rem;
  margin-bottom: 1rem;
}

.button {
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.button:hover {
  background-color: #0056b3;
}

// App.js
import './styles.css';

function App() {
  return (
    <div className="container">
      <h1 className="title">Welcome to React</h1>
      <button className="button">Click me</button>
    </div>
  );
}
```

### Component-Specific CSS
```jsx
// Button.css
.btn {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-primary {
  background-color: #007bff;
  color: white;
}

.btn-primary:hover {
  background-color: #0056b3;
}

.btn-secondary {
  background-color: #6c757d;
  color: white;
}

.btn-secondary:hover {
  background-color: #545b62;
}

.btn-large {
  padding: 15px 30px;
  font-size: 18px;
}

// Button.js
import './Button.css';

function Button({ variant = 'primary', size = 'normal', children, ...props }) {
  const classNames = [
    'btn',
    `btn-${variant}`,
    size === 'large' && 'btn-large'
  ].filter(Boolean).join(' ');
  
  return (
    <button className={classNames} {...props}>
      {children}
    </button>
  );
}
```

## Inline Styles

### Basic Inline Styles
```jsx
function InlineStyleExample() {
  const styles = {
    container: {
      backgroundColor: '#f8f9fa',
      padding: '20px',
      borderRadius: '8px'
    },
    heading: {
      color: '#343a40',
      fontSize: '24px',
      marginBottom: '10px'
    },
    text: {
      color: '#6c757d',
      lineHeight: '1.6'
    }
  };
  
  return (
    <div style={styles.container}>
      <h2 style={styles.heading}>Inline Styles</h2>
      <p style={styles.text}>
        This component uses inline styles defined as JavaScript objects.
      </p>
    </div>
  );
}
```

### Dynamic Inline Styles
```jsx
function DynamicStyles({ theme = 'light', size = 'medium' }) {
  const themes = {
    light: {
      backgroundColor: '#ffffff',
      color: '#333333'
    },
    dark: {
      backgroundColor: '#333333',
      color: '#ffffff'
    }
  };
  
  const sizes = {
    small: { padding: '5px 10px', fontSize: '14px' },
    medium: { padding: '10px 20px', fontSize: '16px' },
    large: { padding: '15px 30px', fontSize: '18px' }
  };
  
  const combinedStyles = {
    ...themes[theme],
    ...sizes[size],
    border: '1px solid #ddd',
    borderRadius: '4px',
    transition: 'all 0.3s ease'
  };
  
  return (
    <div style={combinedStyles}>
      Dynamic styled component
    </div>
  );
}
```

## CSS Classes with Conditions

### Conditional Classes
```jsx
function ConditionalClasses({ isActive, isDisabled, size }) {
  // Manual string concatenation
  const className = `
    button
    ${isActive ? 'button-active' : ''}
    ${isDisabled ? 'button-disabled' : ''}
    ${size ? `button-${size}` : ''}
  `.trim();
  
  return <button className={className}>Button</button>;
}

// Using array and join
function BetterConditionalClasses({ isActive, isDisabled, size }) {
  const classes = [
    'button',
    isActive && 'button-active',
    isDisabled && 'button-disabled',
    size && `button-${size}`
  ].filter(Boolean).join(' ');
  
  return <button className={classes}>Button</button>;
}
```

### Utility Function for Classes
```jsx
// classNames utility
function classNames(...classes) {
  return classes.filter(Boolean).join(' ');
}

// Usage
function Component({ variant, size, isActive, className }) {
  return (
    <div
      className={classNames(
        'base-class',
        variant && `variant-${variant}`,
        size && `size-${size}`,
        isActive && 'active',
        className // Additional classes from props
      )}
    >
      Content
    </div>
  );
}
```

## CSS Variables

### Using CSS Custom Properties
```jsx
// styles.css
:root {
  --primary-color: #007bff;
  --secondary-color: #6c757d;
  --success-color: #28a745;
  --danger-color: #dc3545;
  --spacing-unit: 8px;
  --border-radius: 4px;
}

.button {
  background-color: var(--primary-color);
  padding: calc(var(--spacing-unit) * 2);
  border-radius: var(--border-radius);
}

// Dynamic CSS Variables
function ThemedComponent({ primaryColor, spacing }) {
  const style = {
    '--primary-color': primaryColor || '#007bff',
    '--spacing': `${spacing}px` || '8px'
  };
  
  return (
    <div style={style} className="themed-container">
      <button className="themed-button">
        Themed Button
      </button>
    </div>
  );
}
```

## Responsive Design

### Media Queries in CSS
```jsx
// responsive.css
.container {
  width: 100%;
  padding: 20px;
}

.grid {
  display: grid;
  gap: 20px;
  grid-template-columns: 1fr;
}

/* Tablet */
@media (min-width: 768px) {
  .container {
    max-width: 750px;
    margin: 0 auto;
  }
  
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Desktop */
@media (min-width: 1024px) {
  .container {
    max-width: 1200px;
  }
  
  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* Large Desktop */
@media (min-width: 1440px) {
  .grid {
    grid-template-columns: repeat(4, 1fr);
  }
}
```

### Responsive Utilities
```jsx
// Responsive component
function ResponsiveComponent() {
  const [windowSize, setWindowSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight
  });
  
  useEffect(() => {
    const handleResize = () => {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight
      });
    };
    
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);
  
  const isMobile = windowSize.width < 768;
  const isTablet = windowSize.width >= 768 && windowSize.width < 1024;
  const isDesktop = windowSize.width >= 1024;
  
  return (
    <div className={`
      component
      ${isMobile ? 'component-mobile' : ''}
      ${isTablet ? 'component-tablet' : ''}
      ${isDesktop ? 'component-desktop' : ''}
    `}>
      {isMobile && <MobileLayout />}
      {isTablet && <TabletLayout />}
      {isDesktop && <DesktopLayout />}
    </div>
  );
}
```

## Animations and Transitions

### CSS Animations
```jsx
// animations.css
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slideIn {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(0);
  }
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
  100% {
    transform: scale(1);
  }
}

.fade-in {
  animation: fadeIn 0.5s ease-out;
}

.slide-in {
  animation: slideIn 0.3s ease-out;
}

.pulse {
  animation: pulse 2s infinite;
}

// Component with animations
function AnimatedComponent({ animate }) {
  return (
    <div className={animate ? 'box fade-in' : 'box'}>
      <h2 className="slide-in">Animated Content</h2>
      <button className="pulse">Pulsing Button</button>
    </div>
  );
}
```

### Transition Effects
```jsx
// transitions.css
.card {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.expand-collapse {
  overflow: hidden;
  transition: max-height 0.3s ease;
}

.expand-collapse.collapsed {
  max-height: 0;
}

.expand-collapse.expanded {
  max-height: 500px; /* Adjust based on content */
}

// Expandable component
function ExpandableCard({ title, children }) {
  const [isExpanded, setIsExpanded] = useState(false);
  
  return (
    <div className="card">
      <h3 onClick={() => setIsExpanded(!isExpanded)}>
        {title} {isExpanded ? '−' : '+'}
      </h3>
      <div className={`expand-collapse ${isExpanded ? 'expanded' : 'collapsed'}`}>
        <div className="content">
          {children}
        </div>
      </div>
    </div>
  );
}
```

## Theme Support

### CSS-Based Theming
```jsx
// themes.css
/* Light theme (default) */
:root {
  --bg-primary: #ffffff;
  --bg-secondary: #f8f9fa;
  --text-primary: #212529;
  --text-secondary: #6c757d;
  --border-color: #dee2e6;
}

/* Dark theme */
[data-theme="dark"] {
  --bg-primary: #212529;
  --bg-secondary: #343a40;
  --text-primary: #f8f9fa;
  --text-secondary: #adb5bd;
  --border-color: #495057;
}

.app {
  background-color: var(--bg-primary);
  color: var(--text-primary);
  min-height: 100vh;
}

.card {
  background-color: var(--bg-secondary);
  border: 1px solid var(--border-color);
  color: var(--text-primary);
}

// Theme provider component
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  useEffect(() => {
    document.documentElement.setAttribute('data-theme', theme);
  }, [theme]);
  
  const toggleTheme = () => {
    setTheme(prevTheme => prevTheme === 'light' ? 'dark' : 'light');
  };
  
  return (
    <div className="app">
      <button onClick={toggleTheme}>
        Switch to {theme === 'light' ? 'dark' : 'light'} theme
      </button>
      {children}
    </div>
  );
}
```

## Complex Styling Patterns

### Compound Component Styles
```jsx
// Card.css
.card {
  border: 1px solid #ddd;
  border-radius: 8px;
  overflow: hidden;
}

.card-header {
  background-color: #f8f9fa;
  padding: 15px 20px;
  border-bottom: 1px solid #ddd;
}

.card-body {
  padding: 20px;
}

.card-footer {
  background-color: #f8f9fa;
  padding: 15px 20px;
  border-top: 1px solid #ddd;
}

// Card components
function Card({ children, className }) {
  return (
    <div className={`card ${className || ''}`}>
      {children}
    </div>
  );
}

Card.Header = function CardHeader({ children }) {
  return <div className="card-header">{children}</div>;
};

Card.Body = function CardBody({ children }) {
  return <div className="card-body">{children}</div>;
};

Card.Footer = function CardFooter({ children }) {
  return <div className="card-footer">{children}</div>;
};

// Usage
function Example() {
  return (
    <Card>
      <Card.Header>Card Title</Card.Header>
      <Card.Body>Card content goes here</Card.Body>
      <Card.Footer>Card footer</Card.Footer>
    </Card>
  );
}
```

### Dynamic Styling with Props
```jsx
function DynamicButton({ 
  variant = 'primary',
  size = 'medium',
  fullWidth = false,
  rounded = false,
  children 
}) {
  const baseStyles = {
    border: 'none',
    cursor: 'pointer',
    fontWeight: '500',
    transition: 'all 0.3s ease',
    display: 'inline-block',
    textAlign: 'center'
  };
  
  const variants = {
    primary: {
      backgroundColor: '#007bff',
      color: 'white'
    },
    secondary: {
      backgroundColor: '#6c757d',
      color: 'white'
    },
    outline: {
      backgroundColor: 'transparent',
      color: '#007bff',
      border: '2px solid #007bff'
    }
  };
  
  const sizes = {
    small: {
      padding: '5px 10px',
      fontSize: '14px'
    },
    medium: {
      padding: '10px 20px',
      fontSize: '16px'
    },
    large: {
      padding: '15px 30px',
      fontSize: '18px'
    }
  };
  
  const style = {
    ...baseStyles,
    ...variants[variant],
    ...sizes[size],
    width: fullWidth ? '100%' : 'auto',
    borderRadius: rounded ? '50px' : '4px'
  };
  
  return (
    <button style={style}>
      {children}
    </button>
  );
}
```

## Best Practices

1. **Organize styles** - Keep related styles together
2. **Use CSS variables** - For consistent theming
3. **Mobile-first approach** - Start with mobile styles
4. **Avoid inline styles** - Use for dynamic values only
5. **Namespace classes** - Prevent conflicts with BEM or similar
6. **Optimize for performance** - Minimize re-renders from style changes
7. **Use CSS-in-JS** - For component-scoped styles (covered in other tutorials)
8. **Consistent naming** - Follow a naming convention

CSS styling in React is flexible and powerful. Choose the approach that best fits your project's needs!