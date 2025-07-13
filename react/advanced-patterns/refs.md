# Working with Refs in React

Refs provide a way to access DOM elements and component instances directly in React. They're useful for managing focus, triggering animations, or integrating with third-party libraries.

## useRef Hook

### Basic useRef Usage
```jsx
function TextInput() {
  const inputRef = useRef(null);
  
  const focusInput = () => {
    inputRef.current.focus();
  };
  
  return (
    <div>
      <input
        ref={inputRef}
        type="text"
        placeholder="Click button to focus"
      />
      <button onClick={focusInput}>
        Focus Input
      </button>
    </div>
  );
}
```

### Storing Mutable Values
```jsx
function Timer() {
  const [seconds, setSeconds] = useState(0);
  const [isRunning, setIsRunning] = useState(false);
  const intervalRef = useRef(null);
  
  const startTimer = () => {
    if (!isRunning) {
      setIsRunning(true);
      intervalRef.current = setInterval(() => {
        setSeconds(prev => prev + 1);
      }, 1000);
    }
  };
  
  const stopTimer = () => {
    if (isRunning) {
      setIsRunning(false);
      clearInterval(intervalRef.current);
    }
  };
  
  const resetTimer = () => {
    setSeconds(0);
    setIsRunning(false);
    if (intervalRef.current) {
      clearInterval(intervalRef.current);
    }
  };
  
  useEffect(() => {
    // Cleanup on unmount
    return () => {
      if (intervalRef.current) {
        clearInterval(intervalRef.current);
      }
    };
  }, []);
  
  return (
    <div>
      <h2>Timer: {seconds} seconds</h2>
      <button onClick={startTimer} disabled={isRunning}>
        Start
      </button>
      <button onClick={stopTimer} disabled={!isRunning}>
        Stop
      </button>
      <button onClick={resetTimer}>
        Reset
      </button>
    </div>
  );
}
```

## DOM Manipulation with Refs

### Scrolling and Focus Management
```jsx
function ScrollToElement() {
  const topRef = useRef(null);
  const middleRef = useRef(null);
  const bottomRef = useRef(null);
  
  const scrollToSection = (ref) => {
    ref.current?.scrollIntoView({ 
      behavior: 'smooth',
      block: 'start'
    });
  };
  
  return (
    <div>
      <nav style={{ position: 'fixed', top: 0, background: 'white' }}>
        <button onClick={() => scrollToSection(topRef)}>
          Go to Top
        </button>
        <button onClick={() => scrollToSection(middleRef)}>
          Go to Middle
        </button>
        <button onClick={() => scrollToSection(bottomRef)}>
          Go to Bottom
        </button>
      </nav>
      
      <div style={{ marginTop: '60px' }}>
        <div ref={topRef} style={{ height: '100vh', background: '#f0f8ff' }}>
          <h2>Top Section</h2>
          <p>This is the top of the page</p>
        </div>
        
        <div ref={middleRef} style={{ height: '100vh', background: '#f0fff0' }}>
          <h2>Middle Section</h2>
          <p>This is the middle of the page</p>
        </div>
        
        <div ref={bottomRef} style={{ height: '100vh', background: '#fff0f0' }}>
          <h2>Bottom Section</h2>
          <p>This is the bottom of the page</p>
        </div>
      </div>
    </div>
  );
}
```

### Canvas Integration
```jsx
function CanvasDrawing() {
  const canvasRef = useRef(null);
  const [isDrawing, setIsDrawing] = useState(false);
  const [lastPos, setLastPos] = useState({ x: 0, y: 0 });
  
  useEffect(() => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext('2d');
    
    // Set canvas size
    canvas.width = 800;
    canvas.height = 400;
    
    // Set drawing styles
    ctx.lineCap = 'round';
    ctx.lineJoin = 'round';
    ctx.lineWidth = 2;
    ctx.strokeStyle = '#000';
  }, []);
  
  const getMousePos = (e) => {
    const canvas = canvasRef.current;
    const rect = canvas.getBoundingClientRect();
    return {
      x: e.clientX - rect.left,
      y: e.clientY - rect.top
    };
  };
  
  const startDrawing = (e) => {
    setIsDrawing(true);
    const pos = getMousePos(e);
    setLastPos(pos);
  };
  
  const draw = (e) => {
    if (!isDrawing) return;
    
    const canvas = canvasRef.current;
    const ctx = canvas.getContext('2d');
    const currentPos = getMousePos(e);
    
    ctx.beginPath();
    ctx.moveTo(lastPos.x, lastPos.y);
    ctx.lineTo(currentPos.x, currentPos.y);
    ctx.stroke();
    
    setLastPos(currentPos);
  };
  
  const stopDrawing = () => {
    setIsDrawing(false);
  };
  
  const clearCanvas = () => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext('2d');
    ctx.clearRect(0, 0, canvas.width, canvas.height);
  };
  
  return (
    <div>
      <h3>Canvas Drawing</h3>
      <canvas
        ref={canvasRef}
        onMouseDown={startDrawing}
        onMouseMove={draw}
        onMouseUp={stopDrawing}
        onMouseLeave={stopDrawing}
        style={{ 
          border: '1px solid #ccc',
          cursor: 'crosshair'
        }}
      />
      <br />
      <button onClick={clearCanvas}>Clear Canvas</button>
    </div>
  );
}
```

## forwardRef

### Forwarding Refs to Custom Components
```jsx
// Custom input component that forwards ref
const CustomInput = forwardRef(function CustomInput(props, ref) {
  const { label, error, ...inputProps } = props;
  
  return (
    <div className="form-field">
      {label && <label>{label}</label>}
      <input
        ref={ref}
        className={error ? 'input-error' : 'input-normal'}
        {...inputProps}
      />
      {error && <span className="error-text">{error}</span>}
    </div>
  );
});

// Using the forwarded ref
function Form() {
  const emailRef = useRef(null);
  const passwordRef = useRef(null);
  
  const focusEmail = () => {
    emailRef.current?.focus();
  };
  
  const focusPassword = () => {
    passwordRef.current?.focus();
  };
  
  return (
    <form>
      <CustomInput
        ref={emailRef}
        label="Email"
        type="email"
        placeholder="Enter your email"
      />
      
      <CustomInput
        ref={passwordRef}
        label="Password"
        type="password"
        placeholder="Enter your password"
      />
      
      <div>
        <button type="button" onClick={focusEmail}>
          Focus Email
        </button>
        <button type="button" onClick={focusPassword}>
          Focus Password
        </button>
      </div>
    </form>
  );
}
```

### Complex Component with forwardRef
```jsx
const Modal = forwardRef(function Modal({ 
  isOpen, 
  onClose, 
  title, 
  children 
}, ref) {
  const modalRef = useRef(null);
  
  // Combine external ref with internal ref
  useImperativeHandle(ref, () => ({
    focus: () => {
      modalRef.current?.focus();
    },
    close: onClose,
    getElement: () => modalRef.current
  }));
  
  useEffect(() => {
    if (isOpen && modalRef.current) {
      modalRef.current.focus();
    }
  }, [isOpen]);
  
  if (!isOpen) return null;
  
  return (
    <div className="modal-overlay" onClick={onClose}>
      <div
        ref={modalRef}
        className="modal-content"
        onClick={e => e.stopPropagation()}
        tabIndex={-1}
        role="dialog"
        aria-modal="true"
        aria-labelledby="modal-title"
      >
        <div className="modal-header">
          <h2 id="modal-title">{title}</h2>
          <button onClick={onClose} aria-label="Close modal">
            ×
          </button>
        </div>
        <div className="modal-body">
          {children}
        </div>
      </div>
    </div>
  );
});

// Using the modal with ref
function App() {
  const [isModalOpen, setIsModalOpen] = useState(false);
  const modalRef = useRef(null);
  
  const openModal = () => {
    setIsModalOpen(true);
  };
  
  const closeModal = () => {
    setIsModalOpen(false);
  };
  
  const focusModal = () => {
    modalRef.current?.focus();
  };
  
  return (
    <div>
      <button onClick={openModal}>Open Modal</button>
      <button onClick={focusModal}>Focus Modal</button>
      
      <Modal
        ref={modalRef}
        isOpen={isModalOpen}
        onClose={closeModal}
        title="Example Modal"
      >
        <p>This is modal content</p>
        <button onClick={closeModal}>Close from inside</button>
      </Modal>
    </div>
  );
}
```

## useImperativeHandle

### Exposing Custom API
```jsx
const VideoPlayer = forwardRef(function VideoPlayer({ src }, ref) {
  const videoRef = useRef(null);
  const [isPlaying, setIsPlaying] = useState(false);
  const [currentTime, setCurrentTime] = useState(0);
  const [duration, setDuration] = useState(0);
  
  useImperativeHandle(ref, () => ({
    play: () => {
      videoRef.current?.play();
      setIsPlaying(true);
    },
    pause: () => {
      videoRef.current?.pause();
      setIsPlaying(false);
    },
    stop: () => {
      if (videoRef.current) {
        videoRef.current.pause();
        videoRef.current.currentTime = 0;
        setIsPlaying(false);
        setCurrentTime(0);
      }
    },
    seek: (time) => {
      if (videoRef.current) {
        videoRef.current.currentTime = time;
        setCurrentTime(time);
      }
    },
    setVolume: (volume) => {
      if (videoRef.current) {
        videoRef.current.volume = Math.max(0, Math.min(1, volume));
      }
    },
    isPlaying: () => isPlaying,
    getCurrentTime: () => currentTime,
    getDuration: () => duration
  }));
  
  const handleTimeUpdate = () => {
    if (videoRef.current) {
      setCurrentTime(videoRef.current.currentTime);
    }
  };
  
  const handleLoadedMetadata = () => {
    if (videoRef.current) {
      setDuration(videoRef.current.duration);
    }
  };
  
  return (
    <video
      ref={videoRef}
      src={src}
      onTimeUpdate={handleTimeUpdate}
      onLoadedMetadata={handleLoadedMetadata}
      controls
      style={{ width: '100%', maxWidth: '500px' }}
    />
  );
});

// Using the video player
function VideoApp() {
  const playerRef = useRef(null);
  
  const handlePlay = () => {
    playerRef.current?.play();
  };
  
  const handlePause = () => {
    playerRef.current?.pause();
  };
  
  const handleStop = () => {
    playerRef.current?.stop();
  };
  
  const handleSeek = (seconds) => {
    playerRef.current?.seek(seconds);
  };
  
  const getPlayerInfo = () => {
    if (playerRef.current) {
      const isPlaying = playerRef.current.isPlaying();
      const currentTime = playerRef.current.getCurrentTime();
      const duration = playerRef.current.getDuration();
      
      alert(`Playing: ${isPlaying}, Time: ${currentTime.toFixed(1)}/${duration.toFixed(1)}s`);
    }
  };
  
  return (
    <div>
      <VideoPlayer
        ref={playerRef}
        src="/sample-video.mp4"
      />
      
      <div style={{ marginTop: '10px' }}>
        <button onClick={handlePlay}>Play</button>
        <button onClick={handlePause}>Pause</button>
        <button onClick={handleStop}>Stop</button>
        <button onClick={() => handleSeek(30)}>Seek to 30s</button>
        <button onClick={getPlayerInfo}>Get Info</button>
      </div>
    </div>
  );
}
```

## Refs with Lists and Dynamic Elements

### Managing Multiple Refs
```jsx
function TodoList() {
  const [todos, setTodos] = useState([
    { id: 1, text: 'Learn React refs', completed: false },
    { id: 2, text: 'Build a todo app', completed: false }
  ]);
  
  const [editingId, setEditingId] = useState(null);
  const inputRefs = useRef(new Map());
  
  const addTodo = () => {
    const newTodo = {
      id: Date.now(),
      text: 'New todo',
      completed: false
    };
    setTodos(prev => [...prev, newTodo]);
    setEditingId(newTodo.id);
  };
  
  const startEditing = (id) => {
    setEditingId(id);
    // Focus input after state update
    setTimeout(() => {
      const input = inputRefs.current.get(id);
      if (input) {
        input.focus();
        input.select();
      }
    }, 0);
  };
  
  const saveEdit = (id, newText) => {
    setTodos(prev => prev.map(todo =>
      todo.id === id ? { ...todo, text: newText } : todo
    ));
    setEditingId(null);
  };
  
  const deleteTodo = (id) => {
    setTodos(prev => prev.filter(todo => todo.id !== id));
    inputRefs.current.delete(id);
  };
  
  const setInputRef = (id, element) => {
    if (element) {
      inputRefs.current.set(id, element);
    } else {
      inputRefs.current.delete(id);
    }
  };
  
  return (
    <div>
      <h3>Todo List with Refs</h3>
      <button onClick={addTodo}>Add Todo</button>
      
      <ul>
        {todos.map(todo => (
          <li key={todo.id} style={{ margin: '10px 0' }}>
            {editingId === todo.id ? (
              <EditableInput
                initialValue={todo.text}
                onSave={(text) => saveEdit(todo.id, text)}
                onCancel={() => setEditingId(null)}
                ref={(el) => setInputRef(todo.id, el)}
              />
            ) : (
              <div style={{ display: 'flex', alignItems: 'center', gap: '10px' }}>
                <span>{todo.text}</span>
                <button onClick={() => startEditing(todo.id)}>Edit</button>
                <button onClick={() => deleteTodo(todo.id)}>Delete</button>
              </div>
            )}
          </li>
        ))}
      </ul>
    </div>
  );
}

const EditableInput = forwardRef(function EditableInput({
  initialValue,
  onSave,
  onCancel
}, ref) {
  const [value, setValue] = useState(initialValue);
  
  const handleKeyDown = (e) => {
    if (e.key === 'Enter') {
      onSave(value);
    } else if (e.key === 'Escape') {
      onCancel();
    }
  };
  
  return (
    <div style={{ display: 'flex', gap: '5px' }}>
      <input
        ref={ref}
        value={value}
        onChange={(e) => setValue(e.target.value)}
        onKeyDown={handleKeyDown}
        onBlur={() => onSave(value)}
      />
      <button onClick={() => onSave(value)}>Save</button>
      <button onClick={onCancel}>Cancel</button>
    </div>
  );
});
```

## Third-Party Library Integration

### Integrating with DOM Libraries
```jsx
function ChartComponent({ data }) {
  const chartRef = useRef(null);
  const chartInstance = useRef(null);
  
  useEffect(() => {
    if (!chartRef.current) return;
    
    // Initialize chart library (example with Chart.js)
    const ctx = chartRef.current.getContext('2d');
    
    chartInstance.current = new Chart(ctx, {
      type: 'line',
      data: {
        labels: data.labels,
        datasets: [{
          label: 'Sample Data',
          data: data.values,
          borderColor: 'rgb(75, 192, 192)',
          tension: 0.1
        }]
      },
      options: {
        responsive: true,
        maintainAspectRatio: false
      }
    });
    
    return () => {
      // Cleanup
      if (chartInstance.current) {
        chartInstance.current.destroy();
      }
    };
  }, []);
  
  useEffect(() => {
    // Update chart when data changes
    if (chartInstance.current) {
      chartInstance.current.data.labels = data.labels;
      chartInstance.current.data.datasets[0].data = data.values;
      chartInstance.current.update();
    }
  }, [data]);
  
  return (
    <div style={{ height: '400px', width: '100%' }}>
      <canvas ref={chartRef} />
    </div>
  );
}
```

## Common Patterns and Best Practices

### Ref Callback Pattern
```jsx
function AutoFocusInput({ shouldFocus }) {
  const [inputElement, setInputElement] = useState(null);
  
  // Ref callback function
  const inputRef = useCallback((node) => {
    if (node !== null) {
      setInputElement(node);
      if (shouldFocus) {
        node.focus();
      }
    }
  }, [shouldFocus]);
  
  return (
    <input
      ref={inputRef}
      placeholder="This input auto-focuses"
    />
  );
}
```

### Measuring Elements
```jsx
function MeasuredComponent() {
  const [dimensions, setDimensions] = useState({ width: 0, height: 0 });
  const elementRef = useRef(null);
  
  useEffect(() => {
    const updateDimensions = () => {
      if (elementRef.current) {
        const { offsetWidth, offsetHeight } = elementRef.current;
        setDimensions({ width: offsetWidth, height: offsetHeight });
      }
    };
    
    updateDimensions();
    
    const resizeObserver = new ResizeObserver(updateDimensions);
    if (elementRef.current) {
      resizeObserver.observe(elementRef.current);
    }
    
    return () => {
      resizeObserver.disconnect();
    };
  }, []);
  
  return (
    <div>
      <div
        ref={elementRef}
        style={{
          width: '50%',
          height: '200px',
          background: 'lightblue',
          resize: 'both',
          overflow: 'auto',
          border: '1px solid #ccc'
        }}
      >
        Resize me!
      </div>
      <p>Dimensions: {dimensions.width} x {dimensions.height}</p>
    </div>
  );
}
```

## Best Practices

1. **Use Refs Sparingly**
   - Prefer React's declarative patterns
   - Only use refs when necessary

2. **Common Valid Use Cases**
   - Managing focus, text selection, media playback
   - Triggering animations
   - Integrating with third-party DOM libraries

3. **Avoid Refs For**
   - Anything that can be done declaratively
   - Accessing child component data
   - Triggering re-renders

4. **Cleanup**
   - Clear intervals and timeouts stored in refs
   - Remove event listeners
   - Destroy third-party library instances

5. **Ref Forwarding**
   - Use forwardRef for reusable components
   - Use useImperativeHandle to expose only necessary methods

6. **Performance**
   - Don't overuse refs in lists
   - Use ref callbacks for dynamic elements
   - Consider using keys for list items

Refs are a powerful escape hatch for accessing the DOM directly, but should be used judiciously in React applications!