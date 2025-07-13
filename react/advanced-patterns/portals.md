# React Portals

React Portals provide a way to render children into a DOM node that exists outside the parent component's DOM hierarchy. This is useful for modals, tooltips, dropdowns, and other overlay components.

## Basic Portal Usage

### Simple Portal
```jsx
import { createPortal } from 'react-dom';

function Modal({ isOpen, onClose, children }) {
  if (!isOpen) return null;
  
  // Render into a different DOM node
  return createPortal(
    <div className="modal-overlay" onClick={onClose}>
      <div className="modal-content" onClick={e => e.stopPropagation()}>
        {children}
        <button onClick={onClose}>Close</button>
      </div>
    </div>,
    document.getElementById('modal-root') // Target DOM node
  );
}

// HTML structure needed
/*
<div id="root">
  <!-- Your main React app -->
</div>
<div id="modal-root"></div>
*/

// Usage
function App() {
  const [showModal, setShowModal] = useState(false);
  
  return (
    <div>
      <h1>My App</h1>
      <button onClick={() => setShowModal(true)}>
        Open Modal
      </button>
      
      <Modal isOpen={showModal} onClose={() => setShowModal(false)}>
        <h2>Modal Content</h2>
        <p>This is rendered in a portal!</p>
      </Modal>
    </div>
  );
}
```

### Creating Portal Container
```jsx
// Utility function to create portal container
function createPortalContainer(id) {
  let container = document.getElementById(id);
  
  if (!container) {
    container = document.createElement('div');
    container.id = id;
    document.body.appendChild(container);
  }
  
  return container;
}

// Custom hook for portal management
function usePortal(id = 'portal-root') {
  const [container] = useState(() => createPortalContainer(id));
  
  useEffect(() => {
    return () => {
      // Clean up if container is empty
      if (container.childNodes.length === 0) {
        document.body.removeChild(container);
      }
    };
  }, [container]);
  
  return container;
}

// Portal component using the hook
function Portal({ children, id }) {
  const container = usePortal(id);
  
  return createPortal(children, container);
}

// Usage
function MyModal({ children }) {
  return (
    <Portal id="modal-root">
      <div className="modal">
        {children}
      </div>
    </Portal>
  );
}
```

## Modal Component with Portal

### Full-Featured Modal
```jsx
function Modal({ 
  isOpen, 
  onClose, 
  children,
  closeOnOverlayClick = true,
  closeOnEscape = true,
  size = 'medium'
}) {
  const [container] = useState(() => {
    let el = document.getElementById('modal-root');
    if (!el) {
      el = document.createElement('div');
      el.id = 'modal-root';
      document.body.appendChild(el);
    }
    return el;
  });
  
  // Handle escape key
  useEffect(() => {
    if (!isOpen || !closeOnEscape) return;
    
    const handleEscape = (e) => {
      if (e.key === 'Escape') {
        onClose();
      }
    };
    
    document.addEventListener('keydown', handleEscape);
    return () => document.removeEventListener('keydown', handleEscape);
  }, [isOpen, onClose, closeOnEscape]);
  
  // Prevent body scroll when modal is open
  useEffect(() => {
    if (isOpen) {
      document.body.style.overflow = 'hidden';
      return () => {
        document.body.style.overflow = 'unset';
      };
    }
  }, [isOpen]);
  
  // Focus management
  const modalRef = useRef();
  
  useEffect(() => {
    if (isOpen && modalRef.current) {
      const focusableElements = modalRef.current.querySelectorAll(
        'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
      );
      
      if (focusableElements.length > 0) {
        focusableElements[0].focus();
      }
    }
  }, [isOpen]);
  
  if (!isOpen) return null;
  
  const sizeClasses = {
    small: 'modal-small',
    medium: 'modal-medium',
    large: 'modal-large',
    fullscreen: 'modal-fullscreen'
  };
  
  return createPortal(
    <div
      className="modal-overlay"
      onClick={closeOnOverlayClick ? onClose : undefined}
      role="dialog"
      aria-modal="true"
    >
      <div
        ref={modalRef}
        className={`modal-content ${sizeClasses[size]}`}
        onClick={e => e.stopPropagation()}
        role="document"
      >
        {children}
      </div>
    </div>,
    container
  );
}

// Modal components
function ConfirmModal({ isOpen, onClose, onConfirm, title, message }) {
  return (
    <Modal isOpen={isOpen} onClose={onClose} size="small">
      <div className="confirm-modal">
        <h3>{title}</h3>
        <p>{message}</p>
        <div className="modal-actions">
          <button onClick={onClose}>Cancel</button>
          <button onClick={onConfirm} className="danger">
            Confirm
          </button>
        </div>
      </div>
    </Modal>
  );
}

function FormModal({ isOpen, onClose, onSubmit, title, children }) {
  const handleSubmit = (e) => {
    e.preventDefault();
    onSubmit();
  };
  
  return (
    <Modal isOpen={isOpen} onClose={onClose} size="medium">
      <form onSubmit={handleSubmit}>
        <div className="modal-header">
          <h2>{title}</h2>
          <button type="button" onClick={onClose}>×</button>
        </div>
        <div className="modal-body">
          {children}
        </div>
        <div className="modal-footer">
          <button type="button" onClick={onClose}>Cancel</button>
          <button type="submit">Submit</button>
        </div>
      </form>
    </Modal>
  );
}
```

## Tooltip with Portal

### Positioned Tooltip
```jsx
function Tooltip({ 
  children, 
  content, 
  position = 'top', 
  trigger = 'hover',
  delay = 0 
}) {
  const [isVisible, setIsVisible] = useState(false);
  const [tooltipStyle, setTooltipStyle] = useState({});
  const triggerRef = useRef();
  const tooltipRef = useRef();
  const timeoutRef = useRef();
  
  const showTooltip = () => {
    if (timeoutRef.current) {
      clearTimeout(timeoutRef.current);
    }
    
    timeoutRef.current = setTimeout(() => {
      setIsVisible(true);
      updatePosition();
    }, delay);
  };
  
  const hideTooltip = () => {
    if (timeoutRef.current) {
      clearTimeout(timeoutRef.current);
    }
    setIsVisible(false);
  };
  
  const updatePosition = () => {
    if (!triggerRef.current) return;
    
    const triggerRect = triggerRef.current.getBoundingClientRect();
    const scrollTop = window.pageYOffset;
    const scrollLeft = window.pageXOffset;
    
    const positions = {
      top: {
        left: triggerRect.left + scrollLeft + triggerRect.width / 2,
        top: triggerRect.top + scrollTop - 8,
        transform: 'translate(-50%, -100%)'
      },
      bottom: {
        left: triggerRect.left + scrollLeft + triggerRect.width / 2,
        top: triggerRect.bottom + scrollTop + 8,
        transform: 'translate(-50%, 0)'
      },
      left: {
        left: triggerRect.left + scrollLeft - 8,
        top: triggerRect.top + scrollTop + triggerRect.height / 2,
        transform: 'translate(-100%, -50%)'
      },
      right: {
        left: triggerRect.right + scrollLeft + 8,
        top: triggerRect.top + scrollTop + triggerRect.height / 2,
        transform: 'translate(0, -50%)'
      }
    };
    
    setTooltipStyle(positions[position]);
  };
  
  const eventHandlers = trigger === 'hover' ? {
    onMouseEnter: showTooltip,
    onMouseLeave: hideTooltip
  } : {
    onClick: () => isVisible ? hideTooltip() : showTooltip()
  };
  
  useEffect(() => {
    if (isVisible) {
      const handleResize = () => updatePosition();
      const handleScroll = () => updatePosition();
      
      window.addEventListener('resize', handleResize);
      window.addEventListener('scroll', handleScroll);
      
      return () => {
        window.removeEventListener('resize', handleResize);
        window.removeEventListener('scroll', handleScroll);
      };
    }
  }, [isVisible]);
  
  return (
    <>
      <span ref={triggerRef} {...eventHandlers}>
        {children}
      </span>
      
      {isVisible && createPortal(
        <div
          ref={tooltipRef}
          className={`tooltip tooltip-${position}`}
          style={{
            position: 'absolute',
            zIndex: 9999,
            ...tooltipStyle
          }}
        >
          {content}
        </div>,
        document.body
      )}
    </>
  );
}

// Usage
function TooltipExample() {
  return (
    <div>
      <Tooltip content="This is a tooltip!" position="top">
        <button>Hover me</button>
      </Tooltip>
      
      <Tooltip 
        content="Click tooltip" 
        position="right" 
        trigger="click"
      >
        <span>Click me</span>
      </Tooltip>
    </div>
  );
}
```

## Dropdown with Portal

### Accessible Dropdown
```jsx
function Dropdown({ 
  trigger, 
  children, 
  isOpen, 
  onToggle,
  closeOnClickOutside = true,
  position = 'bottom-left' 
}) {
  const [dropdownStyle, setDropdownStyle] = useState({});
  const triggerRef = useRef();
  const dropdownRef = useRef();
  
  // Handle click outside
  useEffect(() => {
    if (!isOpen || !closeOnClickOutside) return;
    
    const handleClickOutside = (event) => {
      if (
        triggerRef.current?.contains(event.target) ||
        dropdownRef.current?.contains(event.target)
      ) {
        return;
      }
      onToggle(false);
    };
    
    document.addEventListener('mousedown', handleClickOutside);
    return () => document.removeEventListener('mousedown', handleClickOutside);
  }, [isOpen, onToggle, closeOnClickOutside]);
  
  // Position dropdown
  useEffect(() => {
    if (!isOpen || !triggerRef.current) return;
    
    const triggerRect = triggerRef.current.getBoundingClientRect();
    const scrollTop = window.pageYOffset;
    const scrollLeft = window.pageXOffset;
    
    const positions = {
      'bottom-left': {
        left: triggerRect.left + scrollLeft,
        top: triggerRect.bottom + scrollTop + 4
      },
      'bottom-right': {
        right: window.innerWidth - triggerRect.right - scrollLeft,
        top: triggerRect.bottom + scrollTop + 4
      },
      'top-left': {
        left: triggerRect.left + scrollLeft,
        bottom: window.innerHeight - triggerRect.top - scrollTop + 4
      },
      'top-right': {
        right: window.innerWidth - triggerRect.right - scrollLeft,
        bottom: window.innerHeight - triggerRect.top - scrollTop + 4
      }
    };
    
    setDropdownStyle(positions[position]);
  }, [isOpen, position]);
  
  // Handle escape key
  useEffect(() => {
    if (!isOpen) return;
    
    const handleEscape = (e) => {
      if (e.key === 'Escape') {
        onToggle(false);
      }
    };
    
    document.addEventListener('keydown', handleEscape);
    return () => document.removeEventListener('keydown', handleEscape);
  }, [isOpen, onToggle]);
  
  return (
    <>
      <div ref={triggerRef}>
        {trigger}
      </div>
      
      {isOpen && createPortal(
        <div
          ref={dropdownRef}
          className="dropdown-menu"
          style={{
            position: 'absolute',
            zIndex: 1000,
            ...dropdownStyle
          }}
          role="menu"
        >
          {children}
        </div>,
        document.body
      )}
    </>
  );
}

// Usage
function DropdownExample() {
  const [isOpen, setIsOpen] = useState(false);
  
  return (
    <Dropdown
      trigger={
        <button onClick={() => setIsOpen(!isOpen)}>
          Menu {isOpen ? '▲' : '▼'}
        </button>
      }
      isOpen={isOpen}
      onToggle={setIsOpen}
      position="bottom-left"
    >
      <div className="dropdown-item" onClick={() => setIsOpen(false)}>
        Profile
      </div>
      <div className="dropdown-item" onClick={() => setIsOpen(false)}>
        Settings
      </div>
      <div className="dropdown-item" onClick={() => setIsOpen(false)}>
        Logout
      </div>
    </Dropdown>
  );
}
```

## Notification System

### Toast Notifications with Portal
```jsx
// Notification context and provider
const NotificationContext = createContext();

function NotificationProvider({ children }) {
  const [notifications, setNotifications] = useState([]);
  
  const addNotification = (notification) => {
    const id = Date.now().toString();
    const newNotification = {
      id,
      type: 'info',
      duration: 5000,
      ...notification
    };
    
    setNotifications(prev => [...prev, newNotification]);
    
    // Auto remove after duration
    if (newNotification.duration > 0) {
      setTimeout(() => {
        removeNotification(id);
      }, newNotification.duration);
    }
  };
  
  const removeNotification = (id) => {
    setNotifications(prev => prev.filter(n => n.id !== id));
  };
  
  const value = {
    notifications,
    addNotification,
    removeNotification
  };
  
  return (
    <NotificationContext.Provider value={value}>
      {children}
      <NotificationContainer />
    </NotificationContext.Provider>
  );
}

// Notification container (rendered in portal)
function NotificationContainer() {
  const { notifications, removeNotification } = useContext(NotificationContext);
  
  if (notifications.length === 0) return null;
  
  return createPortal(
    <div className="notification-container">
      {notifications.map(notification => (
        <Notification
          key={notification.id}
          notification={notification}
          onClose={() => removeNotification(notification.id)}
        />
      ))}
    </div>,
    document.body
  );
}

// Individual notification component
function Notification({ notification, onClose }) {
  const [isVisible, setIsVisible] = useState(false);
  
  useEffect(() => {
    // Animate in
    setTimeout(() => setIsVisible(true), 10);
  }, []);
  
  const handleClose = () => {
    setIsVisible(false);
    setTimeout(onClose, 300); // Wait for animation
  };
  
  return (
    <div 
      className={`notification notification-${notification.type} ${
        isVisible ? 'notification-visible' : ''
      }`}
    >
      <div className="notification-content">
        <strong>{notification.title}</strong>
        {notification.message && <p>{notification.message}</p>}
      </div>
      <button onClick={handleClose} className="notification-close">
        ×
      </button>
    </div>
  );
}

// Hook to use notifications
function useNotifications() {
  const context = useContext(NotificationContext);
  if (!context) {
    throw new Error('useNotifications must be used within NotificationProvider');
  }
  return context;
}

// Usage
function App() {
  const { addNotification } = useNotifications();
  
  return (
    <div>
      <button onClick={() => addNotification({
        type: 'success',
        title: 'Success!',
        message: 'Operation completed successfully'
      })}>
        Show Success
      </button>
      
      <button onClick={() => addNotification({
        type: 'error',
        title: 'Error!',
        message: 'Something went wrong',
        duration: 0 // Don't auto-dismiss
      })}>
        Show Error
      </button>
    </div>
  );
}

// Root component
function Root() {
  return (
    <NotificationProvider>
      <App />
    </NotificationProvider>
  );
}
```

## Advanced Portal Patterns

### Portal with Event Propagation
```jsx
function EventBubblingPortal({ children, onClickOutside }) {
  const [container] = useState(() => {
    const div = document.createElement('div');
    document.body.appendChild(div);
    return div;
  });
  
  useEffect(() => {
    const handleDocumentClick = (e) => {
      // Check if click is outside the portal content
      if (!container.contains(e.target)) {
        onClickOutside?.(e);
      }
    };
    
    // Listen on document to catch all clicks
    document.addEventListener('click', handleDocumentClick, true);
    
    return () => {
      document.removeEventListener('click', handleDocumentClick, true);
      if (container.parentNode) {
        container.parentNode.removeChild(container);
      }
    };
  }, [container, onClickOutside]);
  
  return createPortal(children, container);
}
```

### Portal with CSS-in-JS
```jsx
import styled from 'styled-components';

const ModalOverlay = styled.div`
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
`;

const ModalContent = styled.div`
  background: white;
  border-radius: 8px;
  padding: 20px;
  max-width: 500px;
  max-height: 80vh;
  overflow-y: auto;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
`;

function StyledModal({ isOpen, onClose, children }) {
  if (!isOpen) return null;
  
  return createPortal(
    <ModalOverlay onClick={onClose}>
      <ModalContent onClick={e => e.stopPropagation()}>
        {children}
      </ModalContent>
    </ModalOverlay>,
    document.getElementById('modal-root')
  );
}
```

## Best Practices

1. **Create Portal Containers Properly**
   - Use stable DOM nodes
   - Clean up containers when unmounting
   - Handle missing containers gracefully

2. **Manage Focus and Accessibility**
   - Trap focus within modals
   - Use proper ARIA attributes
   - Handle escape key navigation

3. **Handle Event Propagation**
   - Stop propagation where needed
   - Listen for clicks outside portal content
   - Consider event delegation

4. **Position Elements Correctly**
   - Calculate positions relative to viewport
   - Handle window resize and scroll
   - Consider different screen sizes

5. **Performance Considerations**
   - Avoid creating portals unnecessarily
   - Use conditional rendering
   - Clean up event listeners

6. **CSS and Styling**
   - Use high z-index values
   - Consider fixed positioning
   - Handle body scroll prevention

Portals are powerful for creating overlay components that break out of the normal component hierarchy while maintaining React's component model!