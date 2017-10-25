
### Before
```javascript



class VisbilityTodoList extends Component {
    componentDidMount () {
        const { store } = this.context;
        this.unsubscribe = store.subscribe(() => 
            this.forceUpdate()
        );
    }

    componentWillUnmount() {
        this.unsubscribe();
    }
  
    render() {
        const props = this.props;
        const { store } = this.context;
        const state = store.getState();

        return (
            <TodoList 
                todos={
                    getVisibleTodos(
                        state.todos,
                        state.visibilityFilter
                    )
                }
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

VisbilityTodoList.contextTypes = {
    store : React.PropTypes.object
};


```

### After
```javascript


const mapStateToProps = (state) => {
    return {
        todos: getVisibleTodos(
            state.todos,
            state.visibilityFilter
        )
    };
};

const mapDispatchToProps = (dispatch) => {
    return {
        onTodoClick: (id) => {
            dispatch({
                type: 'TOGGLE_TODO',
                id
            })
        }
    };
};

const { connect } = ReactRedux;

const VisbilityTodoList = connect(
    mapStateToProps,
    mapDispatchToProps
)(TodoList);



```
