# react 组件

### 1. 组件的基本结构

```jsx
// 首字母大写
function Component(){
	return <h1>This is ES5 Component</h1>
}

// ES6 语法的组件定义，首字母大写，建议使用 ES6
class ES6Component extends React.Component{
	render(){
		return (
			<h1>This is ES5 Component</h1>
		)
	}
}
```

### 2. state && props

state 一般用于组件内部状态变化，即组件内部的数据显示。例如：

```jsx
export default class MyComponent extends Component{
    constructor(props) {
        super(props);
        // 设置组件内部的数据，用对象表示
        this.state = {
            name: "javaliu",
            age: 20
        }
    }
    render(){
        return (
        		// 获取组件内部对象的数据
            <h1>I am {this.state.name}, {this.state.age} years old</h1>
        )
    }
}
```

props 一般用于组件之间的数据传递。例如：

```jsx
export default class MyComponent extends Component{
    constructor(props) {
        super(props);
        this.state = {
            name: "javaliu",
        }
    }
    render(){
        return (
            <h1>I am {this.state.name}, {this.props.age} years old</h1>
        )
    }
}
```
以上是定义的组件，以下是如何使用该组件：

```jsx
import React from 'react';
import ReactDOM from 'react-dom'

import MyComponent from './myComponent';


ReactDOM.render(
    <MyComponent age={22}/>,
    document.getElementById('app')
);
```
### 3. 事件处理

3.1 构造方法绑定函数 

```jsx
import React, {Component} from 'react';

export default class MyComponent extends Component{
    constructor(props) {
        super(props);
        this.state = {
            name: "javaliu",
            age:22
        };
        // 绑定函数的 this 是该组件
        this.handleClick = this.handleClick.bind(this);
    }
    handleClick(){
    	  // 修改状态的方式是使用 setState，没有 name属性，则表示 name 没有被改变。
    	  // 注意此处的 this，this按理应该指的是该函数，但是构造方法进行的重新绑定，此时 this 指的是该组件。
        this.setState({
            age : this.state.age + 1,
        })
    }
    render(){
        return (
        		// jsx 规定顶层元素必须只有一个，此处<div> 包裹 h1 和 button
            <div>
                <h1>I am {this.state.name}, {this.state.age} years old</h1>
                <button onClick={this.handleClick}>涨一岁</button>
            </div>
        )
    }
}
```

3.2 箭头函数取消构造方法绑定（推荐）

```jsx
import React, {Component} from 'react';

export default class MyComponent extends Component{
    constructor(props) {
        super(props);
        this.state = {
            name: "javaliu",
            age:22
        };
        this.handleClick = this.handleClick.bind(this);
    }
    handleClick(){
        this.setState({
            age : this.state.age + 1,
        })
    }
    onChangeValue(e){
    	 // 此处 this 不需要构造方法绑定了；e.target.value 获取文本框的值
        this.setState({
            age : e.target.value
        })
    }
    render(){
        return (
            <div>
                <h1>I am {this.state.name}, {this.state.age} years old</h1>
                <button onClick={this.handleClick}>涨一岁</button>
                <input type='text' onChange={(e)=>{this.onChangeValue(e)}}/>
            </div>
        )
    }
}
```

### 4. 组件的组合方式

### 5. 组件间的数据通信