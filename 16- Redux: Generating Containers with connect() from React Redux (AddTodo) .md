
### Before

In AddTodo , we only need store for dispatch function

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

AddTodo.contextTypes = {
    store : React.PropTypes.object
}


```

### After
//https://github.com/deenjohn/react-redux/blob/master/docs/api.md#examples
Inject just dispatch and don't listen to store

```javascript

const AddTodo = ({ dispatch }) => {
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

AddTodo = connect() (AddTodo)



```
