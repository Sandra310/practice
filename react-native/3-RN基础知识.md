### 控制台
打开模拟器，按住快捷键 command+D 选择远程调试JS，即可打开crome页面看到控制台

### 生命周期
1. 开始->getDefaultProps->getInitialState-> **componentWillMount** -> render -> **componentDidMount** ->运行中
2. 状态state改变-> **shouldComponentUpdate** -> **componentWillUpdate** -> render-> **componentDidUpdate**
3. 属性props改变-> **componentWillReceiveProps** -> **shouldComponentUpdate** -> **componentWillUpdate** -> render-> **componentDidUpdate**
4. 卸载 -> **componentWillUnmount**

### 导入导出
1. 组件导出，上篇有
2. 变量导出
```
var name = '小明'
var age = 29
export {name, age}
或者
export var name = '小明'

//使用
import HelloComponent,{name, age} from './HelloComponent'
<Text>{name}</Text>
```

3. 方法导出
```
export function sum(a,b){
  return a+b
}

//使用
import HelloComponent,{name, age, sum} from './HelloComponent'
<Text onPress={()=>{
  console.log(sum(2,3))
}}>{name}</Text>
```
### props
```
import React, {PropTypes} from 'React'
export default class HelloComponent extents React.Component{
  static defaultProps = {  // 默认值
    name: '小红',
    age: 16
  }
  static propTypes = {  // 指定类型
    name: PropTypes.string,
    age: PropTypes.number.isRequired  //必填
  }
  render(){
    return <Text style={{fontSize:20,backgroundColor:'red'}}>Hello, {this.props.name}</Text>
  }
}


----------------
<HelloComponent name="小明"/>

var params = {name:'小明',age:16, sex:'男'}
<HelloComponent {...params}/>    //使用延展操作符简便赋值
```

### state


### 参考
1. 课程 https://www.imooc.com/learn/808
