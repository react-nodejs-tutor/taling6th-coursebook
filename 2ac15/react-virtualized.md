```jsx
// react-virtualized Todolist.js

import React, { Component } from 'react';
import TodoItem from './TodoItem';
import './TodoList.css';
import { List } from 'react-virtualized';

class TodoList extends Component {
    renderRow = ({ index, key, style, parent }) => {
        const { todos, onToggle, onRemove } = this.props;
        const todo = todos[index];
        return (
            <TodoItem
                id={todo.id}
                checked={todo.checked}
                text={todo.text}
                todo={todo}
                onToggle={onToggle}
                onRemove={onRemove}
                key={key}
                style={style}
            />
        );
    };

    render() {
        const { todos } = this.props;

        return (
            <div className="TodoList">
                <List
                    width={600}
                    height={364}
                    rowCount={todos.length}
                    rowHeight={62}
                    rowRenderer={this.renderRow}
                    list={todos}
                    style={{ outline: 'none' }}
                />
            </div>
        );
    }
}

export default TodoList;
```

```jsx
// react-virtualized Todoitem.js

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
        const { todo, onToggle, onRemove, style } = this.props;

        return (
            <div style={style}>
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
            </div>
        );
    }
}

export default TodoItem;
```



