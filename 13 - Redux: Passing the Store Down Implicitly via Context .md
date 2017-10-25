
### You have to specify contextTypes to mention what context we want to recieve

```javascript

VisbilityTodoList.contextTypes = {
    store : React.PropTypes.object
};


```

functional components recieve context as 2nd argument after props argument

```javascript

const AddTodo = (props, { store }) => {
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

// contextTypes must also be declared
AddTodo.contextTypes = {
    store : React.PropTypes.object
}


```



### Passing store as props to Provider 

```javascript

ReactDOM.render(
    <Provider store={createStore(todoApp)}>
        <TodoApp />
    </Provider>,
    document.getElementById('root')
);

class Provider extends Component {
    getChildContext() {
        return {
            store : this.props.store
        };
    }
    render () {
        return this.props.children;
    }
};

Provider.childContextTypes  = {
    store : React.PropTypes.object
};

```












