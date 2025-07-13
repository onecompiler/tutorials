# Route Parameters in React Router

Route parameters allow you to create dynamic routes that can handle variable segments in the URL. This is essential for building applications with dynamic content like user profiles, product pages, or blog posts.

## Basic Route Parameters

### Single Parameter
```jsx
import { Routes, Route, useParams } from 'react-router-dom';

// Define route with parameter
function App() {
  return (
    <Routes>
      <Route path="/users/:userId" element={<UserProfile />} />
      <Route path="/posts/:postId" element={<BlogPost />} />
    </Routes>
  );
}

// Access parameter in component
function UserProfile() {
  const { userId } = useParams();
  
  return (
    <div>
      <h1>User Profile</h1>
      <p>User ID: {userId}</p>
    </div>
  );
}
```

### Multiple Parameters
```jsx
// Route with multiple parameters
<Route path="/posts/:year/:month/:slug" element={<BlogPost />} />

// Access multiple parameters
function BlogPost() {
  const { year, month, slug } = useParams();
  
  return (
    <div>
      <h1>Blog Post</h1>
      <p>Date: {year}/{month}</p>
      <p>Slug: {slug}</p>
    </div>
  );
}
```

## Optional Parameters

### Using Optional Segments
```jsx
// Optional parameter with ?
<Route path="/products/:category/:subcategory?" element={<Products />} />

function Products() {
  const { category, subcategory } = useParams();
  
  return (
    <div>
      <h1>Products</h1>
      <p>Category: {category}</p>
      {subcategory && <p>Subcategory: {subcategory}</p>}
    </div>
  );
}
```

### Multiple Routes Pattern
```jsx
// Alternative approach for optional params
<Routes>
  <Route path="/users/:userId" element={<UserProfile />} />
  <Route path="/users/:userId/posts" element={<UserPosts />} />
  <Route path="/users/:userId/posts/:postId" element={<UserPost />} />
</Routes>
```

## Working with Parameters

### Fetching Data Based on Parameters
```jsx
function UserProfile() {
  const { userId } = useParams();
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    setLoading(true);
    setError(null);
    
    fetchUser(userId)
      .then(data => setUser(data))
      .catch(err => setError(err.message))
      .finally(() => setLoading(false));
  }, [userId]); // Re-fetch when userId changes
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!user) return <div>User not found</div>;
  
  return (
    <div>
      <h1>{user.name}</h1>
      <p>Email: {user.email}</p>
    </div>
  );
}
```

### Parameter Validation
```jsx
function ProductDetail() {
  const { productId } = useParams();
  const navigate = useNavigate();
  
  // Validate parameter
  useEffect(() => {
    if (!productId || isNaN(productId)) {
      navigate('/products', { replace: true });
      return;
    }
    
    // Additional validation
    if (parseInt(productId) < 1) {
      navigate('/404');
    }
  }, [productId, navigate]);
  
  return <div>Product ID: {productId}</div>;
}
```

## Query Parameters

### Using useSearchParams
```jsx
import { useSearchParams } from 'react-router-dom';

function SearchResults() {
  const [searchParams, setSearchParams] = useSearchParams();
  
  // Get query parameters
  const query = searchParams.get('q');
  const page = searchParams.get('page') || 1;
  const sort = searchParams.get('sort') || 'relevance';
  
  // Update query parameters
  const updatePage = (newPage) => {
    setSearchParams(prev => {
      prev.set('page', newPage);
      return prev;
    });
  };
  
  const updateSort = (newSort) => {
    setSearchParams({
      q: query,
      sort: newSort,
      page: 1 // Reset to first page
    });
  };
  
  return (
    <div>
      <h1>Search Results for: {query}</h1>
      <select value={sort} onChange={(e) => updateSort(e.target.value)}>
        <option value="relevance">Relevance</option>
        <option value="date">Date</option>
        <option value="popularity">Popularity</option>
      </select>
      
      <p>Page {page}</p>
      <button onClick={() => updatePage(parseInt(page) + 1)}>
        Next Page
      </button>
    </div>
  );
}
```

### Building Query Strings
```jsx
function FilteredProducts() {
  const [filters, setFilters] = useState({
    category: '',
    minPrice: '',
    maxPrice: '',
    inStock: false
  });
  
  const navigate = useNavigate();
  
  const applyFilters = () => {
    const params = new URLSearchParams();
    
    Object.entries(filters).forEach(([key, value]) => {
      if (value) {
        params.append(key, value);
      }
    });
    
    navigate(`/products?${params.toString()}`);
  };
  
  return (
    <div>
      <input
        placeholder="Category"
        value={filters.category}
        onChange={(e) => setFilters({...filters, category: e.target.value})}
      />
      <input
        type="number"
        placeholder="Min Price"
        value={filters.minPrice}
        onChange={(e) => setFilters({...filters, minPrice: e.target.value})}
      />
      <input
        type="number"
        placeholder="Max Price"
        value={filters.maxPrice}
        onChange={(e) => setFilters({...filters, maxPrice: e.target.value})}
      />
      <label>
        <input
          type="checkbox"
          checked={filters.inStock}
          onChange={(e) => setFilters({...filters, inStock: e.target.checked})}
        />
        In Stock Only
      </label>
      <button onClick={applyFilters}>Apply Filters</button>
    </div>
  );
}
```

## Advanced Parameter Patterns

### Nested Parameters
```jsx
// Routes configuration
<Routes>
  <Route path="/teams/:teamId" element={<Team />}>
    <Route path="members/:memberId" element={<TeamMember />} />
  </Route>
</Routes>

// Parent component
function Team() {
  const { teamId } = useParams();
  
  return (
    <div>
      <h1>Team {teamId}</h1>
      <Outlet />
    </div>
  );
}

// Child component has access to all params
function TeamMember() {
  const { teamId, memberId } = useParams();
  
  return (
    <div>
      <p>Team: {teamId}</p>
      <p>Member: {memberId}</p>
    </div>
  );
}
```

### Wildcard Parameters
```jsx
// Catch-all route
<Route path="/docs/*" element={<Documentation />} />

function Documentation() {
  const params = useParams();
  const path = params['*']; // Get everything after /docs/
  
  return (
    <div>
      <h1>Documentation</h1>
      <p>Path: {path}</p>
    </div>
  );
}
```

### Parameter Constraints
```jsx
// Custom route matching
function App() {
  return (
    <Routes>
      {/* Only matches numeric IDs */}
      <Route 
        path="/users/:id" 
        element={<UserProfile />}
        // Custom matcher
        loader={({ params }) => {
          if (!/^\d+$/.test(params.id)) {
            throw new Response("Not Found", { status: 404 });
          }
          return null;
        }}
      />
    </Routes>
  );
}
```

## Parameter Hooks and Utilities

### Custom Parameter Hook
```jsx
function useTypedParams() {
  const params = useParams();
  
  return {
    ...params,
    // Type conversions
    getNumber: (key) => {
      const value = params[key];
      return value ? parseInt(value, 10) : null;
    },
    getBoolean: (key) => {
      const value = params[key];
      return value === 'true';
    },
    getArray: (key, separator = ',') => {
      const value = params[key];
      return value ? value.split(separator) : [];
    }
  };
}

// Usage
function Component() {
  const params = useTypedParams();
  const userId = params.getNumber('userId');
  const tags = params.getArray('tags');
  
  return <div>User ID: {userId}</div>;
}
```

### Parameter Validation Hook
```jsx
function useValidatedParams(schema) {
  const params = useParams();
  const navigate = useNavigate();
  const [validatedParams, setValidatedParams] = useState(null);
  
  useEffect(() => {
    try {
      const validated = schema.validate(params);
      setValidatedParams(validated);
    } catch (error) {
      console.error('Invalid parameters:', error);
      navigate('/error');
    }
  }, [params, schema, navigate]);
  
  return validatedParams;
}

// Usage with a simple schema
const userSchema = {
  validate: (params) => {
    if (!params.userId || isNaN(params.userId)) {
      throw new Error('Invalid user ID');
    }
    return {
      userId: parseInt(params.userId)
    };
  }
};

function UserProfile() {
  const params = useValidatedParams(userSchema);
  
  if (!params) return <div>Validating...</div>;
  
  return <div>User ID: {params.userId}</div>;
}
```

## Real-World Examples

### E-commerce Product Page
```jsx
function ProductPage() {
  const { category, productSlug } = useParams();
  const [searchParams] = useSearchParams();
  const [product, setProduct] = useState(null);
  
  // Get variant from query params
  const selectedColor = searchParams.get('color');
  const selectedSize = searchParams.get('size');
  
  useEffect(() => {
    fetchProduct(category, productSlug)
      .then(setProduct)
      .catch(console.error);
  }, [category, productSlug]);
  
  return (
    <div>
      {product && (
        <>
          <nav>
            <Link to="/">Home</Link> /
            <Link to={`/products/${category}`}>{category}</Link> /
            <span>{product.name}</span>
          </nav>
          
          <h1>{product.name}</h1>
          
          <div>
            <h3>Colors:</h3>
            {product.colors.map(color => (
              <Link
                key={color}
                to={`?color=${color}&size=${selectedSize || ''}`}
                className={selectedColor === color ? 'selected' : ''}
              >
                {color}
              </Link>
            ))}
          </div>
          
          <div>
            <h3>Sizes:</h3>
            {product.sizes.map(size => (
              <Link
                key={size}
                to={`?color=${selectedColor || ''}&size=${size}`}
                className={selectedSize === size ? 'selected' : ''}
              >
                {size}
              </Link>
            ))}
          </div>
        </>
      )}
    </div>
  );
}
```

### Blog with Categories and Tags
```jsx
function BlogRoutes() {
  return (
    <Routes>
      <Route path="/blog" element={<BlogList />} />
      <Route path="/blog/category/:category" element={<BlogCategory />} />
      <Route path="/blog/tag/:tag" element={<BlogTag />} />
      <Route path="/blog/:year/:month/:slug" element={<BlogPost />} />
    </Routes>
  );
}

function BlogList() {
  const [searchParams] = useSearchParams();
  const page = searchParams.get('page') || 1;
  const search = searchParams.get('search') || '';
  
  const [posts, setPosts] = useState([]);
  
  useEffect(() => {
    fetchPosts({ page, search }).then(setPosts);
  }, [page, search]);
  
  return (
    <div>
      <h1>Blog</h1>
      <input
        placeholder="Search posts..."
        defaultValue={search}
        onKeyPress={(e) => {
          if (e.key === 'Enter') {
            const newSearch = e.target.value;
            window.location.href = `/blog?search=${newSearch}`;
          }
        }}
      />
      
      {posts.map(post => (
        <article key={post.id}>
          <h2>
            <Link to={`/blog/${post.year}/${post.month}/${post.slug}`}>
              {post.title}
            </Link>
          </h2>
          <p>
            Category: 
            <Link to={`/blog/category/${post.category}`}>
              {post.category}
            </Link>
          </p>
          <p>
            Tags: 
            {post.tags.map(tag => (
              <Link key={tag} to={`/blog/tag/${tag}`}>
                #{tag}
              </Link>
            ))}
          </p>
        </article>
      ))}
    </div>
  );
}
```

## Best Practices

1. **Validate parameters** - Don't trust URL params
2. **Handle missing params** - Provide defaults or redirects
3. **Use semantic URLs** - Make URLs human-readable
4. **Encode special characters** - Use encodeURIComponent
5. **Keep URLs RESTful** - Follow REST conventions
6. **Handle loading states** - While fetching param-based data
7. **Update params carefully** - Preserve other params when updating
8. **Document param formats** - Especially for complex patterns

Route parameters are powerful for creating dynamic, data-driven applications. Use them wisely to build flexible and maintainable routing systems!