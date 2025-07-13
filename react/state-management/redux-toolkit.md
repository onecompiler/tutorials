# Redux Toolkit

Redux Toolkit (RTK) is the official, opinionated, batteries-included toolset for efficient Redux development. It includes utilities to simplify common Redux use cases and best practices.

## Installation

```bash
npm install @reduxjs/toolkit react-redux
```

## Why Redux Toolkit?

Redux Toolkit solves common Redux pain points:
- Configuring a Redux store is too complicated
- Adding many packages to get Redux to work
- Redux requires too much boilerplate code

## Core Concepts

### configureStore

```jsx
import { configureStore } from '@reduxjs/toolkit';
import todosReducer from './features/todos/todosSlice';
import userReducer from './features/user/userSlice';

// Configure store with Redux Toolkit
const store = configureStore({
  reducer: {
    todos: todosReducer,
    user: userReducer
  }
});

export default store;
export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

### createSlice

```jsx
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

interface Todo {
  id: string;
  text: string;
  completed: boolean;
}

interface TodosState {
  items: Todo[];
  filter: 'all' | 'active' | 'completed';
}

const initialState: TodosState = {
  items: [],
  filter: 'all'
};

const todosSlice = createSlice({
  name: 'todos',
  initialState,
  reducers: {
    // Redux Toolkit uses Immer internally, so we can "mutate" state
    addTodo: (state, action: PayloadAction<string>) => {
      state.items.push({
        id: Date.now().toString(),
        text: action.payload,
        completed: false
      });
    },
    
    toggleTodo: (state, action: PayloadAction<string>) => {
      const todo = state.items.find(item => item.id === action.payload);
      if (todo) {
        todo.completed = !todo.completed;
      }
    },
    
    deleteTodo: (state, action: PayloadAction<string>) => {
      state.items = state.items.filter(item => item.id !== action.payload);
    },
    
    setFilter: (state, action: PayloadAction<TodosState['filter']>) => {
      state.filter = action.payload;
    },
    
    clearCompleted: (state) => {
      state.items = state.items.filter(item => !item.completed);
    }
  }
});

// Export actions
export const { 
  addTodo, 
  toggleTodo, 
  deleteTodo, 
  setFilter, 
  clearCompleted 
} = todosSlice.actions;

// Export reducer
export default todosSlice.reducer;

// Selectors
export const selectTodos = (state: RootState) => state.todos.items;
export const selectFilter = (state: RootState) => state.todos.filter;

export const selectVisibleTodos = (state: RootState) => {
  const todos = selectTodos(state);
  const filter = selectFilter(state);
  
  switch (filter) {
    case 'active':
      return todos.filter(todo => !todo.completed);
    case 'completed':
      return todos.filter(todo => todo.completed);
    default:
      return todos;
  }
};
```

## Async Logic with createAsyncThunk

### Basic Async Thunk
```jsx
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

// Define async thunk
export const fetchUserById = createAsyncThunk(
  'users/fetchById',
  async (userId: string) => {
    const response = await fetch(`/api/users/${userId}`);
    if (!response.ok) throw new Error('Failed to fetch user');
    return response.json();
  }
);

// User slice
interface UserState {
  currentUser: User | null;
  loading: boolean;
  error: string | null;
}

const initialState: UserState = {
  currentUser: null,
  loading: false,
  error: null
};

const userSlice = createSlice({
  name: 'user',
  initialState,
  reducers: {
    logout: (state) => {
      state.currentUser = null;
      state.error = null;
    }
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUserById.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchUserById.fulfilled, (state, action) => {
        state.loading = false;
        state.currentUser = action.payload;
      })
      .addCase(fetchUserById.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message || 'Failed to fetch user';
      });
  }
});

export const { logout } = userSlice.actions;
export default userSlice.reducer;
```

### Advanced Async Patterns
```jsx
// Async thunk with parameters and condition
export const fetchTodos = createAsyncThunk(
  'todos/fetch',
  async ({ page = 1, limit = 10 }: { page?: number; limit?: number }) => {
    const response = await fetch(`/api/todos?page=${page}&limit=${limit}`);
    return response.json();
  },
  {
    condition: (params, { getState }) => {
      const { todos } = getState() as RootState;
      if (todos.loading) {
        // Already fetching, don't fetch again
        return false;
      }
    }
  }
);

// Thunk with error handling
export const loginUser = createAsyncThunk(
  'auth/login',
  async (credentials: { email: string; password: string }, { rejectWithValue }) => {
    try {
      const response = await fetch('/api/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(credentials)
      });
      
      const data = await response.json();
      
      if (!response.ok) {
        return rejectWithValue(data.message);
      }
      
      // Save token
      localStorage.setItem('authToken', data.token);
      return data.user;
    } catch (error) {
      return rejectWithValue('Network error');
    }
  }
);

// Auth slice
const authSlice = createSlice({
  name: 'auth',
  initialState: {
    user: null,
    token: localStorage.getItem('authToken'),
    loading: false,
    error: null
  },
  reducers: {
    logout: (state) => {
      state.user = null;
      state.token = null;
      localStorage.removeItem('authToken');
    }
  },
  extraReducers: (builder) => {
    builder
      .addCase(loginUser.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(loginUser.fulfilled, (state, action) => {
        state.loading = false;
        state.user = action.payload;
      })
      .addCase(loginUser.rejected, (state, action) => {
        state.loading = false;
        state.error = action.payload as string;
      });
  }
});
```

## RTK Query

RTK Query is a powerful data fetching and caching solution included in Redux Toolkit.

### Basic API Setup
```jsx
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

// Define API slice
export const apiSlice = createApi({
  reducerPath: 'api',
  baseQuery: fetchBaseQuery({
    baseUrl: '/api',
    prepareHeaders: (headers, { getState }) => {
      const token = (getState() as RootState).auth.token;
      if (token) {
        headers.set('authorization', `Bearer ${token}`);
      }
      return headers;
    }
  }),
  tagTypes: ['Todo', 'User'],
  endpoints: (builder) => ({
    // Queries
    getTodos: builder.query<Todo[], void>({
      query: () => 'todos',
      providesTags: ['Todo']
    }),
    
    getTodoById: builder.query<Todo, string>({
      query: (id) => `todos/${id}`,
      providesTags: (result, error, id) => [{ type: 'Todo', id }]
    }),
    
    // Mutations
    addTodo: builder.mutation<Todo, Partial<Todo>>({
      query: (todo) => ({
        url: 'todos',
        method: 'POST',
        body: todo
      }),
      invalidatesTags: ['Todo']
    }),
    
    updateTodo: builder.mutation<Todo, Partial<Todo> & { id: string }>({
      query: ({ id, ...patch }) => ({
        url: `todos/${id}`,
        method: 'PATCH',
        body: patch
      }),
      invalidatesTags: (result, error, { id }) => [{ type: 'Todo', id }]
    }),
    
    deleteTodo: builder.mutation<void, string>({
      query: (id) => ({
        url: `todos/${id}`,
        method: 'DELETE'
      }),
      invalidatesTags: (result, error, id) => [{ type: 'Todo', id }]
    })
  })
});

// Export hooks
export const {
  useGetTodosQuery,
  useGetTodoByIdQuery,
  useAddTodoMutation,
  useUpdateTodoMutation,
  useDeleteTodoMutation
} = apiSlice;

// Add to store
export const store = configureStore({
  reducer: {
    [apiSlice.reducerPath]: apiSlice.reducer,
    auth: authReducer
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(apiSlice.middleware)
});
```

### Using RTK Query in Components
```jsx
function TodoList() {
  const { data: todos, isLoading, error } = useGetTodosQuery();
  const [addTodo] = useAddTodoMutation();
  const [updateTodo] = useUpdateTodoMutation();
  const [deleteTodo] = useDeleteTodoMutation();
  
  const handleAddTodo = async (text: string) => {
    try {
      await addTodo({ text, completed: false }).unwrap();
      // Success - cache will be automatically updated
    } catch (error) {
      console.error('Failed to add todo:', error);
    }
  };
  
  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error loading todos</div>;
  
  return (
    <ul>
      {todos?.map(todo => (
        <li key={todo.id}>
          <input
            type="checkbox"
            checked={todo.completed}
            onChange={() => updateTodo({ 
              id: todo.id, 
              completed: !todo.completed 
            })}
          />
          <span>{todo.text}</span>
          <button onClick={() => deleteTodo(todo.id)}>Delete</button>
        </li>
      ))}
    </ul>
  );
}
```

### Advanced RTK Query Features
```jsx
// Optimistic updates
const apiSlice = createApi({
  // ... base configuration
  endpoints: (builder) => ({
    updateTodo: builder.mutation({
      query: ({ id, ...patch }) => ({
        url: `todos/${id}`,
        method: 'PATCH',
        body: patch
      }),
      // Optimistic update
      async onQueryStarted({ id, ...patch }, { dispatch, queryFulfilled }) {
        const patchResult = dispatch(
          apiSlice.util.updateQueryData('getTodos', undefined, (draft) => {
            const todo = draft.find(todo => todo.id === id);
            if (todo) {
              Object.assign(todo, patch);
            }
          })
        );
        
        try {
          await queryFulfilled;
        } catch {
          patchResult.undo();
        }
      }
    })
  })
});

// Pagination
const apiSlice = createApi({
  endpoints: (builder) => ({
    getPaginatedTodos: builder.query({
      query: ({ page = 1, limit = 10 }) => 
        `todos?page=${page}&limit=${limit}`,
      serializeQueryArgs: ({ endpointName }) => {
        return endpointName;
      },
      merge: (currentCache, newItems) => {
        currentCache.push(...newItems);
      },
      forceRefetch({ currentArg, previousArg }) {
        return currentArg !== previousArg;
      }
    })
  })
});

// Polling
function LiveData() {
  const { data } = useGetTodosQuery(undefined, {
    pollingInterval: 3000 // Poll every 3 seconds
  });
  
  return <div>{/* render data */}</div>;
}
```

## Complete Example App

### Store Setup
```jsx
// store/index.ts
import { configureStore } from '@reduxjs/toolkit';
import { setupListeners } from '@reduxjs/toolkit/query';
import { apiSlice } from './apiSlice';
import authReducer from './authSlice';
import uiReducer from './uiSlice';

export const store = configureStore({
  reducer: {
    [apiSlice.reducerPath]: apiSlice.reducer,
    auth: authReducer,
    ui: uiReducer
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(apiSlice.middleware)
});

setupListeners(store.dispatch);

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

### Typed Hooks
```typescript
// hooks/redux.ts
import { useDispatch, useSelector, TypedUseSelectorHook } from 'react-redux';
import type { RootState, AppDispatch } from '../store';

export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

### UI Slice with Entity Adapter
```jsx
import { createSlice, createEntityAdapter } from '@reduxjs/toolkit';

// Entity adapter for normalized state
const notificationsAdapter = createEntityAdapter({
  sortComparer: (a, b) => b.timestamp - a.timestamp
});

const uiSlice = createSlice({
  name: 'ui',
  initialState: {
    sidebarOpen: true,
    theme: 'light',
    notifications: notificationsAdapter.getInitialState()
  },
  reducers: {
    toggleSidebar: (state) => {
      state.sidebarOpen = !state.sidebarOpen;
    },
    
    setTheme: (state, action) => {
      state.theme = action.payload;
    },
    
    addNotification: (state, action) => {
      notificationsAdapter.addOne(state.notifications, {
        id: Date.now().toString(),
        ...action.payload,
        timestamp: Date.now()
      });
    },
    
    removeNotification: (state, action) => {
      notificationsAdapter.removeOne(state.notifications, action.payload);
    },
    
    clearNotifications: (state) => {
      notificationsAdapter.removeAll(state.notifications);
    }
  }
});

export const { 
  toggleSidebar, 
  setTheme, 
  addNotification, 
  removeNotification,
  clearNotifications
} = uiSlice.actions;

export default uiSlice.reducer;

// Selectors
export const notificationsSelectors = notificationsAdapter.getSelectors(
  (state: RootState) => state.ui.notifications
);
```

## Performance Optimization

### Memoized Selectors
```jsx
import { createSelector } from '@reduxjs/toolkit';

// Input selectors
const selectTodos = (state: RootState) => state.todos.items;
const selectFilter = (state: RootState) => state.todos.filter;
const selectSearchTerm = (state: RootState) => state.todos.searchTerm;

// Memoized selector
export const selectFilteredTodos = createSelector(
  [selectTodos, selectFilter, selectSearchTerm],
  (todos, filter, searchTerm) => {
    let filtered = todos;
    
    // Apply filter
    if (filter === 'active') {
      filtered = filtered.filter(todo => !todo.completed);
    } else if (filter === 'completed') {
      filtered = filtered.filter(todo => todo.completed);
    }
    
    // Apply search
    if (searchTerm) {
      filtered = filtered.filter(todo =>
        todo.text.toLowerCase().includes(searchTerm.toLowerCase())
      );
    }
    
    return filtered;
  }
);
```

## Middleware

### Custom Middleware
```jsx
const logger = (store) => (next) => (action) => {
  console.group(action.type);
  console.info('dispatching', action);
  const result = next(action);
  console.log('next state', store.getState());
  console.groupEnd();
  return result;
};

const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(logger)
});
```

## Best Practices

1. **Use Redux Toolkit** - It's the modern way to use Redux
2. **Prefer RTK Query** - For data fetching over manual thunks
3. **Use TypeScript** - RTK has excellent TypeScript support
4. **Normalize state** - Use createEntityAdapter for collections
5. **Keep slices focused** - One slice per feature/domain
6. **Use Immer syntax** - Write "mutative" logic in reducers
7. **Memoize selectors** - Use createSelector for derived data
8. **Batch actions** - RTK batches updates automatically

Redux Toolkit dramatically simplifies Redux usage while following best practices by default!