```jsx
// index.js


// promise middleware documentation들어가서 복붙 그리고 suffix customize
// docs/guides/custom-suffixes.md

import ReduxThunk from 'redux-thunk';
import { createPromise } from 'redux-promise-middleware';

const store = createStore(
	rootReducer,
	applyMiddleware(
		ReduxThunk,
		createPromise({
			promiseTypeSuffixes: ['PENDING', 'RESOLVED', 'REJECTED']
		})
	)
);
```

그리고 docs/introduction.md들어가서 사용법을 설명함.

payload에 promise가 와야함을 강조



그리고 post.js 수정.

```jsx
// post.js

import { handleActions } from 'redux-actions';
import axios from 'axios';

//////////////// 추가!!!!!!
const NEWS = 'post/NEWS';
const PENDING = 'post/NEWS_PENDING';
const RESOLVED = 'post/NEWS_RESOLVED';
const REJECTED = 'post/NEWS_REJECTED';

const callNewsAPI = category => {
	return axios.get(
		`https://newsapi.org/v2/top-headlines?country=kr&category=${category}&apiKey=61120652e560407b8e961993f5ebaa8f`
	);
};

/// 액션생성함수랑 원래 getNews 다 날리고 이렇게 작성!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
export const getNews = category => ({
	type: NEWS,
	payload: callNewsAPI(category)
});

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

끝



