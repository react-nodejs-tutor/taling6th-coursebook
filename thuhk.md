input에 카테고리를 입력하면 newsapi에서 해당 카테고리에 맞는 기사를 뽑아옴

post.js를 작성.

```jsx
// store/modules/post.js

import { createAction, handleActions } from 'redux-actions';
import axios from 'axios';

const PENDING = 'post/PENDING';
const RESOLVED = 'post/RESOLVED';
const REJECTED = 'post/REJECTED';

const pending = createAction(PENDING);
const resolved = createAction(RESOLVED, data => data);
const rejected = createAction(REJECTED, error => error);

//////////////////////// callNewsAPI, getNews를 나중에 ///////////////////
const callNewsAPI = category => {
    return axios.get(
        `https://newsapi.org/v2/top-headlines?country=kr&category=${category}&apiKey=61120652e560407b8e961993f5ebaa8f`
    );
};


// return, throw처음에 빼줘도됨
export const getNews = category => dispatch => {
    dispatch(pending());

    return callNewsAPI(category)
        .then(data => {
            dispatch(resolved(data));
        })
        .catch(error => {
            dispatch(rejected(error));
            throw error;
        });
};
//////////////////////// 나중에 ///////////////////


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

그리고 combine reducers에서 합쳐준다.

그리고 App.js작성

```jsx
// App.js

import React, { Component } from 'react';
import { connect } from 'react-redux';
import * as formActions from './store/modules/form';
//1번 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
import * as postActions from './store/modules/post';

import { bindActionCreators } from 'redux';

class App extends Component {
    handleChange = e => {
        const { value } = e.target;
        this.props.FormActions.changeInput(value);
    };

        /////////// 3번!!!!!!!!!!!!!!!!!!!!!!!!!
    handleSubmit = e => {
        e.preventDefault();

        const { PostActions, FormActions, input } = this.props;

        PostActions.getNews(input);
        FormActions.changeInput('');
    };

    render() {
    //4번 !!!!!!!!!!!!!!!!!!!!!!!
        const { input, pending, data, error } = this.props;

        return (
            <div>
                <form onSubmit={this.handleSubmit}>
                    <input value={input} onChange={this.handleChange} />
                    <button>입력</button>
                </form>
                //5번 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
                {pending && <div>loading...</div>}
                {data && (
                    <ul>
                        {data.data.articles.map(item => (
                            <li key={item.url}>
                                <h1>{item.title}</h1>
                                <p>{item.description}</p>
                            </li>
                        ))}
                    </ul>
                )}
                {error && <h1>{error}</h1>}
            </div>
        );
    }
}

//2번 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
export default connect(
    state => ({
        input: state.form.input,
        list: state.form.list,
        pending: state.post.pending,
        data: state.post.data,
        error: state.post.error
    }),
    dispatch => ({
        FormActions: bindActionCreators(formActions, dispatch),
        PostActions: bindActionCreators(postActions, dispatch)
    })
)(App);
```



