# React学习笔记

## 一、进入 JSX 世界

### 1、Hello React

**注意React依赖包引入顺序**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hello React</title>
</head>
<body>
  <div id="app">

  </div>
  <!-- 1、引入 React 核心包 -->
  <script src="../依赖包/旧版本/react.development.js"></script>
  <!-- 2、引入 React Dom 扩展包 -->
  <script src="../依赖包/旧版本/react-dom.development.js"></script>
  <!-- 3、引入Babel，用来编译 JSX 文件 -->
  <script src="../依赖包/旧版本/babel.min.js"></script>
  <!-- 4、写 JSX 代码，注意type是text/babel，最终要浏览器编译为 JSX -->
  <script type="text/babel">
    // 5、定义虚拟DOM，JSX 允许直接赋予变量DOM结构，此处不能用双引号，因为VDom不是字符串
    let VDom = <h1>Hello React</h1>;
    // 6、调用 ReacrDOM的render方法，将虚拟dom渲染到app节点中
    ReactDOM.render(VDom,document.getElementById('app'));
  </script>
</body>
</html>
```

### 2、为什么使用 JSX ？

从以下案例中体会使用 `JSX` 的优势：

**需求**：实现div>div>p结构

#### （1）使用`JSX`实现

```html
<body>
  <div id="app"></div>

  <script src="../依赖包/旧版本/react.development.js"></script>
  <script src="../依赖包/旧版本/react-dom.development.js"></script>
  <script src="../依赖包/旧版本/babel.min.js"></script>
  <script type="text/babel">
    // 需求：实现div>div>p结构
    let VDom = (
      <div className="container">
        <div className="title">
          <p className="title-name">Hello React</p>
        </div>
      </div>
    );
    // 调用 ReacrDOM的render方法，将虚拟dom渲染到app节点中
    ReactDOM.render(VDom,document.getElementById('app'));
  </script>
</body>
```

#### （2）使用 `Js` 实现

```html
<body>
  <div id="app"></div>

  <script src="../依赖包/旧版本/react.development.js"></script>
  <script src="../依赖包/旧版本/react-dom.development.js"></script>
  <script type="text/javascript">
    // 需求：实现div>div>p结构
    let VDom = React.createElement(
      'div',
      {className:'container'},
      React.createElement(
        'div',
        {className:'title'},
        React.createElement(
          'p',
          {className:'title-name'},
          'Hello React'
        )
      )
    )
    // 调用 ReacrDOM的render方法，将虚拟dom渲染到app节点中
    ReactDOM.render(VDom,document.getElementById('app'));
  </script>
</body>
```

**从以上代码中看出，`JSX`其实是React.createElement方法的语法糖，最终babel会将代码编译为 js 格式**。使用 JSX 可以更清晰的写出 DOM 结构。

### 3、JSX 语法规则

**定义**：JSX 全程 javascript XML，是React 用于简化创建虚拟DOM的扩展语法。

#### （1）规则如下：

```jsx
<script type="text/babel">
    /**
     * JSX 的语法规则：
     *  1、定义虚拟DOM时，不能写引号，可以使用()将所写内容进行包裹，()内部可以使用缩进整理结构
     *  2、标签中混入【Js表达式】时，需要使用{}包裹
     *  3、样式的类名不能用class，而是使用className代替
     *  4、内联样式，使用 style={{key:value,key2:value2}} 的形式去写。外层括号代表内部是js表达式，内层括号代表这是一个object类型数据
     *  5、必须只有一个根标签，不能有多个根并行（类似Vue2模版的写法）
     *  6、标签必须闭合，单标签最后必须使用 /
     *  7、标签首字母规则：
     *    （1）若小写字母开头，标签代表HTML同名元素，若HTML不存在此元素，会报错；
     *    （2）若大写字母开头，React会去渲染对应组件，若组件没有定义，会报错；
     */
    let id = 'title-name'
    let text = 'Hello React'
    let VDom = (
      <div className="container" style={{backgroundColor:'orange',fontSize:'23px'}}>
        <div className="title">
          <p className={id}>{text}</p>
        </div>
        <input type="text" />
      </div>
    );
    // 调用 ReacrDOM的render方法，将虚拟dom渲染到app节点中
    ReactDOM.render(VDom,document.getElementById('app'));
  </script>
```

**注意**：

{ } 中写的是 **js表达式**

#### （2）表达式和语句的区别：

**表达式**：会产生一个值，可以放在任何一个需要值的地方，例如：

- a
- a+b
- demo(1)：函数执行会返回一个值（null也算）
- Arr.map()
- Function test ( ) { }

**语句**：例如

- if else 条件判断语句
- for 循环语句
- switch 语句

## 二、React 组件

### 1、组件声明的方式

#### （1）函数式组件

一般用于**简单组件**的封装。

**简单组件**：没有状态

```html
<body>
  <div id="app"></div>
  
  <script src="../依赖包/旧版本/react.development.js"></script>
  <script src="../依赖包/旧版本/react-dom.development.js"></script>
  <script src="../依赖包/旧版本/babel.min.js"></script>
  <script type="text/babel">
    function MyComponent() {
      return <h1>这是函数式组件，用于简单组件的封装 </h1>
    }
    // 6、调用 ReacrDOM的render方法，将虚拟dom渲染到app节点中
    ReactDOM.render(<MyComponent />,document.getElementById('app'));
  </script>
</body>
```



#### （2）类式组件

用于复杂组件的封装

**注意：执行ReactDOM.render(`<MyComponent/>`,document.getElementById('app'));之后发生了什么？**

1. ReactDOM.render解析组件标签，找到 MyComponent 类
2. 发现组件是类式组件，随后new出来该类实例对象，通过实例调用render方法
3. 将render方法返回的虚拟DOM，转为真实DOM，随后呈现在页面上

```html
<body>
  <div id="app"></div>
  
  <script src="../依赖包/旧版本/react.development.js"></script>
  <script src="../依赖包/旧版本/react-dom.development.js"></script>
  <script src="../依赖包/旧版本/babel.min.js"></script>
  <script type="text/babel">
    // 1、必须继承React的Component类
    class MyComponent extends React.Component {
      // 2、必须写render
      render(){
        // render是放在哪里的？----> MyComponent的原型对象上，供【实例使用】
        // render中的this是谁？----> MyComponent的实例对象 <=> MyComponent组件实例对象
        console.log(this);
        // 3、必须有返回
        return <h1>这是类式组件，用于复杂组件的封装 </h1>
      }
    }
    ReactDOM.render(<MyComponent/>,document.getElementById('app'));
  </script>
</body>
```



### 2、组件实例的三大属性

#### （1）state

##### **初始化和修改`state`的传统写法：**

初始化`state`

```Js
class Weather extends React.Component {
  constructor(){
    this.state = {
      weather: '阳光明媚',
      isHot: true,
    }
  }
}

```

修改`state`

**注意**：

- 回调函数中this的指向问题，解决方法是在构造函数中使用bind改变函数this指向。
- 修改state中的数据必须使用`setState`函数

```js
class Weather extends React.Component {

  constructor(props) {
    super(props)
    this.state = {
      weather: '阳光明媚',
      isHot: true,
    }
    // 🌟🌟🌟 改变实例对象中updateWeather函数中的this指向，将this由之前的原型对象改为实例对象
    this.updateWeather = this.updateWeather.bind(this);
  }

  updateWeather() {
    // 🌟🌟 updateWeather放在那里？ ---->  Weather 的原型对象上，供实例使用
    // 🌟🌟 由于updateWeather作为click的回调，不是通过实例调用，而是直接调用。
    // 🌟🌟 类中的方法默认开启局部严格模式，this为undefined
    console.log(this);

    let Hot = this.state.isHot
    // 🌟🌟 不能直接更改state中的数据，必须使用setState
    // this.state.isHot = !this.state.isHot;
    // this.state.weather = this.state.isHot ? '阳光明媚' : '风雨交加';
    this.setState({
      isHot: !Hot,
      weather : Hot ? '风雨交加' : '阳光明媚', // Hot是原来的状态，当Hot为true时，setState将isHot变为false，所以weather需要变成‘风雨交加’
    })
  }

  render(){
    let {isHot, weather} = this.state;
    // 这里不能写this.updateWeather(),因为这里只是放置函数的引用而不是函数的调用结果，写了()，还没有click动作，函数就会执行，
    return <h2 onClick={this.updateWeather}>今天天气{weather}，{isHot?'很热':'很冷'}</h2>
  }
}
```



##### **初始化和修改`state`的改进写法：**

- 改进1:不需要在构造函数中定义this.state，在构造函数外部写入的对象，都是实例对象的属性
- 改进2:箭头函数中的this有定义函数时所处上下文决定，所以this直接是Weather的实例对象

```js
class Weather extends React.Component {
  // 🌟🌟🌟 改进1:不需要在构造函数中定义this.state，在构造函数外部写入的对象，都是实例对象的属性
  state = {
    weather: '阳光明媚',
    isHot: true,
  }
  // 🌟🌟🌟 改进2:箭头函数中的this有定义函数时所处上下文决定，所以this直接是Weather的实例对象
  updateWeather = ()=>{
    let Hot = this.state.isHot;
    this.setState({
      isHot: !Hot,
      weather : Hot ? '风雨交加' : '阳光明媚', // Hot是原来的状态，当Hot为true时，setState将isHot变为false，所以weather需要变成‘风雨交加’
    })
  }
  render(){
    let {isHot, weather} = this.state;
    // 这里不能写this.updateWeather(),因为这里只是放置函数的引用而不是函数的调用结果，写了()，还没有click动作，函数就会执行，
    return <h2 onClick={this.updateWeather}>今天天气{weather}，{isHot?'很热':'很冷'}</h2>
  }
}
```



#### （2）props



#### （3）refs 与事件处理



































