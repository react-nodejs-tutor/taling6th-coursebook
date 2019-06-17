_1강에서 다룬 내용은 pdf ppt슬라이드에 모두 있습니다._

_복습이라고 생각하시고 책읽듯이 읽어주세요_

# JSX

JSX는 리액트에서 return문에 사용하는 문법이다.

얼핏보면 HTML같지만, 실제로는 자바스크립트입니다.

```jsx
return (
    <div className="App">
    <h1>Hello React</h1>
    </div>
);
```

이렇게 컴포넌트에서 JSX를 작성하면 바벨 트렌스파일러가 자동으로 자바스크립트로 변환을 해줍니다.

JSX에는 여러 규칙들이 있습니다

#### 꼭 닫혀야 하는 태그

```jsx
import React, { Component } from 'react';

class App extends Component {
  render() {
    return (
      <div>
        <input type="text" />
      </div>
    );
  }
}

export default App;
```

input 같은 태그도 반드시 닫아주세요.

#### 감싸져 있는 엘리먼트

```jsx
import React, { Component } from 'react';

class App extends Component {
  render() {
    return (
      <div>
        <div>
          Hello
        </div>
        <div>
          Bye
        </div>
      </div>
    );
  }
}

export default App;
```

태그들은 반드시 감싸줍니다. div말고 Fragement를 사용해도된다고 했었죠!

#### JSX안에 자바스크립트 값 사용하기

```jsx
import React, { Component } from 'react';

class App extends Component {
  render() {
    const name = 'react'
    return (
      <div>
        hello {name}!
      </div>
    );
  }
}

export default App;
```

자바스크립트 값을 사용할때는 {}를 사용합니다.

#### 조건부렌더링

```jsx
import React, { Component } from 'react';

class App extends Component {
  render() {
    return (
      <div>
        {
          true
            ? console.log('correct')
            : console.log('incorrect')
        }
      </div>
    );
  }
}

export default App;
```

바벨은 if~else를 transpile하지 못합니다.

3항연산자 또는 && operator를 사용해주세요

#### 스타일은 항상 객체형식으로

```jsx
import React, { Component } from 'react';

class App extends Component {
  render() {
    const style = {
      backgroundColor: 'red',
    };

    return <div style={style}>react</div>;
  }
}

export default App;
```

스타일은 항상 객체형식으로 작성해야합니다.

#### 컴포넌트에 style을 적용할때는 className

```jsx
import React, { Component } from 'react';
import './App.css'

class App extends Component {
  render() {
    return (
      <div className="App">
        react
      </div>
    );
  }
}

export default App;
```

스타일을 적용할때는 class가 아닌 className을 써주세요.

class로 해도되지만 warning이 뜨고 class키워드는 이미 자바스크립트에서 사용중이기때문에 가급적 피합니다.

#### 주석

```jsx
import React, { Component } from 'react';

class App extends Component {
  render() {
    return (
      <div>
        {/* 주석은 이렇게 */}
        <h1>react</h1>
      </div>
    );
  }
}

export default App;
```

주석은 {/\* \*/}형식을 사용합니다.

