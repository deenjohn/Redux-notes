
// https://css-tricks.com/learning-react-redux/

```javascript

function visibilityFilter(state = 'SHOW_ALL', action) {
  switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      return action.filter
    default:
      return state
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_USER':
      return [
        ...state,
        {
          user : action.user,
         
        }
      ]
    
    default:
      return state
  }
}

var action1 = {
  type: 'ADD_USER',
  user: {name: 'Dan'}
};

var action2 = {
  type: 'ADD_USER',
  user: {name: 'John'}
};
 
const { combineReducers, createStore } = Redux;

const reducer = combineReducers({ visibilityFilter, todos })
const store = createStore(reducer);

store.dispatch(action1);
store.dispatch(action2);

console.log(store.getState())

/*
[object Object] {
  todos: [[object Object] {
  user: [object Object] {
    name: "Dan"
  }
}, [object Object] {
  user: [object Object] {
    name: "John"
  }
}],
  visibilityFilter: "SHOW_ALL"
}

*/




```
