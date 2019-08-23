generator 문법부터 알아야함

```jsx
function *test(){
	console.log(1);
	yield '2';
	console.log(3);
	yield '4';
}

const b = test()
b.next()
b.next()
b.next()

const c = next()
c.next().value
c.next().value
c
c.next().value
c
```

시작



```jsx
// index.js

// 이렇게 사가를 적용. 나중에 run을 해줘야함

import { Provider } from 'react-redux';
import createSagaMiddleware from 'redux-saga';

const sagaMiddleware = createSagaMiddleware();

const store = createStore(rootReducer, applyMiddleware(sagaMiddleware));

ReactDOM.render(
	<Provider store={store}>
		<App />
	</Provider>,
	document.getElementById('root')
);

serviceWorker.unregister();

```

```jsx
// post.js

import axios from 'axios';
import { call, put, takeEvery } from 'redux-saga/effects';

...

// 이걸 추가
const CALL_NEWS = 'post/CALL_NEWS';
const PENDING = 'post/PENDING';
const RESOLVED = 'post/RESOLVED';
const REJECTED = 'post/REJECTED';

// 마찬가지로 액션생성 함수추가
export const callNews = createAction(CALL_NEWS, category => category);
const pending = createAction(PENDING);
const resolved = createAction(RESOLVED, data => data);
const rejected = createAction(REJECTED, error => error);

// 추가
function* callNewsSaga(action) {
	yield put(pending());

	try {
		const response = yield call(callNewsAPI, action.payload);
		yield put(resolved(response));
	} catch (e) {
		yield put(rejected(e));
	}
}

// 그리고 쪼그마난 사가들을 한번에 묶어줌
export function* newsSaga() {
	yield takeEvery(CALL_NEWS, callNewsSaga);
}

const initialState = {
	pending: false,
	data: null,
	error: null
};

export default handleActions(
	{
		[PENDING]: (state, action) => ({
			...state,
			pending: true,
			error: false
		}),
		[RESOLVED]: (state, action) => ({
			...state,
			data: action.payload,
			pending: false
		}),
		[REJECTED]: (state, action) => ({
			...state,
			pending: false,
			error: action.payload
		})
	},
	initialState
);

```

```jsx
//modules/index.js


import { combineReducers } from 'redux';
// all이랑 subsaga가져와서 rootsaga로 묶어서 내보냄
import { all } from 'redux-saga/effects';
import { newsSaga } from './post';
import form from './form';
import post from './post';

export function* rootSaga() {
	yield all([newsSaga()]);
}

export default combineReducers({
	form,
	post
});

```

```jsx
// index.js


// rootsaga불러서 run.
import rootReducer, { rootSaga } from './store/modules';

const sagaMiddleware = createSagaMiddleware();

const store = createStore(rootReducer, applyMiddleware(sagaMiddleware));

sagaMiddleware.run(rootSaga);
```

```jsx
// App.js

	handleSubmit = e => {
		e.preventDefault();

		const { PostActions, FormActions, input } = this.props;

               //이거만 바꿔준다 
		PostActions.callNews(input);
		FormActions.changeInput('');
	};
```



