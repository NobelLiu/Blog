---
title: 给 iOS 开发者的 React Native 上手指南
date: 2017-01-10 14:55:08
category: Study
tags:
- React Native
---
{% cq %} 
Learn Once，Write Everywhere。
{% endcq %}  
[![React Native](https://ww4.sinaimg.cn/large/006y8lVagw1fblk7x4m4tj30m80bit9d.jpg)](https://facebook.github.io/react-native/)
>上周在学习 React Native，留下一篇记录，帮助需要的人快速上手。如有纰漏，还请不吝赐教。   


<!-- more --> 
-------
**当前环境：**  
> React Native 0.39 with ES6  
> macOS Sierra 10.12.2 + Xcode 8.2  


##  准备工作
###  安装 Homebrew
[Homebrew — The missing package manager for macOS](http://brew.sh/)
###  安装 Node
`brew install node`
###  安装 Yarn（加速 RN 项目初始化）
`npm install -g yarn`
###  安装 React Native 的命令行工具
`npm install -g react-native-cli `
###  安装 WatchMan（监视文件系统变更)
`brew install watchman `
###  安装 Flow（JS 类型检查)
`brew install flow `
###  安装 Xcode
[Xcode on the Mac App Store](https://itunes.apple.com/cn/app/xcode/id497799835?l=en&mt=12)
###  安装 WebStorm（推荐开发工具)
[WebStorm EAP - WebStorm - Confluence](https://confluence.jetbrains.com/display/WI/WebStorm+EAP)

##  创建项目
1. 打开终端切换至开目录
2. 创建新项目`react-native init YourProjectName`
3. 进入项目目录`cd YourProjectName`
4. 运行项目`react-native run-ios`

##  基础知识
### Component
创建完成项目，并用 WebStorm 加载好项目后，让我们看一下`index.ios.js`这个文件，这个文件的实质作用为软件加载 iOS 平台时，要展示的内容，与之对应的还有 `index.android.js` ，为 Android 平台的入口文件。

``` JSX
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View
} from 'react-native';
    
export default class RN extends Component {
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>
          Welcome to React Native!
        </Text>
        <Text style={styles.instructions}>
          To get started, edit index.ios.js
        </Text>
        <Text style={styles.instructions}>
          Press Cmd+R to reload,{'\n'}
          Cmd+D or shake for dev menu
        </Text>
      </View>
    );
  }
}
    
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  },
});
    
AppRegistry.registerComponent('RN', () => RN);
```


>  React 中页面都是由模块化、可组装的**组件（Component）**组装成的  


首先我们要 `import React` 与 基础组件 `Component`，就像我们的`#import <UIKit/UIKit.h>` 
一般来说，创建一个基础组件要创建一个继承自`Component`的 Class，里面有一个`render`方法用于渲染组件。
`render`方法返回的是这个组件要呈现出来的东西，里面可以放各种各样自定义或系统提供的组件。  

> `render`方法返回的`View`只可以有一个
``` JSX
return (
            <View style={styles.yourStyle}></View>
            <View style={styles.yourStyle}></View>
       );
```

上述返回是不可取的。
`style`后面跟的是该组件的样式，样式的内容参考 CSS 的写法，需要注意的是要采用驼峰命名法。

> background-color  => backgroundColor
> margin-top => marginTop


`style`本身则是`View`对象的一个属性，类似 Objective-C 中 `@property` 的概念，属性的名字可以自己定义，要注意各个不同的组件本身已经提前声明了一些属性。

### Props & State
我们使用两种数据来控制一个组件：props和state。props是在父组件中指定，而且一经指定，在被指定的组件的生命周期中则不再改变，类似`@propety (readOnly)`。 对于需要改变的数据，我们需要使用state。


#### 闪烁的文字
这里制作一段不停闪烁的文字。文字内容本身在组件创建时就已经指定好了，所以文字内容应该是一个`prop`。而文字的显示或隐藏的状态（快速的显隐切换就产生了闪烁的效果）则是随着时间变化的，因此这一状态应该写到`state`中。


``` JSX
class Blink extends Component {
  // constructor 为构造方法，系统自动调用
  constructor(props) {
    super(props);
    // 这里设置 state，添加一项叫做 showText 的布尔类型的属性
    this.state = { showText: true };
    
    // 每1000毫秒对showText状态做一次取反操作
    // setInterval 为无输入参数的方法，有的话在里面填入需要的变量名 
    setInterval(() => {
      // 调用 setState 方法去设置 state
      this.setState({ showText: !this.state.showText });
    }, 1000);
  }
    
  render() {
    // 根据当前showText的值决定是否显示text内容
    let display = this.state.showText ? this.props.text : ' ';
    return (
      // 变量名加一对花括号来读取值
      <Text>{display}</Text>
    );
  }
}
    
class BlinkApp extends Component {
  render() {
    return (
      <View>
        <Blink text='I love to blink' />
    );
  }
}
```
    
#### 表单验证
当邮箱格式不正确或密码长度小于8位时，显示灰色底色，同时登陆按钮无法点击，提示错误。  

``` JSX  
constructor(props) {
        super(props);
        this.state = {
            emailState: false,
            pwdState: false,
        };
    }
    onChangeEmail= (text) => {
        var email = /^([\w-_]+(?:\.[\w-_]+)*)@((?:[a-z0-9]+(?:-[a-zA-Z0-9]+)*)+\.[a-z]{2,6})$/i;
        this.setState({emailState: email.exec(text)});
    }
    onChangePassword= (text) => {
        this.setState({pwdState: text.length > 7});
    }
	
    render() {
        return (
            <View style={styles.container}>
                <TextInput
                    style={{margin: 8, padding: 8, height: 40, backgroundColor: this.state.emailState ? 'white' : 'lightgray'}}
                    placeholder="邮箱"
                    onChangeText={(text) => this.onChangeEmail(text)}
                />
                <TextInput
                    style={{margin: 8, padding: 8, height: 40, backgroundColor: this.state.pwdState ? 'white' : 'lightgray'}}
                    placeholder="密码(>7)"
                    onChangeText={(text) => this.onChangePassword(text)}
                />
                <Button
                    onPress={() => alert('👌')}
                    style={{margin: 8, padding: 8, height: 40}}
                    title={this.state.emailState && this.state.pwdState ? "登陆" : "邮箱或密码不正确"}
                    color="blue"
                    disabled={!(this.state.emailState && this.state.pwdState)}
                    accessibilityLabel="Learn more about this purple button"
                />
            </View>
        );
}  
```
### 更新数据
React 可以轻松的做到在数据改变的时候刷新自己的状态，你需要做的就是 Component 自身去应对这些变化。


我们来写这样一个例子，一个可以显示数字文本的组件，文本的字体大小为显示的数字的值，每秒自增4。  

``` JSX
export default class RN extends Component {
    constructor(props) {
        super(props);
        // 初始化一个值
        this.state = {text: 10};
    
        setInterval(() => {
            // 每1000毫秒对 text + 4
            this.setState({text: this.state.text + 4});
        }, 1000);
    }
    
    render() {
        return (
            <View style={styles.container}>
                // 设置不断变化的 text
                <BiggerText text={this.state.text}/>
            </View>
        );
    }
}
    
class BiggerText extends Component {
    render() {
        return (
            // 同时在字体大小和显示值两个地方使用 text
            <Text style={{fontSize: this.props.text}}>{this.props.text}</Text>
        );
    }
}  

```
### 方法
拿按钮的点击事件为例   
 
``` JSX
funcFromClassFunc= (title) => {
    alert(title);
}

render() {
    return (
        <View style={styles.container}>
            <Button
                onPress={() => alert('funcFromBlock')}
                title='funcFromBlock'
            />
            <Button
                onPress={button => this.funcFromClassFunc('funcFromClassFunc')}
                title='funcFromClassFunc'
            />
        </View>
    );
}
```

上面展示了两种方法的使用方式：`=>`符号前面为输入参数，后面为返回值。
### 展示结构化数据
Key-Value 使用点去取值，数组使用下标`[0]`取值，或使用`map`遍历。  

``` JSX
constructor(props) {
        super(props);
        this.state = {
            datas: [
                {name: 'Tom', age: 5, friends: [{
                    name: "Tom's friend", age: 5,
                }]},
                {name: 'Jerry', age: 3, friends: [{
                    name: "Jerry's friend", age: 3,
                }]},
            ],
        };
    }
render() {
        return (
            <View style={styles.container}>
                {this.state.datas.map((data) => {
                    return <View key={data.name}>
                        <Text>Name:{data.name} Age:{data.age}</Text>
                    </View>;
                })}
            </View>
        );
}
```
### 控件的展示与隐藏
基本来说，我们要隐藏一个控件，那就在控件的`render()`方法中 return `null`，这个控件就可以理解为被隐藏了。  

``` JSX
render() {
        var hidden = false;
        if (hidden) {
            return (
                <View style={styles.container}>
                    <Text>aaa</Text>
                </View>
            );
        }else {
            return null;
        }
}
```	
	
### 页面的跳转
页面的跳转一般会使用`Navigator`， 它使用纯 JavaScript 实现了一个导航栈，因此可以跨平台工作，同时也便于定制。 
  
``` JSX
export default class RN extends Component {
    render() {
        let defaultName = 'FirstPageComponent';
        let defaultComponent = FirstPageComponent;
        return (
            <Navigator
                initialRoute={{ name: defaultName, component: defaultComponent }}
                configureScene={(route) => {
                return Navigator.SceneConfigs.PushFromRight;
              }}
                renderScene={(route, navigator) => {
                let Component = route.component;
                return <Component {...route.params} navigator={navigator} />
              }} />
        );
    }
}
	
class FirstPageComponent extends Component {
    constructor(props) {
        super(props);
        this.state = {};
    }
    _pressButton() {
        // 解构 navigator（在 props 中取出 navigator）
        const { navigator } = this.props;
        if(navigator) {
            navigator.push({
                name: 'SecondPageComponent',
                component: SecondPageComponent,
            })
        }
    }
    render() {
        return (
            <View style={styles.container}>
                <TouchableOpacity onPress={this._pressButton.bind(this)} >
                    <Text>点我跳转</Text>
                </TouchableOpacity>
            </View>
        );
    }
}
	
class SecondPageComponent extends Component {
	
    constructor(props) {
        super(props);
        this.state = {};
    }
	
    _pressButton() {
        const { navigator } = this.props;
        if(navigator) {
            //pop()出栈
            navigator.pop();
        }
    }
	
    render() {
        return (
            <View style={[styles.container, {backgroundColor: 'lightgray'}]}>
                <TouchableOpacity onPress={this._pressButton.bind(this)}>
                    <Text>点我跳回去</Text>
                </TouchableOpacity>
            </View>
        );
    }
}
```
### 编写原生插件
在 Xcode 中新建继承类自`NSObject`，`.h 文件`中引入 
 
1. #import "RCTBridgeModule.h"  
1. #import "RCTLog.h"  

并实现`RCTBridgeModule`协议。`.m 文件`实现中包含`RCT_EXPORT_MODULE()`宏，方法声明通过`RCT_EXPORT_METHOD()`宏来实现。下面是一个使用 iOS 原生 AlertController 的例子。  
>NativeAlert.h

``` objectivec
#import <UIKit/UIKit.h>
#import "RCTBridgeModule.h"
#import "RCTLog.h"
	
@interface NativeAlert : NSObject <RCTBridgeModule>
	
@end
```

>NativeAlert.m   

``` objectivec
#import "NativeAlert.h"
	
@implementation NativeAlert
	
RCT_EXPORT_MODULE();
	
RCT_EXPORT_METHOD(showAlert:(NSString *)title message:(NSString *)message) {
  
  UIViewController *vc = [UIApplication sharedApplication].keyWindow.rootViewController;
  
  UIAlertController *alert = [UIAlertController alertControllerWithTitle:title message:message preferredStyle:UIAlertControllerStyleAlert];
  [alert addAction:[UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleCancel handler:nil]];
  
  [vc presentViewController:alert animated:YES completion:nil];
}
	
@end
```

在 js 文件中要先导入`import {  NativeModules } from 'react-native';`。
用法如下：  

``` JSX
showAlert = () => {
	var NativeAlert = NativeModules.NativeAlert;
	NativeAlert.showAlert('Title', 'Message');
}
	
render() {
	return (
		<View>
			<Button title='NativeAlert' onPress={this.showAlert}></Button>
		</View>
		);
}
```