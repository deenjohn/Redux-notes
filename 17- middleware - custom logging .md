
custom logging

index.js 

```javascript

import ReactDOM from "react-dom";
import { Provider } from "react-redux";
import { createStore, applyMiddleware } from "redux";
import ReduxPromise from "redux-promise";

import App from "./components/app";
import reducers from "./reducers";

function customLog({ dispatch }) {
  return next => action => {
    console.log("middleware ", action);
    next(action);
  };
}

function customReduxPromise({ dispatch }) {
  return next => action => {
    if (!action.payload || !action.payload.then) {
      return next(action);                         // pass on to next middleware
    }
    
    // go to middleware cycle again starting from ist middleware 
    action.payload.then(function(response) {
      const newAction = { ...action, payload: response };
      dispatch(newAction);                              // we dispatch if we ever change the action 
    });
  };
}

const createStoreWithMiddleware = applyMiddleware(
  customReduxPromise,
  customLog
)(createStore);

ReactDOM.render(
  <Provider store={createStoreWithMiddleware(reducers)}>
    <App />
  </Provider>,
  document.querySelector(".container")
);

```
