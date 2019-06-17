# LifeCycle API

#### 컴포넌트 라이프사이클 단계

라이프 사이클은 크게 세 단계로 나뉜다고 했었습니다.

_마운트 - 업데이트 - 언마운트 _인데요, 도표를 보시면 다음과 같습니다.

이 중 자주쓰이는 것들은

render, componentDidMount, shouldComponentUpdate, componentDidUpdate, componentWillUnmount입니다.

나머지 값들은 사용하실일이 드물어요. 실제 프로덕션에서도 이 다섯가지 위주로 life cycle을 관리합니다.![](/assets/life cycle.png)마운트 단계

#### constructor

```jsx
constructor(props) {
  super(props);
}
```

constructor는 컴포넌트가 브라우저에 나타나기전에 호출됩니다. render\(\)전에 호출이되요.

초기 마운트단계에서 실행이되는 메소드입니다.

#### componentDidMount

```jsx
componentDidMount() {
  // 외부 라이브러리 연동: slick, masonry 등
  // 외부 api호출, DOM 에 관련된 작업 등
}
```

cdu는 컴포넌트가 브라우저에 반영이되고 나서 호출이됩니다.

dom관련 작업도 일단 브라우저에 반영이되야 불러서 쓸 수 있겠죠?

또한 해당 컴포넌트에서 필요로 하는 데이터를 요청하기 위해 axios, fetch등을 이용하여 외부 api를 호출하는 작업도 주로 여기서 합니다.

#### 업데이트 단계

#### shouldComponentUpdate

```jsx
shouldComponentUpdate(nextProps, nextState) {
  // return false 하면 업데이트를 안함
  return true;
  // 기본적으로는 true를 반환
}
```

이건 컴포넌트 최적화를 할 때 많이 사용됩니다.

리액트에서는 변화가 발생하는 부분만 업데이트를 해줘서 성능이 꽤 잘 나온다고 했었죠?

하지만, 변화가 발생한 부분만 감지해내기 위해서는 Virtual DOM 에 먼저 그립니다.

이 작업은 그렇게 부하가 많은 작업은 아니지만, 컴포넌트가 무수히 많이 렌더링된다면 얘기가 조금 달라집니다.

버츄얼돔도 CPU를 어느정도 사용하고 있는것은 사실이니까요.

쓸대없이 낭비되고 있는 이 CPU 처리량을 줄여주기 위해서 우리는 Virtual DOM 에 그려주는 것마저 줄여줍니다.

이를 위해  shouldComponentUpdate 를 작성합니다.

이 함수는 기본적으로 true 를 반환합니다.

우리가 따로 작성을 해주어서 조건에 따라 false 를 반환하면 해당 조건에는 render 함수를 호출하지 않습니다.

그리고 그만큼 최적화를 하는 것이죠.

#### componentDidUpdate

```jsx
componentDidUpdate(prevProps, prevState, snapshot) {

}
```

이 API는 컴포넌트에서 render\(\) 를 호출하고난 다음에 발생하게 됩니다.

이 시점에선 this.props 와 this.state 가 바뀌어있습니다.

그리고 파라미터를 통해 이전의 값인 prevProps 와 prevState 를 조회 할 수 있습니다.

그리고, getSnapshotBeforeUpdate 에서 반환한 snapshot 값은 세번째 값으로 받아옵니다.

#### 언마운트 단계

#### componentWillUnmount

```jsx
componentWillUnmount() {
  // 이벤트 및 연동된 외부 라이브러리 제거
}
```

여기서는 주로 등록했었던 이벤트 및 연관 외부 라이브러리 호출등을 제거합니다.

우리는 이러한 여러가지 LifeCycle API를 수업시간내내 프로젝트를 다루면서 사용해볼거에요.

당장 완벽하게 이해가 안되셔도 너무 걱정하지마세요!

