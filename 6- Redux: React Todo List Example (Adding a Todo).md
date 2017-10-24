

```javascript
const todo = (state, action) => {
  switch(action.type) {
    case 'ADD_TODO':
        return {
          id  : action.id,
          text: action.text,
          completed: false
        };
    break;
    case 'TOGGLE_TODO':
        if (state.id !== action.id ) {
          return state;
        }
      
        return {
            ...state,
            completed: !state.completed
        };
    break;
    default:
      return state;
  }
};

const todos = (state=[], action) => {
  switch(action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        todo(undefined, action)
      ];
    break;
    case 'TOGGLE_TODO':
      return state.map(t =>todo(t, action));
    break;
    default:
      return state;
  }
};

const visibilityFilter = (state='SHOW_ALL', action) => {
  switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      return action.filter;
    break;
    default:
      return state;
  }
};

const { combineReducers } = Redux;
const todoApp = combineReducers({
  todos,
  visibilityFilter
});

const { createStore } = Redux;
const store = createStore(todoApp);
console.log(store.getState())
const { Component } = React;

let nextTodoId = 0;



/*

this.props.todos

[[object Object] {
  completed: false,
  id: 0,
  text: "test"
}, [object Object] {
  completed: false,
  id: 1,
  text: "deen"
}]



*/
class TodoApp extends Component {
  render() {
    console.log('render ' , this.props.todos)
    return (
      <div>
        <input ref={node => {
          this.input = node
        }} />
        <button onClick={()=>{
            store.dispatch({
                type: 'ADD_TODO',
                text: this.input.value,
                id  : nextTodoId++
            }); 
            this.input.value = '';
        }}>
        Add Todo</button>
        <ul>
          {this.props.todos.map(todo => 
             <li key={todo.id}>
                {todo.text}
             </li>
          )}
        </ul>
      </div>
    );
  }
}


/*
Current state 
store.getState()

[object Object] {
  todos: [],
  visibilityFilter: "SHOW_ALL"
}
 

*/
const render = () => {
  ReactDOM.render(
    <TodoApp 
      todos={store.getState().todos}
    />,
    document.getElementById('root')
  );
};

store.subscribe(render);
render();



```
