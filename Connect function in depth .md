

### https://github.com/deenjohn/react-redux/blob/master/docs/api.md


```javascript


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

const FilterLink = connect(
    mapStateToLinkProps,
    mapDispatchToLinkProps
)(Link);



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


