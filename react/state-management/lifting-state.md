# Lifting State Up

Lifting state up is a pattern in React where you move state to a common ancestor component when multiple components need to share the same changing data.

## Why Lift State Up?

When several components need to reflect the same changing data, it's recommended to lift the shared state up to their closest common ancestor.

```jsx
// Before: Each component manages its own state
function InputA() {
  const [value, setValue] = useState('');
  return <input value={value} onChange={e => setValue(e.target.value)} />;
}

function InputB() {
  const [value, setValue] = useState('');
  return <input value={value} onChange={e => setValue(e.target.value)} />;
}

// Problem: InputA and InputB can't share their values!
```

## Basic Example

Let's create two inputs that share the same value:

```jsx
function Parent() {
  const [sharedValue, setSharedValue] = useState('');
  
  return (
    <div>
      <InputA value={sharedValue} onChange={setSharedValue} />
      <InputB value={sharedValue} onChange={setSharedValue} />
      <p>Shared value: {sharedValue}</p>
    </div>
  );
}

function InputA({ value, onChange }) {
  return (
    <input 
      value={value} 
      onChange={e => onChange(e.target.value)}
      placeholder="Input A"
    />
  );
}

function InputB({ value, onChange }) {
  return (
    <input 
      value={value} 
      onChange={e => onChange(e.target.value)}
      placeholder="Input B"
    />
  );
}
```

## Temperature Calculator Example

A classic example demonstrating lifting state up:

```jsx
const scaleNames = {
  c: 'Celsius',
  f: 'Fahrenheit'
};

function toCelsius(fahrenheit) {
  return (fahrenheit - 32) * 5 / 9;
}

function toFahrenheit(celsius) {
  return (celsius * 9 / 5) + 32;
}

function tryConvert(temperature, convert) {
  const input = parseFloat(temperature);
  if (Number.isNaN(input)) {
    return '';
  }
  const output = convert(input);
  const rounded = Math.round(output * 1000) / 1000;
  return rounded.toString();
}

function TemperatureInput({ scale, temperature, onTemperatureChange }) {
  const handleChange = (e) => {
    onTemperatureChange(e.target.value);
  };
  
  return (
    <fieldset>
      <legend>Enter temperature in {scaleNames[scale]}:</legend>
      <input value={temperature} onChange={handleChange} />
    </fieldset>
  );
}

function Calculator() {
  const [temperature, setTemperature] = useState('');
  const [scale, setScale] = useState('c');
  
  const handleCelsiusChange = (temperature) => {
    setScale('c');
    setTemperature(temperature);
  };
  
  const handleFahrenheitChange = (temperature) => {
    setScale('f');
    setTemperature(temperature);
  };
  
  const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
  const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;
  
  return (
    <div>
      <TemperatureInput
        scale="c"
        temperature={celsius}
        onTemperatureChange={handleCelsiusChange}
      />
      <TemperatureInput
        scale="f"
        temperature={fahrenheit}
        onTemperatureChange={handleFahrenheitChange}
      />
      <BoilingVerdict celsius={parseFloat(celsius)} />
    </div>
  );
}

function BoilingVerdict({ celsius }) {
  if (celsius >= 100) {
    return <p>The water would boil.</p>;
  }
  return <p>The water would not boil.</p>;
}
```

## Shopping Cart Example

Multiple components need access to cart data:

```jsx
function ShoppingApp() {
  const [cart, setCart] = useState([]);
  
  const addToCart = (product) => {
    setCart(prevCart => {
      const existing = prevCart.find(item => item.id === product.id);
      if (existing) {
        return prevCart.map(item =>
          item.id === product.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      return [...prevCart, { ...product, quantity: 1 }];
    });
  };
  
  const removeFromCart = (productId) => {
    setCart(prevCart => prevCart.filter(item => item.id !== productId));
  };
  
  const updateQuantity = (productId, quantity) => {
    if (quantity === 0) {
      removeFromCart(productId);
    } else {
      setCart(prevCart =>
        prevCart.map(item =>
          item.id === productId
            ? { ...item, quantity }
            : item
        )
      );
    }
  };
  
  const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
  const totalPrice = cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
  
  return (
    <div>
      <Header totalItems={totalItems} />
      <ProductList onAddToCart={addToCart} />
      <Cart 
        items={cart}
        onUpdateQuantity={updateQuantity}
        onRemove={removeFromCart}
        totalPrice={totalPrice}
      />
    </div>
  );
}

function Header({ totalItems }) {
  return (
    <header>
      <h1>Shop</h1>
      <div>Cart ({totalItems} items)</div>
    </header>
  );
}

function ProductList({ onAddToCart }) {
  const products = [
    { id: 1, name: 'Laptop', price: 999 },
    { id: 2, name: 'Mouse', price: 29 },
    { id: 3, name: 'Keyboard', price: 59 }
  ];
  
  return (
    <div>
      <h2>Products</h2>
      {products.map(product => (
        <div key={product.id}>
          <span>{product.name} - ${product.price}</span>
          <button onClick={() => onAddToCart(product)}>
            Add to Cart
          </button>
        </div>
      ))}
    </div>
  );
}

function Cart({ items, onUpdateQuantity, onRemove, totalPrice }) {
  if (items.length === 0) {
    return <p>Your cart is empty</p>;
  }
  
  return (
    <div>
      <h2>Shopping Cart</h2>
      {items.map(item => (
        <CartItem
          key={item.id}
          item={item}
          onUpdateQuantity={onUpdateQuantity}
          onRemove={onRemove}
        />
      ))}
      <div>Total: ${totalPrice.toFixed(2)}</div>
    </div>
  );
}

function CartItem({ item, onUpdateQuantity, onRemove }) {
  return (
    <div>
      <span>{item.name}</span>
      <input
        type="number"
        value={item.quantity}
        onChange={(e) => onUpdateQuantity(item.id, parseInt(e.target.value))}
        min="0"
      />
      <span>${(item.price * item.quantity).toFixed(2)}</span>
      <button onClick={() => onRemove(item.id)}>Remove</button>
    </div>
  );
}
```

## Form State Management

Managing form state in a parent component:

```jsx
function RegistrationForm() {
  const [formData, setFormData] = useState({
    username: '',
    email: '',
    password: '',
    confirmPassword: ''
  });
  
  const [errors, setErrors] = useState({});
  
  const updateField = (field, value) => {
    setFormData(prev => ({ ...prev, [field]: value }));
    // Clear error when user types
    if (errors[field]) {
      setErrors(prev => ({ ...prev, [field]: '' }));
    }
  };
  
  const validate = () => {
    const newErrors = {};
    
    if (!formData.username) {
      newErrors.username = 'Username is required';
    }
    
    if (!formData.email.includes('@')) {
      newErrors.email = 'Valid email is required';
    }
    
    if (formData.password.length < 6) {
      newErrors.password = 'Password must be at least 6 characters';
    }
    
    if (formData.password !== formData.confirmPassword) {
      newErrors.confirmPassword = 'Passwords do not match';
    }
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    if (validate()) {
      console.log('Form submitted:', formData);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <FormField
        label="Username"
        value={formData.username}
        onChange={(value) => updateField('username', value)}
        error={errors.username}
      />
      <FormField
        label="Email"
        type="email"
        value={formData.email}
        onChange={(value) => updateField('email', value)}
        error={errors.email}
      />
      <FormField
        label="Password"
        type="password"
        value={formData.password}
        onChange={(value) => updateField('password', value)}
        error={errors.password}
      />
      <FormField
        label="Confirm Password"
        type="password"
        value={formData.confirmPassword}
        onChange={(value) => updateField('confirmPassword', value)}
        error={errors.confirmPassword}
      />
      <button type="submit">Register</button>
    </form>
  );
}

function FormField({ label, type = 'text', value, onChange, error }) {
  return (
    <div>
      <label>
        {label}:
        <input
          type={type}
          value={value}
          onChange={(e) => onChange(e.target.value)}
        />
      </label>
      {error && <span style={{ color: 'red' }}>{error}</span>}
    </div>
  );
}
```

## When to Lift State Up

### Signs You Need to Lift State:
1. Multiple components need the same data
2. Components need to stay in sync
3. A child needs to update parent's data
4. Sibling components need to communicate

### Example: Tab Component
```jsx
function TabbedInterface() {
  const [activeTab, setActiveTab] = useState(0);
  
  return (
    <div>
      <TabList activeTab={activeTab} onTabChange={setActiveTab} />
      <TabContent activeTab={activeTab} />
    </div>
  );
}

function TabList({ activeTab, onTabChange }) {
  const tabs = ['Profile', 'Settings', 'Notifications'];
  
  return (
    <div className="tab-list">
      {tabs.map((tab, index) => (
        <button
          key={tab}
          className={activeTab === index ? 'active' : ''}
          onClick={() => onTabChange(index)}
        >
          {tab}
        </button>
      ))}
    </div>
  );
}

function TabContent({ activeTab }) {
  const content = [
    <ProfilePanel />,
    <SettingsPanel />,
    <NotificationsPanel />
  ];
  
  return <div className="tab-content">{content[activeTab]}</div>;
}
```

## Alternatives to Lifting State

### When Lifting State Becomes Cumbersome:

1. **Context API** - For deeply nested components
2. **State Management Libraries** - Redux, MobX, Zustand
3. **Component Composition** - Restructure to avoid prop drilling

```jsx
// Instead of lifting state very high
function App() {
  const [user, setUser] = useState(null);
  
  return (
    <Layout user={user}>
      <Dashboard user={user}>
        <Profile user={user} onUpdate={setUser} />
      </Dashboard>
    </Layout>
  );
}

// Consider Context API
const UserContext = React.createContext();

function App() {
  const [user, setUser] = useState(null);
  
  return (
    <UserContext.Provider value={{ user, setUser }}>
      <Layout>
        <Dashboard>
          <Profile />
        </Dashboard>
      </Layout>
    </UserContext.Provider>
  );
}
```

## Best Practices

1. **Lift state only as high as necessary**
2. **Keep state close to where it's used**
3. **Consider component composition first**
4. **Use descriptive handler names** (onUserUpdate vs onChange)
5. **Pass only necessary data** to child components
6. **Consider performance** - lifted state causes more re-renders

## Common Patterns

### Controlled Components Pattern
```jsx
function SearchableList() {
  const [searchTerm, setSearchTerm] = useState('');
  const [selectedItem, setSelectedItem] = useState(null);
  
  const items = ['Apple', 'Banana', 'Cherry', 'Date', 'Elderberry'];
  const filteredItems = items.filter(item =>
    item.toLowerCase().includes(searchTerm.toLowerCase())
  );
  
  return (
    <div>
      <SearchInput value={searchTerm} onChange={setSearchTerm} />
      <ItemList 
        items={filteredItems}
        selectedItem={selectedItem}
        onSelectItem={setSelectedItem}
      />
      {selectedItem && <ItemDetails item={selectedItem} />}
    </div>
  );
}
```

### Two-Way Data Binding Pattern
```jsx
function TwoWayBinding() {
  const [data, setData] = useState({
    field1: '',
    field2: ''
  });
  
  const createHandler = (field) => (value) => {
    setData(prev => ({ ...prev, [field]: value }));
  };
  
  return (
    <div>
      <ControlledInput
        value={data.field1}
        onChange={createHandler('field1')}
      />
      <ControlledInput
        value={data.field2}
        onChange={createHandler('field2')}
      />
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}
```

Lifting state up is a fundamental pattern in React that enables component communication and state sharing. Master this concept to build well-structured React applications!