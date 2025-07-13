# Setting Up React

There are several ways to set up a React development environment. We'll cover the most common approaches, from simple to more advanced setups.

## Quick Start with Create React App

Create React App is the official tool for creating React applications. It sets up a modern web development environment with no configuration.

### Prerequisites
- Node.js (version 14.0 or higher)
- npm (comes with Node.js) or Yarn

### Creating a New React App

```bash
npx create-react-app my-app
cd my-app
npm start
```

This creates a new React app with:
- React, JSX, ES6, and TypeScript support
- Development server with hot reloading
- Build scripts for production
- Testing framework
- CSS and image imports

### Project Structure
```
my-app/
├── node_modules/
├── public/
│   ├── index.html
│   └── favicon.ico
├── src/
│   ├── App.css
│   ├── App.js
│   ├── App.test.js
│   ├── index.css
│   ├── index.js
│   └── logo.svg
├── .gitignore
├── package.json
└── README.md
```

## Using React with CDN (For Learning)

For quick experiments or learning, you can use React via CDN:

```html
<!DOCTYPE html>
<html>
<head>
    <title>React App</title>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body>
    <div id="root"></div>
    
    <script type="text/babel">
        function App() {
            return <h1>Hello, React!</h1>;
        }
        
        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
```

## Manual Setup with Webpack

For more control over your build process:

### 1. Initialize Project
```bash
mkdir my-react-app
cd my-react-app
npm init -y
```

### 2. Install Dependencies
```bash
npm install react react-dom
npm install --save-dev webpack webpack-cli webpack-dev-server
npm install --save-dev @babel/core @babel/preset-env @babel/preset-react babel-loader
npm install --save-dev html-webpack-plugin
```

### 3. Create webpack.config.js
```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      },
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html'
    })
  ],
  devServer: {
    port: 3000,
    open: true
  }
};
```

### 4. Create .babelrc
```json
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

## Using Vite (Modern Alternative)

Vite is a fast build tool that's becoming popular for React development:

```bash
npm create vite@latest my-react-app -- --template react
cd my-react-app
npm install
npm run dev
```

## Essential VS Code Extensions

For a better development experience, install these VS Code extensions:

1. **ES7+ React/Redux/React-Native snippets**
2. **Prettier - Code formatter**
3. **ESLint**
4. **Auto Rename Tag**
5. **Bracket Pair Colorizer**

## Your First React Component

Let's create a simple React component to test our setup:

```jsx
// src/App.js
import React from 'react';

function App() {
  const [count, setCount] = React.useState(0);
  
  return (
    <div style={{ textAlign: 'center', marginTop: '50px' }}>
      <h1>Welcome to React!</h1>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}

export default App;
```

```jsx
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

## Development Tools

### React Developer Tools
Install the React Developer Tools browser extension for:
- Chrome
- Firefox
- Edge

This extension allows you to:
- Inspect React component hierarchy
- View and edit props and state
- Profile performance

### ESLint Configuration
Create `.eslintrc.json` for code quality:

```json
{
  "extends": ["react-app"],
  "rules": {
    "no-console": "warn",
    "no-unused-vars": "warn"
  }
}
```

### Prettier Configuration
Create `.prettierrc` for consistent formatting:

```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2
}
```

## Environment Variables

Create a `.env` file in your project root:

```
REACT_APP_API_URL=http://localhost:3001
REACT_APP_VERSION=1.0.0
```

Access in your code:
```javascript
const apiUrl = process.env.REACT_APP_API_URL;
```

## Folder Structure Best Practices

```
src/
├── components/
│   ├── common/
│   ├── layout/
│   └── features/
├── hooks/
├── services/
├── utils/
├── styles/
├── App.js
└── index.js
```

## Scripts in package.json

```json
{
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "lint": "eslint src/**/*.js",
    "format": "prettier --write src/**/*.js"
  }
}
```

## Next Steps

Now that you have React set up, you're ready to:
1. Learn about JSX syntax
2. Create your first components
3. Understand props and state
4. Build interactive applications

In the next tutorial, we'll dive deep into JSX and understand how React components work!