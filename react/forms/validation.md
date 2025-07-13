# Form Validation in React

Form validation is crucial for ensuring data integrity and providing a good user experience. React offers various approaches to implement form validation effectively.

## Basic Validation

### Simple Required Field Validation
```jsx
function SimpleForm() {
  const [email, setEmail] = useState('');
  const [error, setError] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    
    if (!email) {
      setError('Email is required');
      return;
    }
    
    if (!email.includes('@')) {
      setError('Please enter a valid email');
      return;
    }
    
    console.log('Form submitted:', email);
    setError('');
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={(e) => {
          setEmail(e.target.value);
          setError(''); // Clear error on change
        }}
        placeholder="Enter email"
      />
      {error && <span style={{ color: 'red' }}>{error}</span>}
      <button type="submit">Submit</button>
    </form>
  );
}
```

## Real-time Validation

### Validate on Change
```jsx
function RealtimeValidation() {
  const [formData, setFormData] = useState({
    username: '',
    email: '',
    password: ''
  });
  const [errors, setErrors] = useState({});
  
  const validateField = (name, value) => {
    switch (name) {
      case 'username':
        if (!value) return 'Username is required';
        if (value.length < 3) return 'Username must be at least 3 characters';
        if (!/^[a-zA-Z0-9_]+$/.test(value)) return 'Username can only contain letters, numbers, and underscores';
        break;
      case 'email':
        if (!value) return 'Email is required';
        if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)) return 'Invalid email format';
        break;
      case 'password':
        if (!value) return 'Password is required';
        if (value.length < 8) return 'Password must be at least 8 characters';
        if (!/(?=.*[0-9])/.test(value)) return 'Password must contain at least one number';
        if (!/(?=.*[a-z])/.test(value)) return 'Password must contain at least one lowercase letter';
        if (!/(?=.*[A-Z])/.test(value)) return 'Password must contain at least one uppercase letter';
        break;
      default:
        return '';
    }
    return '';
  };
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    
    // Validate field in real-time
    const error = validateField(name, value);
    setErrors(prev => ({ ...prev, [name]: error }));
  };
  
  return (
    <form>
      <div>
        <input
          name="username"
          value={formData.username}
          onChange={handleChange}
          placeholder="Username"
        />
        {errors.username && <span className="error">{errors.username}</span>}
      </div>
      
      <div>
        <input
          name="email"
          type="email"
          value={formData.email}
          onChange={handleChange}
          placeholder="Email"
        />
        {errors.email && <span className="error">{errors.email}</span>}
      </div>
      
      <div>
        <input
          name="password"
          type="password"
          value={formData.password}
          onChange={handleChange}
          placeholder="Password"
        />
        {errors.password && <span className="error">{errors.password}</span>}
      </div>
    </form>
  );
}
```

## Validation on Blur

```jsx
function BlurValidation() {
  const [formData, setFormData] = useState({
    firstName: '',
    lastName: '',
    age: ''
  });
  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});
  
  const validateField = (name, value) => {
    switch (name) {
      case 'firstName':
      case 'lastName':
        if (!value) return `${name === 'firstName' ? 'First' : 'Last'} name is required`;
        if (value.length < 2) return 'Must be at least 2 characters';
        if (!/^[a-zA-Z\s-']+$/.test(value)) return 'Only letters, spaces, hyphens, and apostrophes allowed';
        break;
      case 'age':
        if (!value) return 'Age is required';
        const ageNum = parseInt(value);
        if (isNaN(ageNum)) return 'Age must be a number';
        if (ageNum < 0 || ageNum > 150) return 'Age must be between 0 and 150';
        break;
    }
    return '';
  };
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };
  
  const handleBlur = (e) => {
    const { name, value } = e.target;
    setTouched(prev => ({ ...prev, [name]: true }));
    
    const error = validateField(name, value);
    setErrors(prev => ({ ...prev, [name]: error }));
  };
  
  return (
    <form>
      <div>
        <input
          name="firstName"
          value={formData.firstName}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="First Name"
        />
        {touched.firstName && errors.firstName && (
          <span className="error">{errors.firstName}</span>
        )}
      </div>
      
      <div>
        <input
          name="lastName"
          value={formData.lastName}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Last Name"
        />
        {touched.lastName && errors.lastName && (
          <span className="error">{errors.lastName}</span>
        )}
      </div>
      
      <div>
        <input
          name="age"
          type="number"
          value={formData.age}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Age"
        />
        {touched.age && errors.age && (
          <span className="error">{errors.age}</span>
        )}
      </div>
    </form>
  );
}
```

## Custom Validation Hook

```jsx
function useValidation(initialValues, validationRules) {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});
  
  const validate = (fieldName = null) => {
    const fieldsToValidate = fieldName ? [fieldName] : Object.keys(validationRules);
    const newErrors = { ...errors };
    
    fieldsToValidate.forEach(field => {
      const rules = validationRules[field];
      if (!rules) return;
      
      const value = values[field];
      let error = '';
      
      for (const rule of rules) {
        const result = rule(value, values);
        if (result !== true) {
          error = result;
          break;
        }
      }
      
      if (error) {
        newErrors[field] = error;
      } else {
        delete newErrors[field];
      }
    });
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  const handleChange = (e) => {
    const { name, value, type, checked } = e.target;
    const newValue = type === 'checkbox' ? checked : value;
    
    setValues(prev => ({ ...prev, [name]: newValue }));
    
    if (touched[name]) {
      validate(name);
    }
  };
  
  const handleBlur = (e) => {
    const { name } = e.target;
    setTouched(prev => ({ ...prev, [name]: true }));
    validate(name);
  };
  
  const handleSubmit = (callback) => (e) => {
    e.preventDefault();
    
    // Touch all fields
    const allTouched = Object.keys(values).reduce(
      (acc, key) => ({ ...acc, [key]: true }),
      {}
    );
    setTouched(allTouched);
    
    if (validate()) {
      callback(values);
    }
  };
  
  return {
    values,
    errors,
    touched,
    handleChange,
    handleBlur,
    handleSubmit,
    validate,
    setValues
  };
}

// Validation rules
const required = (message = 'This field is required') => 
  value => value ? true : message;

const minLength = (min, message) => 
  value => !value || value.length >= min ? true : message || `Must be at least ${min} characters`;

const maxLength = (max, message) => 
  value => !value || value.length <= max ? true : message || `Must be at most ${max} characters`;

const pattern = (regex, message) => 
  value => !value || regex.test(value) ? true : message;

const email = (message = 'Invalid email address') => 
  pattern(/^[^\s@]+@[^\s@]+\.[^\s@]+$/, message);

const matchField = (fieldName, message) => 
  (value, allValues) => value === allValues[fieldName] ? true : message;

// Usage example
function RegistrationForm() {
  const validationRules = {
    username: [
      required(),
      minLength(3, 'Username must be at least 3 characters'),
      pattern(/^[a-zA-Z0-9_]+$/, 'Only letters, numbers, and underscores allowed')
    ],
    email: [
      required(),
      email()
    ],
    password: [
      required(),
      minLength(8),
      pattern(/[0-9]/, 'Must contain at least one number'),
      pattern(/[a-z]/, 'Must contain at least one lowercase letter'),
      pattern(/[A-Z]/, 'Must contain at least one uppercase letter')
    ],
    confirmPassword: [
      required(),
      matchField('password', 'Passwords do not match')
    ]
  };
  
  const {
    values,
    errors,
    touched,
    handleChange,
    handleBlur,
    handleSubmit
  } = useValidation(
    { username: '', email: '', password: '', confirmPassword: '' },
    validationRules
  );
  
  const onSubmit = (data) => {
    console.log('Form submitted:', data);
  };
  
  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div>
        <input
          name="username"
          value={values.username}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Username"
        />
        {touched.username && errors.username && (
          <span className="error">{errors.username}</span>
        )}
      </div>
      
      <div>
        <input
          name="email"
          type="email"
          value={values.email}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Email"
        />
        {touched.email && errors.email && (
          <span className="error">{errors.email}</span>
        )}
      </div>
      
      <div>
        <input
          name="password"
          type="password"
          value={values.password}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Password"
        />
        {touched.password && errors.password && (
          <span className="error">{errors.password}</span>
        )}
      </div>
      
      <div>
        <input
          name="confirmPassword"
          type="password"
          value={values.confirmPassword}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Confirm Password"
        />
        {touched.confirmPassword && errors.confirmPassword && (
          <span className="error">{errors.confirmPassword}</span>
        )}
      </div>
      
      <button type="submit">Register</button>
    </form>
  );
}
```

## Async Validation

```jsx
function AsyncValidationForm() {
  const [username, setUsername] = useState('');
  const [isChecking, setIsChecking] = useState(false);
  const [isAvailable, setIsAvailable] = useState(null);
  const [error, setError] = useState('');
  
  // Simulate API call to check username availability
  const checkUsernameAvailability = async (username) => {
    setIsChecking(true);
    
    // Simulate API delay
    await new Promise(resolve => setTimeout(resolve, 1000));
    
    // Simulate some usernames being taken
    const takenUsernames = ['admin', 'user', 'test'];
    const available = !takenUsernames.includes(username.toLowerCase());
    
    setIsAvailable(available);
    setIsChecking(false);
    
    return available;
  };
  
  useEffect(() => {
    if (!username) {
      setError('');
      setIsAvailable(null);
      return;
    }
    
    if (username.length < 3) {
      setError('Username must be at least 3 characters');
      setIsAvailable(null);
      return;
    }
    
    setError('');
    
    // Debounce the API call
    const timer = setTimeout(() => {
      checkUsernameAvailability(username);
    }, 500);
    
    return () => clearTimeout(timer);
  }, [username]);
  
  return (
    <form>
      <div>
        <input
          value={username}
          onChange={(e) => setUsername(e.target.value)}
          placeholder="Choose a username"
        />
        {isChecking && <span>Checking availability...</span>}
        {error && <span className="error">{error}</span>}
        {isAvailable === true && <span className="success">✓ Available</span>}
        {isAvailable === false && <span className="error">✗ Username taken</span>}
      </div>
    </form>
  );
}
```

## Complex Form Validation

```jsx
function ComplexValidationForm() {
  const [formData, setFormData] = useState({
    personalInfo: {
      firstName: '',
      lastName: '',
      dateOfBirth: ''
    },
    contactInfo: {
      email: '',
      phone: '',
      address: ''
    },
    preferences: {
      newsletter: false,
      notifications: {
        email: false,
        sms: false,
        push: false
      }
    }
  });
  
  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});
  
  const validatePersonalInfo = (data) => {
    const errors = {};
    
    if (!data.firstName) {
      errors.firstName = 'First name is required';
    }
    
    if (!data.lastName) {
      errors.lastName = 'Last name is required';
    }
    
    if (!data.dateOfBirth) {
      errors.dateOfBirth = 'Date of birth is required';
    } else {
      const age = new Date().getFullYear() - new Date(data.dateOfBirth).getFullYear();
      if (age < 18) {
        errors.dateOfBirth = 'You must be at least 18 years old';
      }
    }
    
    return errors;
  };
  
  const validateContactInfo = (data) => {
    const errors = {};
    
    if (!data.email) {
      errors.email = 'Email is required';
    } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(data.email)) {
      errors.email = 'Invalid email format';
    }
    
    if (!data.phone) {
      errors.phone = 'Phone is required';
    } else if (!/^\+?[\d\s-()]+$/.test(data.phone)) {
      errors.phone = 'Invalid phone format';
    }
    
    if (!data.address) {
      errors.address = 'Address is required';
    }
    
    return errors;
  };
  
  const validatePreferences = (data) => {
    const errors = {};
    
    if (data.newsletter && !Object.values(data.notifications).some(v => v)) {
      errors.notifications = 'Select at least one notification method for newsletter';
    }
    
    return errors;
  };
  
  const validateForm = () => {
    const newErrors = {
      personalInfo: validatePersonalInfo(formData.personalInfo),
      contactInfo: validateContactInfo(formData.contactInfo),
      preferences: validatePreferences(formData.preferences)
    };
    
    setErrors(newErrors);
    
    // Check if there are any errors
    const hasErrors = Object.values(newErrors).some(section =>
      Object.keys(section).length > 0
    );
    
    return !hasErrors;
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    
    if (validateForm()) {
      console.log('Form is valid:', formData);
      // Submit form
    } else {
      console.log('Form has errors');
    }
  };
  
  const updateNestedState = (section, field, value) => {
    setFormData(prev => ({
      ...prev,
      [section]: {
        ...prev[section],
        [field]: value
      }
    }));
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <fieldset>
        <legend>Personal Information</legend>
        
        <input
          value={formData.personalInfo.firstName}
          onChange={(e) => updateNestedState('personalInfo', 'firstName', e.target.value)}
          placeholder="First Name"
        />
        {errors.personalInfo?.firstName && (
          <span className="error">{errors.personalInfo.firstName}</span>
        )}
        
        <input
          value={formData.personalInfo.lastName}
          onChange={(e) => updateNestedState('personalInfo', 'lastName', e.target.value)}
          placeholder="Last Name"
        />
        {errors.personalInfo?.lastName && (
          <span className="error">{errors.personalInfo.lastName}</span>
        )}
        
        <input
          type="date"
          value={formData.personalInfo.dateOfBirth}
          onChange={(e) => updateNestedState('personalInfo', 'dateOfBirth', e.target.value)}
        />
        {errors.personalInfo?.dateOfBirth && (
          <span className="error">{errors.personalInfo.dateOfBirth}</span>
        )}
      </fieldset>
      
      <fieldset>
        <legend>Contact Information</legend>
        
        <input
          type="email"
          value={formData.contactInfo.email}
          onChange={(e) => updateNestedState('contactInfo', 'email', e.target.value)}
          placeholder="Email"
        />
        {errors.contactInfo?.email && (
          <span className="error">{errors.contactInfo.email}</span>
        )}
        
        <input
          type="tel"
          value={formData.contactInfo.phone}
          onChange={(e) => updateNestedState('contactInfo', 'phone', e.target.value)}
          placeholder="Phone"
        />
        {errors.contactInfo?.phone && (
          <span className="error">{errors.contactInfo.phone}</span>
        )}
        
        <textarea
          value={formData.contactInfo.address}
          onChange={(e) => updateNestedState('contactInfo', 'address', e.target.value)}
          placeholder="Address"
        />
        {errors.contactInfo?.address && (
          <span className="error">{errors.contactInfo.address}</span>
        )}
      </fieldset>
      
      <fieldset>
        <legend>Preferences</legend>
        
        <label>
          <input
            type="checkbox"
            checked={formData.preferences.newsletter}
            onChange={(e) => updateNestedState('preferences', 'newsletter', e.target.checked)}
          />
          Subscribe to newsletter
        </label>
        
        {formData.preferences.newsletter && (
          <div>
            <p>Notification methods:</p>
            <label>
              <input
                type="checkbox"
                checked={formData.preferences.notifications.email}
                onChange={(e) => setFormData(prev => ({
                  ...prev,
                  preferences: {
                    ...prev.preferences,
                    notifications: {
                      ...prev.preferences.notifications,
                      email: e.target.checked
                    }
                  }
                }))}
              />
              Email
            </label>
            <label>
              <input
                type="checkbox"
                checked={formData.preferences.notifications.sms}
                onChange={(e) => setFormData(prev => ({
                  ...prev,
                  preferences: {
                    ...prev.preferences,
                    notifications: {
                      ...prev.preferences.notifications,
                      sms: e.target.checked
                    }
                  }
                }))}
              />
              SMS
            </label>
            <label>
              <input
                type="checkbox"
                checked={formData.preferences.notifications.push}
                onChange={(e) => setFormData(prev => ({
                  ...prev,
                  preferences: {
                    ...prev.preferences,
                    notifications: {
                      ...prev.preferences.notifications,
                      push: e.target.checked
                    }
                  }
                }))}
              />
              Push Notifications
            </label>
          </div>
        )}
        {errors.preferences?.notifications && (
          <span className="error">{errors.preferences.notifications}</span>
        )}
      </fieldset>
      
      <button type="submit">Submit</button>
    </form>
  );
}
```

## Validation Schema with Yup-like Implementation

```jsx
// Simple schema validation implementation
class Schema {
  constructor(shape) {
    this.shape = shape;
  }
  
  validate(data) {
    const errors = {};
    
    Object.keys(this.shape).forEach(key => {
      const validator = this.shape[key];
      const result = validator.validate(data[key]);
      if (result) {
        errors[key] = result;
      }
    });
    
    return Object.keys(errors).length > 0 ? errors : null;
  }
}

class StringValidator {
  constructor() {
    this.checks = [];
  }
  
  required(message = 'This field is required') {
    this.checks.push(value => !value ? message : null);
    return this;
  }
  
  min(length, message) {
    this.checks.push(value => 
      value && value.length < length 
        ? message || `Must be at least ${length} characters` 
        : null
    );
    return this;
  }
  
  max(length, message) {
    this.checks.push(value => 
      value && value.length > length 
        ? message || `Must be at most ${length} characters` 
        : null
    );
    return this;
  }
  
  email(message = 'Invalid email') {
    this.checks.push(value => 
      value && !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value) 
        ? message 
        : null
    );
    return this;
  }
  
  validate(value) {
    for (const check of this.checks) {
      const error = check(value);
      if (error) return error;
    }
    return null;
  }
}

const string = () => new StringValidator();

// Usage
function SchemaValidationForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    bio: ''
  });
  const [errors, setErrors] = useState({});
  
  const schema = new Schema({
    name: string().required().min(2).max(50),
    email: string().required().email(),
    bio: string().max(500, 'Bio is too long')
  });
  
  const handleSubmit = (e) => {
    e.preventDefault();
    
    const validationErrors = schema.validate(formData);
    if (validationErrors) {
      setErrors(validationErrors);
    } else {
      setErrors({});
      console.log('Valid data:', formData);
    }
  };
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    
    // Clear error for this field
    if (errors[name]) {
      setErrors(prev => ({ ...prev, [name]: null }));
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          name="name"
          value={formData.name}
          onChange={handleChange}
          placeholder="Name"
        />
        {errors.name && <span className="error">{errors.name}</span>}
      </div>
      
      <div>
        <input
          name="email"
          value={formData.email}
          onChange={handleChange}
          placeholder="Email"
        />
        {errors.email && <span className="error">{errors.email}</span>}
      </div>
      
      <div>
        <textarea
          name="bio"
          value={formData.bio}
          onChange={handleChange}
          placeholder="Bio (optional)"
        />
        {errors.bio && <span className="error">{errors.bio}</span>}
      </div>
      
      <button type="submit">Submit</button>
    </form>
  );
}
```

## Best Practices

1. **Validate on the right events** - Usually onBlur for individual fields
2. **Show errors at the right time** - Not while user is typing
3. **Clear error messages** - Use descriptive, actionable messages
4. **Client and server validation** - Never trust client-side validation alone
5. **Accessibility** - Use ARIA attributes for screen readers
6. **Visual feedback** - Use colors and icons to indicate validation state
7. **Progressive enhancement** - Form should work without JavaScript
8. **Consistent validation** - Same rules on client and server

Form validation is essential for data quality and user experience. Implement it thoughtfully to guide users without frustrating them!