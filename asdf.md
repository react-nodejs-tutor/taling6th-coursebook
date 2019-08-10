```jsx
// components/Button/Button.js
import React from 'react';
import './Button.scss';

const Button = ({ text }) => {
	return (
		<button className="button" type="submit">
			{text}
		</button>
	);
};

export default Button;

// components/Button/Button.scss

.button {
	width: 150px;
	height: 100px;
	font-size: 20px;
	background: material-color('blue-grey', '600');
	@include material-shadow(5, 5);
}
```

```jsx
// components/App.js

import React from 'react';
import Button from './Button/Button';

const App = () => {
	return (
		<div>
			<Button text="추가하기" />
		</div>
	);
};

export default App;

// index.js
import React from 'react';
import ReactDOM from 'react-dom';
import App from 'components/App';

import * as serviceWorker from './serviceWorker';

ReactDOM.render(<App />, document.getElementById('root'));

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();

```

```jsx
// styles/base/_reset.scss
* {
	margin: 0;
	padding: 0;
}

// styles/base/_all.scss
@import 'reset';

// styles/main.scss
@import 'base/all';

// src/index.js

// 추가
import 'styles/main.scss';
```

```jsx
// styles/lib/_all.scss
@import '~include-media/dist/include-media';
@import '~sass-material-colors';

// styles/utils.scss
@import 'lib/all';

// Button.js

@import 'utils.scss';
.button {
	width: 150px;
	height: 100px;
	font-size: 20px;
	background: material-color('blue-grey', '600');
}

```

```jsx
// styles/lib/mixin/_material-shadow.scss
@mixin material-shadow($z-depth: 1, $strength: 1, $color: black) {
	@if $z-depth == 1 {
		box-shadow: 0 1px 3px rgba($color, $strength * 0.14), 0 1px 2px rgba($color, $strength * 0.24);
	}
	@if $z-depth == 2 {
		box-shadow: 0 3px 6px rgba($color, $strength * 0.16), 0 3px 6px rgba($color, $strength * 0.23);
	}
	@if $z-depth == 3 {
		box-shadow: 0 10px 20px rgba($color, $strength * 0.19), 0 6px 6px rgba($color, $strength * 0.23);
	}
	@if $z-depth == 4 {
		box-shadow: 0 15px 30px rgba($color, $strength * 0.25),
			0 10px 10px rgba($color, $strength * 0.22);
	}
	@if $z-depth == 5 {
		box-shadow: 0 20px 40px rgba($color, $strength * 0.3),
			0 15px 12px rgba($color, $strength * 0.22);
	}
	@if ($z-depth < 1) or ($z-depth > 5) {
		@warn "$z-depth must be between 1 and 5";
	}
}
// styles/lib/mixin/_all.scss
@import 'material-shadow';

// styles/lib/_all.scss

@import 'mixin/all';
@import '~include-media/dist/include-media';
@import '~sass-material-colors';
```

```jsx
// Button.js
@import 'utils.scss';
.button {
	width: 150px;
	height: 100px;
	font-size: 20px;
	background: material-color('blue-grey', '600');
	@include material-shadow(5, 5);
}

```



