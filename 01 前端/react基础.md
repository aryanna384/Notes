[TOC]



## 脚手架使用

初始化`npx create-react-app my-app`

启动 `npm start`

渲染react元素到web当中

ReactDOM.render()

## JSX

JSX：Javascript XML简写

条件渲染

```react
const isLoading = true
const loadData()=>{
  return isLoading ? (<div>loading...</div>):(<div>loaded</div>)
}
const title=(
  <h1>条件渲染
    {loadData()}
  </h1>
)
ReactDOM.render(title,document.getElementById('root'))
```

列表渲染

```react
const songs = [
  {id:1,name:'song'},
  {id:2,name:'for'},
  { id:3,name:'you'},
]
const list = {
  <ul>
    {songs.map(item => {<li key={item.id}>{item.name}</li>})}
  </ul>
}
```

## 组件

组件表示页面中的部分功能，组合多个组件实现完整的页面功能

特点：可复用、独立、可组合

- 函数组件 

大写开头，必须有返回值没有可以返回null

```react
function Hello(){
  return(
 	<div>函数组件</div>
  )
}
ReactDOM.render(<Hello />,document.getElementById('root'))
```

- 类组件

class 关键字，render()方法返回值

```react
class Hello extends React.Component{
  render(){
    return <div>Hello Class Component！</div>
  }
}
ReactDOM.render(<Hello />,document.getElementById('root'))
```

函数组件没有this

### 有状态组件和无状态组件

有状态组件：函数组件，没有自己的状态，只负责数据展示

无状态组件：类组件，负责更新UI

状态（state）：数据

```react
class App extends React.Component {
    state={
    count:0
  }
	render(){
    return(
    <div>{this.state.count}</div>
  }
}

```

### 状态

state和setState

state值是对象

## 事件处理

### 事件绑定

和DOM事件语法相似，采用驼峰命名

`on+事件名称={事件处理程序}`

`onMouseEnter、onFocus`

onClick = {this.handleClick}

### 事件对象

react事件对象：合成对象

`e.prevent`阻止浏览器的默认行为

### 事件抽离的this指向问题

#### 箭头函数

```react
class App extends React.Component {
   onIncrement(){
     this.setState{(.....)}
   }
	render(){
    // 箭头函数中的this指向外部环境，此处指向render()
    return(
    <button onClick={ () => this.onIncrement() }></button>
  }
}
```

#### bind()

```react
class App extends React.Component {
  constructor(){
    super()
    this.onIncrement = this.onIncremanr.bind(this)
  }
  // onIncrement
	render(){
    // 箭头函数中的this指向外部环境，此处指向render()
    return(
    <button onClick={  this.onIncrement }></button>
  }
}
```

