# Styled Components in React

Styled Components is a CSS-in-JS library that allows you to write CSS code inside JavaScript. It provides component-level styling with dynamic capabilities and automatic critical CSS.

## Getting Started

First, install styled-components:
```bash
npm install styled-components
```

### Basic Usage
```jsx
import styled from 'styled-components';

// Create a styled component
const Button = styled.button`
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  
  &:hover {
    background-color: #0056b3;
  }
`;

// Use it like a regular component
function App() {
  return (
    <div>
      <Button>Click me</Button>
    </div>
  );
}
```

## Props-Based Styling

### Dynamic Styles
```jsx
const Button = styled.button`
  background-color: ${props => props.primary ? '#007bff' : '#6c757d'};
  color: white;
  padding: ${props => props.size === 'large' ? '15px 30px' : '10px 20px'};
  border: none;
  border-radius: 4px;
  font-size: ${props => props.size === 'large' ? '18px' : '16px'};
  cursor: pointer;
  width: ${props => props.fullWidth ? '100%' : 'auto'};
  
  &:hover {
    background-color: ${props => props.primary ? '#0056b3' : '#545b62'};
  }
  
  &:disabled {
    opacity: 0.6;
    cursor: not-allowed;
  }
`;

// Usage
function Example() {
  return (
    <>
      <Button primary>Primary Button</Button>
      <Button>Secondary Button</Button>
      <Button primary size="large">Large Primary</Button>
      <Button fullWidth>Full Width Button</Button>
      <Button disabled>Disabled Button</Button>
    </>
  );
}
```

### Complex Props Logic
```jsx
const Container = styled.div`
  padding: ${props => {
    switch(props.size) {
      case 'small': return '10px';
      case 'medium': return '20px';
      case 'large': return '30px';
      default: return '20px';
    }
  }};
  
  background-color: ${props => props.theme.colors[props.variant] || '#fff'};
  
  border: ${props => props.bordered ? '1px solid #ddd' : 'none'};
  border-radius: ${props => props.rounded ? '8px' : '0'};
  
  box-shadow: ${props => {
    if (props.elevated === 'high') return '0 10px 30px rgba(0,0,0,0.2)';
    if (props.elevated) return '0 2px 10px rgba(0,0,0,0.1)';
    return 'none';
  }};
`;
```

## Extending Styles

### Inheritance
```jsx
// Base button
const Button = styled.button`
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.3s ease;
`;

// Extended button
const PrimaryButton = styled(Button)`
  background-color: #007bff;
  color: white;
  
  &:hover {
    background-color: #0056b3;
  }
`;

const OutlineButton = styled(Button)`
  background-color: transparent;
  color: #007bff;
  border: 2px solid #007bff;
  
  &:hover {
    background-color: #007bff;
    color: white;
  }
`;
```

### Styling Any Component
```jsx
// Style any component that accepts className
const Link = ({ className, children }) => (
  <a className={className} href="#">
    {children}
  </a>
);

const StyledLink = styled(Link)`
  color: #007bff;
  text-decoration: none;
  
  &:hover {
    text-decoration: underline;
  }
`;

// Works with third-party components too
import { Link as RouterLink } from 'react-router-dom';

const StyledRouterLink = styled(RouterLink)`
  color: inherit;
  text-decoration: none;
  
  &:hover {
    color: #007bff;
  }
`;
```

## Theming

### Theme Provider
```jsx
import { ThemeProvider } from 'styled-components';

// Define theme
const lightTheme = {
  colors: {
    primary: '#007bff',
    secondary: '#6c757d',
    success: '#28a745',
    danger: '#dc3545',
    background: '#ffffff',
    text: '#333333'
  },
  spacing: {
    small: '8px',
    medium: '16px',
    large: '24px'
  },
  breakpoints: {
    mobile: '576px',
    tablet: '768px',
    desktop: '1024px'
  }
};

const darkTheme = {
  ...lightTheme,
  colors: {
    ...lightTheme.colors,
    background: '#1a1a1a',
    text: '#ffffff'
  }
};

// Use theme in components
const ThemedButton = styled.button`
  background-color: ${props => props.theme.colors.primary};
  color: white;
  padding: ${props => props.theme.spacing.medium};
  border: none;
  border-radius: 4px;
`;

const Container = styled.div`
  background-color: ${props => props.theme.colors.background};
  color: ${props => props.theme.colors.text};
  padding: ${props => props.theme.spacing.large};
`;

// App with theme
function App() {
  const [isDark, setIsDark] = useState(false);
  const theme = isDark ? darkTheme : lightTheme;
  
  return (
    <ThemeProvider theme={theme}>
      <Container>
        <h1>Themed App</h1>
        <ThemedButton onClick={() => setIsDark(!isDark)}>
          Toggle Theme
        </ThemedButton>
      </Container>
    </ThemeProvider>
  );
}
```

## Global Styles

### Creating Global Styles
```jsx
import { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  
  body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    background-color: ${props => props.theme.colors.background};
    color: ${props => props.theme.colors.text};
    line-height: 1.6;
  }
  
  a {
    color: ${props => props.theme.colors.primary};
    text-decoration: none;
    
    &:hover {
      text-decoration: underline;
    }
  }
  
  h1, h2, h3, h4, h5, h6 {
    margin-bottom: 1rem;
  }
  
  /* Custom scrollbar */
  ::-webkit-scrollbar {
    width: 10px;
  }
  
  ::-webkit-scrollbar-track {
    background: #f1f1f1;
  }
  
  ::-webkit-scrollbar-thumb {
    background: #888;
    border-radius: 5px;
  }
`;

// Use in app
function App() {
  return (
    <ThemeProvider theme={theme}>
      <GlobalStyle />
      <div>Your app content</div>
    </ThemeProvider>
  );
}
```

## Animations

### Keyframes
```jsx
import styled, { keyframes } from 'styled-components';

// Define animation
const fadeIn = keyframes`
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
`;

const rotate = keyframes`
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
`;

const pulse = keyframes`
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
  100% {
    transform: scale(1);
  }
`;

// Apply animations
const FadeInDiv = styled.div`
  animation: ${fadeIn} 0.5s ease-out;
`;

const Spinner = styled.div`
  width: 40px;
  height: 40px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  animation: ${rotate} 1s linear infinite;
`;

const PulseButton = styled.button`
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  animation: ${pulse} 2s ease-in-out infinite;
  
  &:hover {
    animation-play-state: paused;
  }
`;
```

### Dynamic Animations
```jsx
const slideIn = (direction = 'left') => keyframes`
  from {
    transform: translateX(${direction === 'left' ? '-100%' : '100%'});
  }
  to {
    transform: translateX(0);
  }
`;

const AnimatedDiv = styled.div`
  animation: ${props => slideIn(props.direction)} 0.3s ease-out;
`;

// Usage
<AnimatedDiv direction="left">Slides from left</AnimatedDiv>
<AnimatedDiv direction="right">Slides from right</AnimatedDiv>
```

## Advanced Patterns

### Responsive Design
```jsx
const Container = styled.div`
  width: 100%;
  padding: 20px;
  
  /* Mobile first approach */
  @media (min-width: ${props => props.theme.breakpoints.tablet}) {
    max-width: 750px;
    margin: 0 auto;
  }
  
  @media (min-width: ${props => props.theme.breakpoints.desktop}) {
    max-width: 1200px;
  }
`;

// Helper function for media queries
const media = {
  mobile: (styles) => `
    @media (max-width: 576px) {
      ${styles}
    }
  `,
  tablet: (styles) => `
    @media (min-width: 577px) and (max-width: 768px) {
      ${styles}
    }
  `,
  desktop: (styles) => `
    @media (min-width: 769px) {
      ${styles}
    }
  `
};

const ResponsiveGrid = styled.div`
  display: grid;
  gap: 20px;
  grid-template-columns: 1fr;
  
  ${media.tablet(`
    grid-template-columns: repeat(2, 1fr);
  `)}
  
  ${media.desktop(`
    grid-template-columns: repeat(3, 1fr);
  `)}
`;
```

### CSS Grid with Styled Components
```jsx
const Grid = styled.div`
  display: grid;
  grid-template-columns: repeat(${props => props.columns || 12}, 1fr);
  gap: ${props => props.gap || '20px'};
  
  @media (max-width: 768px) {
    grid-template-columns: 1fr;
  }
`;

const GridItem = styled.div`
  grid-column: span ${props => props.span || 1};
  
  @media (max-width: 768px) {
    grid-column: span 1;
  }
`;

// Usage
<Grid columns={12} gap="30px">
  <GridItem span={3}>Sidebar</GridItem>
  <GridItem span={9}>Main Content</GridItem>
</Grid>
```

### Compound Components
```jsx
const Card = styled.div`
  border: 1px solid #ddd;
  border-radius: 8px;
  overflow: hidden;
  background: white;
`;

const CardHeader = styled.div`
  padding: 20px;
  background-color: #f8f9fa;
  border-bottom: 1px solid #ddd;
  font-weight: bold;
`;

const CardBody = styled.div`
  padding: 20px;
`;

const CardFooter = styled.div`
  padding: 20px;
  background-color: #f8f9fa;
  border-top: 1px solid #ddd;
`;

// Add as static properties
Card.Header = CardHeader;
Card.Body = CardBody;
Card.Footer = CardFooter;

// Usage
<Card>
  <Card.Header>Card Title</Card.Header>
  <Card.Body>Card content goes here</Card.Body>
  <Card.Footer>Card footer</Card.Footer>
</Card>
```

### Attrs Method
```jsx
// Add default attributes
const Input = styled.input.attrs(props => ({
  type: props.type || 'text',
  size: props.size || '1em',
}))`
  color: #333;
  font-size: ${props => props.size};
  border: 2px solid #ddd;
  border-radius: 4px;
  padding: 0.5em;
  
  &:focus {
    border-color: #007bff;
    outline: none;
  }
`;

// Dynamic attributes
const PasswordInput = styled.input.attrs({
  type: 'password',
  placeholder: 'Enter password'
})`
  /* styles */
`;
```

## TypeScript Support

### Typed Components
```typescript
import styled from 'styled-components';

interface ButtonProps {
  primary?: boolean;
  size?: 'small' | 'medium' | 'large';
  fullWidth?: boolean;
}

const Button = styled.button<ButtonProps>`
  background-color: ${props => props.primary ? '#007bff' : '#6c757d'};
  padding: ${props => {
    switch(props.size) {
      case 'small': return '5px 10px';
      case 'large': return '15px 30px';
      default: return '10px 20px';
    }
  }};
  width: ${props => props.fullWidth ? '100%' : 'auto'};
`;

// Theme typing
interface Theme {
  colors: {
    primary: string;
    secondary: string;
    background: string;
    text: string;
  };
  spacing: {
    small: string;
    medium: string;
    large: string;
  };
}

// Module augmentation for theme
declare module 'styled-components' {
  export interface DefaultTheme extends Theme {}
}
```

## Performance Optimization

### Memoization
```jsx
import styled, { css } from 'styled-components';

// Memoize complex calculations
const complexStyles = css`
  ${props => {
    // This calculation runs only when props change
    const padding = props.size === 'large' ? 20 : 10;
    const fontSize = props.size === 'large' ? 18 : 16;
    
    return css`
      padding: ${padding}px;
      font-size: ${fontSize}px;
    `;
  }}
`;

const OptimizedButton = styled.button`
  ${complexStyles}
`;

// Use React.memo for component optimization
const Button = React.memo(styled.button`
  /* styles */
`);
```

## Debugging

### Display Names
```jsx
// Add display names for better debugging
const Button = styled.button`
  /* styles */
`;
Button.displayName = 'StyledButton';

// Use babel plugin for automatic display names
// .babelrc
{
  "plugins": [
    ["babel-plugin-styled-components", {
      "displayName": true,
      "fileName": true
    }]
  ]
}
```

## Best Practices

1. **Component organization** - Keep styled components close to their usage
2. **Naming convention** - Use descriptive names (StyledButton vs S.Button)
3. **Theme everything** - Use theme for consistency
4. **Avoid inline styles** - Use props for dynamic styling
5. **Performance** - Use css helper for shared styles
6. **Type safety** - Use TypeScript for better development experience
7. **Debugging** - Enable display names in development
8. **Global styles** - Use sparingly, prefer component styles

Styled Components provides a powerful way to style React applications with the full power of JavaScript!