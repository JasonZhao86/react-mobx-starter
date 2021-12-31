## 一、React

### 1、简介

&emsp;&emsp;React是Facebook开发并开源的前端框架。当时他们的团队在市面上没有找到合适的MVC框架，就自己写了一个Js框架，用来架设大名鼎鼎的Instagram（图片分享社交网络）。2013年，React开源。React解决的是前端MVC框架中的View视图层的问题。

<br />

#### 1.1、Virtual DOM

&emsp;&emsp;DOM（文档对象模型Document Object Model）

&emsp;&emsp;The Document：

```html
<html>
<body>
<h1>Title</h1>
<p>A <em>word</em></p>
</body>
</html>
```

&emsp;&emsp;The DOM Tree：

```shell
DOCUMENT
└── ELEMENT: html
    └── TEXT: '\n'
    └── ELEMENT: body
    |	└── TEXT: '\n'
    |	└── ELEMENT: h1
    |	|	└── TEXT: 'Title'
    |	└── TEXT: '\n'
    |	└── ELEMENT: p
    |	|	└── TEXT: 'A'
    |	|	└── ELEMENT: em
    |	|		└── TEXT: 'word'
    |	└── TEXT: '\n'
    └── TEXT: '\n'
```

&emsp;&emsp;将网页内所有内容映射到一棵树型结构的层级对象模型上，浏览器提供对DOM的支持，JS可以调用DOM API来动态的修改DOM结点，从而达到修改网页的目的，这种修改是在浏览器中完成，浏览器会根据DOM的改变重绘改变的DOM结点部分。

&emsp;&emsp;浏览器加载html页面之后，就会创建一颗DOM树，只要DOM树发生了变化，就会重新渲染，这种渲染分为整页面渲染和局部渲染，整页渲染表现为页面变白，重新加载，局部渲染表现为只刷新发生修改的那部分标签。

&emsp;&emsp;修改DOM重新渲染代价太高，前端框架为了提高效率，尽量减少DOM的重绘，提出了**Virtual DOM**，所有的修改都是先在Virtual DOM上完成的，通过比较算法，找出浏览器DOM之间的差异，使用这个差异操作DOM，浏览器只需要渲染这部分变化就行了。

&emsp;&emsp;所以在React框架中，对HTML的所有的修改首先应用到虚拟DOM中去，当调用渲染方法render时，React框架会将虚拟DOM和浏览器的DOM比较，只渲染有差别的那部分，这样可以做到批量修改一次提交，提高渲染效率（有可能多次修改导致最后的DOM没有发生变化，这时就不渲染）。

&emsp;&emsp;React实现了**DOM Diff**算法可以高效比对Virtual DOM和DOM的差异。

![image-20210201093917290](images/image-20210201093917290.png)

#### 1.2、支持JSX语法

&emsp;&emsp;JSX是一种JavaScript和XML混写的语法，是JavaScript的扩展。

```javascript
React.render(
	<div>
    	<div>
    		<div>content</div>
    	</div>
    </div>,
    document.getElementById('example')
)
```

<br />

### 2、测试程序

&emsp;&emsp;替换`/src/index.js`为下面的代码：

```javascript
import React from 'react';
import ReactDom from 'react-dom';

class Root extends React.Component {
  render(){
    return <div>Welcome zhaoxuan</div>;
  }
}

ReactDom.render(<Root />, document.getElementById('root'));
```

&emsp;&emsp;保存文件后，会自动编译，并重新装载刷新浏览器端页面。

```shell
webpack: Compiling...
Hash: 73658aad17b7c79f7d7c
Version: webpack 2.7.0
Time: 493ms
                                   Asset       Size  Chunks                    Chunk Names
                               bundle.js     1.3 MB       0  [emitted]  [big]  app
    0.6b40b7cf8476537e3375.hot-update.js    2.59 kB    0, 0  [emitted]         app, app
    6b40b7cf8476537e3375.hot-update.json   43 bytes          [emitted]
                           bundle.js.map    1.63 MB       0  [emitted]         app
0.6b40b7cf8476537e3375.hot-update.js.map  742 bytes    0, 0  [emitted]         app, app
webpack: Compiled successfully.
```

<br />

#### 2.1、程序解释

&emsp;&emsp;`import React from "react";`导入reac模块，解决组件类的继承等。

&emsp;&emsp;`import ReactDom from "react-dom";`导入react的DOM模块，解决DOM树的绘制。

&emsp;&emsp;`class Root extends React.Component`组件类定义，从`React.Component`类上继承。这个类生成`JSXElement`对象，即React元素对象。

&emsp;&emsp;`render()`渲染函数。返回组件中渲染的内容。注意，只能返回**唯一一个顶级元素**回去。

&emsp;&emsp;`ReactDom.render(<Root/>, document.getElementByid("root"));`第一个参数是JSXElement对象，第二个是DOM的Element元素。将React元素添加到DOM的Element元素中并渲染。

&emsp;&emsp;还可以使用`React.createElement`创建react元素，第一参数是React组件或者一个HTML的标签名称（例如div、span）。

```javascript
return React.createElement("div", null, "welcome zhaoxuan");
ReactDom.render(React.createElement(Root), document.getElementById('root'));
```

&emsp;&emsp;改写后代码为：

```javascript
import React from 'react';
import ReactDom from 'react-dom';

class Root extends React.Component {
  render(){
    // return <div>Welcome zhaoxuan</div>;
    return React.createElement("div", null, "welcome zhaoxuan");
  }
}

// ReactDom.render(<Root />, document.getElementById('root'));
ReactDom.render(React.createElement(Root), document.getElementById('root'));
```

&emsp;&emsp;很明显JSX更简洁易懂，推荐使用SX语法。增加一个子元素：

```javascript
import React from 'react';
import ReactDom from 'react-dom';


class SubEle extends React.Component {
  render (){
    return <div>sub content</div>;
  }
}

class Root extends React.Component {
  render(){
    return (
      <div>
        <h2>Welcome zhaoxuan</h2>
        <hr />
        <SubEle />
      </div>
    );
  }
}

ReactDom.render(<Root />, document.getElementById('root'));
```

注意：

*   React组件的render函数return值只能是一个顶级元素。
*   JSX语法是XML，要求所有元素必须闭合，注意`<br />`不能写成`<br>`

<br />

### 3、JSX规范

*   React中约定**标签中首字母小写就是html标记，首字母大写就是React组件**。
*   要求严格的HTML标记，即所有标签都必须闭合。br也应该写成`<br />`，`/`前留一个空格。
*   return值中，单行**JSXElement对象**省略小括号，多行请使用小括号。
*   元素有嵌套，建议多行，注意缩进。
*   **JSX表达式**：JSX中的表达式使用`{}`大括号括起来，这种表达式一般可以认为是JS的代码行，例如表达式求值，JS对象取属性，调用JS函数等，如果大括号内使用了引号，则引号内的东西将被作为字符串处理。例如`<div>{'2>1?true:false'}</div>`里面的表达式成了字符串了。

<br />

### 4、组件状态state

&emsp;&emsp;**每一个React组件都有一个状态变量state** ，它是一个JavaScript对象，可以为它定义属性来保存值。如果状态变化了，会触发UI的重新渲染。使用`setstate()`方法可以修改state值。注意：**state是组件自己内部使用的，是组件私有的属性**。即组件的state状态对象是私有的，只有组件本身才有权访问，也就是说只有在组件内部才能够访问和修改改组件的state对象。在其他组件中是无权访问另外一个组件的state对象的。组件的state如果发生了变化就会触发render函数的调用执行，重新渲染绘制DOM树节点。

&emsp;&emsp;依然修改`/src/index.js`：

```javascript
import React from 'react';
import ReactDom from 'react-dom';


class Root extends React.Component {
  // 修改组件原始的state对象，注意不是定义，使用let、const、var关键字会抛异常
  state = {
    p1: "zhaoxuan",
    p2: ".com"
  }

  render(){
    // setState(...): Cannot update during an existing state transition (such as within `render` or another component's constructor)，不可以对还在更新中的state使用setState修改state的属性值，但是可以用最原始的方式修改，如下：
    // this.setState({p2: ".org"});   
    this.state.p2 = ".org";
    return (
      <div>
        <h2>Welcome to {this.state.p1}{this.state.p2}</h2>
      </div>
    );
  }
}

ReactDom.render(<Root />, document.getElementById('root'));
```

&emsp;&emsp;如果将`this.state.p2 = ".org";“`改为`this.setState({p2: ".org"});`就会抛异常。也可以使用延时函数`setTimeout(() => this.setState({p2: ".org"}), 5000）;`规避这个问题：

```javascript
import React from 'react';
import ReactDom from 'react-dom';


class Root extends React.Component {
  state = {
    p1: "zhaoxuan",
    p2: ".com"
  }

  render(){
    setTimeout(() => this.setState({p2: ".org"}), 5000);
    // this.state.p2 = ".org";
    return (
      <div>
        <h2>Welcome to {this.state.p1}{this.state.p2}</h2>
      </div>
    );
  }
}

ReactDom.render(<Root />, document.getElementById('root'));
```

<br />

#### 4.1、复杂状态例子

&emsp;&emsp;先看一个网页：

```javascript
<html lang="en">
<head>
    <script type="text/javascript">
        function getEventTrigger(event){
            x = event.target;		// 从事件中获取元素
            alert("点击的元素的id是：" + x.id)
        }
    </script>
</head>
<body>
    <div id="root", onmousedown="getEventTrigger(event)">
        点击这句话，会产生一个事件，并弹出一个警示框
    </div>
</body>
</html>
```

&emsp;&emsp;div的id是root ，鼠标按下事件捆绑了一个函数，只要鼠标按下就会触发调用getEventTriger函数，浏览器会送给它一个参数event, event是事件对象，当事件触发时， event包含触发这个事件的对象。

<br />

##### 4.1.1、HTML DOM的JavaScript事件

| 属性        | 此事件发生在何时                                             |
| ----------- | ------------------------------------------------------------ |
| onabort     | 图像的加载被中断                                             |
| onblur      | 元素失去焦点                                                 |
| onchange    | 域的内容被改变。例如：当用户在input标签的输入框中填写注册用户名时，输入框的内容是在变化的，这时就会产生onchange事件，通过该事件，我们就可以监控内容发生变化后应该触发的某些动作：例如onchange触发的回调函数中判断当用户输入字符串长度小于4个时，不做处理，当大于4个字符的时候，悄悄的向后台的服务端发起AJAX请求，请求该用户名是否已经存在，服务端响应一个json数据，回调函数收到该json数据后做判断，将内容显示在网页，提示用户名是否已存在。 |
| onclick     | 当用户点击某个对象时调用的事件句柄                           |
| ondblclick  | 当用户双击某个对象时调用的事件句柄                           |
| onerror     | 在加载文档或图像时发生错误                                   |
| onfocus     | 元素获得焦点。当鼠标移动到某个元素中时，就获得了该元素的焦点，就会触发一个onfocus事件，利用该事件的回调函数就可以做相应的处理，例如，当用户的鼠标移动到input输入框时，调用onfocus的回调函数，向input框中填充默认的提示信息。 |
| onkeydown   | 某个键盘按键被按下，并没有被松开。                           |
| onkeypress  | 某个键盘按键被按下并松开                                     |
| onkeyup     | 某个键盘按键被松开                                           |
| onload      | 一张页面或一幅图像完成加载。网页页面渲染完成或者图片加载完成（图片通常会通过img标签发起一个新的请求）后触发onload事件。前面我们说过JS中的AMD异步模块加载技术，即模块加载不会影响他后面语句的执行，当模块加载完成后，会调用加载这些模块的语句的回调函数，通知它模块加载好了，这样就可以安全的使用模块资源了。因此这个时候就特别适合使用onload，当整个页面全部加载好之后触发onload的回调函数，该回调函数就可以引用页面中的已加载好的标签对象了，否则没有加载完成就引用会引发异常。 |
| onmousedown | 鼠标按钮被按下                                               |
| onmousemove | 鼠标被移动                                                   |
| onmouseout  | 鼠标从某元素移开                                             |
| onmouseover | 鼠标移到某元素之上，鼠标移动到某个元素之上或者移开该元素之外就会相应的触发onmouseover和onmouseout事件。例如，鼠标移动到某个元素之上后改变鼠标的样式。 |
| onmouseup   | 鼠标按键被松开                                               |
| onreset     | 重置按钮被点击                                               |
| onresize    | 窗口或框架被重新调整大小                                     |
| onselect    | 文本被选中                                                   |
| onsubmit    | 确认按钮被点击。submit标签被点击后，触发onsubmit事件，通过该事件的回调函数，就可以做相应的处理。 |
| onunload    | 用户退出页面                                                 |

<br />

##### 4.1.2、使用react改造上诉复杂例子

&emsp;&emsp;使用React实现上面的传统的HTML：

```javascript
import React from 'react';
import ReactDom from 'react-dom';


class Toggle extends React.Component {
  state = {
    flag: false
  }

  clickHandler (event){
    console.log(event.target.id);
    console.log(event.target === this);
    // 调用时，如果不绑定this，则此时的this就不是Toggle组件实例，新语法新办法在此处不适用，因为和JSX搅合在一起了
    console.log(this);
    console.log(this.state);
    this.setState({flag: !this.state.flag});
    // 此时用这种方法修改会发现页面上显示有延时，刚好为Root组件中定义的5秒
    // this.state.flag = !this.state.flag;
  }

  render (){  /* 注意一定要绑定this，onclick写成小驼峰 */
    return <div id="t1" onClick={this.clickHandler.bind(this)}>
      点击这句话，会触发一个事件。{this.state.flag.toString()}
    </div>;
  }
}

class Root extends React.Component {
  state = {
    p1: "zhaoxuan",
    p2: ".com"
  }

  render (){
    setTimeout(() => this.setState({p2: ".org"}), 5000);
    return (
      <div>
        <div>Welcome to {this.state.p1}{this.state.p2}</div>
        <hr />
        <Toggle />
      </div>
    );
  }
}

ReactDom.render(<Root />, document.getElementById('root'));
```

&emsp;&emsp;Toggle类有自己的state属性。当render完成后，网页上有一个div标签， div标签对象捆绑了一个click事件的处理函数，div标签内有文本内容。如果通过点击左键，就触发了click方法关联的clickHandler函数，在这个函数里将状态值改变。状态值state的改变将引render重绘。如果组件自己的state变了，只会触发自己的render方法重绘。

&emsp;&emsp;注意：`{this.clickHandler.bind(this)}` ，不能外加引号，`this.clickHandler.bind(this)`**一定要绑定this** ，否则当触发捆绑的函数时， this是由函数执行的上下文决定的， this已经不是触发事件的元素对象了。`console.log(event.target.id)`，取回的产生事件的对象的id，但是**这不是我们封装的组件对象**。所以`console.log(event.target===this)`是false，所以这里一定要用this ，而这个this是通过绑定来的。

&emsp;&emsp;`{this.state.flag.toString()}`获取到的为true，是JS中的特定的boolean数据类型，因此它不是字符串，要想作为字符串输出在html中，必须要转换成字符串。

<br />

##### 4.1.3、React中的事件

*   使用**小驼峰**命名；
*   使用JSX表达式，表达式中指定事件处理函数的名称，**不能加括号调用**；
*   不能使用**return false** ，如果要阻止事件默认行为，使用`event.preventDefault()`；

<br />

### 5、属性props

&emsp;&emsp;把React组件当做标签使用，可以为其增加属性，在html中这种属性称为**attribute**，而在React中称为**props**，语法格式如下：

```javascript
<Toggle name="school" parent={this} />
```

*   `name ="school"`，这个属性会作为一个单一的对象传递给组件，加入到组件的props属性中；

*   `parent = {this}`，注意这个this是在Root元素中，指的是Root组件本身，必须使用JSX表达式；
*   在Root中使用JSX语法为Toggle增加子元素，这些子元素也会被加入Toggle组件的`props.children`属性中；

```javascript
import React from 'react';
import ReactDom from 'react-dom';


class Toggle extends React.Component {
  state = {
    flag: false
  }

  clickHandler (event){
    console.log(event.target.id);
    console.log(event.target === this);
    console.log(this);
    console.log(this.state);
    console.log(this.props.children);		// 数组，toggle元素的所有子元素
    // 尝试修改props中的属性值，会抛出异常：TypeError: Cannot assign to read only property 'name' of object '#<Object>'
    // this.props.name = 'W3CSCHOOL';
    this.setState({flag: !this.state.flag});
  }

  render (){
    return <div id="t1" onClick={this.clickHandler.bind(this)}>
      点击这句话，会触发一个事件。{this.state.flag.toString()}<br />
      显示props：<br />
      {this.props.name}: {this.props.parent.state.p1 + this.props.parent.state.p2}<br />
    </div>;
  }
}

class Root extends React.Component {
  state = {
    p1: "zhaoxuan",
    p2: ".com"
  }

  render (){
    setTimeout(() => this.setState({p2: ".org"}), 5000);
    return (
      <div>
        <div>Welcome to {this.state.p1}{this.state.p2}</div>
        <br />
        <Toggle name="w3cschool" parent={this}>	 {/* 自定义2个属性通过props传给Toggle组件对象 */}
          <hr />		{/* 子元素通过props.children访问 */}
          <span>我是Toggle元素的子元素span</span>	{/* 子元素通过props.children访问 */}
        </Toggle>
      </div>
    );
  }
}

ReactDom.render(<Root />, document.getElementById('root'));
```

&emsp;&emsp;应该说， **state是私有private属性，组件外无法直接访问**。可以修改state ，但是**建议使用setstate**方法。props是公有public属性，组件外也可以访问，但**只读**。

<br />

### 6、构造器constructor

&emsp;&emsp;使用ES6的构造器，要提供一个参数props ，并把这个参数使用super传递给父类，Root组件中引用`<Togle />`子组件时（相当于实例化该Togle组件类），创建了props属性，并传给了其构造器constructor，所以`props`和`this.props`实际上是一模一样的东西，因此也就不用this来引用props了，直接写props即可：

```javascript
import React from 'react';
import ReactDom from 'react-dom';


class Toggle extends React.Component {
  constructor (props){
    super(props);
    console.log(this.props === props)         //true
    this.state = {		//组件有了构造器，就可以将state定义在构造器中了，更类似于python中的__init__构造器。
      flag: false
    }
  }
  
  clickHandler (event){
    console.log(event.target.id);
    console.log(event.target === this);
    console.log(this);
    console.log(this.state);
    console.log(this.props.children);
    this.setState({flag: !this.state.flag});
  }

  render (){
    return <div id="t1" onClick={this.clickHandler.bind(this)}>
      点击这句话，会触发一个事件。{this.state.flag.toString()}<br />
      显示props：<br />
      {this.props.name}: {this.props.parent.state.p1 + this.props.parent.state.p2}<br />
    </div>;
  }
}

class Root extends React.Component {
  constructor (props){
    super(props);
    this.state = {
      p1: "zhaoxuan",
      p2: ".com"
    }
  }

  render (){
    setTimeout(() => this.setState({p2: ".org"}), 5000);
    return (
      <div>
        <div>Welcome to {this.state.p1}{this.state.p2}</div>
        <br />
        <Toggle name="w3cschool" parent={this}>
          <hr />
          <span>我是Toggle元素的子元素span</span>
        </Toggle>
      </div>
    );
  }
}

ReactDom.render(<Root />, document.getElementById('root'));
```

<br />

### 7、组件的生命周期

组件的生命周期可分成三个状态：

*   Mounting：已插入真实DOM
*   Updating：正在被重新渲染
*   unmounting：已移出真实DOM

<br />

&emsp;&emsp;组件的生命周期状态，说明在不同时机访问组件，组件正处在生命周期的不同状态上。在不同的生命周期状态访问组件，就会被组件对应生命周期的hook钩子函数捕获，生命周期的方法如下：

*   装载组件触发：
    *   `componentwillMount`：在渲染前调用，在客户端也在服务端。只会在装载之前调用一次。
    *   `componentDidMount`：在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过`this.getDOMNode()`来进行访问。如果你想和其他javascript框架一起使用，可以在这个方法中调用`setTimeout`，`setinterval`或者发送AJAX请求等操作（防止异部操作阻塞UI），只在装载完成后调用一次，在render之后。
*   更新组件触发，这些方法不会在首次render组件的周期调用：
    *   `componentwillReceiveProps(nextProps)`：在组件接收到一个新的prop时被调用。这个方法在初始化
        render时不会被调用。
    *   `shouldComponentUpdate(nextProps, nextState)`：返回一个布尔值。在组件接收到新的props或者state
        时被调用。在初始化时或者使用forceUpdate时不被调用。
        *   可以在你确认不需要更新组件时使用。
            *   如果设置为false ，就是不允许更新组件，那么`componentWillUupdate`, `componentDidUpdate`不
                会执行。
    *   `componentwillUpdate(nextProps, nextState)`：在组件接收到新的props或者state但还没有render时被
        调用。在初始化时不会被调用。
    *   `componentDidUpdate(prevProps, prevstate)`：在组件完成更新后立即调用。在初始化时不会被调用。
*   卸载组件触发：
    *   `componentWillUnmount`：在组件从DOM中移除的时候立刻被调用。

![image-20210201155122525](images/image-20210201155122525.png)

&emsp;&emsp;React组件完整的生命周期状态机如上图所示：**constructor构造器是最早执行的函数**，它实际上包括两个部分，`getDefaultProps`和`getInitialState`。触发更新生命周期函数，需要更新state或props.

&emsp;&emsp;因此，重新编写`/src/index.js`。构造两个组件，在子组件Sub中，加入所有生命周期函数。下面的例子添加是装载、卸载组件的生命周期函数：

```javascript
import React from 'react';
import ReactDom from 'react-dom';


class Sub extends React.Component {
  constructor (props){
    super(props);
    console.log('Sub constructor...');
    this.state = {count: 0};
  }

  handClick (event){
    this.setState({count: this.state.count + 1})
  }

  render (){
    console.log("Sub render")
    return <div id='sub' onClick={this.handClick.bind(this)}>
      Sub's count: {this.state.count}
    </div>;
  }

  componentWillMount (){
    // constructor之后，第一次render之前
    console.log("Sub componentWillMount");
  }

  componentDidMount (){
    // 第一次render之后
    console.log("Sub componentDidMount");
  }

  componentWillUnmount (){
    // 清理工作
    console.log("Sub componentWillUnmount");
  }
}


class Root extends React.Component {
  constructor (props){
    console.log("Root Constructor...")
    super(props);
    this.state = {}
  }

  render (){
    console.log("Root render...")
    return <div>
      <Sub />
    </div>;
  }
}

ReactDom.render(<Root />, document.getElementById('root'));
```

&emsp;&emsp;上面可以看到顺序是：`constructor -> componentWillMount -> render-> componentDidMount -> state或props改变 -> render`

&emsp;&emsp;增加更新组件函数，为了演示props的改变，为Root元素增加一个click事件处理函数，注意，在React中写样式同样需要使用大括号括起来，大括号里面再写一个大括号表示JS中的对象，多个属性之间使用逗号隔开，这点和html中的css样式写法一一致：

```javascript
import React from 'react';
import ReactDom from 'react-dom';


class Sub extends React.Component {
  constructor (props){
    super(props);
    console.log('Sub constructor...');
    this.state = {count: 0};
  }

  handClick (event){
    this.setState({count: this.state.count + 1})
  }

  render (){
    console.log("Sub render")
    return <div style={{height: 200 + 'px', color: 'red', backgroundColor: '#f0f0f0'}}>
      <a id="sub" onClick={this.handClick.bind(this)}>
        Sub's count: {this.state.count}
      </a>
    </div>;
  }

  componentWillMount (){
    // constructor之后，第一次render之前
    console.log("Sub componentWillMount");
  }

  componentDidMount (){
    // 第一次render之后
    console.log("Sub componentDidMount");
  }

  componentWillUnmount (){
    // 清理工作
    console.log("Sub componentWillUnmount");
  }

  componentWillReceiveProps (nextProps){
    // props变更时，接到新props了，交给shouldComponentUpdate
	// props在组件内只读，不能修改，只能从外部改变
    console.log(this.props)		// {}
    console.log(nextProps)		// {}	即使Sub组件没有定义props属性，依然会触发componentWillReceiveProps
    console.log("Sub componentWillReceiveProps", this.state.count);
  }

  shouldComponentUpdate (nextProps, nextState){
    // 是否组件更新， props或state方式改变时，返回布尔值， true才会更新
    console.log("Sub shouldComponentUpdate", this.state.count, nextState);
    return true;		// return false将拦截更新
  }

  componentWillUpdate (nextProps, nextState){
    // 同意更新后，真正更新前，之后调用render
    console.log("Sub componentWillUpdate", this.state.count, nextState);
  }

  componentDidUpdate (preProps, preState){
    // 同意更新后，真正更新后，在render之后调用，render渲染之后，count的值才真正修改了，之前都是未改变状态
    console.log("Sub componentDidUpdate", this.state.count, preState);
  }
}


class Root extends React.Component {
  constructor (props){
    console.log("Root Constructor...")
    super(props);
    this.state = {flag: true, name: 'root'}
  }

  handClick (event){
    this.setState({
      flag: !this.state.flag,
      name: this.state.flag?this.state.name.toLowerCase():this.state.name.toUpperCase()
    });
  }
  render (){
    console.log("Root render...")
    return <div id='root' onClick={this.handClick.bind(this)}>
      My Name is {this.state.name}
      <Sub />	{/* 父组件的render ，会引起下一级组件的更新流程，导致props重新发送，即使子组件props没有改变过 */}
    </div>;
  }
}

ReactDom.render(<Root />, document.getElementById('root'));
```

&emsp;&emsp;`componentwillMount`：第一次装载，在首次render之前。例如控制state, props

&emsp;&emsp;`componentDidMount`：第一次装载结束，在首次render之后。例如控制state, props

&emsp;&emsp;`componentWillReceiveProps`：在组件内部， props是只读不可变的，但是这个函数可以接收到新的props ，可以对props做一些处理，`this.props = {name:'roooooot'];`这就是偷梁换柱。`componentwillReceiveProps`触发，也会走`shouldcomponentUpdate`

&emsp;&emsp;`shouldComponentUpdate`：判断是否需要组件更新，就是是否render ，精确的控制渲染，提高性能。

&emsp;&emsp;`componentWillUpdate`：在除了首次render外，每次render前执行，`componentDidUpdate`在render之后调用。

&emsp;&emsp;不过，大多数时候，用不上这些函数，这些钩子函数是为了精确的控制。

<br />

### 8、无状态组件

&emsp;&emsp;React从15.0开始支持无状态组件，定义如下：

```javascript
import React from 'react';
import ReactDom from 'react-dom';


function Root(props) {
  return <div>Welcome to {props.schoolName}</div>
}

ReactDom.render(<Root schoolName='w3cschool' />, document.getElementById('root'))
```

&emsp;&emsp;无状态组件，也叫**函数式组件**。开发中，很多情况下，组件其实很简单，不需要state状态，也不需要使用生命周期函数。无状态组件很好的满足了需要。
&emsp;&emsp;**无状态组件函数应该提供一个参数props** ，**返回一个React元素**。**无状态组件函数本身就是render函数**。无状态组件的函数名也应该首字母大写。

&emsp;&emsp;用箭头函数改写上面的代码：

```javascript
import React from 'react';
import ReactDom from 'react-dom';


let Root = props => <div>Welcome to {props.schoolName}</div>

ReactDom.render(<Root schoolName='w3cschool' />, document.getElementById('root'))
```

