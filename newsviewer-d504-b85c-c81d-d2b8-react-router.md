기존에는 tab메뉴를 state로 관리했었죠?

이번에는 리액트 라우터를 적용해 보겠습니다.

우선 리액트 라우터를 설치하세요. 그리고 적용합니다.

```jsx
$ yarn add react-router-dom
```

```jsx
// src/index.js

import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';
import { BrowserRouter as Router } from 'react-router-dom';

ReactDOM.render(
	<Router>
		<App />
	</Router>,
	document.getElementById('root')
);

serviceWorker.unregister();
```

우리가 이번 프로젝트에서 만들어야할 페이지는 단 하나입니다.

src/pages/NewsPage.js를 만들고 다음과 같은 코드를 입력합니다.

```jsx
// src/pages/NewsPage.js

import React, { Component } from 'react';
import NewsList from '../components/NewsList';
import Categories from '../components/Categories';

export default class extends Component {
	render() {
		const { match } = this.props;
		const category = match.params.category || 'all';

		return (
			<div>
				<Categories />
				<NewsList category={category} />
			</div>
		);
	}
}
```

우리는 현재 선택된 카테고리값을 URL 파라미터를 통해 사용하므로 Categories에 현재 선택된 카테고리값을 알려줄 필요도없고 onSelect함수를 전달해 줄 필요도 없습니다.

다 만드셨으면 App 에서 기존에 있던 내용들을 다 지우고 Route 를 정의해주세요.

```jsx
import React, { Component } from 'react';
import NewsPage from './pages/NewsPage';
import { Route } from 'react-router-dom';

class App extends Component {
	render() {
		return (
			<div>
				<Route path="/:category?" component={NewsPage} />
			</div>
		);
	}
}

export default App;
```

위 코드에서 사용된 path 에 /:category? 이런식으로 맨 뒤에 물음표 문자가 들어가있는데요, 

이 의미는 category 값이 선택적\(optional\)이라는 의미입니다. 

즉, 있을수도 있고 없을 수도 있다는 것이죠.

만약에 category URL 파라미터가 없다면 전체 카테고리를 선택한것으로 간주하게 됩니다.

이어서 NavLink를 사용해봅시다.

Categories 에서 기존에 onSelect 함수를 호출하여 카테고리를 선택하고, 

선택된 카테고리에 다른 스타일을 주는 기능을 NavLink 로 대체하겠습니다.

div, a, button, input 처럼 일반 HTML 요소가 아닌 특정 컴포넌트에 styled-components 를 사용할 땐 

styled\(컴포넌트이름\)\`\` 와 같은 형식으로 사용합니다.

```jsx
// src/components/Categories.js

import React from 'react';
import styled from 'styled-components';
import { NavLink } from 'react-router-dom';

const categories = [
	{
		name: 'all',
		text: '전체보기'
	},
	{
		name: 'business',
		text: '비지니스'
	},
	{
		name: 'entertainment',
		text: '엔터테인먼트'
	},
	{
		name: 'health',
		text: '건강'
	},
	{
		name: 'science',
		text: '과학'
	},
	{
		name: 'sports',
		text: '운동'
	},
	{
		name: 'technology',
		text: '기술'
	}
];

const CategoriesBlock = styled.div`
	padding-bottom: 3rem;
	width: 960px;
	margin: 0 auto;
	margin-top: 1rem;

	ul {
		overflow: hidden;
		padding: 0;
		margin: 0;
	}
`;

const Category = styled(NavLink)`
	float: left;
	list-style-type: none;
	color: inherit;
	text-decoration: none;

	span {
		display: block;
		padding: 0.25rem 0.5rem;
	}

	&.active {
		color: blue;
		font-weight: bold;
	}
`;

const Categories = () => {
	return (
		<CategoriesBlock>
			{categories.map(c => (
				<Category
					key={c.name}
					to={c.name === 'all' ? '/' : `/${c.name}`}
					exact={c.name === 'all'}
					activeClassName="active"
				>
					<span>{c.text}</span>
				</Category>
			))}
		</CategoriesBlock>
	);
};

export default Categories;

```

이상.  


