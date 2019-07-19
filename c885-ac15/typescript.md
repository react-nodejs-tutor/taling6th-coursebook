도움이 될 만한 문서 [https://typescript-kr.github.io/](https://typescript-kr.github.io/) 

\(하지만, 굳이 기초부터 하나하나 살펴보지 않아도 직접 사용해보면서 기본적인 사용법은 금방 터득할 수 있습니다\)

**ts를 적용한 프로젝트를 만들때는 --typescript 플래그를 붙여서 생성합니다.**

```terminal
yarn create react-app project-name --typescript
```

## **React Typescript**

let., const사용

```js
let text: string = '';
let number: number = 5;
const array: number[] = [1, 2, 3];
```

오류

```jsx
number = 'asdf';
text = 5;

array.push('asdf');
```

함수사용

```jsx
function fn(a:number, b:number):number {
    return a+b
}

function fn(a:number | string){
    return a
}

function fn(arr:number[]):number[]{
    return arr.map(x => x+1);
}
```

type사용

```jsx
type MyObj = {
    id: number,
    text: string
}

const obj:MyObj = {
    id: 1,
    text: "something"
}
```

Interface사용

```jsx
interface MyObj {
    id: number,
    text: string
}

const obj:MyObj = {
    id: 1,
    text: "something"
}
```

**type과 interface차이는 extends**

```jsx
interface Person {
  name: string;
}

const person: Person = {
  name: 'kim'
};

interface Programmer extends Person {
  skills: string[];
}

const programmer: Programmer = {
  name: 'kim',
  skills: ['react', 'nodejs']
};

function printSkills(p: Programmer){
  console.log(p.skills);
}
```

**type에서 extends를 구현하려면 & 연산자 사용**

```jsx
type Person = {
  name: string;
}

const person: Person = {
  name: 'kim'
};

type Programmer =  Person & {
  skills: string[];
}

const programmer: Programmer = {
  name: '홍길동',
  skills: ['golang', 'react']
};
```

제너릭 사용

```jsx
function fn<T>(param:T){
    return param;
}

fn<number>(10)
fn<string>("test")
```

## **리액트 컴포넌트에 타입스크립트 적용하기**

[TypeScript React Code Snippets](https://marketplace.visualstudio.com/items?itemName=infeng.vscode-react-typescript)

vscode marketplace에서 Typescript React Code Snippet사용하면 편합니다.

\(tsrcc, tsrsfc등 사용가능\)

```jsx
// src/Counter.tsx

import * as React from 'react';

interface CounterProps {
  onIncrement(): void;
  onDecrement(): void;
  number: number;
}

const Counter: React.FC<CounterProps> = props => {
  return (
    <div>
      <h1>{props.number}</h1>
      <button onClick={props.onIncrement}>+1</button>
      <button onClick={props.onDecrement}>-1</button>
    </div>
  );
};

export default Counter;
```

```jsx
// src/App.js

import * as React from 'react';
import Counter from './Counter';

export interface AppProps {}

export interface AppState {
  number: number;
}

export default class App extends React.Component<AppProps, AppState> {
  state = {
    number: 1
  };

  handleIncrement = () => {
    this.setState({
      number: this.state.number + 1
    });
  };

  handleDecrement = () => {
    this.setState({
      number: this.state.number + 1
    });
  };

  public render() {
    return (
      <Counter
        number={this.state.number}
        onIncrement={this.handleIncrement}
        onDecrement={this.handleDecrement}
      />
    );
  }
}
```

&lt;Counter /&gt; 처럼 컴포넌트를 사용하는 단계에서 number, onIncrement, onDecement등 어떤 props등이 사용되는지 확인가능

cf\) redux사용 할 때는 typesafe actions, react-router-dom등의 써드파티 라이브러리 등에는 @types/ 네임스페이스 사용

기본적인 사용방법은 다 같음.

