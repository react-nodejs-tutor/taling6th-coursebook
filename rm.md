```jsx
// clone하면 input입력하면 list에 추가되는거까지 만들어져있음.
// 여기서부터 시작.

//src/middleware.js

const middleware = store => next => action => {
    console.log(store.getState());

    console.log(action);

    next(action);

    console.log(store.getState());
};

export default middleware;
```

```jsx
// index.js

import { createStore, applyMiddleware } from 'redux'
// 미들웨어 가져와서 applymiddleware에 집어넣음
import middleware from './middleware';

const store = createStore(rootReducer, applyMiddleware(middleware));

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root')
);

serviceWorker.unregister();
```

```jsx
// 여기로 돌아와서 설명

// middleware 가 이렇게 생겼기 때문에, thunk에서 dispatch, getState를 비구조화할당해서 받아올 수 있는것임

const middleware = store => next => action => {
    console.log(store.getState());

    console.log(action);

// 곧 next를 실행하면 reducer가 실행되는것..

// 그런데 reducer는 순수해야함
// 비동기작업을 처리하고싶으면??
// action을 next를 하지않음으로써 reducer로 안넘겨주면된다

    next(action);

    console.log(store.getState());
};

export default middleware;
```

thunk를 이용해서 원래 있던 insert를 비동기로 바꿔보자.

insertAsnyc는 호출하면 함수랑 리턴하는 함수이다.

즉, 액션을 함수로 만들어보자.

```jsx
// form.js

import { createAction, handleActions } from 'redux-actions';

const CHANGE_INPUT = 'form/CHANGE_INPUT';
const INSERT = 'form/INSERT';

let id = 1;
export const changeInput = createAction(CHANGE_INPUT, text => text);
export const insert = createAction(INSERT, text => ({ text, id: id++ }));

// 여기!!!!!!!!!!!!!!!!!!!!!!!!!!
export const insertAsync = input => dispatch => {
    setTimeout(() => {
        dispatch(insert(input));
    }, 1000);
};

const initialState = {
    input: '',
    list: []
};

export default handleActions(
    {
        [CHANGE_INPUT]: (state, action) => ({
            ...state,
            input: action.payload
        }),
        [INSERT]: (state, action) => ({
            ...state,
            list: state.list.concat({
                id: action.payload.id,
                text: action.payload.text
            })
        })
    },
    initialState
);
```

```jsx
// index.js

import ReduxThunk from 'redux-thunk';

const store = createStore(rootReducer, applyMiddleware(middleware, ReduxThunk));
```

```jsx
// App.js

// 먼저 bindActionCreators형식으로 바꾼다.
// 그다음에 FormActions.insertAsync로 치환

import React, { Component } from 'react';
import { connect } from 'react-redux';
import * as formActions from './store/modules/form';
import { bindActionCreators } from 'redux';

class App extends Component {
    handleChange = e => {
        const { value } = e.target;
        this.props.FormActions.changeInput(value);
    };


// 여기!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    handleSubmit = e => {
        e.preventDefault();

        const { FormActions, input } = this.props;

        FormActions.insertAsync(input);
        FormActions.changeInput('');
    };

    render() {
        const { input, list } = this.props;

        return (
            <div>
                <form action="">
                    <input value={input} onChange={this.handleChange} />
                    <button onClick={this.handleSubmit}>입력</button>
                </form>
                {list.map(item => {
                    return <div key={item.id}>{item.text}</div>;
                })}
            </div>
        );
    }
}

export default connect(
    ({ form: { input, list } }) => ({
        input,
        list
    }),
    dispatch => ({
        FormActions: bindActionCreators(formActions, dispatch)
    })
)(App);
```

그다음은 실사용측면에서의 axios 다음 chapter

