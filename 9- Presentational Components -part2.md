### Earlier

```javascript

TodoApp

      <p>
          Show:
          {' '}
          <FilterLink
            filter='SHOW_ALL'
            currentFilter={visibilityFilter}  
            >
            All
          </FilterLink>
          {' '}
          <FilterLink
            filter='SHOW_ACTIVE'
            currentFilter={visibilityFilter}
            >
            Active
          </FilterLink>
          {' '}
          <FilterLink
            filter='SHOW_COMPLETED'
            currentFilter={visibilityFilter}
            >
            Completed
          </FilterLink>
        </p>
      </div>
    );

```


### Later

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

................
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



```



### Full code

http://jsbin.com/tudowu/edit?js,output



