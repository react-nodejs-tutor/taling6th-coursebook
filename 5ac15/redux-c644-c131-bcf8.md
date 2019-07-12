**완성본 코드입니다.**

createStore함수로 스토어를 만들고 스토어의 인자로 루트리듀서, 그리고 리덕스 개발자도구를 넣습니다.

또한 Provider로 리액트 프로젝트에 리덕스를 적용합니다.

```jsx
// src/index.js

import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';
import { createStore } from 'redux';
import rootReducer from './store/modules';
import { Provider } from 'react-redux';

const devTools =
	window.__REDUX_DEVTOOLS_EXTENSION__ &&
	window.__REDUX_DEVTOOLS_EXTENSION__();
const store = createStore(rootReducer, devTools);

ReactDOM.render(
	<Provider store={store}>
		<App />
	</Provider>,
	document.getElementById('root')
);

serviceWorker.unregister();
```

counter모듈

```jsx
// src/store/modules/counter.js

const CHANGE_COLOR = 'counter/CHANGE_COLOR';
const INCREMENT = 'counter/INCREMENT';
const DECREMENT = 'counter/DECREMENT';

export const changeColor = color => ({ type: CHANGE_COLOR, color });
export const increment = () => ({ type: INCREMENT });
export const decrement = () => ({ type: DECREMENT });

const initialState = {
	color: 'red',
	number: 0
};

export default function counter(state = initialState, action) {
	switch (action.type) {
		case CHANGE_COLOR:
			return {
				...state,
				color: action.color
			};
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
		default:
			return state;
	}
}
```

```jsx
// src/store/modules/waiting.js
import { createAction, handleActions } from 'redux-actions';

const CHANGE_INPUT = 'waiting/CHANGE_INPUT';
const CREATE = 'waiting/CREATE';
const ENTER = 'waiting/ENTER';
const LEAVE = 'waiting/LEAVE';

let id = 1;
export const changeInput = createAction(CHANGE_INPUT, text => text);
export const create = createAction(CREATE, name => ({ name, id: id++ }));
export const enter = createAction(ENTER, id => id);
export const leave = createAction(LEAVE, id => id);

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

		[CREATE]: (state, action) => ({
			...state,
			list: state.list.concat({
				id: action.payload.id,
				name: action.payload.name,
				entered: false
			})
		}),

		[ENTER]: (state, action) => ({
			...state,
			list: state.list.map(item =>
				item.id === action.payload
					? { ...item, entered: !item.entered }
					: item
			)
		}),

		[LEAVE]: (state, action) => ({
			...state,
			list: state.list.filter(item => item.id !== action.payload)
		})
	},
	initialState
);
```

루트 리듀서에 집어넣습니다.

```jsx
// src/store/modules/index.js

import { combineReducers } from 'redux';
import counter from './counter';
import waiting from './waiting';

export default combineReducers({
	counter,
	waiting
});
```

그리고 컨테이너 컴포넌트, 프레젠테이셔널 컴포넌트를 만들고 연결해줍니다.

```jsx
// src/containers/PaletteContainer.js
import React, { Component } from 'react';
import { connect } from 'react-redux';
import Palette from '../components/Palette';
import { changeColor } from '../store/modules/counter';

class PaletteContainer extends Component {
	render() {
		const { color, changeColor } = this.props;

		return <Palette selected={color} onSelect={changeColor} />;
	}
}

const mapStateToProps = state => ({
	color: state.counter.color
});

const mapDispatchToProps = dispatch => ({
	changeColor: color => dispatch(changeColor(color))
});

export default connect(
	mapStateToProps,
	mapDispatchToProps
)(PaletteContainer);
```

```jsx
// src/components/Palette.js
import React from 'react';
import './Palette.css';

const colors = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet'];

const PaletteItem = ({ color, active, onClick }) => {
	return (
		<div
			className={`PaletteItem ${active ? 'active' : ''}`}
			style={{ backgroundColor: color }}
			onClick={onClick}
		/>
	);
};

const Palette = ({ selected, onSelect }) => {
	return (
		<div className="Palette">
			<h2>Color Palette</h2>
			<div className="colors">
				{colors.map(color => (
					<PaletteItem
						color={color}
						key={color}
						active={selected === color}
						onClick={() => onSelect(color)}
					/>
				))}
			</div>
		</div>
	);
};

export default Palette;
```

```jsx
// src/containers/CounterContainer.js

import React, { Component } from 'react';
import { connect } from 'react-redux';
import Counter from '../components/Counter';
import { increment, decrement } from '../store/modules/counter';

class CounterContainer extends Component {
	render() {
		const { color, number, increment, decrement } = this.props;

		return (
			<Counter
				value={number}
				color={color}
				onIncrement={increment}
				onDecrement={decrement}
			/>
		);
	}
}

const mapStateToProps = state => ({
	color: state.counter.color,
	number: state.counter.number
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

```jsx
// src/components/Counter.js
import React from 'react';
import './Counter.css';

const Counter = ({ value, color, onIncrement, onDecrement }) => {
	return (
		<div className="Counter">
			<h1 style={{ color }}>{value}</h1>
			<button onClick={onIncrement}>+</button>
			<button onClick={onDecrement}>-</button>
		</div>
	);
};

export default Counter;
```

```jsx
// src/containers/WaitingListContainer.js
import React, { Component } from 'react';
import WaitingList from '../components/WaitingList';
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';
import * as waitingActions from '../store/modules/waiting';

class WaitingListContainer extends Component {
	render() {
		const { input, list, waitingActions } = this.props;

		return (
			<WaitingList
				waitingList={list}
				input={input}
				waitingActions={waitingActions}
			/>
		);
	}
}

const mapStateToProps = state => ({
	input: state.waiting.input,
	list: state.waiting.list
});

const mapDispatchToProps = dispatch => ({
	waitingActions: bindActionCreators(waitingActions, dispatch)
});

export default connect(
	mapStateToProps,
	mapDispatchToProps
)(WaitingListContainer);
```

```jsx
// src/components/WaitingList.js
import React from 'react';
import './WaitingList.css';

const WaitingItem = ({ id, text, entered, onEnter, onLeave }) => {
	return (
		<li>
			<div className={`text ${entered ? 'entered' : ''}`}>{text}</div>
			<div className="buttons">
				<button onClick={() => onEnter(id)}>enter</button>
				<button onClick={() => onLeave(id)}>leave</button>
			</div>
		</li>
	);
};

const WaitingList = ({ input, waitingList, waitingActions }) => {
	return (
		<div className="WaitingList">
			<h2>WaitingList</h2>
			<form
				onSubmit={e => {
					e.preventDefault();
					waitingActions.create(input);
					waitingActions.changeInput('');
				}}
			>
				<input
					value={input}
					onChange={e => waitingActions.changeInput(e.target.value)}
				/>
				<button>go</button>
			</form>
			<ul>
				{waitingList.map(item => (
					<WaitingItem
						key={item.id}
						id={item.id}
						text={item.name}
						onEnter={waitingActions.enter}
						onLeave={waitingActions.leave}
					/>
				))}
			</ul>
		</div>
	);
};

export default WaitingList;
```

App.js에서는 반드시 컨테이너 컴포넌트를 보여주도록 합니다.

```jsx
// src/App.js
import React, { Component } from 'react';

import './App.css';
import PaletteContainer from './containers/PaletteContainer';
import CounterContainer from './containers/CounterContainer';
import WaitingListContainer from './containers/WaitingListContainer';

class App extends Component {
	render() {
		return (
			<div className="App">
				<PaletteContainer />
				<CounterContainer />
				<WaitingListContainer />
			</div>
		);
	}
}

export default App;

```



