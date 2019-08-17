```jsx
// src/store/modules/counter.js

const INCREMENT = 'counter/INCREMENT';
const DECREMENT = 'counter/DECREMENT';
const CHANGE_COLOR = 'counter/CHANGE_COLOR';

export const increment = () => ({ type: INCREMENT });
export const decrement = () => ({ type: DECREMENT });
export const changeColor = color => ({ type: CHANGE_COLOR, color });

const initialState = {
	number: 0,
	color: '#bfcd7e'
};

export default function counter(state = initialState, action) {
	switch (action.type) {
		case INCREMENT:
			return {
				...state,
				number: state.number + 1
			};
		case DECREMENT:
			return {
				...state,
				number: state.number - 1
			};
		case CHANGE_COLOR:
			return {
				...state,
				color: action.color
			};
		default:
			return state;
	}
}

```

그리고 루트리듀서로 합쳐준다.

```jsx
import { combineReducers } from 'redux';
import counter from './counter';

export default combineReducers({
	counter
});

```

루트리듀서 만들었으면, 스토어만들고 그 안에 루트리듀서 넣고 값 확인해보기

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
// **** (1) createStore 와 루트 리듀서 불러오기
import { createStore } from 'redux';
import rootReducer from './store/modules';

import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';

// **** (2) 스토어를 만들고 현재 값 확인해보기
const store = createStore(rootReducer);
console.log(store.getState());

ReactDOM.render(<App />, document.getElementById('root'));
registerServiceWorker();
```

우리는 getState안씀. 리덕스 개발자도구있으므로 그걸 만들고 적용해보기.

그리고 Provider로 감싸는 것 까지.

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import rootReducer from './store/modules';

const devTools =
	window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__();

const store = createStore(rootReducer, devTools);

ReactDOM.render(
	<Provider store={store}>
		<App />
	</Provider>,
	document.getElementById('root')
);

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();
```

이제 우리 react 프로젝트가 스토어안에있는 값을 사용할 준비가 된 것

이제 connect를 이용해서 꺼내오면됨

containers를 만들어서 연결

먼저 CounterContainer부터

```jsx
// src/containers/CounterContainer.js

import React, { Component } from 'react';
import Counter from '../components/Counter';
import { connect } from 'react-redux';
import { increment, decrement } from '../store/modules/counter';

class CounterContainer extends Component {
	render() {
		const { increment, decrement, number } = this.props;

		return (
			<div>
				<Counter
					onIncrement={increment}
					onDecrement={decrement}
					number={number}
				/>
			</div>
		);
	}
}

const mapStateToProps = ({ counter }) => ({
	number: counter.number
});

const mapDispatchToProps = dispatch => ({
	increment: () => dispatch(increment()),
	decrement: () => dispatch(decrement())
});

export default connect(
	mapStateToProps,
	mapDispatchToProps
)(CounterContainer);
```

여기까지는 number증가하는거 감소하는거만 구현한 것.

그리고 프레젠테이셔널 컴포넌트에서 가져옴

```jsx
//src/components/Counter.js
import React, { Component } from 'react';
import './Counter.css';

class Counter extends Component {
	render() {
		const { onIncrement, onDecrement, number } = this.props;

		return (
			<div className="Counter">
				<h1>{number}</h1>
				<div className="btn-wrapper">
					<button onClick={onIncrement}>+</button>
					<button onClick={onDecrement}>-</button>
				</div>
			</div>
		);
	}
}

export default Counter;
```

그다음에 ColorSquareContainer구현

```jsx
// src/containers/ColorSquareContainer.js
import React, { Component } from 'react';
import ColorSquare from '../components/ColorSquare';
import { connect } from 'react-redux';
import { changeColor } from '../store/modules/counter';

class ColorSquareContainer extends Component {
	render() {
		const { number, color, changeColor } = this.props;

		return (
			<div>
				<ColorSquare selected={color} onSelect={changeColor} number={number} />
			</div>
		);
	}
}

const mapStateToProps = state => ({
	color: state.counter.color,
	number: state.counter.number
});

const mapDispatchToProps = dispatch => ({
	changeColor: color => dispatch(changeColor(color))
});

export default connect(
	mapStateToProps,
	mapDispatchToProps
)(ColorSquareContainer);
```

ColorSquare컴포넌트 구현

```jsx
// src/components/ColorSquare.js
import React, { Component } from 'react';
import './ColorSquare.css';

const colors = ['#bfcd7e', '#7E57C2', '#EA80FC', '#00BCD4'];

class Color extends Component {
	render() {
		const { color, active, onClick } = this.props;

		const style = {
			backgroundColor: color
		};

		return (
			<div
				className={`Color ${active ? 'active' : ''}`}
				style={style}
				onClick={onClick}
			/>
		);
	}
}

class ColorSquare extends Component {
	render() {
		const { selected, onSelect, number } = this.props;

		const style = {
			width: 200 + 10 * number,
			height: 200 + 10 * number
		};

		return (
			<div className="ColorSquare" style={style}>
				{colors.map(color => {
					return (
						<Color
							key={color}
							color={color}
							active={selected === color}
							onClick={() => onSelect(color)}
						/>
					);
				})}
			</div>
		);
	}
}

export default ColorSquare;
```

Counter도 color를연결해줘야함

```jsx
// src/components/Counter.js
import React, { Component } from 'react';
import './Counter.css';

class Counter extends Component {
	render() {
		const { onIncrement, onDecrement, number, color } = this.props;

		const style = {
			color
		};

		return (
			<div className="Counter">
				<h1 style={style}>{number}</h1>
				<div className="btn-wrapper">
					<button onClick={onIncrement}>+</button>
					<button onClick={onDecrement}>-</button>
				</div>
			</div>
		);
	}
}

export default Counter;
```

그다음에 리스트

```jsx
//src/store/modules/colorList.js
import { createAction, handleActions } from 'redux-actions';

const CHANGE_INPUT = 'colorList/CHANGE_INPUT';
const INSERT = 'colorList/INSERT';
const UPDATE = 'colorList/UPDATE';
const REMOVE = 'colorList/REMOVE';

let id = 1;

export const changeInput = createAction(CHANGE_INPUT, color => color);
export const insert = createAction(INSERT, color => ({ color, id: id++ }));
export const update = createAction(UPDATE, id => id);
export const remove = createAction(REMOVE, id => id);

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
				color: action.payload.color
			})
		}),
		[UPDATE]: (state, action) => ({
			...state,
			list: state.list.map(color => {
				if (color.id === action.payload) {
					return {
						...color,
						color: 'black'
					};
				} else return color;
			})
		}),
		[REMOVE]: (state, action) => ({
			...state,
			list: state.list.filter(color => color.id !== action.payload)
		})
	},
	initialState
);

```

그다음은 루트리듀서 합쳐주기, 그다음 컨테이너로 연결

```jsx
//src/containers/ColorListContainer.js

import React, { Component } from 'react';
import ColorList from '../components/ColorList';
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';
import * as colorListActions from '../store/modules/colorList';
import * as counterActions from '../store/modules/counter';

class ColorListContainer extends Component {
	render() {
		const { input, list, ColorListActions, CounterActions } = this.props;

		return (
			<div>
				<ColorList
					input={input}
					list={list}
					ColorListActions={ColorListActions}
					CounterActions={CounterActions}
				/>
			</div>
		);
	}
}

const mapStateToProps = ({ colorList: { input, list } }) => ({
	input,
	list
});

const mapDispatchToProps = dispatch => ({
	ColorListActions: bindActionCreators(colorListActions, dispatch),
	CounterActions: bindActionCreators(counterActions, dispatch)
});

export default connect(
	mapStateToProps,
	mapDispatchToProps
)(ColorListContainer);

```

그리고 프레젠테이셔널 구현

```jsx
import React, { Component } from 'react';
import './ColorList.css';

class ColorList extends Component {
	handleChange = e => {
		const { ColorListActions } = this.props;
		ColorListActions.changeInput(e.target.value);
	};

	handleSubmit = e => {
		e.preventDefault();
		const { CounterActions, ColorListActions, input } = this.props;
		ColorListActions.insert(input);
		CounterActions.changeColor(input);
		ColorListActions.changeInput('');
	};

	handleUpdate = id => {
		const { ColorListActions } = this.props;
		ColorListActions.update(id);
	};

	handleRemove = id => {
		const { ColorListActions } = this.props;
		ColorListActions.remove(id);
	};

	render() {
		const { input, list } = this.props;
		return (
			<div>
				<form className="ColorList" onSubmit={this.handleSubmit}>
					<input
						placeholder="원하는 색을 입력하세요"
						value={input}
						onChange={this.handleChange}
					/>
				</form>
				<ul>
					{list.map(color => {
						return (
							<div
								key={color.id}
								style={{
									backgroundColor: color.color,
									width: '50px',
									height: '50px',
									float: 'left'
								}}
								onClick={() => this.handleUpdate(color.id)}
								onContextMenu={e => {
									e.preventDefault();
									this.handleRemove(color.id);
								}}
							/>
						);
					})}
				</ul>
			</div>
		);
	}
}

export default ColorList;

```



