# Props

#### Props의 기본

props는 부모로부터 내려받는다고 했었죠?

부모컴포넌트에서 자식컴포넌트에게 props를 집어넣으면 자식컴포넌트는 this.props로 해당값을 받습니다.

App.js \(부모컴포넌트\)

```jsx
import React, { Component } from 'react';
import Name from './Name';

class App extends Component {
  render() {
    return (
      <Name name="장동건" />
    );
  }
}

export default App;
```

Name.js\(자식컴포넌트\)

```jsx
import React, { Component } from 'react';

class Name extends Component {
  render() {
    return (
      <div>
        hello <b>{this.props.name}!</b>
      </div>
    );
  }
}

export default Name;
```

defaultProps, prop Types를 자주 사용할일은 드뭅니다.

이에 대한 보충설명은 ppt를 참고해주세요.

#### children props

App.js

```jsx
import React, { Component } from 'react';
import Wrapper from './Wrapper';
import Name from './Name';

class App extends Component {
  render() {
    return (
      <Wrapper>
        <Name />
      </Wrapper>
    );
  }
}

export default App;
```

이렇게하면 위에서 작성한 Name컴포넌트의 내용이 Wrapper컴포넌트의 children props로 들어갑니다.

children props는 부모에서 내려주지않아도 컴포넌트 자체적을로 가지고있는 props에요.

Wrapper.js

```jsx
import React from 'react';

const Wrapper = ({ children }) => {
  return <div>{children}</div>;
};

export default Wrapper;
```

children은 const {children} = props와 같은 비구조화 할당을 했습니다.

# State

#### State의 기본

props처럼 부모 컴포넌트가 내려주는 것 말고, 컴포넌트 자신이 어떤 값을 관리하고싶어요.

그때는 state를 사용합니다.

```jsx
import React, { Component } from 'react';

class Counter extends Component {
  state = {
    number: 0
  }

  handleIncrease = () => {
    this.setState({
      number: this.state.number + 1
    });
  }

  render() {
    return (
      <div>
        <h1>counter example</h1>
        <div>numbe : {this.state.number}</div>
        <br />
        <button onClick={this.handleIncrease}>+</button>
      </div>
    );
  }
}

export default Counter;
```

props와 마찬가지로 state도 this.state를 통해서 접근합니다.

state를 변화시킬때는 그냥 하는게 아니라 setState 함수를 사용한다고 했었죠?

setState는 비동기적으로 작동해요.

이건 추후 수업시간에 비동기에 대해서 알아볼때 알려드릴게요.

또한 state는 불변성 관리를 해줘야합니다.

불변성관리에 대해서는 todolist실습을 하면서 알아볼게요.

```jsx
import React, { Component } from 'react';

class Counter extends Component {
  constructor(props) {
    super(props);
    this.state = {
      number: 0
    }
  }
  ...

}
```

원래 state는 이런식으로 작성했었다고 했죠?

하지만 class field문법을 통해 constructor없이도 사용할 수 있습니다.

