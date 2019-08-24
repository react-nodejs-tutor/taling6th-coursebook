```jsx

```





불변성 지켜줘야함

```jsx
import React, { useState } from 'react';

const App = () => {
	const [{ count, count2 }, setCount] = useState({ count: 0, count2: 0 });

	return (
		<div>
			<h1>{count}</h1>
			<h1>{count2}</h1>
			<button
				onClick={() =>
					setCount(currentState => ({
						...currentState,
						count: currentState.count + 1
					}))
				}
			>
				increase
			</button>
		</div>
	);
};

export default App;

```

두개?

```jsx
import React, { useState } from 'react';

const App = () => {
	const [username, setUsername] = useState('');
	const [password, setPassword] = useState('');
	const [email, setEmail] = useState('');

	return (
		<div>
			<input
				name="username"
				value={username}
				onChange={e => setUsername(e.target.value)}
			/>
			<input
				name="password"
				value={password}
				onChange={e => setPassword(e.target.value)}
			/>
			<input
				name="email"
				value={email}
				onChange={e => setEmail(e.target.value)}
			/>
		</div>
	);
};

export default App;
```

불필요한 중복 제거? =&gt; 커스텀훅.

```jsx
// useForm.js
import { useState } from 'react';

const useForm = initialForm => {
	const [values, setValues] = useState(initialForm);

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

export default useForm;

```

```jsx
// App.js
import React from 'react';
import useForm from './useForm';

const App = () => {
	const [values, handleChange] = useForm({
		username: '',
		password: '',
		email: ''
	});

	return (
		<div>
			<input name="username" value={values.username} onChange={handleChange} />
			<input name="password" value={values.password} onChange={handleChange} />
			<input name="email" value={values.email} onChange={handleChange} />
		</div>
	);
};

export default App;

```

useEffect?









