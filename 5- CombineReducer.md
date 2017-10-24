


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
      console.log('ADD_TODO')
      return [
        ...state,
        todo(undefined, action)
      ];
    break;
    case 'TOGGLE_TODO':
      console.log('TOGGLE_TODO')
      return state.map(t =>todo(t, action));
    break;
    default:
      console.log('todos default')
      return state;
  }
};

const visibilityFilter = (state='SHOW_ALL', action) => {
  switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      console.log('SET_VISIBILITY_FILTER')
      return action.filter;
    break;
    default:
      console.log('visibilityFilter default')
      return state;
  }
};

const combineReducers = (reducers) => {
	return (state = {}, action) => {
		return Object.keys(reducers).reduce(
			(nextState, key) => {
				nextState[key] = reducers[key](
					state[key],
					action
				);
				return nextState;
			}, {}
		);
	};
};

const todoApp = combineReducers({
	todos,
	visibilityFilter
});

const { createStore } = Redux;
const store = createStore(todoApp);


/*

"todos default"
"visibilityFilter default"

"Initial State"
[object Object] {
  todos: [],
  visibilityFilter: "SHOW_ALL"
}
"-------------"

*/


```
