
custom logging

index.js 

```javascript

import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "react-redux";

//createstore
import { createStore, applyMiddleware } from "redux";
import ReduxPromise from "redux-promise";

import App from "./components/app";
import reducers from "./reducers";

//middleware
function customLog() {
  return next => action => {
    console.log("middleware ", action);
    next(action);
  };
}

//middleware
function customReduxPromise({ dispatch }) {
  return next => action => {
    if (!action.payload || !action.payload.then) {
      
      return next(action);
      // data is recieved from promisified
      // pass on to next middleware ie reducer in this case
    }
    // go to middleware cycle again starting from ist middleware
    action.payload.then(function(response) {
      const newAction = { ...action, payload: response };
      dispatch(newAction); // we dispatch if we ever change the action
    });
  };
}

//we will use this custom hoc createstore function for reducers
const createStoreWithMiddleware = applyMiddleware(
  customLog,
  customReduxPromise
)(createStore);

ReactDOM.render(
  <Provider store={createStoreWithMiddleware(reducers)}>
    <App />
  </Provider>,
  document.querySelector(".container")
);

```
