# Redux Basics

Redux is a predictable state container for JavaScript applications. It helps you write applications that behave consistently, run in different environments, and are easy to test.

## Core Concepts

Redux has three core principles:
1. **Single source of truth** - The entire state is stored in one object
2. **State is read-only** - The only way to change state is by dispatching actions
3. **Changes are made with pure functions** - Reducers specify how state changes

## Installation

```bash
npm install redux react-redux
```

## Basic Redux Setup

### Actions
```jsx
// Action types
const ADD_TODO = 'ADD_TODO';
const TOGGLE_TODO = 'TOGGLE_TODO';
const DELETE_TODO = 'DELETE_TODO';
const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER';

// Action creators
const addTodo = (text) => ({
  type: ADD_TODO,
  payload: {
    id: Date.now(),
    text,
    completed: false
  }
});

const toggleTodo = (id) => ({
  type: TOGGLE_TODO,
  payload: id
});

const deleteTodo = (id) => ({
  type: DELETE_TODO,
  payload: id
});

const setVisibilityFilter = (filter) => ({
  type: SET_VISIBILITY_FILTER,
  payload: filter
});

// Visibility filters
export const VisibilityFilters = {
  SHOW_ALL: 'SHOW_ALL',
  SHOW_COMPLETED: 'SHOW_COMPLETED',
  SHOW_ACTIVE: 'SHOW_ACTIVE'
};
```

### Reducers
```jsx
// Initial state
const initialState = {
  todos: [],
  visibilityFilter: VisibilityFilters.SHOW_ALL
};

// Todos reducer
const todosReducer = (state = [], action) => {
  switch (action.type) {
    case ADD_TODO:
      return [...state, action.payload];
      
    case TOGGLE_TODO:
      return state.map(todo =>
        todo.id === action.payload
          ? { ...todo, completed: !todo.completed }
          : todo
      );
      
    case DELETE_TODO:
      return state.filter(todo => todo.id !== action.payload);
      
    default:
      return state;
  }
};

// Visibility filter reducer
const visibilityFilterReducer = (
  state = VisibilityFilters.SHOW_ALL,
  action
) => {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return action.payload;
    default:
      return state;
  }
};

// Root reducer
const rootReducer = (state = initialState, action) => {
  return {
    todos: todosReducer(state.todos, action),
    visibilityFilter: visibilityFilterReducer(state.visibilityFilter, action)
  };
};

// Or use combineReducers
import { combineReducers } from 'redux';

const rootReducer = combineReducers({
  todos: todosReducer,
  visibilityFilter: visibilityFilterReducer
});
```

### Store
```jsx
import { createStore } from 'redux';

// Create store
const store = createStore(rootReducer);

// Get current state
console.log(store.getState());

// Subscribe to changes
const unsubscribe = store.subscribe(() => {
  console.log('State changed:', store.getState());
});

// Dispatch actions
store.dispatch(addTodo('Learn Redux'));
store.dispatch(addTodo('Build an app'));
store.dispatch(toggleTodo(1));

// Unsubscribe
unsubscribe();
```

## React Redux Integration

### Provider Setup
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore } from 'redux';
import rootReducer from './reducers';
import App from './App';

const store = createStore(
  rootReducer,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

### Using Hooks

#### useSelector and useDispatch
```jsx
import React, { useState } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { addTodo, toggleTodo, deleteTodo, setVisibilityFilter } from './actions';

function TodoApp() {
  const todos = useSelector(state => state.todos);
  const filter = useSelector(state => state.visibilityFilter);
  const dispatch = useDispatch();
  
  const [inputValue, setInputValue] = useState('');
  
  const handleAddTodo = (e) => {
    e.preventDefault();
    if (inputValue.trim()) {
      dispatch(addTodo(inputValue));
      setInputValue('');
    }
  };
  
  const getVisibleTodos = () => {
    switch (filter) {
      case VisibilityFilters.SHOW_COMPLETED:
        return todos.filter(todo => todo.completed);
      case VisibilityFilters.SHOW_ACTIVE:
        return todos.filter(todo => !todo.completed);
      default:
        return todos;
    }
  };
  
  const visibleTodos = getVisibleTodos();
  
  return (
    <div>
      <form onSubmit={handleAddTodo}>
        <input
          value={inputValue}
          onChange={(e) => setInputValue(e.target.value)}
          placeholder="Add todo..."
        />
        <button type="submit">Add</button>
      </form>
      
      <div>
        <button
          onClick={() => dispatch(setVisibilityFilter(VisibilityFilters.SHOW_ALL))}
          disabled={filter === VisibilityFilters.SHOW_ALL}
        >
          All
        </button>
        <button
          onClick={() => dispatch(setVisibilityFilter(VisibilityFilters.SHOW_ACTIVE))}
          disabled={filter === VisibilityFilters.SHOW_ACTIVE}
        >
          Active
        </button>
        <button
          onClick={() => dispatch(setVisibilityFilter(VisibilityFilters.SHOW_COMPLETED))}
          disabled={filter === VisibilityFilters.SHOW_COMPLETED}
        >
          Completed
        </button>
      </div>
      
      <ul>
        {visibleTodos.map(todo => (
          <li key={todo.id}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => dispatch(toggleTodo(todo.id))}
            />
            <span
              style={{
                textDecoration: todo.completed ? 'line-through' : 'none'
              }}
            >
              {todo.text}
            </span>
            <button onClick={() => dispatch(deleteTodo(todo.id))}>
              Delete
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

## Advanced Patterns

### Action Constants
```jsx
// actionTypes.js
export const TodoActionTypes = {
  ADD_TODO: 'todos/ADD_TODO',
  TOGGLE_TODO: 'todos/TOGGLE_TODO',
  DELETE_TODO: 'todos/DELETE_TODO',
  UPDATE_TODO: 'todos/UPDATE_TODO',
  CLEAR_COMPLETED: 'todos/CLEAR_COMPLETED'
};

export const UserActionTypes = {
  SET_USER: 'user/SET_USER',
  UPDATE_USER: 'user/UPDATE_USER',
  LOGOUT: 'user/LOGOUT'
};
```

### Complex State Management
```jsx
// User reducer with more complex state
const initialUserState = {
  currentUser: null,
  isLoading: false,
  error: null
};

const userReducer = (state = initialUserState, action) => {
  switch (action.type) {
    case 'user/LOGIN_REQUEST':
      return {
        ...state,
        isLoading: true,
        error: null
      };
      
    case 'user/LOGIN_SUCCESS':
      return {
        ...state,
        currentUser: action.payload,
        isLoading: false,
        error: null
      };
      
    case 'user/LOGIN_FAILURE':
      return {
        ...state,
        currentUser: null,
        isLoading: false,
        error: action.payload
      };
      
    case 'user/LOGOUT':
      return initialUserState;
      
    default:
      return state;
  }
};
```

### Middleware

#### Logger Middleware
```jsx
const logger = store => next => action => {
  console.group(action.type);
  console.info('dispatching', action);
  const result = next(action);
  console.log('next state', store.getState());
  console.groupEnd();
  return result;
};

// Apply middleware
import { createStore, applyMiddleware } from 'redux';

const store = createStore(
  rootReducer,
  applyMiddleware(logger)
);
```

#### Redux Thunk
```jsx
// Install redux-thunk
// npm install redux-thunk

import thunk from 'redux-thunk';

const store = createStore(
  rootReducer,
  applyMiddleware(thunk, logger)
);

// Async action creators
const fetchUser = (userId) => {
  return async (dispatch, getState) => {
    dispatch({ type: 'user/FETCH_REQUEST' });
    
    try {
      const response = await fetch(`/api/users/${userId}`);
      const user = await response.json();
      
      dispatch({
        type: 'user/FETCH_SUCCESS',
        payload: user
      });
    } catch (error) {
      dispatch({
        type: 'user/FETCH_FAILURE',
        payload: error.message
      });
    }
  };
};

// Usage in component
function UserProfile({ userId }) {
  const dispatch = useDispatch();
  const { currentUser, isLoading, error } = useSelector(state => state.user);
  
  useEffect(() => {
    dispatch(fetchUser(userId));
  }, [dispatch, userId]);
  
  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!currentUser) return <div>No user found</div>;
  
  return <div>{currentUser.name}</div>;
}
```

## Selectors

### Basic Selectors
```jsx
// selectors.js
export const getTodos = state => state.todos;
export const getVisibilityFilter = state => state.visibilityFilter;

export const getVisibleTodos = state => {
  const todos = getTodos(state);
  const filter = getVisibilityFilter(state);
  
  switch (filter) {
    case VisibilityFilters.SHOW_COMPLETED:
      return todos.filter(t => t.completed);
    case VisibilityFilters.SHOW_ACTIVE:
      return todos.filter(t => !t.completed);
    default:
      return todos;
  }
};

export const getTodoById = (state, id) => {
  return state.todos.find(todo => todo.id === id);
};

// Usage
function TodoList() {
  const visibleTodos = useSelector(getVisibleTodos);
  
  return (
    <ul>
      {visibleTodos.map(todo => (
        <TodoItem key={todo.id} id={todo.id} />
      ))}
    </ul>
  );
}
```

### Memoized Selectors with Reselect
```jsx
// npm install reselect
import { createSelector } from 'reselect';

// Input selectors
const getTodos = state => state.todos;
const getFilter = state => state.visibilityFilter;

// Memoized selector
export const getVisibleTodos = createSelector(
  [getTodos, getFilter],
  (todos, filter) => {
    switch (filter) {
      case VisibilityFilters.SHOW_COMPLETED:
        return todos.filter(t => t.completed);
      case VisibilityFilters.SHOW_ACTIVE:
        return todos.filter(t => !t.completed);
      default:
        return todos;
    }
  }
);

// Selector with props
export const getTodoById = createSelector(
  [getTodos, (state, id) => id],
  (todos, id) => todos.find(todo => todo.id === id)
);
```

## Redux DevTools

### Setup
```jsx
const store = createStore(
  rootReducer,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);

// Or with middleware
import { composeWithDevTools } from 'redux-devtools-extension';

const store = createStore(
  rootReducer,
  composeWithDevTools(
    applyMiddleware(thunk, logger)
  )
);
```

## Folder Structure

### Duck Pattern
```
src/
  store/
    index.js
    todos/
      actions.js
      reducer.js
      selectors.js
      types.js
    user/
      actions.js
      reducer.js
      selectors.js
      types.js
```

### Feature-based Structure
```
src/
  features/
    todos/
      TodoList.js
      TodoItem.js
      todosSlice.js
    user/
      UserProfile.js
      userSlice.js
  store/
    index.js
```

## Performance Optimization

### Normalized State
```jsx
// Instead of nested data
const badState = {
  posts: [
    {
      id: 1,
      title: 'Post 1',
      author: { id: 1, name: 'John' },
      comments: [
        { id: 1, text: 'Great!', author: { id: 2, name: 'Jane' } }
      ]
    }
  ]
};

// Use normalized state
const goodState = {
  posts: {
    byId: {
      1: { id: 1, title: 'Post 1', authorId: 1, commentIds: [1] }
    },
    allIds: [1]
  },
  users: {
    byId: {
      1: { id: 1, name: 'John' },
      2: { id: 2, name: 'Jane' }
    },
    allIds: [1, 2]
  },
  comments: {
    byId: {
      1: { id: 1, text: 'Great!', authorId: 2, postId: 1 }
    },
    allIds: [1]
  }
};
```

### React.memo with useSelector
```jsx
const TodoItem = React.memo(({ id }) => {
  const todo = useSelector(state => getTodoById(state, id));
  const dispatch = useDispatch();
  
  return (
    <li>
      <input
        type="checkbox"
        checked={todo.completed}
        onChange={() => dispatch(toggleTodo(id))}
      />
      <span>{todo.text}</span>
    </li>
  );
});
```

## TypeScript with Redux

### Typed Actions and Reducers
```typescript
// types.ts
export interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

export interface TodosState {
  todos: Todo[];
  filter: string;
}

// Action types
export const ADD_TODO = 'ADD_TODO';
export const TOGGLE_TODO = 'TOGGLE_TODO';

interface AddTodoAction {
  type: typeof ADD_TODO;
  payload: Todo;
}

interface ToggleTodoAction {
  type: typeof TOGGLE_TODO;
  payload: number;
}

export type TodoActionTypes = AddTodoAction | ToggleTodoAction;

// Reducer
const todosReducer = (
  state: Todo[] = [],
  action: TodoActionTypes
): Todo[] => {
  switch (action.type) {
    case ADD_TODO:
      return [...state, action.payload];
    case TOGGLE_TODO:
      return state.map(todo =>
        todo.id === action.payload
          ? { ...todo, completed: !todo.completed }
          : todo
      );
    default:
      return state;
  }
};
```

## Best Practices

1. **Keep state flat** - Avoid deeply nested objects
2. **Normalize data** - Store entities by ID
3. **Use action creators** - Don't dispatch plain objects
4. **Write pure reducers** - No side effects
5. **Use selectors** - Derive data in selectors, not components
6. **Split reducers** - Keep them focused and small
7. **Type your Redux** - Use TypeScript for better DX
8. **Use Redux DevTools** - Essential for debugging

Redux provides predictable state management for complex applications. Consider Redux Toolkit for a more modern approach!