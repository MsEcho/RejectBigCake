# Taro自定义组件中传递JSX问题
参考文档 [(Taro官网 => Children 与组合)](http://taro-docs.jd.com/taro/docs/children/#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)

1.当自定义组件中只需要传递一个子组件时可以使用 this.props.children。例如：
```
class Dialog extends Componet {
  render() {
    return (
      <View className='dialog'>
        <View className='header'>对话框</View>
        <View className='body'>
          {this.props.children}
        </View>
        <View className='footer'>-- divider --</View>
      </View>
    )
  }
}

<Dialog>
    <View>2021-02-27，加班的一个周六下午</View>
</Diaglog>
```
2.当自定义组件中只需要传递多个子组件时可以使用 renderXXX 方法。例如：
```
class Dialog extends Componet {
  render() {
    return (
      <View className='dialog'>
        <View className='header'>
          {this.props.renderHeader}
        </View>
        <View className='body'>
          {this.props.renderContent}
        </View>
        <View className='footer'>
          {this.props.renderFooter}
        </View>
      </View>
    )
  }
}

<Dialog
  renderHeader={<View>F**K Saturday</View>}
  renderContent={<View>2021-02-27，加班的一个周六下午</View>}
  renderFooter={
    <Block>
      <Button>摸鱼</Button>    
      <Button>大胆摸鱼</Button>    
      <Button>嚣张摸鱼</Button>    
    </Block>
  }
/>
```
PS：这里的 renderXXX 方法只能传递“单个JSX元素”，在传递函数式组件时需要额外注意以下的情况：
```
class Dialog extends Componet {
  render() {
    return (
      <View className='dialog'>
        <View className='header'>对话框</View>
        <View className='body'>
          {this.props.children}
        </View>
        <View className='footer'>-- divider --</View>
      </View>
    )
  }
}

function Lol() {
  return <Text>好久没和JIYOUs打LOL了</Text>
}
// 以下三种都可以正常运行
<Dialog renderContent={() => 'fffff****kkk'} />
<Dialog renderContent={() => <Text>fffff****kkk</Text>} />
<Dialog renderContent={() => <Lol />} />

// 下面的做法将会导致报错
<Dialog renderContent={
    function Lol() {
    return <Text>好久没和JIYOUs打LOL了</Text>
    }
  }
/>
```
如有错误，感谢指正。
