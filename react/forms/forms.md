# Forms in React

Forms are essential for user input in web applications. React handles forms differently than traditional HTML, providing more control over form data and behavior.

## Controlled Components

In React, form elements typically maintain their own state. Controlled components make React state the "single source of truth."

### Basic Input
```jsx
function ControlledInput() {
  const [value, setValue] = useState('');
  
  const handleChange = (e) => {
    setValue(e.target.value);
  };
  
  return (
    <div>
      <input 
        type="text" 
        value={value} 
        onChange={handleChange} 
      />
      <p>You typed: {value}</p>
    </div>
  );
}
```

### Multiple Inputs
```jsx
function UserForm() {
  const [formData, setFormData] = useState({
    firstName: '',
    lastName: '',
    email: ''
  });
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prevData => ({
      ...prevData,
      [name]: value
    }));
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Form submitted:', formData);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        name="firstName"
        value={formData.firstName}
        onChange={handleChange}
        placeholder="First Name"
      />
      <input
        type="text"
        name="lastName"
        value={formData.lastName}
        onChange={handleChange}
        placeholder="Last Name"
      />
      <input
        type="email"
        name="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

## Different Form Elements

### Textarea
```jsx
function TextareaExample() {
  const [message, setMessage] = useState('');
  
  return (
    <div>
      <textarea
        value={message}
        onChange={(e) => setMessage(e.target.value)}
        rows={4}
        cols={50}
        placeholder="Enter your message..."
      />
      <p>Character count: {message.length}</p>
    </div>
  );
}
```

### Select
```jsx
function SelectExample() {
  const [selectedFruit, setSelectedFruit] = useState('apple');
  
  return (
    <div>
      <select 
        value={selectedFruit} 
        onChange={(e) => setSelectedFruit(e.target.value)}
      >
        <option value="apple">Apple</option>
        <option value="banana">Banana</option>
        <option value="orange">Orange</option>
        <option value="grape">Grape</option>
      </select>
      <p>You selected: {selectedFruit}</p>
    </div>
  );
}
```

### Multiple Select
```jsx
function MultiSelectExample() {
  const [selectedOptions, setSelectedOptions] = useState([]);
  
  const handleChange = (e) => {
    const options = [...e.target.selectedOptions];
    const values = options.map(option => option.value);
    setSelectedOptions(values);
  };
  
  return (
    <div>
      <select multiple value={selectedOptions} onChange={handleChange}>
        <option value="react">React</option>
        <option value="angular">Angular</option>
        <option value="vue">Vue</option>
        <option value="svelte">Svelte</option>
      </select>
      <p>Selected: {selectedOptions.join(', ')}</p>
    </div>
  );
}
```

### Checkbox
```jsx
function CheckboxExample() {
  const [isChecked, setIsChecked] = useState(false);
  
  return (
    <label>
      <input
        type="checkbox"
        checked={isChecked}
        onChange={(e) => setIsChecked(e.target.checked)}
      />
      I agree to the terms and conditions
    </label>
  );
}
```

### Multiple Checkboxes
```jsx
function CheckboxGroup() {
  const [checkedItems, setCheckedItems] = useState({
    option1: false,
    option2: false,
    option3: false
  });
  
  const handleChange = (e) => {
    const { name, checked } = e.target;
    setCheckedItems(prev => ({
      ...prev,
      [name]: checked
    }));
  };
  
  return (
    <div>
      <label>
        <input
          type="checkbox"
          name="option1"
          checked={checkedItems.option1}
          onChange={handleChange}
        />
        Option 1
      </label>
      <label>
        <input
          type="checkbox"
          name="option2"
          checked={checkedItems.option2}
          onChange={handleChange}
        />
        Option 2
      </label>
      <label>
        <input
          type="checkbox"
          name="option3"
          checked={checkedItems.option3}
          onChange={handleChange}
        />
        Option 3
      </label>
    </div>
  );
}
```

### Radio Buttons
```jsx
function RadioExample() {
  const [selectedOption, setSelectedOption] = useState('option1');
  
  return (
    <div>
      <label>
        <input
          type="radio"
          value="option1"
          checked={selectedOption === 'option1'}
          onChange={(e) => setSelectedOption(e.target.value)}
        />
        Option 1
      </label>
      <label>
        <input
          type="radio"
          value="option2"
          checked={selectedOption === 'option2'}
          onChange={(e) => setSelectedOption(e.target.value)}
        />
        Option 2
      </label>
      <label>
        <input
          type="radio"
          value="option3"
          checked={selectedOption === 'option3'}
          onChange={(e) => setSelectedOption(e.target.value)}
        />
        Option 3
      </label>
    </div>
  );
}
```

## File Input
```jsx
function FileUpload() {
  const [file, setFile] = useState(null);
  const [preview, setPreview] = useState(null);
  
  const handleFileChange = (e) => {
    const selectedFile = e.target.files[0];
    setFile(selectedFile);
    
    // Create preview for images
    if (selectedFile && selectedFile.type.startsWith('image/')) {
      const reader = new FileReader();
      reader.onloadend = () => {
        setPreview(reader.result);
      };
      reader.readAsDataURL(selectedFile);
    } else {
      setPreview(null);
    }
  };
  
  return (
    <div>
      <input
        type="file"
        onChange={handleFileChange}
        accept="image/*"
      />
      {file && (
        <div>
          <p>File name: {file.name}</p>
          <p>File size: {(file.size / 1024).toFixed(2)} KB</p>
          <p>File type: {file.type}</p>
          {preview && <img src={preview} alt="Preview" style={{ maxWidth: 200 }} />}
        </div>
      )}
    </div>
  );
}
```

## Uncontrolled Components

Sometimes you might want to use uncontrolled components with refs:

```jsx
function UncontrolledForm() {
  const inputRef = useRef(null);
  const fileRef = useRef(null);
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Input value:', inputRef.current.value);
    console.log('File:', fileRef.current.files[0]);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input ref={inputRef} type="text" defaultValue="Hello" />
      <input ref={fileRef} type="file" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

## Complex Form Example

```jsx
function RegistrationForm() {
  const [formData, setFormData] = useState({
    username: '',
    email: '',
    password: '',
    confirmPassword: '',
    country: '',
    gender: '',
    interests: [],
    newsletter: false
  });
  
  const [errors, setErrors] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);
  
  const countries = ['USA', 'Canada', 'UK', 'Australia'];
  const interestOptions = ['Sports', 'Music', 'Movies', 'Books', 'Travel'];
  
  const handleChange = (e) => {
    const { name, value, type, checked } = e.target;
    
    if (type === 'checkbox' && name === 'interests') {
      setFormData(prev => ({
        ...prev,
        interests: checked
          ? [...prev.interests, value]
          : prev.interests.filter(interest => interest !== value)
      }));
    } else if (type === 'checkbox') {
      setFormData(prev => ({ ...prev, [name]: checked }));
    } else {
      setFormData(prev => ({ ...prev, [name]: value }));
    }
    
    // Clear error when user types
    if (errors[name]) {
      setErrors(prev => ({ ...prev, [name]: '' }));
    }
  };
  
  const validate = () => {
    const newErrors = {};
    
    if (!formData.username) {
      newErrors.username = 'Username is required';
    } else if (formData.username.length < 3) {
      newErrors.username = 'Username must be at least 3 characters';
    }
    
    if (!formData.email) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = 'Email is invalid';
    }
    
    if (!formData.password) {
      newErrors.password = 'Password is required';
    } else if (formData.password.length < 6) {
      newErrors.password = 'Password must be at least 6 characters';
    }
    
    if (formData.password !== formData.confirmPassword) {
      newErrors.confirmPassword = 'Passwords do not match';
    }
    
    if (!formData.country) {
      newErrors.country = 'Please select a country';
    }
    
    if (!formData.gender) {
      newErrors.gender = 'Please select a gender';
    }
    
    if (formData.interests.length === 0) {
      newErrors.interests = 'Please select at least one interest';
    }
    
    return newErrors;
  };
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    const validationErrors = validate();
    if (Object.keys(validationErrors).length > 0) {
      setErrors(validationErrors);
      return;
    }
    
    setIsSubmitting(true);
    
    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 2000));
      console.log('Form submitted:', formData);
      alert('Registration successful!');
      
      // Reset form
      setFormData({
        username: '',
        email: '',
        password: '',
        confirmPassword: '',
        country: '',
        gender: '',
        interests: [],
        newsletter: false
      });
    } catch (error) {
      alert('Registration failed!');
    } finally {
      setIsSubmitting(false);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          type="text"
          name="username"
          value={formData.username}
          onChange={handleChange}
          placeholder="Username"
        />
        {errors.username && <span className="error">{errors.username}</span>}
      </div>
      
      <div>
        <input
          type="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          placeholder="Email"
        />
        {errors.email && <span className="error">{errors.email}</span>}
      </div>
      
      <div>
        <input
          type="password"
          name="password"
          value={formData.password}
          onChange={handleChange}
          placeholder="Password"
        />
        {errors.password && <span className="error">{errors.password}</span>}
      </div>
      
      <div>
        <input
          type="password"
          name="confirmPassword"
          value={formData.confirmPassword}
          onChange={handleChange}
          placeholder="Confirm Password"
        />
        {errors.confirmPassword && <span className="error">{errors.confirmPassword}</span>}
      </div>
      
      <div>
        <select
          name="country"
          value={formData.country}
          onChange={handleChange}
        >
          <option value="">Select Country</option>
          {countries.map(country => (
            <option key={country} value={country}>{country}</option>
          ))}
        </select>
        {errors.country && <span className="error">{errors.country}</span>}
      </div>
      
      <div>
        <label>
          <input
            type="radio"
            name="gender"
            value="male"
            checked={formData.gender === 'male'}
            onChange={handleChange}
          />
          Male
        </label>
        <label>
          <input
            type="radio"
            name="gender"
            value="female"
            checked={formData.gender === 'female'}
            onChange={handleChange}
          />
          Female
        </label>
        <label>
          <input
            type="radio"
            name="gender"
            value="other"
            checked={formData.gender === 'other'}
            onChange={handleChange}
          />
          Other
        </label>
        {errors.gender && <span className="error">{errors.gender}</span>}
      </div>
      
      <div>
        <p>Interests:</p>
        {interestOptions.map(interest => (
          <label key={interest}>
            <input
              type="checkbox"
              name="interests"
              value={interest}
              checked={formData.interests.includes(interest)}
              onChange={handleChange}
            />
            {interest}
          </label>
        ))}
        {errors.interests && <span className="error">{errors.interests}</span>}
      </div>
      
      <div>
        <label>
          <input
            type="checkbox"
            name="newsletter"
            checked={formData.newsletter}
            onChange={handleChange}
          />
          Subscribe to newsletter
        </label>
      </div>
      
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Submitting...' : 'Register'}
      </button>
    </form>
  );
}
```

## Form Utilities

### Custom Form Hook
```jsx
function useForm(initialValues, validate) {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);
  
  const handleChange = (e) => {
    const { name, value, type, checked } = e.target;
    setValues(prev => ({
      ...prev,
      [name]: type === 'checkbox' ? checked : value
    }));
  };
  
  const handleBlur = (e) => {
    const { name } = e.target;
    setTouched(prev => ({ ...prev, [name]: true }));
    
    if (validate) {
      const validationErrors = validate(values);
      setErrors(validationErrors);
    }
  };
  
  const handleSubmit = (onSubmit) => async (e) => {
    e.preventDefault();
    
    if (validate) {
      const validationErrors = validate(values);
      setErrors(validationErrors);
      setTouched(
        Object.keys(values).reduce((acc, key) => ({ ...acc, [key]: true }), {})
      );
      
      if (Object.keys(validationErrors).length > 0) {
        return;
      }
    }
    
    setIsSubmitting(true);
    await onSubmit(values);
    setIsSubmitting(false);
  };
  
  const reset = () => {
    setValues(initialValues);
    setErrors({});
    setTouched({});
    setIsSubmitting(false);
  };
  
  return {
    values,
    errors,
    touched,
    isSubmitting,
    handleChange,
    handleBlur,
    handleSubmit,
    reset
  };
}

// Usage
function ContactForm() {
  const initialValues = { name: '', email: '', message: '' };
  
  const validate = (values) => {
    const errors = {};
    if (!values.name) errors.name = 'Name is required';
    if (!values.email) errors.email = 'Email is required';
    if (!values.message) errors.message = 'Message is required';
    return errors;
  };
  
  const {
    values,
    errors,
    touched,
    isSubmitting,
    handleChange,
    handleBlur,
    handleSubmit,
    reset
  } = useForm(initialValues, validate);
  
  const onSubmit = async (data) => {
    console.log('Form data:', data);
    // Submit to API
    reset();
  };
  
  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        name="name"
        value={values.name}
        onChange={handleChange}
        onBlur={handleBlur}
        placeholder="Name"
      />
      {touched.name && errors.name && <span>{errors.name}</span>}
      
      <input
        name="email"
        value={values.email}
        onChange={handleChange}
        onBlur={handleBlur}
        placeholder="Email"
      />
      {touched.email && errors.email && <span>{errors.email}</span>}
      
      <textarea
        name="message"
        value={values.message}
        onChange={handleChange}
        onBlur={handleBlur}
        placeholder="Message"
      />
      {touched.message && errors.message && <span>{errors.message}</span>}
      
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Sending...' : 'Send'}
      </button>
    </form>
  );
}
```

## Best Practices

1. **Always use controlled components** when possible
2. **Prevent default form submission** to handle it with JavaScript
3. **Validate on blur** for better user experience
4. **Show errors clearly** but not aggressively
5. **Disable submit button** while submitting
6. **Reset form** after successful submission
7. **Use proper input types** for better mobile experience
8. **Group related fields** for better organization

Forms are crucial for user interaction. Master these patterns to create intuitive and user-friendly forms!