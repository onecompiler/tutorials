# Component Composition

Component composition is a powerful pattern in React that allows you to build complex UIs by combining simpler components. It promotes reusability and maintainability.

## What is Composition?

Composition is about building components from other components, using props to pass data and functionality. It's React's recommended approach over inheritance.

```jsx
// Instead of inheritance
// class SpecialButton extends Button { } ❌

// Use composition
function SpecialButton(props) {
  return (
    <Button color="red" size="large" {...props}>
      <span>✨</span> {props.children}
    </Button>
  );
}
```

## Basic Composition

### Composing Simple Components
```jsx
function Avatar({ src, alt, size = 50 }) {
  return (
    <img 
      src={src} 
      alt={alt}
      style={{ 
        width: size, 
        height: size, 
        borderRadius: '50%' 
      }}
    />
  );
}

function UserInfo({ name, title }) {
  return (
    <div className="user-info">
      <h3>{name}</h3>
      <p>{title}</p>
    </div>
  );
}

function UserCard({ user }) {
  return (
    <div className="user-card">
      <Avatar src={user.avatar} alt={user.name} />
      <UserInfo name={user.name} title={user.title} />
    </div>
  );
}
```

## Children Props

The special `children` prop allows components to be composed like HTML elements:

```jsx
function Card({ children, title }) {
  return (
    <div className="card">
      {title && <h2 className="card-title">{title}</h2>}
      <div className="card-content">
        {children}
      </div>
    </div>
  );
}

function App() {
  return (
    <Card title="User Profile">
      <p>Name: John Doe</p>
      <p>Email: john@example.com</p>
      <button>Edit Profile</button>
    </Card>
  );
}
```

## Multiple Composition Slots

Components can have multiple slots for composition:

```jsx
function Layout({ header, sidebar, content, footer }) {
  return (
    <div className="layout">
      <header className="header">{header}</header>
      <div className="main">
        <aside className="sidebar">{sidebar}</aside>
        <main className="content">{content}</main>
      </div>
      <footer className="footer">{footer}</footer>
    </div>
  );
}

function App() {
  return (
    <Layout
      header={<Header />}
      sidebar={<Navigation />}
      content={<MainContent />}
      footer={<Footer />}
    />
  );
}
```

## Specialized Components

Create specialized versions of generic components:

```jsx
// Generic button component
function Button({ variant, size, children, ...props }) {
  return (
    <button 
      className={`btn btn-${variant} btn-${size}`}
      {...props}
    >
      {children}
    </button>
  );
}

// Specialized buttons
function PrimaryButton(props) {
  return <Button variant="primary" {...props} />;
}

function DangerButton(props) {
  return <Button variant="danger" {...props} />;
}

function IconButton({ icon, children, ...props }) {
  return (
    <Button {...props}>
      <span className="icon">{icon}</span>
      {children}
    </Button>
  );
}
```

## Compound Components

Components that work together to form a cohesive unit:

```jsx
// Tab component system
function Tabs({ children, defaultTab = 0 }) {
  const [activeTab, setActiveTab] = useState(defaultTab);
  
  return (
    <div className="tabs">
      {React.Children.map(children, (child) =>
        React.cloneElement(child, { activeTab, setActiveTab })
      )}
    </div>
  );
}

function TabList({ children, activeTab, setActiveTab }) {
  return (
    <div className="tab-list">
      {React.Children.map(children, (child, index) =>
        React.cloneElement(child, {
          isActive: activeTab === index,
          onClick: () => setActiveTab(index)
        })
      )}
    </div>
  );
}

function Tab({ children, isActive, onClick }) {
  return (
    <button 
      className={`tab ${isActive ? 'active' : ''}`}
      onClick={onClick}
    >
      {children}
    </button>
  );
}

function TabPanels({ children, activeTab }) {
  return (
    <div className="tab-panels">
      {React.Children.toArray(children)[activeTab]}
    </div>
  );
}

function TabPanel({ children }) {
  return <div className="tab-panel">{children}</div>;
}

// Usage
function App() {
  return (
    <Tabs defaultTab={0}>
      <TabList>
        <Tab>Profile</Tab>
        <Tab>Settings</Tab>
        <Tab>Notifications</Tab>
      </TabList>
      <TabPanels>
        <TabPanel>Profile content...</TabPanel>
        <TabPanel>Settings content...</TabPanel>
        <TabPanel>Notifications content...</TabPanel>
      </TabPanels>
    </Tabs>
  );
}
```

## Render Props Pattern

Pass a function as a prop to share code between components:

```jsx
function MouseTracker({ render }) {
  const [position, setPosition] = useState({ x: 0, y: 0 });
  
  useEffect(() => {
    const handleMouseMove = (e) => {
      setPosition({ x: e.clientX, y: e.clientY });
    };
    
    window.addEventListener('mousemove', handleMouseMove);
    return () => window.removeEventListener('mousemove', handleMouseMove);
  }, []);
  
  return render(position);
}

// Usage
function App() {
  return (
    <div>
      <MouseTracker 
        render={({ x, y }) => (
          <p>Mouse position: {x}, {y}</p>
        )}
      />
      
      <MouseTracker 
        render={({ x, y }) => (
          <div 
            style={{
              position: 'absolute',
              left: x - 10,
              top: y - 10,
              width: 20,
              height: 20,
              backgroundColor: 'red',
              borderRadius: '50%'
            }}
          />
        )}
      />
    </div>
  );
}
```

## Props Forwarding

Forward props to child components while adding your own logic:

```jsx
function FancyInput({ label, error, ...inputProps }) {
  return (
    <div className="form-field">
      {label && <label>{label}</label>}
      <input 
        className={error ? 'error' : ''}
        {...inputProps}
      />
      {error && <span className="error-message">{error}</span>}
    </div>
  );
}

// All standard input props are forwarded
<FancyInput 
  label="Email"
  type="email"
  placeholder="Enter email"
  value={email}
  onChange={handleChange}
  error={emailError}
/>
```

## Component as Props

Pass components as props for maximum flexibility:

```jsx
function List({ items, ItemComponent, EmptyComponent }) {
  if (items.length === 0) {
    return EmptyComponent ? <EmptyComponent /> : <p>No items</p>;
  }
  
  return (
    <ul>
      {items.map(item => (
        <li key={item.id}>
          <ItemComponent item={item} />
        </li>
      ))}
    </ul>
  );
}

// Different item components
function UserItem({ item }) {
  return <span>{item.name} - {item.email}</span>;
}

function ProductItem({ item }) {
  return <span>{item.name} - ${item.price}</span>;
}

// Usage
<List 
  items={users}
  ItemComponent={UserItem}
  EmptyComponent={() => <p>No users found</p>}
/>
```

## HOC vs Composition

Instead of Higher-Order Components, often composition is clearer:

```jsx
// Instead of HOC
// const EnhancedComponent = withAuth(MyComponent); ❌

// Use composition
function ProtectedRoute({ component: Component, ...rest }) {
  const isAuthenticated = useAuth();
  
  if (!isAuthenticated) {
    return <Navigate to="/login" />;
  }
  
  return <Component {...rest} />;
}

// Usage
<ProtectedRoute component={Dashboard} />
```

## Real-World Example: Form Builder

```jsx
function Form({ children, onSubmit }) {
  const [values, setValues] = useState({});
  const [errors, setErrors] = useState({});
  
  const setValue = (name, value) => {
    setValues(prev => ({ ...prev, [name]: value }));
  };
  
  const setError = (name, error) => {
    setErrors(prev => ({ ...prev, [name]: error }));
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    onSubmit(values);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      {React.Children.map(children, child =>
        React.cloneElement(child, {
          setValue,
          setError,
          values,
          errors
        })
      )}
    </form>
  );
}

function Field({ name, label, type = 'text', setValue, values, errors }) {
  return (
    <div className="field">
      <label>{label}</label>
      <input
        type={type}
        value={values[name] || ''}
        onChange={(e) => setValue(name, e.target.value)}
      />
      {errors[name] && <span className="error">{errors[name]}</span>}
    </div>
  );
}

function SubmitButton({ children }) {
  return <button type="submit">{children}</button>;
}

// Usage
function ContactForm() {
  const handleSubmit = (values) => {
    console.log('Form values:', values);
  };
  
  return (
    <Form onSubmit={handleSubmit}>
      <Field name="name" label="Name" />
      <Field name="email" label="Email" type="email" />
      <Field name="message" label="Message" type="textarea" />
      <SubmitButton>Send Message</SubmitButton>
    </Form>
  );
}
```

## Best Practices

### 1. Keep Components Focused
```jsx
// Good - Single responsibility
function UserAvatar({ user }) {
  return <img src={user.avatar} alt={user.name} />;
}

function UserName({ user }) {
  return <h3>{user.name}</h3>;
}

// Compose them
function UserHeader({ user }) {
  return (
    <div>
      <UserAvatar user={user} />
      <UserName user={user} />
    </div>
  );
}
```

### 2. Use Composition for Variations
```jsx
// Instead of props for every variation
// <Button primary large icon="star" /> ❌

// Compose variations
<PrimaryButton size="large">
  <Icon name="star" /> Click me
</PrimaryButton>
```

### 3. Extract Reusable Patterns
```jsx
function WithLoading({ loading, children }) {
  if (loading) {
    return <Spinner />;
  }
  return children;
}

function WithError({ error, children }) {
  if (error) {
    return <ErrorMessage error={error} />;
  }
  return children;
}

// Usage
<WithError error={error}>
  <WithLoading loading={loading}>
    <DataDisplay data={data} />
  </WithLoading>
</WithError>
```

## Common Patterns

### Container and Presentational
```jsx
// Container - handles logic
function UserListContainer() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    fetchUsers().then(data => {
      setUsers(data);
      setLoading(false);
    });
  }, []);
  
  return <UserList users={users} loading={loading} />;
}

// Presentational - handles display
function UserList({ users, loading }) {
  if (loading) return <Loading />;
  
  return (
    <ul>
      {users.map(user => (
        <UserItem key={user.id} user={user} />
      ))}
    </ul>
  );
}
```

### Layout Components
```jsx
function PageLayout({ children }) {
  return (
    <div className="page">
      <Header />
      <main className="content">
        {children}
      </main>
      <Footer />
    </div>
  );
}

function DashboardLayout({ children }) {
  return (
    <PageLayout>
      <div className="dashboard">
        <Sidebar />
        <div className="dashboard-content">
          {children}
        </div>
      </div>
    </PageLayout>
  );
}
```

## Summary

Component composition is fundamental to React development. It enables:
- Code reusability
- Flexible component design
- Clear component relationships
- Maintainable code structure
- Separation of concerns

Master composition patterns and you'll write more maintainable and flexible React applications!