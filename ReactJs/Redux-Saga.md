## redux saga
An intuitive Redux side effect manager.You can use this library to manage requests and more than.
___
We already knew about sending a request API in SpringBoot project.
To send a request to API gateway with redux-saga, do like this.

1) > yarn add redux-saga

2) you need an **_action_** file to manage actions.    
```
export const REQUEST_ADD_USER = "REQUEST_ADD_USER"
export const ADD_USER = "ADD_USER"
export const REQUEST_DELETE_USER = "REQUEST_DELETE_USER"
export const DELETE_USER = "DELETE_USER"
export const REQUEST_FETCH_USERS = "REQUEST_FETCH_USERS"

export const addUser = (user) => ({
	type: ADD_USER,
	payload: user,
})

export const deleteUser = (userId) => ({
	type: DELETE_USER,
	payload: userId,
})
```

When you need an action, you can import that.
3) You need an **_api.js_** file to manage requests.
An example method to send a request to add user to API gateway that receive data in json form**_(@RequestBody)_**
and return response.   
```
import axios from "axios"
import env from "@beam-australia/react-env"

export const sendRequestAdduser = async (user) => {
	try {
		const request = {
			/* eslint-disable */
			method:"post",
			url:`${env("API_HOST")}/users`,
			data: {
				"firstName":user.firstName,
				"lastName":user.lastName,
				"emailAddress":user.emailAddress
			},
			headers: {
				"content-type": "application/json",
				/* eslint-enable */
			},
		}
		const response = await axios(request)
		return response.data
	} catch (e) {}
}
```
[more details about axios](https://blog.logrocket.com/how-to-make-http-requests-like-a-pro-with-axios/)

3) Create a **_sagas.js_** file.
an example to manage the request to add a user that created in **_api.js_**
```
import { put, takeEvery } from "redux-saga/effects"

import {addUser,REQUEST_ADD_USER,} from "./actions"
import {sendRequestAdduser,} from "./api"

export function* addUserManagement(action) {
	try {
		const user = yield sendRequestAdduser(action.payload)
		yield put(addUser(user))
	} catch (e) {}
}

export default function* UsersSaga() {
	yield takeEvery(REQUEST_ADD_USER, addUserManagement)
}
```

### put
Creates an Effect description that instructs the middleware to schedule the dispatching of an action to the store.
This dispatch may not be immediate since other tasks might lie ahead in the saga task queue or still be in progress.
**_yield Put_** perform a dispatch to store.

### takeEvery(pattern, saga, ...args)
Spawns a saga on each action dispatched to the Store that matches pattern.
```	
yield takeEvery(REQUEST_ADD_USER, addUserManagement)
```
If you dispatch REQUEST_ADD_USER, redux saga will call addUserManagement.

### generator function or ( function* )
Generator functions get a parameter named action that whenever you perform a dispatch to store, it receives the action from it.

4) To active redux-saga on project, put below codes in **_store.js_** file.
```
import { createStore, applyMiddleware } from "redux"
import createSagaMiddleware from "redux-saga"

import UsersSaga from "./Sagas"
import { DELETE_USER, ADD_USER } from "./actions"

const sagaMiddleWare = createSagaMiddleware()

export const store = createStore(
	(state, { type, payload }) => {
		switch (type) {
			case ADD_USER:
				if (!state) return (state = [payload])
				return (state = [...state, payload])
			case DELETE_USER:
				const stateAfterUserRemoval = state.filter(
					(user) => user.id !== payload
				)
				if (stateAfterUserRemoval.length === 0) return (state = null)
				return stateAfterUserRemoval
			default:
				return state
		}
	},
	null,
	applyMiddleware(sagaMiddleWare)
)

sagaMiddleWare.run(UsersSaga)

export default store
```   
 
## Results
Finally, The **_UsersSaga_** generator function works in such a way that when you make a dispatch to the store,
**_Redux-Saga_** notices via **_takeEvery_** function and calls the **_addUserManagement_** generator function.
the **_addUserManagement_** generator function also calls the **_sendRequestAdduser_** function, so that the request 
sent to the backend section.
The **_sendRequestAdduser_** function then takes the response from backend (API gateway) and return it to the
**_addUserManagement_** generator function. The **_addUserManagement_** Generator function enters the response(user)
into the **_store_** via **_put(dispatch)_**.
