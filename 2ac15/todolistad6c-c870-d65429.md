```jsx
//App.js

import React, { Component } from 'react';
import './App.css';
import CreateForm from './CreateForm';
import TodoList from './TodoList';

class App extends Component {
	id = 1;

	state = {
		todos: []
	};

	handleSubmit = text => {
		const { todos } = this.state;
		this.setState({
			todos: todos.concat({
				id: this.id++,
				text,
				checked: false
			})
		});
	};

	handleToggle = id => {
		const { todos } = this.state;
		this.setState({
			todos: todos.map(todo => {
				if (todo.id === id) {
					return { ...todo, checked: !todo.checked };
				}
				return todo;
			})
		});
	};

	handleRemove = id => {
		const { todos } = this.state;
		this.setState({
			todos: todos.filter(todo => todo.id !== id)
		});
	};

	render() {
		const { todos } = this.state;

		return (
			<div className="App">
				<h3>TODO LIST</h3>
				<CreateForm onSubmit={this.handleSubmit} />
				<TodoList
					todos={todos}
					onToggle={this.handleToggle}
					onRemove={this.handleRemove}
				/>
			</div>
		);
	}
}

export default App;

```

```jsx
// App.css
.App {
	max-width: 640px;
	margin: 0 auto;
	overflow: hidden;
	font-size: 1.5rem;
}

.App h3 {
	margin: 30px 0 0 0;
	padding: 30px 0;
	background-color: plum;
	text-align: center;
	color: #eee;
}
```

```jsx
// CreateForm.js
import React, { Component } from 'react';
import './CreateForm.css';

class CreateForm extends Component {
	state = {
		input: ''
	};

	handleChange = e => {
		this.setState({
			input: e.target.value
		});
	};

	handleCreate = e => {
		e.preventDefault();
		this.props.onSubmit(this.state.input);
		this.setState({
			input: ''
		});
	};

	render() {
		return (
			<div className="CreateForm">
				<form className="form_container" onSubmit={this.handleCreate}>
					<input
						placeholder="something to do?"
						value={this.state.input}
						onChange={this.handleChange}
					/>
					<button type="submit">추가</button>
				</form>
			</div>
		);
	}
}

export default CreateForm;

```

```jsx
// CreateForm.css
.CreateForm {
	padding: 30px 0;
	background-color: #eee;
}

.form_container {
	width: 80%;
	margin: 0 auto;
	overflow: hidden;
}

.form_container input {
	width: 85%;
	font-size: 20px;
	height: 40px;
	float: left;
	border: 0;
	border-bottom: 1px solid #ddd;
	border-radius: 15px;
}

.form_container input:focus {
	outline: none;
}

.form_container button {
	border-radius: 20px;
	width: 13%;
	float: right;
	height: 45px;
	font-size: 15px;
}
```

```jsx
//TodoList.js
import React, { Component } from 'react';
import TodoItem from './TodoItem';
import './TodoList.css';

class TodoList extends Component {
	render() {
		const { todos, onToggle, onRemove } = this.props;

		return (
			<div className="TodoList">
				{todos.map(todo => (
					<TodoItem
						key={todo.id}
						id={todo.id}
						checked={todo.checked}
						text={todo.text}
						onToggle={onToggle}
						onRemove={onRemove}
						todo={todo}
					/>
				))}
			</div>
		);
	}
}

export default TodoList;

```

```jsx
//TodoList.css
.TodoList {
	background-color: #fff;
	padding: 50px 30px;
}

```

```jsx
//TodoItem.js
import React, { Component } from 'react';
import './TodoItem.css';

class TodoItem extends Component {
	shouldComponentUpdate(nextProps, nextState) {
		if (this.props.todo !== nextProps.todo) {
			return true;
		} else {
			return false;
		}
	}

	render() {
		const { todo, onToggle, onRemove } = this.props;

		return (
			<div
				className={`TodoItem ${todo.checked && 'active'}`}
				onClick={() => onToggle(todo.id)}
			>
				<div className="check">&#10004;</div>
				<div
					className="remove"
					onClick={e => {
						e.stopPropagation();
						onRemove(todo.id);
					}}
				>
					[지우기]
				</div>
				<div className="text">{todo.text}</div>
			</div>
		);
	}
}

export default TodoItem;

```

```jsx
//TodoItem.css
.TodoItem {
	margin-bottom: 30px;
	border-bottom: 1px dotted #bbb;
	padding-bottom: 10px;
	overflow: hidden;
	font-size: 1.4rem;
	cursor: pointer;
}

.TodoItem:hover .check,
.TodoItem:hover .remove {
	opacity: 0.5;
}

.TodoItem:last-child {
	margin-bottom: 0;
}

.TodoItem.active {
	text-decoration: line-through;
	color: #dda0dd;
}

.TodoItem.active .check {
	opacity: 0.8;
}

.TodoItem .check {
	float: left;
	opacity: 0;
}

.TodoItem .text {
	margin-left: 35px;
	overflow: hidden;
}

.TodoItem .remove {
	float: right;
	font-size: 1.2rem;
	opacity: 0;
}

```

```jsx
//index.css
h1,
h2,
h3,
h4,
h5,
h6,
body {
	margin: 0;
	padding: 0;
}

body {
	background-color: #dedede;
	font-size: 10px;
}

```

```jsx
//index.js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(<App />, document.getElementById('root'));

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();

```



