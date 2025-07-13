# Event Handling in React

Event handling in React is similar to handling events in DOM elements, but with some syntactic differences. React events are named using camelCase and you pass functions as event handlers.

## Basic Event Handling

### Simple Click Event
```jsx
function Button() {
  const handleClick = () => {
    alert('Button clicked!');
  };
  
  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

### Inline Event Handlers
```jsx
function Button() {
  return (
    <button onClick={() => alert('Clicked!')}>
      Click me
    </button>
  );
}
```

## Event Object

React wraps native events in SyntheticEvent for cross-browser compatibility:

```jsx
function Form() {
  const handleSubmit = (e) => {
    e.preventDefault(); // Prevent default form submission
    console.log('Form submitted');
  };
  
  const handleInput = (e) => {
    console.log('Input value:', e.target.value);
    console.log('Input name:', e.target.name);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input name="username" onChange={handleInput} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

## Common Event Types

### Mouse Events
```jsx
function MouseEvents() {
  return (
    <div
      onClick={() => console.log('Clicked')}
      onDoubleClick={() => console.log('Double clicked')}
      onMouseEnter={() => console.log('Mouse entered')}
      onMouseLeave={() => console.log('Mouse left')}
      onMouseMove={(e) => console.log(`Position: ${e.clientX}, ${e.clientY}`)}
      onMouseDown={() => console.log('Mouse down')}
      onMouseUp={() => console.log('Mouse up')}
      style={{ padding: 50, backgroundColor: 'lightblue' }}
    >
      Interact with me!
    </div>
  );
}
```

### Keyboard Events
```jsx
function KeyboardEvents() {
  const handleKeyPress = (e) => {
    console.log('Key pressed:', e.key);
    console.log('Key code:', e.keyCode);
    console.log('Ctrl pressed:', e.ctrlKey);
    console.log('Shift pressed:', e.shiftKey);
  };
  
  return (
    <input
      onKeyDown={(e) => console.log('Key down:', e.key)}
      onKeyUp={(e) => console.log('Key up:', e.key)}
      onKeyPress={handleKeyPress}
      placeholder="Type something..."
    />
  );
}
```

### Form Events
```jsx
function FormEvents() {
  return (
    <form>
      <input
        onChange={(e) => console.log('Changed:', e.target.value)}
        onFocus={() => console.log('Focused')}
        onBlur={() => console.log('Blurred')}
        placeholder="Text input"
      />
      
      <select onChange={(e) => console.log('Selected:', e.target.value)}>
        <option value="1">Option 1</option>
        <option value="2">Option 2</option>
      </select>
      
      <textarea
        onSelect={(e) => console.log('Text selected')}
        onCopy={() => console.log('Copied')}
        onPaste={() => console.log('Pasted')}
        onCut={() => console.log('Cut')}
      />
    </form>
  );
}
```

## Passing Arguments to Event Handlers

### Using Arrow Functions
```jsx
function ItemList() {
  const items = ['Apple', 'Banana', 'Orange'];
  
  const handleClick = (item, index) => {
    console.log(`Clicked ${item} at index ${index}`);
  };
  
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>
          <button onClick={() => handleClick(item, index)}>
            {item}
          </button>
        </li>
      ))}
    </ul>
  );
}
```

### Using bind
```jsx
function ItemList() {
  const items = ['Apple', 'Banana', 'Orange'];
  
  const handleClick = (item, index, event) => {
    console.log(`Clicked ${item} at index ${index}`);
  };
  
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>
          <button onClick={handleClick.bind(this, item, index)}>
            {item}
          </button>
        </li>
      ))}
    </ul>
  );
}
```

## Event Delegation

```jsx
function TodoList() {
  const [todos, setTodos] = useState([
    { id: 1, text: 'Learn React', done: false },
    { id: 2, text: 'Build an app', done: false }
  ]);
  
  // Single handler for all todo actions
  const handleTodoAction = (e) => {
    const action = e.target.dataset.action;
    const todoId = parseInt(e.target.dataset.id);
    
    switch (action) {
      case 'toggle':
        setTodos(todos.map(todo =>
          todo.id === todoId ? { ...todo, done: !todo.done } : todo
        ));
        break;
      case 'delete':
        setTodos(todos.filter(todo => todo.id !== todoId));
        break;
    }
  };
  
  return (
    <ul onClick={handleTodoAction}>
      {todos.map(todo => (
        <li key={todo.id}>
          <input
            type="checkbox"
            checked={todo.done}
            data-action="toggle"
            data-id={todo.id}
            readOnly
          />
          <span style={{ textDecoration: todo.done ? 'line-through' : 'none' }}>
            {todo.text}
          </span>
          <button data-action="delete" data-id={todo.id}>
            Delete
          </button>
        </li>
      ))}
    </ul>
  );
}
```

## Preventing Default Behavior

```jsx
function LinkComponent() {
  const handleClick = (e) => {
    e.preventDefault();
    console.log('Link clicked but navigation prevented');
  };
  
  return (
    <a href="https://example.com" onClick={handleClick}>
      Click me (won't navigate)
    </a>
  );
}

function FormComponent() {
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Form submitted without page reload');
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input type="text" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

## Stop Propagation

```jsx
function EventPropagation() {
  return (
    <div onClick={() => console.log('Outer div clicked')}>
      <button onClick={(e) => {
        e.stopPropagation();
        console.log('Button clicked (propagation stopped)');
      }}>
        Click me
      </button>
    </div>
  );
}
```

## Custom Events

```jsx
function CustomEventComponent() {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    const handleCustomEvent = (e) => {
      setData(e.detail);
    };
    
    window.addEventListener('myCustomEvent', handleCustomEvent);
    
    return () => {
      window.removeEventListener('myCustomEvent', handleCustomEvent);
    };
  }, []);
  
  const triggerCustomEvent = () => {
    const event = new CustomEvent('myCustomEvent', {
      detail: { message: 'Hello from custom event!' }
    });
    window.dispatchEvent(event);
  };
  
  return (
    <div>
      <button onClick={triggerCustomEvent}>Trigger Custom Event</button>
      {data && <p>Received: {data.message}</p>}
    </div>
  );
}
```

## Touch Events

```jsx
function TouchComponent() {
  const [touchPos, setTouchPos] = useState({ x: 0, y: 0 });
  
  const handleTouchMove = (e) => {
    const touch = e.touches[0];
    setTouchPos({ x: touch.clientX, y: touch.clientY });
  };
  
  return (
    <div
      onTouchStart={() => console.log('Touch started')}
      onTouchMove={handleTouchMove}
      onTouchEnd={() => console.log('Touch ended')}
      style={{
        width: 200,
        height: 200,
        backgroundColor: 'lightgreen',
        touchAction: 'none'
      }}
    >
      Touch me!
      <p>Position: {touchPos.x}, {touchPos.y}</p>
    </div>
  );
}
```

## Drag and Drop Events

```jsx
function DragDropComponent() {
  const [draggedItem, setDraggedItem] = useState(null);
  const [items, setItems] = useState(['Item 1', 'Item 2', 'Item 3']);
  
  const handleDragStart = (e, item) => {
    setDraggedItem(item);
    e.dataTransfer.effectAllowed = 'move';
  };
  
  const handleDragOver = (e) => {
    e.preventDefault();
    e.dataTransfer.dropEffect = 'move';
  };
  
  const handleDrop = (e, targetItem) => {
    e.preventDefault();
    
    const draggedIndex = items.indexOf(draggedItem);
    const targetIndex = items.indexOf(targetItem);
    
    const newItems = [...items];
    newItems.splice(draggedIndex, 1);
    newItems.splice(targetIndex, 0, draggedItem);
    
    setItems(newItems);
    setDraggedItem(null);
  };
  
  return (
    <ul>
      {items.map((item, index) => (
        <li
          key={index}
          draggable
          onDragStart={(e) => handleDragStart(e, item)}
          onDragOver={handleDragOver}
          onDrop={(e) => handleDrop(e, item)}
          style={{
            padding: 10,
            margin: 5,
            backgroundColor: 'lightgray',
            cursor: 'move'
          }}
        >
          {item}
        </li>
      ))}
    </ul>
  );
}
```

## Performance Considerations

### Avoid Creating Functions in Render
```jsx
// ❌ Creates new function on every render
function BadExample() {
  return (
    <button onClick={() => console.log('Clicked')}>
      Click me
    </button>
  );
}

// ✅ Better - function created once
function GoodExample() {
  const handleClick = useCallback(() => {
    console.log('Clicked');
  }, []);
  
  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

### Debouncing Events
```jsx
function SearchInput() {
  const [value, setValue] = useState('');
  const [searchTerm, setSearchTerm] = useState('');
  
  useEffect(() => {
    const timer = setTimeout(() => {
      setSearchTerm(value);
    }, 500);
    
    return () => clearTimeout(timer);
  }, [value]);
  
  return (
    <div>
      <input
        value={value}
        onChange={(e) => setValue(e.target.value)}
        placeholder="Search..."
      />
      <p>Searching for: {searchTerm}</p>
    </div>
  );
}
```

### Throttling Events
```jsx
function useThrottle(callback, delay) {
  const lastRun = useRef(Date.now());
  
  return useCallback((...args) => {
    if (Date.now() - lastRun.current >= delay) {
      callback(...args);
      lastRun.current = Date.now();
    }
  }, [callback, delay]);
}

function ScrollComponent() {
  const [scrollY, setScrollY] = useState(0);
  
  const handleScroll = useThrottle(() => {
    setScrollY(window.scrollY);
  }, 100);
  
  useEffect(() => {
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, [handleScroll]);
  
  return <div>Scroll position: {scrollY}</div>;
}
```

## Best Practices

1. **Use semantic event names** - onClick not handleClick for props
2. **Prevent default when needed** - Forms, links, etc.
3. **Clean up event listeners** - Prevent memory leaks
4. **Use event delegation** - For dynamic lists
5. **Debounce/throttle** - For performance-sensitive events
6. **Avoid inline functions** - When possible, for performance
7. **Handle errors gracefully** - Try-catch in event handlers

Event handling is fundamental to creating interactive React applications. Master these patterns to build responsive user interfaces!