## React基础
### 1、组件 & Props
#### 定义组件：函数、类
#### props的只读性

```
function Welcome(props){
  return <h1>{props.name}</h1>
}
```
```
class Welcome extends React.Component{
  render(){
    return <h1>{this.props.name}</h1>
  }
}
```
### 2、state & 生命周期
#### 不能直接更新状态
#### 状态更新可能异步
#### 单向数据流
```
constructor(props){
  super(props)
  this.state = {date: new Date()}
}
this.setState((prevState, props)=>{
  counter: prevState.counter + props.increament
})
```
异步但是可以增加回调，或者使用setTimeOut来达到更新完state再调用方法
```
this.setState({
  selection: value
}, this.fireOnSelect)

this.setState({
  selection: value
});

setTimeout(this.fireOnSelect, 0);
```

### 3、事件处理
#### 绑定属性驼峰命名
#### 绑定this
```
constructor(props){
  this.handleClick = this.handleClick.bind(this)
}
handleClick(){...}
<button onClick={this.handleClick(id)}>
```
```
<button onClick={this.handleClick.bind(this,id)}>
```
```
//箭头函数
<button onClick={(e)=>this.handleClick(id,e)}>
```
### 4、条件渲染
#### 与运算符&
```
{xx.length && <h1>...</h1>}
```

### 5、列表 & keys
keys可在DOM中某元素被增加或删除时帮助React识别变化，列表渲染常用map

### 6、表单
```
//若有多个输入，可以给每个元素name，根据event.target.name判断
<input value.. onChange={this.handleChange}/>
handleChange(e){
...
}
```
### 7、组合 vs 继承
一般不用继承建议使用组合
children在内部包，命名写在属性上
```
function FancyBorder(props){
  return <div>{props.children}</div>
}
function WelcomeDialog(){
  return (<FancyBorder><h1>children</h1></FancyBorder>)
}
```
```
//命名
{props.left}{pros.right}
<FancyBorder left={<div></div>} right={<div></div}></FancyBorder>
```
### 8、React理念
#### 第一步：把 UI 划分出组件层级
#### 第二步：用 React 创建一个静态版本
此时不要用state，大项目建议自底向上构建组件，小可上到下，顶部组件通过props传入数据，若干组件得到并渲染
#### 第三步：定义 UI 状态的最小(但完整)表示
使用state，不包含以下3种：
1. 它是通过 props 从父级传来的吗？
2. 它随着时间推移不变吗？
3. 你能够根据组件中任何其他的 state 或 props 把它计算出来吗？
#### 第四步：确定你的 State 应该位于哪里
对应的每一个state：
1. 确定每一个需要这个 state 来渲染的组件。
2. 找到一个公共所有者组件(一个在层级上高于所有其他需要这个 state 的组件的组件)
3. 这个公共所有者组件或另一个层级更高的组件应该拥有这个 state。
4. 如果你没有找到可以拥有这个 state 的组件，创建一个仅用来保存状态的组件并把它加入比这个公共所有者组件层级更高的地方。
#### 第五步：添加反向数据流
使用例如onChange等调用setState


