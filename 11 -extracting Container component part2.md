http://redux.js.org/docs/basics/UsageWithReact.html

### Redux: Extracting Container Components (VisibleTodoList, AddTodo)

### Before

```javascript

const TodoApp = ({
  todos, 
  visibilityFilter
}) => (
  <div>
    <AddTodo
      onAddClick={ text => 
        store.dispatch({
          type: 'ADD_TODO',
          id:nextTodoId++,
          text
        })
      }
    />
    <TodoList 
      todos={
        getVisibleTodos(
          todos,
          visibilityFilter
        )
      }
      onTodoClick={ id => 
        store.dispatch({
             type : 'TOGGLE_TODO',
             id
        })
      } />
    <Footer
      visibilityFilter={visibilityFilter}
      onFilterClick={ filter => 
        store.dispatch({
          type : 'SET_VISIBILITY_FILTER',
          filter
        })
      }
    />
  </div>
);

const render = () => {
  ReactDOM.render(
    <TodoApp 
      {...store.getState()}
    />,
    document.getElementById('root')
  );
};

store.subscribe(render);
render();

```
### After

```javascript

const TodoApp = () => (
    <div>
        <AddTodo />
        <VisbilityTodoList />
        <Footer />
    </div>
);

ReactDOM.render(
    <TodoApp />,
    document.getElementById('root')
);


```

### Before

```javascript
<Footer
      visibilityFilter={visibilityFilter}
      onFilterClick={ filter => 
        store.dispatch({
          type : 'SET_VISIBILITY_FILTER',
          filter
        })
      }
    />

```

### After

```javascript

const Footer = () => (
    <p>
        Show:
        {' '}
        <FilterLink
            filter='SHOW_ALL'
            >
            All
        </FilterLink>
        {' '}
        <FilterLink
            filter='SHOW_ACTIVE'
            >
            Active
        </FilterLink>
            {' '}
        <FilterLink
            filter='SHOW_COMPLETED'
            >
            Completed
        </FilterLink>
    </p>
);

//container
class FilterLink extends Component {
    componentDidMount () {
        this.unsubscribe = store.subscribe(() => 
            this.forceUpdate()
        );
    }

    componentWillUnmount() {
        this.unsubscribe();
    }
  
    render() {
        const props = this.props;
        const state = store.getState();

    return (
        <Link
            active={
                // filter = 'SHOW_ALL' // filter='SHOW_ACTIVE'
                
                props.filter ===
                state.visibilityFilter
            }
            onClick={() => 
                store.dispatch({
                    type  : 'SET_VISIBILITY_FILTER',
                    filter: props.filter
                });
              
            }
        >
        {props.children}
        </Link>
    );
  }
} 



```


### Before

```javascript

const AddTodo = ({
  onAddClick
}) => {
  let input;
  return (
    <div>
      <input ref={node => {
        input = node
      }} />
      <button onClick={()=>{
          onAddClick(input.value);
          input.value = '';
      }}>
      Add Todo
      </button>
    </div>
  );
};



```

### After

```javascript

//container
const AddTodo = () => {
    let input;
    return (
        <div>
            <input ref={ node => {
                input = node
            }} />
            <button onClick={()=>{
                 
                    store.dispatch({
                      type : 'ADD_TODO',
                      id   : nextTodoId++,
                      text : input.value
                })
                
                input.value = '';
            }}>
            Add Todo
            </button>
        </div>
    );
};





```


### Full code

```javascript


//reducer
const todo = (state, action) => {
    switch (action.type) {
        case 'ADD_TODO':
            return {
                id       : action.id,
                text     : action.text,
                completed: false
            };
        case 'TOGGLE_TODO':
            if (state.id !== action.id) {
                return state;
            }
            return {
                ...state,
                completed: !state.completed
            };
        default:
            return state;
    }
};

//reducer
const todos = (state = [], action) => {
    switch (action.type) {
        case 'ADD_TODO':
            return [
                ...state,
                todo(undefined, action)
            ];
        case 'TOGGLE_TODO':
            return state.map(t => todo(t, action));
        default:
            return state;
    }
};

//reducer
const visibilityFilter = (state = 'SHOW_ALL', action) => {
    switch (action.type) {
        case 'SET_VISIBILITY_FILTER':
            return action.filter;
        default:
            return state;
    }
};

const { combineReducers } = Redux;
const todoApp = combineReducers({
    // state is a combination of 2 : todos [] , visibilityFilter
    todos,
    visibilityFilter
});

const { createStore } = Redux;
const store = createStore(todoApp);
const { Component } = React;

const Link = ({
    active,
    children,
    onClick
}) => {
    if (active) {
        // no link href
        return <span>{children}</span>;
    }
            
    return (
        <a href="#"
            onClick={(e) => {
            e.preventDefault();
            onClick();
        }}
        >
        {children}
        </a>
    );
};

//container
class FilterLink extends Component {
    componentDidMount () {
        this.unsubscribe = store.subscribe(() => 
            this.forceUpdate()
        );
    }

    componentWillUnmount() {
        this.unsubscribe();
    }
  
    render() {
        const props = this.props;
        const state = store.getState();

    return (
        <Link
            active={
                // filter = 'SHOW_ALL' // filter='SHOW_ACTIVE'
                
                props.filter ===
                state.visibilityFilter
            }
            onClick={() => 
                store.dispatch({
                    type  : 'SET_VISIBILITY_FILTER',
                    filter: props.filter
                });
              
            }
        >
        {props.children}
        </Link>
    );
  }
} 

const Footer = () => (
    <p>
        Show:
        {' '}
        <FilterLink
            filter='SHOW_ALL'
            >
            All
        </FilterLink>
        {' '}
        <FilterLink
            filter='SHOW_ACTIVE'
            >
            Active
        </FilterLink>
            {' '}
        <FilterLink
            filter='SHOW_COMPLETED'
            >
            Completed
        </FilterLink>
    </p>
);

const Todo = ({
    onClick,
    completed,
    text
}) => (
    <li 
        onClick={onClick}
        style={{
            textDecoration: 
                completed ?
                'line-through' : 
                'none'
            }}>
        {text}
    </li>
);


const TodoList = ({
    todos,
    onTodoClick
}) => (
    <ul>
        {todos.map(todo=> 
            <Todo
                key={todo.id}
                {...todo}
                onClick={() => onTodoClick(todo.id)}
            />
        )}
    </ul>
);

let nextTodoId = 0;

//container
const AddTodo = () => {
    let input;
    return (
        <div>
            <input ref={ node => {
                input = node
            }} />
            <button onClick={()=>{
                 
                    store.dispatch({
                      type : 'ADD_TODO',
                      id   : nextTodoId++,
                      text : input.value
                })
                
                input.value = '';
            }}>
            Add Todo
            </button>
        </div>
    );
};

//reducer

const getVisibleTodos = (todos, filter) => {
    switch (filter) {
    case 'SHOW_ALL':
        return todos;
    case 'SHOW_COMPLETED':
        return todos.filter(t => t.completed);
    case 'SHOW_ACTIVE':
        return todos.filter(t => !t.completed);
    }
};

//container
class VisbilityTodoList extends Component {
    componentDidMount () {
        this.unsubscribe = store.subscribe(() => 
            this.forceUpdate()
        );
    }

    componentWillUnmount() {
        this.unsubscribe();
    }
  
    render() {
        const props = this.props;
      
        const state = store.getState();

        return (
            <TodoList 
                todos={
                    // return todos;
                    // return todos.filter(t => t.completed);
                    // return todos.filter(t => !t.completed);
                    getVisibleTodos(
                        state.todos,
                        state.visibilityFilter
                    )
                }
                // onClick={() => onTodoClick(todo.id)}
                onTodoClick={id =>
                    store.dispatch({
                        type: 'TOGGLE_TODO',
                        id
                    })
                }
            />
        );
    }
}

const TodoApp = () => (
    <div>
        <AddTodo />
        <VisbilityTodoList />
        <Footer />
    </div>
);

ReactDOM.render(
    <TodoApp />,
    document.getElementById('root')
);


 

```
