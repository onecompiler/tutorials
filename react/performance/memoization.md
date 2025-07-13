# Memoization in React

Memoization is an optimization technique that stores the results of expensive function calls and returns the cached result when the same inputs occur again. React provides several built-in memoization tools.

## React.memo

React.memo is a higher-order component that memoizes the result of a component.

### Basic Usage
```jsx
// Without React.memo - re-renders on every parent render
function ExpensiveComponent({ data }) {
  console.log('ExpensiveComponent rendered');
  
  return (
    <div>
      <h2>Data Analysis</h2>
      <p>Items: {data.length}</p>
      <p>Total: {data.reduce((sum, item) => sum + item.value, 0)}</p>
    </div>
  );
}

// With React.memo - only re-renders when props change
const MemoizedComponent = React.memo(function ExpensiveComponent({ data }) {
  console.log('MemoizedComponent rendered');
  
  return (
    <div>
      <h2>Data Analysis</h2>
      <p>Items: {data.length}</p>
      <p>Total: {data.reduce((sum, item) => sum + item.value, 0)}</p>
    </div>
  );
});

// Parent component
function Dashboard() {
  const [count, setCount] = useState(0);
  const data = [
    { id: 1, value: 100 },
    { id: 2, value: 200 },
    { id: 3, value: 300 }
  ];
  
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>
        Count: {count}
      </button>
      <MemoizedComponent data={data} />
    </div>
  );
}
```

### Custom Comparison Function
```jsx
// Component that renders user info
const UserCard = React.memo(
  function UserCard({ user, onSelect }) {
    console.log(`UserCard ${user.id} rendered`);
    
    return (
      <div className="user-card" onClick={() => onSelect(user.id)}>
        <h3>{user.name}</h3>
        <p>{user.email}</p>
        <p>Last active: {user.lastActive}</p>
      </div>
    );
  },
  // Custom comparison function
  (prevProps, nextProps) => {
    // Return true if props are equal (skip re-render)
    // Return false if props are different (re-render)
    
    // Only re-render if user.id or user.name changes
    // Ignore lastActive changes
    return (
      prevProps.user.id === nextProps.user.id &&
      prevProps.user.name === nextProps.user.name &&
      prevProps.user.email === nextProps.user.email
    );
  }
);

// Usage
function UserList() {
  const [users, setUsers] = useState([
    { id: 1, name: 'John', email: 'john@example.com', lastActive: new Date() },
    { id: 2, name: 'Jane', email: 'jane@example.com', lastActive: new Date() }
  ]);
  
  const handleSelect = useCallback((userId) => {
    console.log('Selected user:', userId);
  }, []);
  
  // Update lastActive every second
  useEffect(() => {
    const interval = setInterval(() => {
      setUsers(prevUsers => 
        prevUsers.map(user => ({
          ...user,
          lastActive: new Date()
        }))
      );
    }, 1000);
    
    return () => clearInterval(interval);
  }, []);
  
  return (
    <div>
      {users.map(user => (
        <UserCard
          key={user.id}
          user={user}
          onSelect={handleSelect}
        />
      ))}
    </div>
  );
}
```

## useMemo Hook

useMemo memoizes the result of a computation.

### Expensive Calculations
```jsx
function DataAnalytics({ sales, expenses }) {
  // Expensive calculation - only runs when dependencies change
  const analytics = useMemo(() => {
    console.log('Calculating analytics...');
    
    const totalSales = sales.reduce((sum, sale) => sum + sale.amount, 0);
    const totalExpenses = expenses.reduce((sum, expense) => sum + expense.amount, 0);
    const profit = totalSales - totalExpenses;
    const profitMargin = (profit / totalSales) * 100;
    
    const monthlySales = sales.reduce((acc, sale) => {
      const month = new Date(sale.date).getMonth();
      acc[month] = (acc[month] || 0) + sale.amount;
      return acc;
    }, {});
    
    const topProducts = sales
      .reduce((acc, sale) => {
        acc[sale.product] = (acc[sale.product] || 0) + sale.amount;
        return acc;
      }, {});
    
    return {
      totalSales,
      totalExpenses,
      profit,
      profitMargin,
      monthlySales,
      topProducts
    };
  }, [sales, expenses]); // Only recalculate when sales or expenses change
  
  return (
    <div>
      <h2>Analytics Dashboard</h2>
      <div>Total Sales: ${analytics.totalSales}</div>
      <div>Total Expenses: ${analytics.totalExpenses}</div>
      <div>Profit: ${analytics.profit}</div>
      <div>Profit Margin: {analytics.profitMargin.toFixed(2)}%</div>
    </div>
  );
}
```

### Referential Equality
```jsx
function SearchableList({ items }) {
  const [searchTerm, setSearchTerm] = useState('');
  const [sortOrder, setSortOrder] = useState('asc');
  
  // Without useMemo - creates new array every render
  // const filteredAndSortedItems = items
  //   .filter(item => item.name.toLowerCase().includes(searchTerm.toLowerCase()))
  //   .sort((a, b) => {
  //     return sortOrder === 'asc' 
  //       ? a.name.localeCompare(b.name)
  //       : b.name.localeCompare(a.name);
  //   });
  
  // With useMemo - only creates new array when dependencies change
  const filteredAndSortedItems = useMemo(() => {
    console.log('Filtering and sorting items...');
    
    return items
      .filter(item => item.name.toLowerCase().includes(searchTerm.toLowerCase()))
      .sort((a, b) => {
        return sortOrder === 'asc' 
          ? a.name.localeCompare(b.name)
          : b.name.localeCompare(a.name);
      });
  }, [items, searchTerm, sortOrder]);
  
  // Memoize style objects
  const containerStyle = useMemo(() => ({
    display: 'flex',
    flexDirection: 'column',
    gap: '10px',
    padding: '20px'
  }), []);
  
  return (
    <div style={containerStyle}>
      <input
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search..."
      />
      <button onClick={() => setSortOrder(order => order === 'asc' ? 'desc' : 'asc')}>
        Sort: {sortOrder}
      </button>
      <ItemList items={filteredAndSortedItems} />
    </div>
  );
}

// Child component that benefits from referential equality
const ItemList = React.memo(({ items }) => {
  console.log('ItemList rendered');
  
  return (
    <ul>
      {items.map(item => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
});
```

## useCallback Hook

useCallback memoizes functions to maintain referential equality.

### Event Handlers
```jsx
function TodoApp() {
  const [todos, setTodos] = useState([
    { id: 1, text: 'Learn React', completed: false },
    { id: 2, text: 'Build an app', completed: false }
  ]);
  
  // Without useCallback - new function every render
  // const handleToggle = (id) => {
  //   setTodos(prev => prev.map(todo =>
  //     todo.id === id ? { ...todo, completed: !todo.completed } : todo
  //   ));
  // };
  
  // With useCallback - stable function reference
  const handleToggle = useCallback((id) => {
    setTodos(prev => prev.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
  }, []); // Empty deps because setTodos is stable
  
  const handleDelete = useCallback((id) => {
    setTodos(prev => prev.filter(todo => todo.id !== id));
  }, []);
  
  const handleAdd = useCallback((text) => {
    setTodos(prev => [...prev, {
      id: Date.now(),
      text,
      completed: false
    }]);
  }, []);
  
  return (
    <div>
      <TodoInput onAdd={handleAdd} />
      <TodoList
        todos={todos}
        onToggle={handleToggle}
        onDelete={handleDelete}
      />
    </div>
  );
}

// Child components with React.memo
const TodoInput = React.memo(({ onAdd }) => {
  console.log('TodoInput rendered');
  const [input, setInput] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    if (input.trim()) {
      onAdd(input);
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
});

const TodoList = React.memo(({ todos, onToggle, onDelete }) => {
  console.log('TodoList rendered');
  
  return (
    <ul>
      {todos.map(todo => (
        <TodoItem
          key={todo.id}
          todo={todo}
          onToggle={onToggle}
          onDelete={onDelete}
        />
      ))}
    </ul>
  );
});

const TodoItem = React.memo(({ todo, onToggle, onDelete }) => {
  console.log(`TodoItem ${todo.id} rendered`);
  
  return (
    <li>
      <input
        type="checkbox"
        checked={todo.completed}
        onChange={() => onToggle(todo.id)}
      />
      <span style={{ textDecoration: todo.completed ? 'line-through' : 'none' }}>
        {todo.text}
      </span>
      <button onClick={() => onDelete(todo.id)}>Delete</button>
    </li>
  );
});
```

### Dependencies in useCallback
```jsx
function SearchComponent() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  const [filters, setFilters] = useState({ category: 'all', sortBy: 'relevance' });
  
  // Incorrect - missing dependencies
  // const search = useCallback(async () => {
  //   const data = await fetchResults(query, filters);
  //   setResults(data);
  // }, []); // Bug: query and filters changes won't trigger new search
  
  // Correct - includes all dependencies
  const search = useCallback(async () => {
    console.log('Searching for:', query, 'with filters:', filters);
    const data = await fetchResults(query, filters);
    setResults(data);
  }, [query, filters]); // Re-creates function when query or filters change
  
  // Debounced search
  const debouncedSearch = useMemo(
    () => debounce(search, 300),
    [search]
  );
  
  useEffect(() => {
    if (query) {
      debouncedSearch();
    }
  }, [query, debouncedSearch]);
  
  return (
    <div>
      <SearchInput value={query} onChange={setQuery} />
      <SearchFilters filters={filters} onChange={setFilters} />
      <SearchResults results={results} />
    </div>
  );
}
```

## Advanced Memoization Patterns

### Memoizing Complex Objects
```jsx
function useComplexState() {
  const [data, setData] = useState({ count: 0, items: [] });
  
  // Memoize methods that operate on state
  const actions = useMemo(() => ({
    increment: () => setData(prev => ({ ...prev, count: prev.count + 1 })),
    decrement: () => setData(prev => ({ ...prev, count: prev.count - 1 })),
    addItem: (item) => setData(prev => ({ 
      ...prev, 
      items: [...prev.items, item] 
    })),
    removeItem: (id) => setData(prev => ({ 
      ...prev, 
      items: prev.items.filter(item => item.id !== id) 
    }))
  }), []); // Empty deps because setData is stable
  
  // Memoize derived values
  const summary = useMemo(() => ({
    totalItems: data.items.length,
    totalValue: data.items.reduce((sum, item) => sum + item.value, 0),
    averageValue: data.items.length > 0 
      ? data.items.reduce((sum, item) => sum + item.value, 0) / data.items.length 
      : 0
  }), [data.items]);
  
  return {
    data,
    actions,
    summary
  };
}
```

### Selective Memoization
```jsx
function DataGrid({ rows, columns, onCellEdit }) {
  // Only memoize expensive computations
  const processedRows = useMemo(() => {
    console.log('Processing rows...');
    return rows.map(row => ({
      ...row,
      // Expensive computation
      computed: columns.reduce((acc, col) => {
        if (col.compute) {
          acc[col.field] = col.compute(row);
        }
        return acc;
      }, {})
    }));
  }, [rows, columns]);
  
  // Don't memoize simple operations
  const visibleColumns = columns.filter(col => !col.hidden);
  
  // Memoize callbacks passed to many children
  const handleCellEdit = useCallback((rowId, field, value) => {
    onCellEdit({ rowId, field, value });
  }, [onCellEdit]);
  
  return (
    <table>
      <thead>
        <tr>
          {visibleColumns.map(col => (
            <th key={col.field}>{col.label}</th>
          ))}
        </tr>
      </thead>
      <tbody>
        {processedRows.map(row => (
          <Row
            key={row.id}
            row={row}
            columns={visibleColumns}
            onCellEdit={handleCellEdit}
          />
        ))}
      </tbody>
    </table>
  );
}

const Row = React.memo(({ row, columns, onCellEdit }) => {
  return (
    <tr>
      {columns.map(col => (
        <Cell
          key={col.field}
          value={row[col.field]}
          computed={row.computed[col.field]}
          onEdit={(value) => onCellEdit(row.id, col.field, value)}
        />
      ))}
    </tr>
  );
});
```

### Context Memoization
```jsx
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  // Memoize context value to prevent unnecessary re-renders
  const value = useMemo(() => ({
    theme,
    toggleTheme: () => setTheme(t => t === 'light' ? 'dark' : 'light'),
    setTheme
  }), [theme]);
  
  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}

// Or split into separate contexts
function AppProviders({ children }) {
  const [theme, setTheme] = useState('light');
  const [user, setUser] = useState(null);
  
  // Static context (rarely changes)
  const themeActions = useMemo(() => ({
    toggleTheme: () => setTheme(t => t === 'light' ? 'dark' : 'light'),
    setTheme
  }), []);
  
  // Dynamic context (changes frequently)
  const userActions = useMemo(() => ({
    login: async (credentials) => { /* ... */ },
    logout: () => setUser(null),
    updateProfile: (updates) => setUser(prev => ({ ...prev, ...updates }))
  }), []);
  
  return (
    <ThemeContext.Provider value={theme}>
      <ThemeActionsContext.Provider value={themeActions}>
        <UserContext.Provider value={user}>
          <UserActionsContext.Provider value={userActions}>
            {children}
          </UserActionsContext.Provider>
        </UserContext.Provider>
      </ThemeActionsContext.Provider>
    </ThemeContext.Provider>
  );
}
```

## Custom Memoization Hook
```jsx
function useMemoizedCallback(callback, deps) {
  // Use ref to store the latest callback
  const callbackRef = useRef(callback);
  
  // Update ref when callback changes
  useLayoutEffect(() => {
    callbackRef.current = callback;
  });
  
  // Return stable callback that calls the latest version
  return useCallback((...args) => {
    return callbackRef.current(...args);
  }, deps);
}

// Usage - stable reference even if callback implementation changes
function Component({ onSave }) {
  const stableSave = useMemoizedCallback(onSave, []);
  
  return <ExpensiveChild onSave={stableSave} />;
}
```

## Performance Monitoring
```jsx
function useMemoWithLogging(factory, deps, name) {
  const startTime = performance.now();
  const result = useMemo(factory, deps);
  const endTime = performance.now();
  
  useEffect(() => {
    console.log(`${name} memoization took ${endTime - startTime}ms`);
  });
  
  return result;
}

// Usage
function Component({ data }) {
  const processedData = useMemoWithLogging(
    () => expensiveProcessing(data),
    [data],
    'DataProcessing'
  );
  
  return <div>{/* Use processedData */}</div>;
}
```

## When NOT to Memoize

```jsx
// Don't memoize primitive values
const BadExample = () => {
  // Unnecessary - primitive values are already cheap
  const count = useMemo(() => 5, []);
  const name = useMemo(() => 'John', []);
  
  // Don't memoize simple calculations
  const double = useMemo(() => count * 2, [count]);
  
  // Don't memoize components without React.memo
  const button = useMemo(() => <button>Click me</button>, []);
};

// Don't over-memoize
const OverMemoized = ({ items }) => {
  // Too much memoization can hurt performance
  const item1 = useMemo(() => items[0], [items]);
  const item2 = useMemo(() => items[1], [items]);
  const item3 = useMemo(() => items[2], [items]);
  
  // Better: just use items directly
  return (
    <div>
      {items.slice(0, 3).map(item => <div key={item.id}>{item.name}</div>)}
    </div>
  );
};
```

## Best Practices

1. **Profile first** - Don't memoize without measuring
2. **Memoize expensive operations** - Not simple calculations
3. **Use correct dependencies** - Include all used values
4. **Combine with React.memo** - For component memoization
5. **Consider trade-offs** - Memoization uses memory
6. **Test memoization** - Ensure it's working as expected
7. **Document why** - Explain why memoization is needed
8. **Review regularly** - Remove unnecessary memoization

Memoization is a powerful optimization technique, but use it wisely to avoid over-optimization!