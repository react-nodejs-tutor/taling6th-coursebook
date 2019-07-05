연습자료

```terminal
git clone https://github.com/react-nodejs-tutor/hoc-prac-1st.git
```

```jsx
// src/withData.js

import React, { Component } from 'react';
import axios from 'axios';

const withData = url => WrappedComponent => {
	return class extends Component {
		state = {
			data: null
		};

		callAPI = async () => {
			const response = await axios.get(url);
			this.setState({
				data: response.data.articles
			});
		};

		componentDidMount() {
			this.callAPI();
		}

		render() {
			return <WrappedComponent {...this.props} data={this.state.data} />;
		}
	};
};

export default withData;
```

```jsx
// src/America.js

import React, { Component } from 'react';
import withData from './withData';

class America extends Component {
	render() {
		const { data } = this.props;

		return (
			<div>
				{data && (
					<textarea rows={100} cols={100}>
						{JSON.stringify(data, null, 3)}
					</textarea>
				)}
			</div>
		);
	}
}

export default withData(
	'https://newsapi.org/v2/top-headlines?country=us&apiKey=61120652e560407b8e961993f5ebaa8f'
)(America);

```

```jsx
// src/Germany.js

import React, { Component } from 'react';
import withData from './withData';

class Germany extends Component {
	render() {
		const { data } = this.props;

		return (
			<div>
				{data && (
					<textarea rows={100} cols={100}>
						{JSON.stringify(data, null, 3)}
					</textarea>
				)}
			</div>
		);
	}
}

export default withData(
	'https://newsapi.org/v2/top-headlines?country=de&apiKey=61120652e560407b8e961993f5ebaa8f'
)(Germany);
```

이상

