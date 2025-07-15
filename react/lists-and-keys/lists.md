# Rendering Lists in React

Lists are common in web applications. React provides powerful ways to render lists of data efficiently using JavaScript's array methods and the key prop.

## Basic List Rendering

Use the `map()` method to transform arrays into lists of elements:

```jsx
function NumberList() {
  const numbers = [1, 2, 3, 4, 5];
  
  return (
    <ul>
      {numbers.map(number => (
        <li key={number}>{number}</li>
      ))}
    </ul>
  );
}
```

## Rendering Complex Data

```jsx
function UserList() {
  const users = [
    { id: 1, name: 'Alice', email: 'alice@example.com' },
    { id: 2, name: 'Bob', email: 'bob@example.com' },
    { id: 3, name: 'Charlie', email: 'charlie@example.com' }
  ];
  
  return (
    <div className="user-list">
      {users.map(user => (
        <div key={user.id} className="user-card">
          <h3>{user.name}</h3>
          <p>{user.email}</p>
        </div>
      ))}
    </div>
  );
}
```

## Extracting List Item Components

Keep your code organized by extracting list items into separate components:

```jsx
function TodoItem({ todo, onToggle, onDelete }) {
  return (
    <li className={todo.completed ? 'completed' : ''}>
      <input
        type="checkbox"
        checked={todo.completed}
        onChange={() => onToggle(todo.id)}
      />
      <span>{todo.text}</span>
      <button onClick={() => onDelete(todo.id)}>Delete</button>
    </li>
  );
}

function TodoList({ todos, onToggle, onDelete }) {
  return (
    <ul className="todo-list">
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
}
```

## Conditional Rendering in Lists

### Filtering Items
```jsx
function TaskList({ tasks, showCompleted }) {
  const filteredTasks = showCompleted 
    ? tasks 
    : tasks.filter(task => !task.completed);
  
  return (
    <ul>
      {filteredTasks.map(task => (
        <li key={task.id}>
          {task.title}
        </li>
      ))}
    </ul>
  );
}
```

### Conditional Elements
```jsx
function ProductList({ products }) {
  return (
    <div className="product-grid">
      {products.map(product => (
        <div key={product.id} className="product-card">
          <h3>{product.name}</h3>
          <p>${product.price}</p>
          {product.onSale && <span className="sale-badge">SALE!</span>}
          {product.stock === 0 && <span className="out-of-stock">Out of Stock</span>}
        </div>
      ))}
    </div>
  );
}
```

## Empty States

Handle empty lists gracefully:

```jsx
function ItemList({ items, title }) {
  if (items.length === 0) {
    return (
      <div className="empty-state">
        <p>No {title.toLowerCase()} found.</p>
        <button>Add your first {title.toLowerCase()}</button>
      </div>
    );
  }
  
  return (
    <ul>
      {items.map(item => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```

## Nested Lists

Render lists within lists:

```jsx
function CategoryList({ categories }) {
  return (
    <div className="categories">
      {categories.map(category => (
        <div key={category.id} className="category">
          <h2>{category.name}</h2>
          <ul>
            {category.items.map(item => (
              <li key={item.id}>{item.name}</li>
            ))}
          </ul>
        </div>
      ))}
    </div>
  );
}

// Example data
const categories = [
  {
    id: 1,
    name: 'Fruits',
    items: [
      { id: 11, name: 'Apple' },
      { id: 12, name: 'Banana' }
    ]
  },
  {
    id: 2,
    name: 'Vegetables',
    items: [
      { id: 21, name: 'Carrot' },
      { id: 22, name: 'Broccoli' }
    ]
  }
];
```

## Sorting Lists

```jsx
function SortableList({ items }) {
  const [sortBy, setSortBy] = useState('name');
  const [sortOrder, setSortOrder] = useState('asc');
  
  const sortedItems = [...items].sort((a, b) => {
    const aValue = a[sortBy];
    const bValue = b[sortBy];
    
    if (sortOrder === 'asc') {
      return aValue > bValue ? 1 : -1;
    } else {
      return aValue < bValue ? 1 : -1;
    }
  });
  
  return (
    <div>
      <div className="sort-controls">
        <select value={sortBy} onChange={(e) => setSortBy(e.target.value)}>
          <option value="name">Name</option>
          <option value="price">Price</option>
          <option value="date">Date</option>
        </select>
        <button onClick={() => setSortOrder(sortOrder === 'asc' ? 'desc' : 'asc')}>
          {sortOrder === 'asc' ? '↑' : '↓'}
        </button>
      </div>
      <ul>
        {sortedItems.map(item => (
          <li key={item.id}>
            {item.name} - ${item.price}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

## Pagination

Handle large lists with pagination:

```jsx
function PaginatedList({ items, itemsPerPage = 10 }) {
  const [currentPage, setCurrentPage] = useState(1);
  
  const totalPages = Math.ceil(items.length / itemsPerPage);
  const startIndex = (currentPage - 1) * itemsPerPage;
  const endIndex = startIndex + itemsPerPage;
  const currentItems = items.slice(startIndex, endIndex);
  
  return (
    <div>
      <ul>
        {currentItems.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
      
      <div className="pagination">
        <button 
          onClick={() => setCurrentPage(prev => Math.max(prev - 1, 1))}
          disabled={currentPage === 1}
        >
          Previous
        </button>
        
        <span>Page {currentPage} of {totalPages}</span>
        
        <button 
          onClick={() => setCurrentPage(prev => Math.min(prev + 1, totalPages))}
          disabled={currentPage === totalPages}
        >
          Next
        </button>
      </div>
    </div>
  );
}
```

## Infinite Scroll

Load more items as user scrolls:

```jsx
function InfiniteList() {
  const [items, setItems] = useState([]);
  const [loading, setLoading] = useState(false);
  const [page, setPage] = useState(1);
  const [hasMore, setHasMore] = useState(true);
  
  const loadMore = useCallback(async () => {
    if (loading || !hasMore) return;
    
    setLoading(true);
    try {
      const response = await fetch(`/api/items?page=${page}`);
      const newItems = await response.json();
      
      if (newItems.length === 0) {
        setHasMore(false);
      } else {
        setItems(prev => [...prev, ...newItems]);
        setPage(prev => prev + 1);
      }
    } finally {
      setLoading(false);
    }
  }, [page, loading, hasMore]);
  
  useEffect(() => {
    loadMore();
  }, []);
  
  useEffect(() => {
    const handleScroll = () => {
      if (window.innerHeight + window.scrollY >= document.body.offsetHeight - 100) {
        loadMore();
      }
    };
    
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, [loadMore]);
  
  return (
    <div>
      <ul>
        {items.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
      {loading && <p>Loading more...</p>}
      {!hasMore && <p>No more items</p>}
    </div>
  );
}
```

## Virtualized Lists

For very large lists, render only visible items:

```jsx
import { FixedSizeList } from 'react-window';

function VirtualizedList({ items }) {
  const Row = ({ index, style }) => (
    <div style={style}>
      {items[index].name}
    </div>
  );
  
  return (
    <FixedSizeList
      height={600}
      itemCount={items.length}
      itemSize={50}
      width="100%"
    >
      {Row}
    </FixedSizeList>
  );
}
```

## Dynamic Lists

Add, remove, and update list items:

```jsx
function DynamicList() {
  const [items, setItems] = useState([
    { id: 1, text: 'Item 1' },
    { id: 2, text: 'Item 2' }
  ]);
  const [newItem, setNewItem] = useState('');
  
  const addItem = () => {
    if (newItem.trim()) {
      setItems([...items, {
        id: Date.now(),
        text: newItem
      }]);
      setNewItem('');
    }
  };
  
  const removeItem = (id) => {
    setItems(items.filter(item => item.id !== id));
  };
  
  const updateItem = (id, newText) => {
    setItems(items.map(item =>
      item.id === id ? { ...item, text: newText } : item
    ));
  };
  
  return (
    <div>
      <div className="add-item">
        <input
          value={newItem}
          onChange={(e) => setNewItem(e.target.value)}
          placeholder="Add new item"
        />
        <button onClick={addItem}>Add</button>
      </div>
      
      <ul>
        {items.map(item => (
          <li key={item.id}>
            <input
              value={item.text}
              onChange={(e) => updateItem(item.id, e.target.value)}
            />
            <button onClick={() => removeItem(item.id)}>Remove</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

## Grouped Lists

Group items by a property:

```jsx
function GroupedList({ items }) {
  const groupedItems = items.reduce((groups, item) => {
    const group = item.category;
    if (!groups[group]) groups[group] = [];
    groups[group].push(item);
    return groups;
  }, {});
  
  return (
    <div>
      {Object.entries(groupedItems).map(([category, items]) => (
        <div key={category} className="group">
          <h3>{category}</h3>
          <ul>
            {items.map(item => (
              <li key={item.id}>{item.name}</li>
            ))}
          </ul>
        </div>
      ))}
    </div>
  );
}
```

## Performance Tips

### 1. Use React.memo for List Items
```jsx
const ListItem = React.memo(({ item, onUpdate }) => {
  console.log(`Rendering ${item.id}`);
  return (
    <li>
      {item.name}
      <button onClick={() => onUpdate(item.id)}>Update</button>
    </li>
  );
});
```

### 2. Stable Handler References
```jsx
function OptimizedList({ items }) {
  const handleUpdate = useCallback((id) => {
    // Update logic
  }, []);
  
  return (
    <ul>
      {items.map(item => (
        <ListItem 
          key={item.id} 
          item={item} 
          onUpdate={handleUpdate}
        />
      ))}
    </ul>
  );
}
```

### 3. Avoid Index as Key
```jsx
// ❌ Bad - can cause issues with reordering
{items.map((item, index) => <li key={index}>{item}</li>)}

// ✅ Good - stable unique identifier
{items.map(item => <li key={item.id}>{item.name}</li>)}
```

## Common Patterns

### Search and Filter
```jsx
function SearchableList({ items }) {
  const [search, setSearch] = useState('');
  
  const filteredItems = items.filter(item =>
    item.name.toLowerCase().includes(search.toLowerCase())
  );
  
  return (
    <div>
      <input
        type="text"
        placeholder="Search..."
        value={search}
        onChange={(e) => setSearch(e.target.value)}
      />
      <ul>
        {filteredItems.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

## Best Practices

1. **Always use keys** - Unique and stable identifiers
2. **Extract list items** - Create separate components
3. **Handle empty states** - Show meaningful messages
4. **Optimize performance** - Use React.memo and useCallback
5. **Avoid array index as key** - Unless list is static
6. **Consider virtualization** - For very large lists
7. **Implement loading states** - For async data
8. **Add error handling** - For data fetching

Lists are fundamental to React applications. Master these patterns and you'll be able to handle any list rendering scenario!