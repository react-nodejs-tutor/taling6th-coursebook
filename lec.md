```jsx
// src/App
import React, { useState } from 'react';

const App = () => {
    const [{ count1, count2 }, setCount] = useState({ count1: 0, count2: 0 });

    return (
        <div>
            <h1>{count1}</h1>
            <h1>{count2}</h1>
            <button
                onClick={() =>
                    setCount(currentState => ({
                        ...currentState,
                        count1: currentState.count1 + 1
                    }))
                }
            >
                handle
            </button>
        </div>
    );
};

export default App;
```

이렇게하는거나

```jsx
카운트 두개써서 하는거나 똑같다는걸 보여줌
```

이번에는 form

```jsx
import React, { useState } from 'react';

const App = () => {
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');
    const [username, setUsername] = useState('');

    return (
        <div>
            <input value={email} onChange={e => setEmail(e.target.value)} />
            <input value={password} onChange={e => setPassword(e.target.value)} />
            <input value={username} onChange={e => setUsername(e.target.value)} />
        </div>
    );
};

export default App;


위와 같은 코드

import React, { useState } from 'react';

const App = () => {
    const [values, setValues] = useState({
        email: '',
        password: '',
        username: ''
    });

    return (
        <div>
            <input
                value={values.email}
                onChange={e =>
                    setValues({
                        ...values,
                        email: e.target.value
                    })
                }
            />
            <input
                value={values.password}
                onChange={e =>
                    setValues({
                        ...values,
                        password: e.target.value
                    })
                }
            />
            <input
                value={values.username}
                onChange={e =>
                    setValues({
                        ...values,
                        username: e.target.value
                    })
                }
            />
        </div>
    );
};

export default App;
```

useForm을 사용하면?

```jsx
// useForm

import { useState } from 'react';

export const useForm = initialValue => {
    const [values, setValues] = useState(initialValue);

    return [
        values,
        e => {
            setValues({
                ...values,
                [e.target.name]: e.target.value
            });
        }
    ];
};
```

```jsx
import React from 'react';
import { useForm } from './UseForm';

const App = () => {
    const [values, handleChange] = useForm({
        email: '',
        password: '',
        username: ''
    });

    return (
        <div>
            <input value={values.username} onChange={handleChange} name="username" />
            <input value={values.email} onChange={handleChange} name="email" />
            <input
                value={values.password}
                onChange={handleChange}
                name="password"
                type="password"
            />
        </div>
    );
};

export default App;
```

useEffect

```jsx
// Root.js

import React, { useState } from 'react';
import App from './App';

const Root = () => {
    const [visible, setVisible] = useState(true);

    return (
        <div>
            {visible && <App />}
            <button onClick={() => setVisible(!visible)}>
                {visible ? 'hide' : 'show'}
            </button>
        </div>
    );
};

export default Root;
```

```jsx
import React, { useEffect } from 'react';
import { useForm } from './UseForm';

const App = () => {
    const [values, handleChange] = useForm({
        email: '',
        password: '',
        username: ''
    });

    useEffect(() => {
        console.log('render');
        return () => {
            console.log('unmount');
        };
    })

    return (
        <div>
            <input value={values.email} onChange={handleChange} name="email" />
            <input value={values.username} onChange={handleChange} name="username" />
            <input
                value={values.password}
                onChange={handleChange}
                name="password"
                type="password"
            />
        </div>
    );
};

export default App;
```

그다음에 clean up이 update때도 된다는걸 보여줄것

* 만약 email이 바뀔때만 console.log를 찍고, clean up은 언마운트때만 하고싶으면?
* 만약 마운트때는 실행안하고 업데이트때만 실행하고싶으면?

useEffect예시

```jsx
import React, { useEffect } from 'react';
import { useForm } from './UseForm';

const App = () => {
	const [values, setValues] = useForm({
		email: '',
		password: '',
		username: ''
	});

	useEffect(() => {
		const onMouseMove = e => {
			console.log(e);
		};

		window.addEventListener('mousemove', onMouseMove);

		return () => {
			window.removeEventListener('mousemove', onMouseMove);
		};
	}, []);

	return (
		<div>
			<input name="email" value={values.email} onChange={setValues} />
			<input name="password" value={values.password} onChange={setValues} />
			<input name="username" value={values.username} onChange={setValues} />
		</div>
	);
};

export default App;
```

useEffect로 axios사용해보기 useAxios

```jsx
// useAxios.js

import { useState, useEffect } from 'react';
import axios from 'axios';

export const useAxios = url => {
	const [state, setState] = useState({ data: null, loading: false });

	useEffect(() => {
		setState(currentState => ({
			...currentState,
			loading: true
		}));

		axios.get(url).then(response => {
			setState(currentState => ({
				...currentState,
				data: response.data.body
			}));

			setState(currentState => ({
				...currentState,
				loading: false
			}));
		});
	}, [url]);

	return state;
};
```

그리고 console.log찍어보기

```jsx
// src/App.js

import React, { useState } from 'react';
import { useAxios } from './useAxios';

const App = () => {
	const [count, setCount] = useState(1);
	const { data, loading } = useAxios(
		`https://jsonplaceholder.typicode.com/posts/${count}`
	);
	console.log(loading);

	return (
		<div>
			<div>{loading ? 'loading..' : data}</div>
			<hr />
			<button onClick={() => setCount(count + 1)}>click</button>
		</div>
	);
};

export default App;

```

localStorage이용해서 state저장하기

```jsx
// src/App.js

import React, { useState, useEffect } from 'react';
import { useAxios } from './useAxios';

const App = () => {
	const [count, setCount] = useState(() => localStorage.getItem('count') || 1);
	const { data, loading } = useAxios(
		`https://jsonplaceholder.typicode.com/posts/${count}`
	);

	useEffect(() => {
		localStorage.setItem('count', count);
	}, [count]);

	return (
		<div>
			<div>{loading ? 'loading..' : data}</div>
			<p>count: {count}</p>
			<button onClick={() => setCount(count + 1)}>click</button>
			<hr />
		</div>
	);
};

export default App;
```

그리고 useRef까지

```jsx
import React, { useState, useEffect, useRef } from 'react';
import { useAxios } from './useAxios';
import { useForm } from './useForm';

const App = () => {
	const [values, setValues] = useForm({
		email: '',
		username: '',
		password: ''
	});

	const [count, setCount] = useState(() =>
		JSON.parse(localStorage.getItem('count') || 1)
	);

	const { data, loading } = useAxios(
		`https://jsonplaceholder.typicode.com/posts/${count}`
	);

	const inputRef = useRef(null);
	const isInitialMount = useRef(true);

	useEffect(() => {
		localStorage.setItem('count', JSON.stringify(count));
	}, [count]);

	useEffect(() => {
		if (isInitialMount.current) {
			isInitialMount.current = false;
		} else {
			console.log('hello');
		}
	});

	return (
		<div>
			<input
				ref={inputRef}
				value={values.email}
				onChange={setValues}
				name="email"
			/>
			<input value={values.username} onChange={setValues} name="username" />
			<button onClick={() => inputRef.current.focus()}>focus</button>
			<hr />
			<div>{loading ? 'loading..' : data}</div>
			<p>count: {count}</p>
			<button onClick={() => setCount(count + 1)}>click</button>
			<hr />
		</div>
	);
};

export default App;

```

useMemo

```jsx
import React, { useRef, useMemo } from 'react';
import { useForm } from './useForm';
import { useAxios } from './useAxios';

// 함수새로추가
function compute(str) {
	if (!str) return 0;
	console.log('computing...');
	const arr = str.split(' ');
	let longestSize = 0;
	arr.forEach(word => {
		if (longestSize < word.length) {
			longestSize = word.length;
		}
	});
	return longestSize;
}

const App = () => {
	const [values, setValues] = useForm({ username: '', password: '' });
	const usernameRef = useRef();

	const { data } = useAxios('https://jsonplaceholder.typicode.com/posts/1');

       // 이거추가
	const getLongest = useMemo(() => compute(data), [data]);

	return (
		<div>
		// 여기추가
			<ul>
				{data &&
					data.split(' ').map((item, index) => {
						return <li key={index}>{item}</li>;
					})}
			</ul>
			<p>{getLongest}</p>
			<hr />
			<input
				ref={usernameRef}
				value={values.username}
				onChange={setValues}
				name="username"
			/>
			<input value={values.password} onChange={setValues} name="password" />
			<button
				onClick={() => {
					usernameRef.current.focus();
				}}
			>
				focus
			</button>
		</div>
	);
};

export default App;

```



