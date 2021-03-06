---
layout: post
title: 부모 컴포넌트의 상태(State) 변경을 통한 고객 정보 갱신 [React와 Node.js를 활용한 고객 관리 시스템]
date: 2021-06-30 21:03:36 +0530
categories: Javascript NodeJS React
---

기본적으로 React는 Single Page Aplication(SPA) 형태로 동작.
따라서 전체페이지를 window.locaiton.reload(); 와 같은 형태로 전체 페이지를 새로고침 하는 것은 비 효율적이며
고객 추가 컴포넌트에서 부모 컴포넌트의 상태를 변경하여 필요한 부분만 새로고침 할 수 있도록 해야함
기본적으로 부모 컴포넌트에서 자식 컴포넌트로 함수를 props 형태로 건내주는 방식으로 구현

```javascript
class App extends Component{

  constructor(props){
    super(porps);
    this.state = {
      customers: '',
      completed: 0
    }
  }

  stateRefresh = () => {
    this.setState({
      customers: '',
      completed: 0
    });
    this.callApi()
    .then(res => this.setState({customers: res}))
    .catch(err => console.log(err));
  }

  componentDidMount() {
    this.timer = setInterval(this.progress, 20);
    this.callApi()
      .then(res => this.setState({customers: res}))
      .catch(err => console.log(err));
  }
```

App.js 에서 기존의 state 초기화 변수를 제거후 생성자를 이용해 구현
stateRefresh 함수는 state를 초기화 해줄 수 있게 해주는 함수, 고객 목록을 새롭게 불러와야 하므로 고객 데이터를 불러오는 부분을 입력.

```javascript
<CustomerAdd stateRefresh={this.stateRefresh} />
```

App.js 파일의 render() 부분에서 CustomerAdd 를 화면에 출력할 때 props값으로 stateRefresh를 설정

후에 CustomerAdd.js 파일에서 새로고침 부분을 아래와 같이 수정

```javascript
this.props.stateRefresh();
```
