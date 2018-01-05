## React基础
### 1、传值
```
import React from 'react'
import { View,Text,StyleSheet,Button } from 'react-native'
import { Carousel } from 'antd-mobile';

export default class Swiper extends React.Component{
  constructor(){
    super(); //在使用ES6的class时候，必须调用 super(); 方法才能在继承父类的子类中正确获取到类型的 this
    this.state = {   //用来保存状态，改变值只能通过setState
      value: null   
    }
  }
  render(){   //如下展示了组件的使用Carousel、改变状态值，父组件通信 this.props.value,设置样式，包括行内及Viewbox
    return (
      <View>
        <Text onClick={() => alert('click')}>
          {this.props.value.toString()} {this.state.value}
        </Text>
        <Button style={{height:30}} title="Press me" onPress={() => this.setState({value: 'X'})}></Button>
        <Carousel>
          <View style={[ styles.Viewbox ]}>
            <Text>1</Text>
          </View>
          <View style={[ styles.Viewbox ]}>
            <Text>2</Text>
          </View>
        </Carousel>
      </View>
    )
  }
}

const styles = StyleSheet.create({
  Viewbox:{
    height: 115,
    backgroundColor: '#DFDFDF'
  }
})
```
