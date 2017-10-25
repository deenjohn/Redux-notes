
Use React-redux library 

<script src="//cdnjs.cloudflare.com/ajax/libs/react-redux/4.0.0/react-redux.js"></script>

### Before
```javascript

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

const { createStore } = Redux;

ReactDOM.render(
    <Provider store={createStore(todoApp)}>
        <TodoApp />
    </Provider>,
    document.getElementById('root')
);




```





### After

```javascript

const { Provider } = ReactRedux;
const { createStore } = Redux;

ReactDOM.render(
    <Provider store={createStore(todoApp)}>
        <TodoApp />
    </Provider>,
    document.getElementById('root')
);



```
