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
* 만약 email이 바뀔때만 console.log를 찍고, clean up은 언마운트때만 하고싶으면?





