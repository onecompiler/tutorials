# Testing Basics in React

Testing ensures your React components work correctly and helps prevent bugs. This tutorial covers the fundamentals of testing React components using Jest and React Testing Library.

## Testing Philosophy

React Testing Library follows the principle: **"The more your tests resemble the way your software is used, the more confidence they can give you."**

Instead of testing implementation details, focus on:
- What users see and interact with
- How components behave from a user's perspective
- Accessibility and semantic HTML

## Setup

Most React projects come with Jest and React Testing Library pre-configured. If you need to set them up:

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
```

```jsx
// src/setupTests.js
import '@testing-library/jest-dom';
```

## Basic Component Testing

### Simple Component Test
```jsx
// Button.js
function Button({ children, onClick, disabled = false }) {
  return (
    <button onClick={onClick} disabled={disabled}>
      {children}
    </button>
  );
}

export default Button;

// Button.test.js
import { render, screen, fireEvent } from '@testing-library/react';
import Button from './Button';

describe('Button', () => {
  test('renders button text', () => {
    render(<Button>Click me</Button>);
    
    const button = screen.getByText('Click me');
    expect(button).toBeInTheDocument();
  });
  
  test('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    
    const button = screen.getByText('Click me');
    fireEvent.click(button);
    
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
  
  test('is disabled when disabled prop is true', () => {
    render(<Button disabled>Click me</Button>);
    
    const button = screen.getByText('Click me');
    expect(button).toBeDisabled();
  });
});
```

### Testing State Changes
```jsx
// Counter.js
import { useState } from 'react';

function Counter({ initialCount = 0 }) {
  const [count, setCount] = useState(initialCount);
  
  return (
    <div>
      <span data-testid="count">{count}</span>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
      <button onClick={() => setCount(count - 1)}>
        Decrement
      </button>
      <button onClick={() => setCount(0)}>
        Reset
      </button>
    </div>
  );
}

export default Counter;

// Counter.test.js
import { render, screen, fireEvent } from '@testing-library/react';
import Counter from './Counter';

describe('Counter', () => {
  test('displays initial count', () => {
    render(<Counter initialCount={5} />);
    expect(screen.getByTestId('count')).toHaveTextContent('5');
  });
  
  test('increments count when increment button is clicked', () => {
    render(<Counter />);
    
    const incrementButton = screen.getByText('Increment');
    fireEvent.click(incrementButton);
    
    expect(screen.getByTestId('count')).toHaveTextContent('1');
  });
  
  test('decrements count when decrement button is clicked', () => {
    render(<Counter initialCount={5} />);
    
    const decrementButton = screen.getByText('Decrement');
    fireEvent.click(decrementButton);
    
    expect(screen.getByTestId('count')).toHaveTextContent('4');
  });
  
  test('resets count to zero when reset button is clicked', () => {
    render(<Counter initialCount={10} />);
    
    const resetButton = screen.getByText('Reset');
    fireEvent.click(resetButton);
    
    expect(screen.getByTestId('count')).toHaveTextContent('0');
  });
});
```

## Queries

### Finding Elements
```jsx
// UserProfile.js
function UserProfile({ user }) {
  if (!user) {
    return <div>No user found</div>;
  }
  
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
      <img src={user.avatar} alt={`${user.name}'s avatar`} />
      <button aria-label="Edit profile">Edit</button>
    </div>
  );
}

// UserProfile.test.js
import { render, screen } from '@testing-library/react';
import UserProfile from './UserProfile';

const mockUser = {
  name: 'John Doe',
  email: 'john@example.com',
  avatar: 'https://example.com/avatar.jpg'
};

describe('UserProfile', () => {
  test('displays user information', () => {
    render(<UserProfile user={mockUser} />);
    
    // By text
    expect(screen.getByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('john@example.com')).toBeInTheDocument();
    
    // By role
    expect(screen.getByRole('heading', { name: 'John Doe' })).toBeInTheDocument();
    expect(screen.getByRole('button', { name: 'Edit profile' })).toBeInTheDocument();
    
    // By alt text
    expect(screen.getByAltText("John Doe's avatar")).toBeInTheDocument();
  });
  
  test('shows no user message when user is null', () => {
    render(<UserProfile user={null} />);
    
    expect(screen.getByText('No user found')).toBeInTheDocument();
    expect(screen.queryByRole('heading')).not.toBeInTheDocument();
  });
});
```

### Query Priority Order
```jsx
// Recommended query priority:
describe('Query examples', () => {
  test('query priority examples', () => {
    render(
      <div>
        <label htmlFor="username">Username</label>
        <input
          id="username"
          placeholder="Enter username"
          data-testid="username-input"
        />
        <button type="submit">Submit</button>
      </div>
    );
    
    // 1. Accessible to everyone (preferred)
    screen.getByRole('textbox', { name: /username/i });
    screen.getByLabelText(/username/i);
    
    // 2. Semantic queries
    screen.getByPlaceholderText(/enter username/i);
    screen.getByText(/submit/i);
    
    // 3. Test IDs (last resort)
    screen.getByTestId('username-input');
  });
});
```

## Form Testing

### Input Handling
```jsx
// LoginForm.js
import { useState } from 'react';

function LoginForm({ onSubmit }) {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [errors, setErrors] = useState({});
  
  const handleSubmit = (e) => {
    e.preventDefault();
    const newErrors = {};
    
    if (!email) newErrors.email = 'Email is required';
    if (!password) newErrors.password = 'Password is required';
    
    if (Object.keys(newErrors).length > 0) {
      setErrors(newErrors);
      return;
    }
    
    onSubmit({ email, password });
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="email">Email</label>
        <input
          id="email"
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
        {errors.email && <span role="alert">{errors.email}</span>}
      </div>
      
      <div>
        <label htmlFor="password">Password</label>
        <input
          id="password"
          type="password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
        {errors.password && <span role="alert">{errors.password}</span>}
      </div>
      
      <button type="submit">Login</button>
    </form>
  );
}

export default LoginForm;

// LoginForm.test.js
import { render, screen, fireEvent } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import LoginForm from './LoginForm';

describe('LoginForm', () => {
  test('submits form with email and password', async () => {
    const user = userEvent.setup();
    const mockSubmit = jest.fn();
    
    render(<LoginForm onSubmit={mockSubmit} />);
    
    // Fill out form
    await user.type(screen.getByLabelText(/email/i), 'test@example.com');
    await user.type(screen.getByLabelText(/password/i), 'password123');
    
    // Submit form
    await user.click(screen.getByRole('button', { name: /login/i }));
    
    expect(mockSubmit).toHaveBeenCalledWith({
      email: 'test@example.com',
      password: 'password123'
    });
  });
  
  test('shows validation errors for empty fields', async () => {
    const user = userEvent.setup();
    const mockSubmit = jest.fn();
    
    render(<LoginForm onSubmit={mockSubmit} />);
    
    // Submit form without filling fields
    await user.click(screen.getByRole('button', { name: /login/i }));
    
    expect(screen.getByText('Email is required')).toBeInTheDocument();
    expect(screen.getByText('Password is required')).toBeInTheDocument();
    expect(mockSubmit).not.toHaveBeenCalled();
  });
  
  test('updates input values when typing', async () => {
    const user = userEvent.setup();
    
    render(<LoginForm onSubmit={jest.fn()} />);
    
    const emailInput = screen.getByLabelText(/email/i);
    const passwordInput = screen.getByLabelText(/password/i);
    
    await user.type(emailInput, 'test@example.com');
    await user.type(passwordInput, 'password123');
    
    expect(emailInput).toHaveValue('test@example.com');
    expect(passwordInput).toHaveValue('password123');
  });
});
```

## Testing Async Behavior

### Async Data Loading
```jsx
// UserList.js
import { useState, useEffect } from 'react';

function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    fetchUsers()
      .then(setUsers)
      .catch(setError)
      .finally(() => setLoading(false));
  }, []);
  
  if (loading) return <div>Loading users...</div>;
  if (error) return <div>Error: {error.message}</div>;
  
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

// Mock API function
async function fetchUsers() {
  const response = await fetch('/api/users');
  if (!response.ok) throw new Error('Failed to fetch users');
  return response.json();
}

export default UserList;

// UserList.test.js
import { render, screen, waitFor } from '@testing-library/react';
import UserList from './UserList';

// Mock the fetch function
global.fetch = jest.fn();

describe('UserList', () => {
  beforeEach(() => {
    fetch.mockClear();
  });
  
  test('displays loading state initially', () => {
    fetch.mockResolvedValueOnce({
      ok: true,
      json: async () => []
    });
    
    render(<UserList />);
    
    expect(screen.getByText('Loading users...')).toBeInTheDocument();
  });
  
  test('displays users after successful fetch', async () => {
    const mockUsers = [
      { id: 1, name: 'John Doe' },
      { id: 2, name: 'Jane Smith' }
    ];
    
    fetch.mockResolvedValueOnce({
      ok: true,
      json: async () => mockUsers
    });
    
    render(<UserList />);
    
    // Wait for users to load
    await waitFor(() => {
      expect(screen.getByText('John Doe')).toBeInTheDocument();
    });
    
    expect(screen.getByText('Jane Smith')).toBeInTheDocument();
    expect(screen.queryByText('Loading users...')).not.toBeInTheDocument();
  });
  
  test('displays error message when fetch fails', async () => {
    fetch.mockRejectedValueOnce(new Error('Network error'));
    
    render(<UserList />);
    
    await waitFor(() => {
      expect(screen.getByText('Error: Network error')).toBeInTheDocument();
    });
    
    expect(screen.queryByText('Loading users...')).not.toBeInTheDocument();
  });
});
```

## Custom Hooks Testing

### Testing Hook Logic
```jsx
// useCounter.js
import { useState } from 'react';

function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);
  
  const increment = () => setCount(c => c + 1);
  const decrement = () => setCount(c => c - 1);
  const reset = () => setCount(initialValue);
  
  return {
    count,
    increment,
    decrement,
    reset
  };
}

export default useCounter;

// useCounter.test.js
import { renderHook, act } from '@testing-library/react';
import useCounter from './useCounter';

describe('useCounter', () => {
  test('initializes with default value', () => {
    const { result } = renderHook(() => useCounter());
    
    expect(result.current.count).toBe(0);
  });
  
  test('initializes with custom value', () => {
    const { result } = renderHook(() => useCounter(10));
    
    expect(result.current.count).toBe(10);
  });
  
  test('increments count', () => {
    const { result } = renderHook(() => useCounter());
    
    act(() => {
      result.current.increment();
    });
    
    expect(result.current.count).toBe(1);
  });
  
  test('decrements count', () => {
    const { result } = renderHook(() => useCounter(5));
    
    act(() => {
      result.current.decrement();
    });
    
    expect(result.current.count).toBe(4);
  });
  
  test('resets to initial value', () => {
    const { result } = renderHook(() => useCounter(10));
    
    act(() => {
      result.current.increment();
      result.current.increment();
    });
    
    expect(result.current.count).toBe(12);
    
    act(() => {
      result.current.reset();
    });
    
    expect(result.current.count).toBe(10);
  });
});
```

## Context Testing

### Testing Context Providers
```jsx
// ThemeContext.js
import { createContext, useContext, useState } from 'react';

const ThemeContext = createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(prevTheme => prevTheme === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

export function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
}

// ThemedButton.js
import { useTheme } from './ThemeContext';

function ThemedButton() {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <button
      onClick={toggleTheme}
      style={{
        backgroundColor: theme === 'light' ? '#fff' : '#333',
        color: theme === 'light' ? '#333' : '#fff'
      }}
    >
      Current theme: {theme}
    </button>
  );
}

export default ThemedButton;

// ThemedButton.test.js
import { render, screen, fireEvent } from '@testing-library/react';
import { ThemeProvider } from './ThemeContext';
import ThemedButton from './ThemedButton';

// Helper function to render with context
function renderWithTheme(ui) {
  return render(<ThemeProvider>{ui}</ThemeProvider>);
}

describe('ThemedButton', () => {
  test('displays current theme', () => {
    renderWithTheme(<ThemedButton />);
    
    expect(screen.getByText('Current theme: light')).toBeInTheDocument();
  });
  
  test('toggles theme when clicked', () => {
    renderWithTheme(<ThemedButton />);
    
    const button = screen.getByRole('button');
    fireEvent.click(button);
    
    expect(screen.getByText('Current theme: dark')).toBeInTheDocument();
  });
  
  test('throws error when used outside provider', () => {
    // Suppress console.error for this test
    const originalError = console.error;
    console.error = jest.fn();
    
    expect(() => {
      render(<ThemedButton />);
    }).toThrow('useTheme must be used within ThemeProvider');
    
    console.error = originalError;
  });
});
```

## Testing with Mock Functions

### Jest Mocks
```jsx
// Timer.js
import { useState, useEffect } from 'react';

function Timer({ onTick }) {
  const [seconds, setSeconds] = useState(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(prev => {
        const newValue = prev + 1;
        onTick?.(newValue);
        return newValue;
      });
    }, 1000);
    
    return () => clearInterval(interval);
  }, [onTick]);
  
  return <div>Timer: {seconds}s</div>;
}

export default Timer;

// Timer.test.js
import { render, screen, act } from '@testing-library/react';
import Timer from './Timer';

jest.useFakeTimers();

describe('Timer', () => {
  afterEach(() => {
    jest.clearAllTimers();
  });
  
  test('starts at 0 seconds', () => {
    render(<Timer />);
    
    expect(screen.getByText('Timer: 0s')).toBeInTheDocument();
  });
  
  test('increments every second', () => {
    render(<Timer />);
    
    // Fast forward 3 seconds
    act(() => {
      jest.advanceTimersByTime(3000);
    });
    
    expect(screen.getByText('Timer: 3s')).toBeInTheDocument();
  });
  
  test('calls onTick with current value', () => {
    const mockOnTick = jest.fn();
    
    render(<Timer onTick={mockOnTick} />);
    
    act(() => {
      jest.advanceTimersByTime(2000);
    });
    
    expect(mockOnTick).toHaveBeenCalledTimes(2);
    expect(mockOnTick).toHaveBeenNthCalledWith(1, 1);
    expect(mockOnTick).toHaveBeenNthCalledWith(2, 2);
  });
  
  test('cleans up timer on unmount', () => {
    const { unmount } = render(<Timer />);
    
    // Spy on clearInterval
    const clearIntervalSpy = jest.spyOn(global, 'clearInterval');
    
    unmount();
    
    expect(clearIntervalSpy).toHaveBeenCalled();
    clearIntervalSpy.mockRestore();
  });
});
```

## Best Practices

1. **Test behavior, not implementation** - Focus on what users experience
2. **Use meaningful test names** - Describe what the test does
3. **Keep tests simple** - One assertion per test when possible
4. **Use setup and teardown** - Clean up after tests
5. **Mock external dependencies** - Don't test the network
6. **Test error states** - Ensure graceful error handling
7. **Use accessibility queries** - Prefer role-based queries
8. **Write tests first** - TDD approach helps design better APIs

Testing is essential for maintaining reliable React applications. Start with these basics and gradually add more sophisticated testing patterns!