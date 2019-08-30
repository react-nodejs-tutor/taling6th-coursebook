## Hooks

#### **1. useState**

가장 기본적인 코드부터 알아볼게요.

useState를 이용해 state로 count를 선언하고 해당 state값을 관리하는 코드입니다.

```jsx
// src/App.js

import React, {useState} from 'react';

const App = () => {

  const [count, setCount] = useState(0);

  return (
    <div>
      <h1>{count}</h1> 
      <button onClick={() => setCount(count + 1)}>클릭</button>
    </div>
  );
};

export default App;
```

위 코드를 클래스 컴포넌트로 구현하면 다음과 같습니다.

```jsx
// src/App.js

import React, { Component } from 'react';

class App extends Component {

  state = {
    count: 0
  }

  handleClick = () => {
    this.setState({
      count: this.state.count + 1
    })
  }

  render() {
    return (
      <div>
        <h1>{this.state.count}</h1>
        <button onClick={this.handleClick}>클릭</button> 
      </div>
    );
  }
}

export default App;
```

즉, useState의 파라미터인 0은 우리가 관리할 state인 count입니다.

useState가 반환하는 값을 배열 비구조화할당으로 받아왔는데요,

두번째 비구조화할당 값으로 받아온 setCount는 count 상태값을 관리하는 함수입니다.

이번에는 useState로 두가지 상태값을 관리해볼까요?

그냥 useState를 두 번 쓰면 됩니다.

```jsx
// src/App.js

import React, {useState} from 'react';

const App = () => {
  
  const [count, setCount] = useState(0);
  const [count2, setCount2] = useState(0);

  return (
    <div>
      <h1>{count}</h1>  
      <h1>{count2}</h1>  
      <button onClick={() => setCount(count + 1)}>클릭</button>
      <button onClick={() => setCount2(count2 + 1)}>클릭2</button>
    </div>
  );
};

export default App;
```

쉽죠?

이번에는 useState를 한번 써서 해볼텐데요, 우선 코드를 확인해보세요

```jsx
// src/App.js

import React, { useState } from 'react';

const App = () => {
	const [values, setValues] = useState({ count: 0, count2: 0 });

	return (
		<div>
			<h1>{values.count}</h1>
			<h1>{values.count2}</h1>
			<button
				onClick={() =>
					setValues(currentValues => ({
						...values,
						count: currentValues.count + 1
					}))
				}
			>
				클릭
			</button>
		</div>
	);
};

export default App;
```

setState를 사용해서 state를 관리할때 setState안에 바로 객체를 집어넣기도 했지만 함수를 집어넣어서 관리했던 것 기억나시나요?

useState훅 안에도 마찬가지로 함수가 들어갈 수 있습니다.

다만 setState와 다른점은 불변성을 직접 유지해줘야 한다는 것입니다. 리덕스처럼요.

따라서 ...를 통해 shallow copy를 꼭 해주어야 합니다. 그렇지 않으면 제대로 작동하지 않아요. 

...으로 복사를 해주지않으면 기존 count, count2였던 state가 count로 바뀌게됩니다. count2는 사라지게 되는 것이죠.

이해가 잘 되지 않는다면 코드를 바꿔가면서 실험해보세요.

#### 2**. useEffect**

개인적으로 useEffect가 정말 엄청나다고 생각합니다.

우리가 라이프사이클 메소드를 배울때 render시점 update시점 unmount시점 이렇게 세 가지 시점으로 나누어서 배웠던 것 기억나시나요?

**render시점에서의 componentDidMount\(cdm\)**

**update시점에서의 componentDidUpdate\(cdu\)**

**unmount시점에서의 componentDidUnmount\(cdu\)**

이렇게 각 시점에 맞는 대표적인 라이프사이클 메소드가 있었는데요,

useEffect를 사용하면 이 세가지를 한번에 사용할 수 있습니다.

시작해볼까요?

```jsx
// src/App.js



```



**advanced..**

문제점이 뭘까?

```jsx
import React, { useState } from 'react';

const App = () => {
	const [count, setCount] = useState(0);

	const handleClick = () => {
		setTimeout(() => {
			alert(`${count}번 클릭했습니다`);
		}, 2000);
	};

	return (
		<div>
			<h1>{count}</h1>
			<button onClick={() => setCount(count + 1)}>클릭</button>
			<button onClick={handleClick}>alert</button>
		</div>
	);
};

export default App;

```

useRef

```jsx
import React, { useState, useRef } from 'react';

const App = () => {
	const [count, setCount] = useState(0);

	const countRef = useRef(count);
	countRef.current = count;

	const handleClick = () => {
		setTimeout(() => {
			alert(`${countRef.current}번 클릭했습니다`);
		}, 2000);
	};

	return (
		<div>
			<h1>{count}</h1>
			<button onClick={() => setCount(count + 1)}>클릭</button>
			<button onClick={handleClick}>alert</button>
		</div>
	);
};

export default App;
```

useEffect와 함께 사용해보자

```jsx
import React, { useState, useRef, useEffect } from 'react';

const App = () => {
	const [count, setCount] = useState(0);

	const countRef = useRef(count);
	countRef.current = count;

	useEffect(() => {
		setTimeout(() => {
			alert(`${countRef.current}번 클릭`);
		}, 2000);
	}, []);

	return (
		<div>
			<h1>{count}</h1>
			<button onClick={() => setCount(count + 1)}>클릭</button>
		</div>
	);
};

export default App;
```

















