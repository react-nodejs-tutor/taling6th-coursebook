### input 관리

```jsx
import React, { Component } from 'react';

class App extends Component {
  state = {
    input: ''
  };
  handleChange = e => {
    this.setState({
      input: e.target.value
    });
  };
  render() {
    return (
      <div>
        <input value={this.state.input} onChange={this.handleChange} />
        <p>현재 input 값: {this.state.input}</p>
      </div>
    );
  }
}

export default App;
```

#### 복수 input관리

```jsx
import React, { Component } from 'react';

class App extends Component {
  state = {
    username: '',
    job: ''
  };
  handleChange = e => {
    // name 과 value 를 이벤트 객체에서 조회합니다.
    const { value, name } = e.target;
    this.setState({
      [name]: value
    });
  };
  render() {
    const { username, job } = this.state;
    return (
      <div>
        <input
          name="username"
          placeholder="유저명"
          value={username}
          onChange={this.handleChange}
        />
        <input
          name="job"
          placeholder="직업"
          value={job}
          onChange={this.handleChange}
        />
        <p>
          {username} 의 직업은 {job}입니다.
        </p>
      </div>
    );
  }
}

export default App;
```

#### 배열에값을 추가하고 보여주기

```jsx
import React, { Component } from 'react';

class App extends Component {
  id = 1;
  state = {
    username: '',
    job: '',
    list: []
  };
  handleChange = e => {
    // name 과 value 를 이벤트 객체에서 조회합니다.
    const { value, name } = e.target;
    this.setState({
      [name]: value
    });
  };
  handleInsert = () => {
    const { username, job, list } = this.state;
    this.setState({
      username: '',
      job: '',
      list: list.concat({ username, job })
    });
    this.id += 1; // 새 데이터 만들때마다 +1
  };
  render() {
    const { username, job, list } = this.state;
    return (
      <div>
        <input
          name="username"
          placeholder="유저명"
          value={username}
          onChange={this.handleChange}
        />
        <input
          name="job"
          placeholder="직업"
          value={job}
          onChange={this.handleChange}
        />
        <button onClick={this.handleInsert}>추가</button>
        <p>
          {username} 의 직업은 {job}입니다.
        </p>
        <ul>
          {list.map((item, index) => (
            <li key={index}>
              {item.username}({item.job})
            </li>
          ))}
        </ul>
      </div>
    );
  }
}

export default App;
```

#### 삭제구현

```jsx
import React, { Component } from 'react';

class App extends Component {
  state = {
    username: '',
    job: '',
    list: []
  };
  id = 1;
  handleChange = e => {
    // name 과 value 를 이벤트 객체에서 조회합니다.
    const { value, name } = e.target;
    this.setState({
      [name]: value
    });
  };
  handleInsert = () => {
    const { username, job, list } = this.state;
    this.setState({
      username: '',
      job: '',
      list: list.concat({ id: this.id, username, job })
    });
    this.id += 1; // 새 데이터 만들때마다 +1
  };
  handleRemove = id => {
    const { list } = this.state;
    this.setState({
      list: list.filter(item => item.id !== id)
    });
  };
  render() {
    const { username, job, list } = this.state;
    return (
      <div>
        <input
          name="username"
          placeholder="유저명"
          value={username}
          onChange={this.handleChange}
        />
        <input
          name="job"
          placeholder="직업"
          value={job}
          onChange={this.handleChange}
        />
        <button onClick={this.handleInsert}>추가</button>
        <p>
          {username} 의 직업은 {job}입니다.
        </p>
        <ul>
          {list.map(item => (
            <li key={item.id}>
              {item.username}({item.job}){' '}
              <span
                onClick={() => this.handleRemove(item.id)}
              >
                [삭제]
              </span>
            </li>
          ))}
        </ul>
      </div>
    );
  }
}

export default App;
```

**form사용 **

```jsx
        <form onSubmit={this.handleInsert}>
          <input
            name="username"
            placeholder="유저명"
            value={username}
            onChange={this.handleChange}
          />
          <input
            name="job"
            placeholder="직업"
            value={job}
            onChange={this.handleChange}
          />
          <button type="submit">추가</button>
        </form>
```

```jsx
  handleInsert = e => {
    e.preventDefault();
    const { username, job, list } = this.state;
    this.setState({
      username: '',
      job: '',
      list: list.concat({ id: this.id, username, job })
    });
    this.id += 1; // 새 데이터 만들때마다 +1
  };
```

#### ref로 dom컨트롤

```jsx
import React, { Component } from 'react';

class App extends Component {
  state = {
    username: '',
    job: '',
    list: []
  };
  id = 1;
  usernameInput = null;
  handleChange = e => {
    // name 과 value 를 이벤트 객체에서 조회합니다.
    const { value, name } = e.target;
    this.setState({
      [name]: value
    });
  };
  handleInsert = e => {
    e.preventDefault();
    const { username, job, list } = this.state;
    this.setState({
      username: '',
      job: '',
      list: list.concat({ id: this.id, username, job })
    });
    this.id += 1; // 새 데이터 만들때마다 +1
  };
  handleRemove = id => {
    const { list } = this.state;
    this.setState({
      list: list.filter(item => item.id !== id)
    });
  };

  render() {
    const { username, job, list } = this.state;
    return (
      <div>
        <form onSubmit={this.handleInsert}>
          <input
            name="username"
            placeholder="유저명"
            value={username}
            onChange={this.handleChange}
            ref={ref => {
              this.usernameInput = ref;
            }}
          />
          <input
            name="job"
            placeholder="직업"
            value={job}
            onChange={this.handleChange}
          />
          <button type="submit">추가</button>
        </form>
        <p>
          {username} 의 직업은 {job}입니다.
        </p>
        <ul>
          {list.map(item => (
            <li key={item.id}>
              {item.username}({item.job}){' '}
              <span
                style={{ cursor: 'pointer' }}
                onClick={() => this.handleRemove(item.id)}
              >
                [삭제]
              </span>
            </li>
          ))}
        </ul>
      </div>
    );
  }
}

export default App;
```

```jsx
  handleInsert = e => {
    e.preventDefault();
    const { username, job, list } = this.state;
    this.setState({
      username: '',
      job: '',
      list: list.concat({ id: this.id, username, job })
    });
    this.id += 1; // 새 데이터 만들때마다 +1

    if (!this.usernameInput) return; // ref 가 만약에 제대로 설정이 안되었다면 무시
    this.usernameInput.focus();
  };
```

**React.createRef\(\)를 사용하면 ref={this.usernameInput}을 사용하고 this.usernameInput.current.focus사용!**

실습끝



