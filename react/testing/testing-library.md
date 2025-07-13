# React Testing Library

React Testing Library is a simple and complete testing utility that encourages good testing practices. It focuses on testing components the way users interact with them.

## Core Principles

1. **Test what users see** - Query by text, labels, and roles
2. **Test behavior, not implementation** - Avoid testing internal state
3. **Find elements like users do** - Use accessible queries first
4. **Write maintainable tests** - Tests should break when behavior changes

## Queries Reference

### Query Types

```jsx
// getBy* - Throws error if not found (use for elements that should exist)
screen.getByText('Submit');

// queryBy* - Returns null if not found (use for elements that might not exist)
screen.queryByText('Optional text');

// findBy* - Returns promise, waits for element (use for async elements)
await screen.findByText('Async content');

// getAllBy*, queryAllBy*, findAllBy* - Multiple elements
screen.getAllByRole('button');
```

### Query Priority

```jsx
describe('Query examples', () => {
  const TestComponent = () => (
    <div>
      <h1>Dashboard</h1>
      <label htmlFor="username">Username</label>
      <input
        id="username"
        placeholder="Enter your username"
        aria-label="Username input"
        data-testid="username-field"
      />
      <button type="submit">Submit Form</button>
      <img src="/logo.png" alt="Company logo" />
    </div>
  );
  
  beforeEach(() => {
    render(<TestComponent />);
  });
  
  test('accessible queries (highest priority)', () => {
    // By role (most preferred)
    screen.getByRole('heading', { name: 'Dashboard' });
    screen.getByRole('textbox', { name: 'Username' });
    screen.getByRole('button', { name: 'Submit Form' });
    
    // By label text
    screen.getByLabelText('Username');
    screen.getByLabelText(/username/i); // Case insensitive regex
  });
  
  test('semantic queries (medium priority)', () => {
    // By placeholder text
    screen.getByPlaceholderText('Enter your username');
    
    // By text content
    screen.getByText('Dashboard');
    screen.getByText(/submit/i);
    
    // By alt text
    screen.getByAltText('Company logo');
  });
  
  test('test id queries (last resort)', () => {
    // Only use when other queries don't work
    screen.getByTestId('username-field');
  });
});
```

## Advanced Queries

### Custom Text Matcher
```jsx
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>
          <span className={todo.completed ? 'completed' : ''}>
            {todo.text}
          </span>
          <button>
            {todo.completed ? 'Mark Incomplete' : 'Mark Complete'}
          </button>
        </li>
      ))}
    </ul>
  );
}

describe('TodoList queries', () => {
  const todos = [
    { id: 1, text: 'Learn React Testing Library', completed: false },
    { id: 2, text: 'Write comprehensive tests', completed: true }
  ];
  
  beforeEach(() => {
    render(<TodoList todos={todos} />);
  });
  
  test('finds todos with different matchers', () => {
    // Exact string match
    screen.getByText('Learn React Testing Library');
    
    // Regex match
    screen.getByText(/learn react/i);
    
    // Function matcher
    screen.getByText((content, element) => {
      return element?.tagName.toLowerCase() === 'span' && 
             content.includes('Testing Library');
    });
  });
  
  test('queries with multiple criteria', () => {
    // Find button with specific text for incomplete todo
    const incompleteButton = screen.getByRole('button', { 
      name: 'Mark Complete' 
    });
    expect(incompleteButton).toBeInTheDocument();
    
    // Find all buttons and filter
    const buttons = screen.getAllByRole('button');
    expect(buttons).toHaveLength(2);
  });
});
```

### Within Queries
```jsx
function UserCard({ user, onEdit, onDelete }) {
  return (
    <div data-testid={`user-card-${user.id}`}>
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      <div className="actions">
        <button onClick={() => onEdit(user.id)}>Edit</button>
        <button onClick={() => onDelete(user.id)}>Delete</button>
      </div>
    </div>
  );
}

function UserList({ users, onEdit, onDelete }) {
  return (
    <div>
      {users.map(user => (
        <UserCard
          key={user.id}
          user={user}
          onEdit={onEdit}
          onDelete={onDelete}
        />
      ))}
    </div>
  );
}

describe('UserList with within queries', () => {
  const users = [
    { id: 1, name: 'John Doe', email: 'john@example.com' },
    { id: 2, name: 'Jane Smith', email: 'jane@example.com' }
  ];
  
  test('interacts with specific user card', () => {
    const onEdit = jest.fn();
    const onDelete = jest.fn();
    
    render(<UserList users={users} onEdit={onEdit} onDelete={onDelete} />);
    
    // Get specific user card
    const johnCard = screen.getByTestId('user-card-1');
    
    // Query within that card
    const johnEditButton = within(johnCard).getByText('Edit');
    const johnDeleteButton = within(johnCard).getByText('Delete');
    
    fireEvent.click(johnEditButton);
    expect(onEdit).toHaveBeenCalledWith(1);
    
    fireEvent.click(johnDeleteButton);
    expect(onDelete).toHaveBeenCalledWith(1);
  });
});
```

## User Events

### userEvent vs fireEvent
```jsx
import userEvent from '@testing-library/user-event';

function ContactForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    subject: 'general',
    message: '',
    newsletter: false
  });
  
  const handleChange = (e) => {
    const { name, value, type, checked } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: type === 'checkbox' ? checked : value
    }));
  };
  
  return (
    <form>
      <input
        name="name"
        value={formData.name}
        onChange={handleChange}
        placeholder="Full name"
      />
      
      <input
        name="email"
        type="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email address"
      />
      
      <select
        name="subject"
        value={formData.subject}
        onChange={handleChange}
      >
        <option value="general">General Inquiry</option>
        <option value="support">Support</option>
        <option value="billing">Billing</option>
      </select>
      
      <textarea
        name="message"
        value={formData.message}
        onChange={handleChange}
        placeholder="Your message"
      />
      
      <label>
        <input
          name="newsletter"
          type="checkbox"
          checked={formData.newsletter}
          onChange={handleChange}
        />
        Subscribe to newsletter
      </label>
      
      <button type="submit">Send Message</button>
    </form>
  );
}

describe('ContactForm user interactions', () => {
  // userEvent provides more realistic user interactions
  test('user can fill out the form', async () => {
    const user = userEvent.setup();
    
    render(<ContactForm />);
    
    // Type in text inputs
    await user.type(
      screen.getByPlaceholderText('Full name'),
      'John Doe'
    );
    
    await user.type(
      screen.getByPlaceholderText('Email address'),
      'john@example.com'
    );
    
    // Select from dropdown
    await user.selectOptions(
      screen.getByRole('combobox'),
      'support'
    );
    
    // Type in textarea
    await user.type(
      screen.getByPlaceholderText('Your message'),
      'I need help with my account'
    );
    
    // Check checkbox
    await user.click(screen.getByLabelText('Subscribe to newsletter'));
    
    // Verify form state
    expect(screen.getByPlaceholderText('Full name')).toHaveValue('John Doe');
    expect(screen.getByPlaceholderText('Email address')).toHaveValue('john@example.com');
    expect(screen.getByRole('combobox')).toHaveValue('support');
    expect(screen.getByPlaceholderText('Your message')).toHaveValue('I need help with my account');
    expect(screen.getByLabelText('Subscribe to newsletter')).toBeChecked();
  });
  
  // Compare with fireEvent (less realistic)
  test('fireEvent alternative (not recommended)', () => {
    render(<ContactForm />);
    
    const nameInput = screen.getByPlaceholderText('Full name');
    
    // fireEvent doesn't trigger all the same events as real user interaction
    fireEvent.change(nameInput, { target: { value: 'John Doe' } });
    
    expect(nameInput).toHaveValue('John Doe');
  });
});
```

### Keyboard Interactions
```jsx
function SearchBox({ onSearch }) {
  const [query, setQuery] = useState('');
  const [isOpen, setIsOpen] = useState(false);
  
  const handleSubmit = (e) => {
    e.preventDefault();
    onSearch(query);
  };
  
  const handleKeyDown = (e) => {
    if (e.key === 'Escape') {
      setIsOpen(false);
      setQuery('');
    }
  };
  
  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input
          type="search"
          value={query}
          onChange={(e) => setQuery(e.target.value)}
          onFocus={() => setIsOpen(true)}
          onKeyDown={handleKeyDown}
          placeholder="Search..."
          aria-expanded={isOpen}
        />
        <button type="submit">Search</button>
      </form>
      
      {isOpen && query && (
        <div role="listbox">
          <div role="option">Search for "{query}"</div>
        </div>
      )}
    </div>
  );
}

describe('SearchBox keyboard interactions', () => {
  test('opens dropdown on focus and closes on escape', async () => {
    const user = userEvent.setup();
    const onSearch = jest.fn();
    
    render(<SearchBox onSearch={onSearch} />);
    
    const searchInput = screen.getByPlaceholderText('Search...');
    
    // Focus input to open dropdown
    await user.click(searchInput);
    expect(searchInput).toHaveFocus();
    expect(searchInput).toHaveAttribute('aria-expanded', 'true');
    
    // Type to show suggestions
    await user.type(searchInput, 'react');
    expect(screen.getByText('Search for "react"')).toBeInTheDocument();
    
    // Press Escape to close and clear
    await user.keyboard('{Escape}');
    expect(searchInput).toHaveValue('');
    expect(searchInput).toHaveAttribute('aria-expanded', 'false');
    expect(screen.queryByRole('listbox')).not.toBeInTheDocument();
  });
  
  test('submits search on Enter', async () => {
    const user = userEvent.setup();
    const onSearch = jest.fn();
    
    render(<SearchBox onSearch={onSearch} />);
    
    const searchInput = screen.getByPlaceholderText('Search...');
    
    await user.type(searchInput, 'react testing');
    await user.keyboard('{Enter}');
    
    expect(onSearch).toHaveBeenCalledWith('react testing');
  });
});
```

## Testing Async Behavior

### waitFor and findBy
```jsx
function AsyncUserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    if (!userId) return;
    
    setLoading(true);
    setError(null);
    
    fetchUser(userId)
      .then(userData => {
        setUser(userData);
        setLoading(false);
      })
      .catch(err => {
        setError(err.message);
        setLoading(false);
      });
  }, [userId]);
  
  if (loading) return <div>Loading user...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!user) return <div>User not found</div>;
  
  return (
    <div>
      <h1>{user.name}</h1>
      <p>Email: {user.email}</p>
      <p>Joined: {new Date(user.joinDate).toLocaleDateString()}</p>
    </div>
  );
}

// Mock API function
const fetchUser = jest.fn();

describe('AsyncUserProfile', () => {
  beforeEach(() => {
    fetchUser.mockClear();
  });
  
  test('displays loading state initially', () => {
    fetchUser.mockImplementation(() => 
      new Promise(resolve => setTimeout(() => resolve({
        name: 'John Doe',
        email: 'john@example.com',
        joinDate: '2023-01-15'
      }), 100))
    );
    
    render(<AsyncUserProfile userId="123" />);
    
    expect(screen.getByText('Loading user...')).toBeInTheDocument();
  });
  
  test('displays user data after loading', async () => {
    const mockUser = {
      name: 'John Doe',
      email: 'john@example.com',
      joinDate: '2023-01-15'
    };
    
    fetchUser.mockResolvedValue(mockUser);
    
    render(<AsyncUserProfile userId="123" />);
    
    // Using findBy* for async elements
    expect(await screen.findByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('Email: john@example.com')).toBeInTheDocument();
    
    // Wait for loading to disappear
    await waitFor(() => {
      expect(screen.queryByText('Loading user...')).not.toBeInTheDocument();
    });
  });
  
  test('displays error message on failure', async () => {
    fetchUser.mockRejectedValue(new Error('Network error'));
    
    render(<AsyncUserProfile userId="123" />);
    
    expect(await screen.findByText('Error: Network error')).toBeInTheDocument();
  });
  
  test('handles user not found', async () => {
    fetchUser.mockResolvedValue(null);
    
    render(<AsyncUserProfile userId="123" />);
    
    expect(await screen.findByText('User not found')).toBeInTheDocument();
  });
  
  test('refetches when userId changes', async () => {
    const user1 = { name: 'John', email: 'john@example.com', joinDate: '2023-01-15' };
    const user2 = { name: 'Jane', email: 'jane@example.com', joinDate: '2023-02-20' };
    
    fetchUser
      .mockResolvedValueOnce(user1)
      .mockResolvedValueOnce(user2);
    
    const { rerender } = render(<AsyncUserProfile userId="1" />);
    
    // Wait for first user
    expect(await screen.findByText('John')).toBeInTheDocument();
    
    // Change userId
    rerender(<AsyncUserProfile userId="2" />);
    
    // Should show loading again
    expect(screen.getByText('Loading user...')).toBeInTheDocument();
    
    // Wait for second user
    expect(await screen.findByText('Jane')).toBeInTheDocument();
    
    expect(fetchUser).toHaveBeenCalledTimes(2);
  });
});
```

## Custom Render Functions

### Provider Wrapper
```jsx
// test-utils.js
import { render } from '@testing-library/react';
import { BrowserRouter } from 'react-router-dom';
import { ThemeProvider } from './contexts/ThemeContext';
import { AuthProvider } from './contexts/AuthContext';

// Custom render function with providers
function customRender(ui, {
  route = '/',
  theme = 'light',
  user = null,
  ...renderOptions
} = {}) {
  window.history.pushState({}, 'Test page', route);
  
  function Wrapper({ children }) {
    return (
      <BrowserRouter>
        <AuthProvider initialUser={user}>
          <ThemeProvider initialTheme={theme}>
            {children}
          </ThemeProvider>
        </AuthProvider>
      </BrowserRouter>
    );
  }
  
  return render(ui, { wrapper: Wrapper, ...renderOptions });
}

// Re-export everything
export * from '@testing-library/react';
export { customRender as render };

// Component that uses context
function Navigation() {
  const { user, logout } = useAuth();
  const { theme, toggleTheme } = useTheme();
  
  return (
    <nav>
      <Link to="/">Home</Link>
      {user ? (
        <>
          <Link to="/profile">Profile</Link>
          <button onClick={logout}>Logout</button>
        </>
      ) : (
        <Link to="/login">Login</Link>
      )}
      <button onClick={toggleTheme}>
        Theme: {theme}
      </button>
    </nav>
  );
}

// Test using custom render
describe('Navigation', () => {
  test('shows login link when not authenticated', () => {
    render(<Navigation />);
    
    expect(screen.getByText('Login')).toBeInTheDocument();
    expect(screen.queryByText('Profile')).not.toBeInTheDocument();
  });
  
  test('shows user menu when authenticated', () => {
    const mockUser = { id: 1, name: 'John Doe' };
    
    render(<Navigation />, { user: mockUser });
    
    expect(screen.getByText('Profile')).toBeInTheDocument();
    expect(screen.getByText('Logout')).toBeInTheDocument();
    expect(screen.queryByText('Login')).not.toBeInTheDocument();
  });
  
  test('can toggle theme', async () => {
    const user = userEvent.setup();
    
    render(<Navigation />, { theme: 'light' });
    
    const themeButton = screen.getByText('Theme: light');
    await user.click(themeButton);
    
    expect(screen.getByText('Theme: dark')).toBeInTheDocument();
  });
});
```

## Testing Custom Hooks

### Complex Hook Testing
```jsx
// useApi.js
import { useState, useEffect, useCallback } from 'react';

function useApi(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  
  const fetchData = useCallback(async () => {
    if (!url) return;
    
    setLoading(true);
    setError(null);
    
    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      const result = await response.json();
      setData(result);
    } catch (err) {
      setError(err);
    } finally {
      setLoading(false);
    }
  }, [url]);
  
  useEffect(() => {
    fetchData();
  }, [fetchData]);
  
  const refetch = useCallback(() => {
    fetchData();
  }, [fetchData]);
  
  return { data, loading, error, refetch };
}

export default useApi;

// useApi.test.js
import { renderHook, waitFor } from '@testing-library/react';
import useApi from './useApi';

global.fetch = jest.fn();

describe('useApi', () => {
  beforeEach(() => {
    fetch.mockClear();
  });
  
  test('fetches data successfully', async () => {
    const mockData = { id: 1, name: 'Test Item' };
    fetch.mockResolvedValueOnce({
      ok: true,
      json: async () => mockData
    });
    
    const { result } = renderHook(() => useApi('/api/items'));
    
    // Initially loading
    expect(result.current.loading).toBe(true);
    expect(result.current.data).toBe(null);
    expect(result.current.error).toBe(null);
    
    // Wait for data to load
    await waitFor(() => {
      expect(result.current.loading).toBe(false);
    });
    
    expect(result.current.data).toEqual(mockData);
    expect(result.current.error).toBe(null);
    expect(fetch).toHaveBeenCalledWith('/api/items');
  });
  
  test('handles fetch error', async () => {
    fetch.mockResolvedValueOnce({
      ok: false,
      status: 404
    });
    
    const { result } = renderHook(() => useApi('/api/items'));
    
    await waitFor(() => {
      expect(result.current.loading).toBe(false);
    });
    
    expect(result.current.data).toBe(null);
    expect(result.current.error).toBeInstanceOf(Error);
    expect(result.current.error.message).toBe('HTTP error! status: 404');
  });
  
  test('can refetch data', async () => {
    const mockData = { id: 1, name: 'Test Item' };
    fetch.mockResolvedValue({
      ok: true,
      json: async () => mockData
    });
    
    const { result } = renderHook(() => useApi('/api/items'));
    
    await waitFor(() => {
      expect(result.current.loading).toBe(false);
    });
    
    // Refetch
    act(() => {
      result.current.refetch();
    });
    
    expect(result.current.loading).toBe(true);
    
    await waitFor(() => {
      expect(result.current.loading).toBe(false);
    });
    
    expect(fetch).toHaveBeenCalledTimes(2);
  });
  
  test('does not fetch when url is null', () => {
    renderHook(() => useApi(null));
    
    expect(fetch).not.toHaveBeenCalled();
  });
});
```

## Best Practices

1. **Use accessible queries** - Prefer getByRole, getByLabelText
2. **Write user-focused tests** - Test what users actually do
3. **Keep tests simple** - One behavior per test
4. **Use userEvent** - More realistic than fireEvent
5. **Test error states** - Don't just test the happy path
6. **Mock external dependencies** - Control what you're testing
7. **Use custom render functions** - Simplify provider setup
8. **Clean up properly** - Reset mocks and timers

React Testing Library makes it easy to write tests that give you confidence in your React components!