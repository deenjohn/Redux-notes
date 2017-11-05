
custom logging

index.js 

```javascript

import reducers from "./reducers";

function customLog({ dispatch }) {
  return next => action => {
    console.log("middleware ", action);
    next(action);
  };
}
const createStoreWithMiddleware = applyMiddleware(ReduxPromise, Log)(
  createStore
);


ReactDOM.render(
  <Provider store={createStoreWithMiddleware(reducers)}>
    <App />
  </Provider>,
  document.querySelector(".container")
);

```
