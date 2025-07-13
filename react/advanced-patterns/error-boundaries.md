# Error Boundaries in React

Error boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of crashing the entire application.

## Basic Error Boundary

### Simple Error Boundary
```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null, errorInfo: null };
  }
  
  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    // Log the error to an error reporting service
    console.error('Error caught by boundary:', error, errorInfo);
    
    this.setState({
      error,
      errorInfo
    });
  }
  
  render() {
    if (this.state.hasError) {
      return (
        <div style={{ padding: '20px', textAlign: 'center' }}>
          <h2>Something went wrong</h2>
          <details style={{ whiteSpace: 'pre-wrap' }}>
            {this.state.error && this.state.error.toString()}
            <br />
            {this.state.errorInfo.componentStack}
          </details>
        </div>
      );
    }
    
    return this.props.children;
  }
}

// Usage
function App() {
  return (
    <ErrorBoundary>
      <Header />
      <MainContent />
      <Footer />
    </ErrorBoundary>
  );
}
```

## Advanced Error Boundary

### Enhanced Error Boundary with Features
```jsx
class AdvancedErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      hasError: false,
      error: null,
      errorInfo: null,
      eventId: null
    };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    const { onError, enableLogging = true } = this.props;
    
    // Log to console in development
    if (process.env.NODE_ENV === 'development') {
      console.group('🚨 Error Boundary Caught An Error');
      console.error('Error:', error);
      console.error('Error Info:', errorInfo);
      console.error('Component Stack:', errorInfo.componentStack);
      console.groupEnd();
    }
    
    // Log to external service
    if (enableLogging) {
      this.logErrorToService(error, errorInfo);
    }
    
    // Call custom error handler
    if (onError) {
      onError(error, errorInfo);
    }
    
    this.setState({
      error,
      errorInfo
    });
  }
  
  logErrorToService = (error, errorInfo) => {
    // Example with Sentry
    if (window.Sentry) {
      window.Sentry.withScope((scope) => {
        scope.setTag('errorBoundary', true);
        scope.setContext('errorInfo', errorInfo);
        const eventId = window.Sentry.captureException(error);
        this.setState({ eventId });
      });
    }
    
    // Example with custom logging service
    fetch('/api/errors', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        error: error.toString(),
        stack: error.stack,
        componentStack: errorInfo.componentStack,
        timestamp: new Date().toISOString(),
        userAgent: navigator.userAgent,
        url: window.location.href
      })
    }).catch(err => {
      console.error('Failed to log error:', err);
    });
  };
  
  handleReset = () => {
    this.setState({
      hasError: false,
      error: null,
      errorInfo: null,
      eventId: null
    });
  };
  
  render() {
    const { hasError, error, errorInfo, eventId } = this.state;
    const { fallback, children, showDetails = false } = this.props;
    
    if (hasError) {
      // Custom fallback component
      if (fallback) {
        return fallback(error, errorInfo, this.handleReset);
      }
      
      return (
        <div className="error-boundary">
          <div className="error-content">
            <h1>🚨 Oops! Something went wrong</h1>
            <p>We're sorry, but something unexpected happened.</p>
            
            <div className="error-actions">
              <button onClick={this.handleReset} className="retry-button">
                Try Again
              </button>
              <button onClick={() => window.location.reload()}>
                Reload Page
              </button>
            </div>
            
            {eventId && (
              <p className="error-id">
                Error ID: <code>{eventId}</code>
              </p>
            )}
            
            {showDetails && process.env.NODE_ENV === 'development' && (
              <details className="error-details">
                <summary>Error Details (Development Only)</summary>
                <pre>
                  <strong>Error:</strong> {error.toString()}
                  {'\n\n'}
                  <strong>Component Stack:</strong>
                  {errorInfo.componentStack}
                  {'\n\n'}
                  <strong>Error Stack:</strong>
                  {error.stack}
                </pre>
              </details>
            )}
          </div>
        </div>
      );
    }
    
    return children;
  }
}

// Usage with custom fallback
function CustomErrorFallback(error, errorInfo, retry) {
  return (
    <div className="custom-error">
      <h2>Something went wrong in this section</h2>
      <p>Don't worry, the rest of the app is still working!</p>
      <button onClick={retry}>Try Again</button>
    </div>
  );
}

function App() {
  return (
    <AdvancedErrorBoundary 
      fallback={CustomErrorFallback}
      onError={(error, errorInfo) => {
        console.log('Custom error handler:', error);
      }}
      showDetails={true}
    >
      <Dashboard />
    </AdvancedErrorBoundary>
  );
}
```

## Granular Error Boundaries

### Component-Level Error Boundaries
```jsx
// Wrap individual components
function Dashboard() {
  return (
    <div className="dashboard">
      <ErrorBoundary fallback={SidebarErrorFallback}>
        <Sidebar />
      </ErrorBoundary>
      
      <ErrorBoundary fallback={MainContentErrorFallback}>
        <MainContent />
      </ErrorBoundary>
      
      <ErrorBoundary fallback={WidgetErrorFallback}>
        <Widgets />
      </ErrorBoundary>
    </div>
  );
}

function SidebarErrorFallback() {
  return (
    <div className="sidebar-error">
      <p>Sidebar couldn't load</p>
      <button onClick={() => window.location.reload()}>
        Reload
      </button>
    </div>
  );
}

function MainContentErrorFallback() {
  return (
    <div className="main-content-error">
      <h2>Content Error</h2>
      <p>The main content failed to load.</p>
    </div>
  );
}
```

### Route-Level Error Boundaries
```jsx
import { Routes, Route } from 'react-router-dom';

function App() {
  return (
    <Routes>
      <Route path="/" element={
        <ErrorBoundary fallback={HomeErrorFallback}>
          <Home />
        </ErrorBoundary>
      } />
      
      <Route path="/profile" element={
        <ErrorBoundary fallback={ProfileErrorFallback}>
          <Profile />
        </ErrorBoundary>
      } />
      
      <Route path="/dashboard/*" element={
        <ErrorBoundary fallback={DashboardErrorFallback}>
          <Dashboard />
        </ErrorBoundary>
      } />
    </Routes>
  );
}
```

## Async Error Handling

### Handling Promise Rejections
```jsx
class AsyncErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('AsyncErrorBoundary caught an error:', error, errorInfo);
  }
  
  componentDidMount() {
    // Listen for unhandled promise rejections
    window.addEventListener('unhandledrejection', this.handlePromiseRejection);
  }
  
  componentWillUnmount() {
    window.removeEventListener('unhandledrejection', this.handlePromiseRejection);
  }
  
  handlePromiseRejection = (event) => {
    console.error('Unhandled promise rejection:', event.reason);
    
    // Optionally treat promise rejections as errors
    if (this.props.catchAsyncErrors) {
      this.setState({
        hasError: true,
        error: new Error(`Async Error: ${event.reason}`)
      });
      
      // Prevent the default browser behavior
      event.preventDefault();
    }
  };
  
  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h2>Async Error Occurred</h2>
          <p>{this.state.error.message}</p>
          <button onClick={() => this.setState({ hasError: false, error: null })}>
            Retry
          </button>
        </div>
      );
    }
    
    return this.props.children;
  }
}

// Component that might have async errors
function AsyncComponent() {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    // This promise rejection would be caught by AsyncErrorBoundary
    fetch('/api/data')
      .then(response => {
        if (!response.ok) throw new Error('Failed to fetch');
        return response.json();
      })
      .then(setData)
      .catch(error => {
        // If you want the error boundary to catch this,
        // you need to throw in a component lifecycle
        throw error;
      });
  }, []);
  
  return <div>{data ? JSON.stringify(data) : 'Loading...'}</div>;
}
```

## Error Boundary Hook (React 18+)

### Using Error Boundaries with Hooks
```jsx
// Custom hook to throw errors that error boundaries can catch
function useErrorHandler() {
  const [, setError] = useState();
  
  return useCallback((error) => {
    setError(() => {
      throw error;
    });
  }, []);
}

// Functional component that can trigger error boundary
function FunctionalComponentWithErrors() {
  const throwError = useErrorHandler();
  
  const handleAsyncError = async () => {
    try {
      await riskyAsyncOperation();
    } catch (error) {
      throwError(error); // This will be caught by error boundary
    }
  };
  
  return (
    <div>
      <button onClick={() => throwError(new Error('Manual error'))}>
        Throw Error
      </button>
      <button onClick={handleAsyncError}>
        Async Error
      </button>
    </div>
  );
}

// React 18 alternative: use ErrorBoundary from react-error-boundary library
import { ErrorBoundary } from 'react-error-boundary';

function ErrorFallback({ error, resetErrorBoundary }) {
  return (
    <div role="alert">
      <h2>Something went wrong:</h2>
      <pre>{error.message}</pre>
      <button onClick={resetErrorBoundary}>Try again</button>
    </div>
  );
}

function App() {
  return (
    <ErrorBoundary
      FallbackComponent={ErrorFallback}
      onError={(error, errorInfo) => {
        console.log('Error:', error);
        console.log('Error Info:', errorInfo);
      }}
      onReset={() => {
        // Reset app state
        window.location.reload();
      }}
    >
      <MyApp />
    </ErrorBoundary>
  );
}
```

## Error Boundary Context

### Providing Error Context
```jsx
const ErrorContext = createContext();

class ErrorBoundaryProvider extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      errors: []
    };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    const errorData = {
      id: Date.now(),
      error,
      errorInfo,
      timestamp: new Date().toISOString()
    };
    
    this.setState(prevState => ({
      errors: [...prevState.errors, errorData]
    }));
  }
  
  clearError = (errorId) => {
    this.setState(prevState => ({
      errors: prevState.errors.filter(err => err.id !== errorId),
      hasError: prevState.errors.length <= 1 ? false : prevState.hasError
    }));
  };
  
  clearAllErrors = () => {
    this.setState({ errors: [], hasError: false });
  };
  
  render() {
    const contextValue = {
      errors: this.state.errors,
      clearError: this.clearError,
      clearAllErrors: this.clearAllErrors
    };
    
    return (
      <ErrorContext.Provider value={contextValue}>
        {this.state.hasError ? (
          <ErrorDisplay errors={this.state.errors} onClear={this.clearError} />
        ) : (
          this.props.children
        )}
      </ErrorContext.Provider>
    );
  }
}

function ErrorDisplay({ errors, onClear }) {
  return (
    <div className="error-display">
      <h2>Errors Occurred ({errors.length})</h2>
      {errors.map(({ id, error, timestamp }) => (
        <div key={id} className="error-item">
          <p><strong>Error:</strong> {error.message}</p>
          <p><small>Time: {new Date(timestamp).toLocaleString()}</small></p>
          <button onClick={() => onClear(id)}>Dismiss</button>
        </div>
      ))}
    </div>
  );
}

// Hook to use error context
function useErrors() {
  const context = useContext(ErrorContext);
  if (!context) {
    throw new Error('useErrors must be used within ErrorBoundaryProvider');
  }
  return context;
}
```

## Testing Error Boundaries

### Unit Testing Error Boundaries
```jsx
import { render, screen } from '@testing-library/react';
import ErrorBoundary from './ErrorBoundary';

// Component that throws an error
function ThrowError({ shouldThrow }) {
  if (shouldThrow) {
    throw new Error('Test error');
  }
  return <div>No error</div>;
}

describe('ErrorBoundary', () => {
  // Suppress console.error for cleaner test output
  const originalError = console.error;
  beforeAll(() => {
    console.error = jest.fn();
  });
  afterAll(() => {
    console.error = originalError;
  });
  
  test('renders children when there is no error', () => {
    render(
      <ErrorBoundary>
        <ThrowError shouldThrow={false} />
      </ErrorBoundary>
    );
    
    expect(screen.getByText('No error')).toBeInTheDocument();
  });
  
  test('renders error message when child component throws', () => {
    render(
      <ErrorBoundary>
        <ThrowError shouldThrow={true} />
      </ErrorBoundary>
    );
    
    expect(screen.getByText(/something went wrong/i)).toBeInTheDocument();
  });
  
  test('calls onError callback when error occurs', () => {
    const onError = jest.fn();
    
    render(
      <ErrorBoundary onError={onError}>
        <ThrowError shouldThrow={true} />
      </ErrorBoundary>
    );
    
    expect(onError).toHaveBeenCalledWith(
      expect.any(Error),
      expect.objectContaining({
        componentStack: expect.any(String)
      })
    );
  });
});
```

## Error Boundary Patterns

### Retry Mechanism
```jsx
class RetryErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { 
      hasError: false, 
      retryCount: 0,
      error: null
    };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }
  
  componentDidCatch(error, errorInfo) {
    const { maxRetries = 3, onError } = this.props;
    
    if (onError) {
      onError(error, errorInfo, this.state.retryCount);
    }
    
    // Auto-retry if under limit
    if (this.state.retryCount < maxRetries) {
      setTimeout(() => {
        this.setState(prevState => ({
          hasError: false,
          retryCount: prevState.retryCount + 1,
          error: null
        }));
      }, 1000 * Math.pow(2, this.state.retryCount)); // Exponential backoff
    }
  }
  
  handleManualRetry = () => {
    this.setState({
      hasError: false,
      retryCount: 0,
      error: null
    });
  };
  
  render() {
    const { hasError, retryCount, error } = this.state;
    const { maxRetries = 3, children } = this.props;
    
    if (hasError) {
      if (retryCount >= maxRetries) {
        return (
          <div className="retry-error-boundary">
            <h2>Something went wrong</h2>
            <p>We tried {maxRetries} times but couldn't recover.</p>
            <p>Error: {error?.message}</p>
            <button onClick={this.handleManualRetry}>
              Try Again Manually
            </button>
          </div>
        );
      }
      
      return (
        <div className="retry-error-boundary">
          <p>Something went wrong. Retrying... (Attempt {retryCount + 1}/{maxRetries})</p>
        </div>
      );
    }
    
    return children;
  }
}
```

## Best Practices

1. **Place Error Boundaries Strategically**
   - Don't wrap every component
   - Place at route and feature boundaries
   - Consider component importance

2. **Provide Meaningful Fallbacks**
   - Show user-friendly error messages
   - Provide recovery options
   - Maintain app functionality where possible

3. **Log Errors Appropriately**
   - Send to error monitoring services
   - Include relevant context
   - Respect user privacy

4. **Test Error Scenarios**
   - Test error boundary rendering
   - Test error logging
   - Test recovery mechanisms

5. **Handle Async Errors**
   - Error boundaries don't catch async errors
   - Use try-catch for promises
   - Consider global error handlers

Error boundaries are essential for building resilient React applications that gracefully handle unexpected errors!