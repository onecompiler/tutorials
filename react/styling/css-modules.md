# CSS Modules in React

CSS Modules provide a way to write CSS that's scoped locally to components, avoiding global namespace pollution and style conflicts. They're a popular choice for styling React applications.

## What are CSS Modules?

CSS Modules automatically generate unique class names, ensuring styles don't leak between components. Files must be named with `.module.css` extension.

### Basic Setup
```jsx
// Button.module.css
.button {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.primary {
  background-color: #007bff;
  color: white;
}

.primary:hover {
  background-color: #0056b3;
}

.secondary {
  background-color: #6c757d;
  color: white;
}

.secondary:hover {
  background-color: #545b62;
}

// Button.js
import styles from './Button.module.css';

function Button({ variant = 'primary', children }) {
  return (
    <button className={`${styles.button} ${styles[variant]}`}>
      {children}
    </button>
  );
}
```

## Composing Classes

### Multiple Classes
```jsx
// Card.module.css
.card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 20px;
}

.elevated {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
}

.bordered {
  border: 1px solid #ddd;
}

.compact {
  padding: 10px;
}

// Card.js
import styles from './Card.module.css';

function Card({ elevated, bordered, compact, children }) {
  const classNames = [
    styles.card,
    elevated && styles.elevated,
    bordered && styles.bordered,
    compact && styles.compact
  ].filter(Boolean).join(' ');
  
  return (
    <div className={classNames}>
      {children}
    </div>
  );
}
```

### Composition in CSS
```jsx
// Typography.module.css
.base {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  line-height: 1.6;
}

.heading {
  composes: base;
  font-weight: 700;
  margin-bottom: 0.5em;
}

.h1 {
  composes: heading;
  font-size: 2.5rem;
}

.h2 {
  composes: heading;
  font-size: 2rem;
}

.paragraph {
  composes: base;
  margin-bottom: 1em;
  color: #333;
}

// Typography.js
import styles from './Typography.module.css';

export function H1({ children }) {
  return <h1 className={styles.h1}>{children}</h1>;
}

export function H2({ children }) {
  return <h2 className={styles.h2}>{children}</h2>;
}

export function Paragraph({ children }) {
  return <p className={styles.paragraph}>{children}</p>;
}
```

## Global Styles

### Using :global
```jsx
// Layout.module.css
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

/* Global styles */
:global(.no-scroll) {
  overflow: hidden;
}

:global(body) {
  margin: 0;
  padding: 0;
}

/* Global within local */
.header :global(.logo) {
  width: 150px;
  height: auto;
}

// Layout.js
import styles from './Layout.module.css';

function Layout({ children }) {
  const toggleScroll = () => {
    document.body.classList.toggle('no-scroll');
  };
  
  return (
    <div className={styles.container}>
      <header className={styles.header}>
        <img src="/logo.png" className="logo" alt="Logo" />
      </header>
      {children}
    </div>
  );
}
```

## Dynamic Styling

### Conditional Classes
```jsx
// Alert.module.css
.alert {
  padding: 15px;
  border-radius: 4px;
  margin-bottom: 20px;
  border: 1px solid transparent;
}

.success {
  background-color: #d4edda;
  border-color: #c3e6cb;
  color: #155724;
}

.warning {
  background-color: #fff3cd;
  border-color: #ffeeba;
  color: #856404;
}

.error {
  background-color: #f8d7da;
  border-color: #f5c6cb;
  color: #721c24;
}

.dismissible {
  padding-right: 40px;
  position: relative;
}

.dismissButton {
  position: absolute;
  top: 15px;
  right: 15px;
  background: none;
  border: none;
  font-size: 20px;
  cursor: pointer;
  color: inherit;
  opacity: 0.5;
}

.dismissButton:hover {
  opacity: 1;
}

// Alert.js
import { useState } from 'react';
import styles from './Alert.module.css';

function Alert({ type = 'info', dismissible = false, children }) {
  const [visible, setVisible] = useState(true);
  
  if (!visible) return null;
  
  const classNames = [
    styles.alert,
    styles[type],
    dismissible && styles.dismissible
  ].filter(Boolean).join(' ');
  
  return (
    <div className={classNames}>
      {children}
      {dismissible && (
        <button
          className={styles.dismissButton}
          onClick={() => setVisible(false)}
          aria-label="Dismiss"
        >
          ×
        </button>
      )}
    </div>
  );
}
```

## CSS Variables with Modules

### Dynamic Theming
```jsx
// Theme.module.css
.themed {
  --primary-color: #007bff;
  --secondary-color: #6c757d;
  --background-color: #ffffff;
  --text-color: #333333;
  --border-radius: 4px;
  --spacing-unit: 8px;
}

.themed.dark {
  --primary-color: #0d6efd;
  --secondary-color: #6c757d;
  --background-color: #212529;
  --text-color: #ffffff;
}

.container {
  background-color: var(--background-color);
  color: var(--text-color);
  padding: calc(var(--spacing-unit) * 2);
}

.button {
  background-color: var(--primary-color);
  color: white;
  padding: var(--spacing-unit) calc(var(--spacing-unit) * 2);
  border: none;
  border-radius: var(--border-radius);
}

// ThemedComponent.js
import { useState } from 'react';
import styles from './Theme.module.css';

function ThemedComponent() {
  const [isDark, setIsDark] = useState(false);
  
  return (
    <div className={`${styles.themed} ${isDark ? styles.dark : ''}`}>
      <div className={styles.container}>
        <h1>Themed Component</h1>
        <button
          className={styles.button}
          onClick={() => setIsDark(!isDark)}
        >
          Toggle Theme
        </button>
      </div>
    </div>
  );
}
```

## Advanced Patterns

### Component Library with CSS Modules
```jsx
// components/Button/Button.module.css
.button {
  font-family: inherit;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  font-weight: 500;
}

/* Variants */
.primary {
  background-color: #007bff;
  color: white;
}

.secondary {
  background-color: #6c757d;
  color: white;
}

.outline {
  background-color: transparent;
  border: 2px solid #007bff;
  color: #007bff;
}

/* Sizes */
.small {
  padding: 5px 10px;
  font-size: 14px;
}

.medium {
  padding: 10px 20px;
  font-size: 16px;
}

.large {
  padding: 15px 30px;
  font-size: 18px;
}

/* States */
.disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.loading {
  color: transparent;
  position: relative;
}

.loading::after {
  content: '';
  position: absolute;
  width: 16px;
  height: 16px;
  top: 50%;
  left: 50%;
  margin-left: -8px;
  margin-top: -8px;
  border: 2px solid #f3f3f3;
  border-top: 2px solid #333;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

// components/Button/Button.js
import styles from './Button.module.css';

function Button({
  variant = 'primary',
  size = 'medium',
  disabled = false,
  loading = false,
  children,
  className,
  ...props
}) {
  const classes = [
    styles.button,
    styles[variant],
    styles[size],
    disabled && styles.disabled,
    loading && styles.loading,
    className
  ].filter(Boolean).join(' ');
  
  return (
    <button
      className={classes}
      disabled={disabled || loading}
      {...props}
    >
      {children}
    </button>
  );
}

export default Button;
```

### Layout System
```jsx
// Grid.module.css
.container {
  width: 100%;
  padding-right: 15px;
  padding-left: 15px;
  margin-right: auto;
  margin-left: auto;
}

.row {
  display: flex;
  flex-wrap: wrap;
  margin-right: -15px;
  margin-left: -15px;
}

.col {
  position: relative;
  width: 100%;
  padding-right: 15px;
  padding-left: 15px;
  flex: 1 0 0%;
}

/* Column sizes */
.col1 { flex: 0 0 8.333333%; max-width: 8.333333%; }
.col2 { flex: 0 0 16.666667%; max-width: 16.666667%; }
.col3 { flex: 0 0 25%; max-width: 25%; }
.col4 { flex: 0 0 33.333333%; max-width: 33.333333%; }
.col6 { flex: 0 0 50%; max-width: 50%; }
.col12 { flex: 0 0 100%; max-width: 100%; }

/* Responsive */
@media (min-width: 768px) {
  .container {
    max-width: 750px;
  }
  
  .mdCol6 { flex: 0 0 50%; max-width: 50%; }
  .mdCol4 { flex: 0 0 33.333333%; max-width: 33.333333%; }
}

@media (min-width: 1024px) {
  .container {
    max-width: 1200px;
  }
  
  .lgCol3 { flex: 0 0 25%; max-width: 25%; }
  .lgCol4 { flex: 0 0 33.333333%; max-width: 33.333333%; }
}

// Grid.js
import styles from './Grid.module.css';

export function Container({ children, className }) {
  return (
    <div className={`${styles.container} ${className || ''}`}>
      {children}
    </div>
  );
}

export function Row({ children, className }) {
  return (
    <div className={`${styles.row} ${className || ''}`}>
      {children}
    </div>
  );
}

export function Col({ 
  children, 
  size = 12, 
  md, 
  lg, 
  className 
}) {
  const classes = [
    styles.col,
    styles[`col${size}`],
    md && styles[`mdCol${md}`],
    lg && styles[`lgCol${lg}`],
    className
  ].filter(Boolean).join(' ');
  
  return (
    <div className={classes}>
      {children}
    </div>
  );
}
```

## Utility Classes

### Creating Utilities
```jsx
// utilities.module.css
/* Spacing */
.mt1 { margin-top: 0.25rem; }
.mt2 { margin-top: 0.5rem; }
.mt3 { margin-top: 1rem; }
.mt4 { margin-top: 1.5rem; }
.mt5 { margin-top: 3rem; }

.mb1 { margin-bottom: 0.25rem; }
.mb2 { margin-bottom: 0.5rem; }
.mb3 { margin-bottom: 1rem; }
.mb4 { margin-bottom: 1.5rem; }
.mb5 { margin-bottom: 3rem; }

.p1 { padding: 0.25rem; }
.p2 { padding: 0.5rem; }
.p3 { padding: 1rem; }
.p4 { padding: 1.5rem; }
.p5 { padding: 3rem; }

/* Display */
.dBlock { display: block; }
.dInline { display: inline; }
.dInlineBlock { display: inline-block; }
.dFlex { display: flex; }
.dNone { display: none; }

/* Flexbox */
.flexRow { flex-direction: row; }
.flexColumn { flex-direction: column; }
.justifyStart { justify-content: flex-start; }
.justifyCenter { justify-content: center; }
.justifyEnd { justify-content: flex-end; }
.justifyBetween { justify-content: space-between; }
.alignStart { align-items: flex-start; }
.alignCenter { align-items: center; }
.alignEnd { align-items: flex-end; }

/* Text */
.textLeft { text-align: left; }
.textCenter { text-align: center; }
.textRight { text-align: right; }
.fontBold { font-weight: bold; }
.fontNormal { font-weight: normal; }

// Using utilities
import styles from './Component.module.css';
import utils from './utilities.module.css';

function Component() {
  return (
    <div className={`${styles.container} ${utils.p3} ${utils.mb4}`}>
      <h2 className={`${styles.title} ${utils.textCenter} ${utils.mb3}`}>
        Title
      </h2>
      <div className={`${utils.dFlex} ${utils.justifyBetween}`}>
        <span>Left</span>
        <span>Right</span>
      </div>
    </div>
  );
}
```

## TypeScript Support

### Type-Safe CSS Modules
```typescript
// Button.module.css.d.ts
export const button: string;
export const primary: string;
export const secondary: string;
export const small: string;
export const medium: string;
export const large: string;

// Or use typed-css-modules for auto-generation
// Button.tsx
import styles from './Button.module.css';

interface ButtonProps {
  variant?: 'primary' | 'secondary';
  size?: 'small' | 'medium' | 'large';
  children: React.ReactNode;
}

function Button({ 
  variant = 'primary', 
  size = 'medium', 
  children 
}: ButtonProps) {
  return (
    <button className={`${styles.button} ${styles[variant]} ${styles[size]}`}>
      {children}
    </button>
  );
}
```

## Best Practices

### File Organization
```
src/
  components/
    Button/
      Button.js
      Button.module.css
      Button.test.js
      index.js
    Card/
      Card.js
      Card.module.css
      index.js
  styles/
    globals.css
    variables.css
    utilities.module.css
```

### Naming Conventions
```jsx
// Component.module.css
/* Use camelCase for class names */
.componentContainer { }
.componentHeader { }
.componentBody { }

/* Use descriptive names */
.primaryButton { } /* Better than .btn1 */
.errorMessage { }  /* Better than .red-text */

/* Avoid generic names that might conflict */
.customCard { }    /* Better than .card */
.appHeader { }     /* Better than .header */
```

## Debugging Tips

### Development vs Production
```jsx
// In development, class names are readable:
// Button_button__2Ce79 Button_primary__1jIb1

// In production, they're minified:
// a b

// For debugging in production, configure webpack:
// webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        test: /\.module\.css$/,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              modules: {
                localIdentName: '[name]__[local]___[hash:base64:5]'
              }
            }
          }
        ]
      }
    ]
  }
};
```

## Best Practices Summary

1. **One module per component** - Keep styles close to components
2. **Use composition** - Leverage composes for shared styles
3. **Avoid @import in modules** - Import in JavaScript instead
4. **Keep specificity low** - Use single class selectors
5. **Use CSS variables** - For dynamic theming
6. **Type your modules** - Use TypeScript declarations
7. **Consistent naming** - Follow a naming convention
8. **Document complex styles** - Add comments for clarity

CSS Modules provide excellent encapsulation and are a great choice for component-based styling in React!