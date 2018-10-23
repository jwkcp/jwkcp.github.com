---
layout: post
title: 리액트(React)에서 조건에 따라 className 추가하는 방법
tags: react
comments: true
---
           
조건에 따라 className의 항목을 넣고 싶을 땐 아래와 같이 하면 된다. 이 예제에서는 isOn 이라는 state 값이 토글 될 때 마다 부트스트랩의 btn-light와 btn-dark 값을 번갈아가며 바꿔준다.
    
~~~
constructor(props) {
  super(props);
  this.state = {
    isOn: true
  }
}

...

handleToggle = () => {
  const { isOn } = this.state;
  this.setState({
    isOn: !isOn
  });
}

...

render() {
  return (

    // 이 부분이 조건에 따른 분기점
    <button className={"btn " + (this.state.isOn ? 'btn-dark' : 'btn-light')} onClick={this.handleToggle}>
      {this.state.isOn ? 'On' : 'Off'}
    </button>
  );
};

~~~

   