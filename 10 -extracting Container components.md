
Make FilterLink a container component 




### Before 

```javascript
TodoApp

<Footer
      visibilityFilter={visibilityFilter}
      onFilterClick={ filter => 
        store.dispatch({
          type : 'SET_VISIBILITY_FILTER',
          filter
        })
      }
    />
........................................

// Footer pass the visibilityFilter to FilterLink in props 'currentFilter'
// Footer only passes the visibilityFilter and onFilterClick to FilterLink
// problem : have to pass a lot of props to child component like 'FilterLink'
// parent component need to know a lot about what child component need

const Footer = ({
  visibilityFilter,
  onFilterClick
}) => (
  <p>
    Show:
    {' '}
    <FilterLink
      filter='SHOW_ALL'
      currentFilter={visibilityFilter}  
      onClick={onFilterClick}
      >
      All
    </FilterLink>
    {' '}
    <FilterLink
      filter='SHOW_ACTIVE'
      currentFilter={visibilityFilter}
      onClick={onFilterClick}
      >
      Active
    </FilterLink>
    {' '}
    <FilterLink
      filter='SHOW_COMPLETED'
      currentFilter={visibilityFilter}
      onClick={onFilterClick}
      >
      Completed
    </FilterLink>
  </p>
);


............................

//FilterLink needs to know currentfilter
// so it need props from parent 'Footer'

const FilterLink = ({
  filter, 
  currentFilter,
  children,
  onClick
}) => {
  if (filter === currentFilter) {
    return <span>{children}</span>;
  }
            
  return (
    <a href="#"
      onClick={(e) => {
        e.preventDefault();
        onClick(filter);
      }}
    >
      {children}
    </a>
  );
};



```

### After 

```javascript


Footer component is a presentational component and is decoupled from its child component FilterLink.
FilterLink is a container component and self-sufficient 
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



....................................


//FilterLink re-renders all the Links everytime there is a state update
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
                props.filter ===
                state.visibilityFilter
            }
            onClick={() => 
                store.dispatch({
                    type  : 'SET_VISIBILITY_FILTER',
                    filter: props.filter
                })
            }
        >
        {props.children}
        </Link>
    );
  }
} 



...................................................

//Link gets active , children and onClick as props and only concerns with rendering 
const Link = ({
    active,
    children,
    onClick
}) => {
    if (active) {
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


```









