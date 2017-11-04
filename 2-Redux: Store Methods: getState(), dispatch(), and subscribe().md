
http://redux.js.org/docs/basics/Store.html

```javascript

//reducer 

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
} 

const { createStore } = Redux;
const store = createStore(counter); // inside store , state = reducer(state, action);
const render = () => {
  document.body.innerText = store.getState();
}

store.subscribe(render);
render();

document.addEventListener('click', () =>{
  store.dispatch({type: 'INCREMENT'});        //action :  {type: 'INCREMENT'}
});

```

