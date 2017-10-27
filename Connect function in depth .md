

### https://github.com/deenjohn/react-redux/blob/master/docs/api.md


```javascript

//[mapStateToProps(state, [ownProps]): stateProps] (Function):

const mapStateToLinkProps = (
    state,
    ownProps
) => {
  console.log(ownProps)  //<Footer <FilterLink filter='SHOW_ALL'
    return {
        active : ownProps.filter ===
        state.visibilityFilter
    };
};

// [mapDispatchToProps(dispatch, [ownProps]): dispatchProps]

const mapDispatchToLinkProps = (
    dispatch,
    ownProps
) => {
    return {
        onClick : () => {
            dispatch({
                type  : 'SET_VISIBILITY_FILTER',
                filter: ownProps.filter
            })
        }
    };
};

//[mergeProps(stateProps, dispatchProps, ownProps): props] (Function): 
//  active , onClick , props

const FilterLink = connect(
    mapStateToLinkProps,
    mapDispatchToLinkProps
)(Link);


// Link

const Link = ({ active, children, onClick }) => {
  if (active) {
    return <span>{children}</span>
  }
  
  /*
  
  const Link = (props ) =>{
  
  console.log(props)
      
 let {active,children,onClick } = props;

  3 times <FilterLink  .. /> component is called
  
  
  [object Object] {
  active: true,
  children: "All",
  filter: "SHOW_ALL",
  onClick: function onClick() {
            dispatch(setVisibilityFilter(ownProps.filter));
        }
  }
  
[object Object] {
  active: false,
  children: "Active",
  filter: "SHOW_ACTIVE",
  onClick: function onClick() {
            dispatch(setVisibilityFilter(ownProps.filter));
        }
  }
  
[object Object] {
  active: false,
  children: "Completed",
  filter: "SHOW_COMPLETED",
  onClick: function onClick() {
            dispatch(setVisibilityFilter(ownProps.filter));
        }
}
  
  
  */

  return (
    <a
      href="#"
      onClick={e => {
        e.preventDefault()
        onClick()
      }}
    >
      {children}
    </a>
  )
}

Link.propTypes = {
  active: PropTypes.bool.isRequired,
  children: PropTypes.node.isRequired,
  onClick: PropTypes.func.isRequired
}

export default Link


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


```


