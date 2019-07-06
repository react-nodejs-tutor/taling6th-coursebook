연습자료

```jsx
git clone https://github.com/react-nodejs-tutor/hoc-prac-2nd.git
```

```jsx
// src/withForm.js

import React, { Component } from 'react';

const withForm = (initialForm, reset) => WrappedComponent => {
    return class WithForm extends Component {
        state = initialForm || {};

        handleChange = e => {
            const { name, value } = e.target;

            this.state({
                [name]: value
            });
        };

        handleSubmit = e => {
            e.preventDefault();
            if (this.props.onSubmit) {
                this.props.onSubmit(this.state);
            }
            if (reset) {
                this.setState(initialForm);
            }
        };

        render() {
            return (
                <WrappedComponent
                    {...this.props}
                    form={this.state}
                    onChange={this.handleChange}
                    onSubmit={this.handleSubmit}
                />
            );
        }
    };
};

export default withForm;
```

```jsx
// src/LoginForm.js
import React, { Component } from 'react';
import withForm from './withForm';

class LoginForm extends Component {
    render() {
        const { username, password } = this.props.form;
        return (
            <form className="LoginForm" onSubmit={this.props.onSubmit}>
                <input
                    onChange={this.props.onChange}
                    value={username}
                    name="username"
                    placeholder="계정"
                />
                <input
                    onChange={this.props.onChange}
                    value={password}
                    name="password"
                    type="password"
                    placeholder="비밀번호"
                />
                <button>로그인</button>
            </form>
        );
    }
}

export default withForm({ username: '', password: '' }, true)(LoginForm);
```

```jsx
// src/BlogPostForm.js

import React, { Component } from 'react';
import withForm from './withForm';

class BlogPostForm extends Component {
    render() {
        const { title, content, tags } = this.props.form;
        return (
            <form className="BlogPostForm" onSubmit={this.props.onSubmit}>
                <input
                    value={title}
                    onChange={this.props.onChange}
                    name="title"
                    placeholder="제목"
                />
                <textarea
                    value={content}
                    onChange={this.props.onChange}
                    name="content"
                    placeholder="내용"
                />
                <input
                    value={tags}
                    onChange={this.props.onChange}
                    name="tags"
                    placeholder="태그"
                />
                <button>작성</button>
            </form>
        );
    }
}

export default withForm({ title: '', content: '', tags: '' }, true)(
    BlogPostForm
);
```



