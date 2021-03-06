
```javascript


const counter = (state = 0, action) => {
    switch (action.type) {
      case 'INCREMENT':
        return state + 1;
      break;
      case 'DECREMENT':
        return state - 1;
      break;
      default:
        return state;
    }
}; 

const createStore = (reducer) => {
  let state;
  let listeners = [];
  
  const getState = () => state; 
  const dispatch = (action) => {
    state = reducer(state, action);
    listeners.forEach(listener => listener());
  };
  const subscribe = (listener) => {
    listeners.push(listener);
    return () => {
      listeners = listeners.filter(l => l !== listener);
    }
  };
  
  dispatch({});
  
  return { getState, dispatch, subscribe };
};

const store = createStore(counter); // runs all the code inside createStore
 // first time default state = 0 is returned

```

