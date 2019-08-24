module부터작성

```jsx
// store/modules/colorList.js

import { createAction, handleActions } from 'redux-actions';

const CHANGE_INPUT = 'colorList/CHANGE_INPUT';
const INSERT = 'colorList/INSERT';
const UPDATE = 'colorList/UPDATE';
const REMOVE = 'colorList/REMOVE';

let id = 1;
export const changeInput = createAction(CHANGE_INPUT, text => text);
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
                color: action.payload.color,
                opacity: 1
            })
        }),
        [UPDATE]: (state, action) => ({
            ...state,
            list: state.list.map(color => {
                if (color.id === action.payload) {
                    return { ...color, opacity: color.opacity - 0.1 };
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

root Reducer통합

```jsx
// store/modules/index.js

import { combineReducers } from 'redux';
import counter from './counter';
import colorList from './colorList';

export default combineReducers({
    counter,
    colorList
});
```

```jsx
// containers/ColorListContainer.js

// counterActions도 불러와야한다는거 잊지말기
// 색을 추가할때, counter숫자도 색 변경해야하므로

import React, { Component } from 'react';
import ColorList from '../components/ColorList';
import { connect } from 'react-redux';
import * as colorListActions from '../store/modules/colorList';
import * as counterActions from '../store/modules/counter';
import { bindActionCreators } from 'redux';

class ColorListContainer extends Component {
    render() {
        const { input, list, ColorListActions, CounterActions } = this.props;

        return (
            <ColorList
                input={input}
                list={list}
                ColorListActions={ColorListActions}
                CounterActions={CounterActions}
            />
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

```jsx
// components/ColorList.js

import React, { Component } from 'react';
import ListItem from './ListItem';
import './ColorList.css';

class ColorList extends Component {
    handleChange = e => {
        const { value } = e.target;
        const { ColorListActions } = this.props;
        ColorListActions.changeInput(value);
    };

    handleSubmit = e => {
        e.preventDefault();
        const { input, ColorListActions, CounterActions } = this.props;
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
        const { list, input } = this.props;

        return (
            <div>
                <form className="ColorList" onSubmit={this.handleSubmit}>
                    <input
                        placeholder="원하는 색을 입력하세요"
                        value={input}
                        onChange={this.handleChange}
                    />
                </form>
                {list.map(item => {
                    return (
                        <ListItem
                            key={item.id}
                            item={item}
                            style={{
                                backgroundColor: item.color,
                                opacity: item.opacity,
                                width: '50px',
                                height: '50px',
                                float: 'left'
                            }}
                            onUpdate={this.handleUpdate}
                            onRemove={this.handleRemove}
                        />
                    );
                })}
            </div>
        );
    }
}

export default ColorList;
```

ListItem은 다음처럼 생김

```jsx
// ListItem.js

import React, { Component } from 'react';

class ListItem extends Component {
	shouldComponentUpdate(nextProps, nextState) {
		return this.props.item !== nextProps.item;
	}

	render() {
		const { style, onUpdate, onRemove, item } = this.props;

		return (
			<div
				style={style}
				onClick={() => onUpdate(item.id)}
				onContextMenu={e => {
					e.preventDefault();
					onRemove(item.id);
				}}
			/>
		);
	}
}

export default ListItem;

```



마지막App에서 컨테이너렌더

