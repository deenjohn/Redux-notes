


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

const createStore = (reducer) => {  // counter  is reducer here  , createStore(counter);
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

const store = createStore(counter);
const render = () => {
  document.body.innerText = store.getState();
}

store.subscribe(render);
render();

document.addEventListener('click', () =>{
  store.dispatch({type: 'INCREMENT'});
});


```
