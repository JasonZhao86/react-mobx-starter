## 一、React项目

### 1、项目依赖安装

&emsp;&emsp;将项目开发基础文件`react-mobx-starter-master.zip`解压缩，并用这个目录作为项目根目录。在项目根目录中，执行下面的命令，就会自动按照`package.json`的配置安装依赖模块。

```shell
$ npm install
或者
$ npm i
```

&emsp;&emsp;安装完成后，会生成一个目录`node-modules` ，里面是安装的所有依赖的模块。

<br />

### 2、项目整体说明

&emsp;&emsp;项目目录结构：

```shell
[root@ssp react-mobx-starter-master]# tree -a
.
├── .babelrc
├── .gitignore
├── index.html
├── jsconfig.json
├── LICENSE
├── .npmrc
├── package.json
├── README.md
├── src
│   ├── App.js
│   ├── AppState.js
│   ├── index.html
│   └── index.js
├── node_modules
│   ├── ...
├── webpack.config.dev.js
└── webpack.config.prod.js
```

<br />

### 3、配置文件详解

#### 3.1、package.json

&emsp;&emsp;`$npm init`产生的文件，里面记录项目信息，所有项目依赖。

<br />

##### 3.1.1、版本管理

```javascript
"repository": {
    "type": "git",
    "url": "https://192.168.124.135/react-mobx/react-mobx-starter.git"
}
```

<br />

##### 3.1.2、项目管理

```javascript
"scripts": {
    "test": "jest",
    "start": "webpack-dev-server --config webpack.config.dev.js --hot --inline",
    "build": "webpack -p --config webpack.config.prod.js"
},
```

&emsp;&emsp;start指定启动webpack的dev server，用于开发的WEB Server ，主要提供2个功能：静态文件支持、自动刷新和热替换。

&emsp;&emsp;**HMR（Hot Module replacement）**：HRM可以在应用程序运行中替换、添加、删除模块，而无需重载页面，只把变化部分替换掉。不使用HMR则自动刷新会导致这个页面刷新。

*   `--hot`：启动HMR
*   `--inline`：默认模式，使用HMR的时候建议开启`inline`模式。热替换时会有消息显示在控制台。

`build`使用`webpack`构建打包。对应`$npm run build`

<br />

##### 3.1.3、项目依赖

&emsp;&emsp;devDependencies开发时依赖，不会打包到目标文件中。对应`npm install xxx --save-dev`。例如babel的一些
依赖，只是为了帮我们转译代码，没有必要发布到生产环境中。

&emsp;&emsp;dependencies运行时依赖，会打包到项目中。对应`npm install xxx --save`。

<br />

##### 3.1.4、开发时依赖

```javascript
"devDependencies": {
    "babel-core": "^6.24.1",
    "babel-jest": "^19.0.0",
    "babel-loader": "^6.4.1",
    "babel-plugin-transform-decorators-legacy": "^1.3.4",
    "babel-plugin-transform-runtime": "^6.23.0",
    "babel-preset-env": "^1.4.0",
    "babel-preset-es2015": "^6.24.1",
    "babel-preset-es2017": "^6.24.1",
    "babel-preset-react": "^6.24.1",
    "babel-preset-stage-0": "^6.24.1",
    "css-loader": "^0.28.0",
    "html-webpack-plugin": "^2.28.0",
    "jest": "^19.0.2",
    "less": "^2.7.2",
    "less-loader": "^4.0.3",
    "react-hot-loader": "^3.0.0-beta.6",
    "source-map": "^0.5.6",
    "source-map-loader": "^0.2.1",
    "style-loader": "^0.16.1",
    "uglify-js": "^2.8.22",
    "webpack": "^2.4.1",
    "webpack-dev-server": "^2.4.2"
  },
```

版本号指定：

*   `版本号`：只安装指定的版本号
*   `~版本号`：例如`~1.2.3`表示安装`1.2.x`中最新版本，不低于`1.2.3`，但不能安装`1.3.x`
*   `^版本号`：例如`^2.4.1`表示安装`2.x.x`最新版本不低于`2.4.1`
*   `latest` ：安装最新版本

<br />

babel转译，因为开发用了很多ES6语法。从`6.x`开始babel拆分成很多插件，需要什么引入什么：

*   `babel-core`：babel核心。
*   `babel-loader`：用于webpack的babel loader，webpack是基于loader的。
*   `babel-preset-xxx`：预设的转换插件。
*   `babel-plugin-transform-decorators-legacy`：下面的课程用到了装饰器，这个插件就是转换装饰器用的。

<br />

css样式相关的包括：`css-loader`, `less`, `less-loader`, `style-loader`

`react-hot-loader`：react热加载插件，希望改动保存后，直接在页面上直接反馈出来，不需要手动刷新。

`source-map`：文件打包，js会合并或者压缩，没法调试，用它来看js原文件（JS源代码）是什么。

`source-map-loader`：source-map也需要webpack的loader。

`webpack`：打包工具，`2.4.1`发布于2017年4月，当前`2.7.0`发布于2017年7月。

`webpack-dev-server`：启动一个开发用的web server。

<br />

##### 3.1.5、运行时依赖

```javascript
"dependencies": {
   "antd": "^2.9.1",
   "axios": "^0.16.1",
   "babel-polyfill": "^6.23.0",
   "babel-runtime": "^6.23.0",
   "mobx": "^3.1.9",
   "mobx-react": "^4.1.8",
   "mobx-react-devtools": "^4.2.11",
   "prop-types": "^15.5.8",
   "react": "^15.5.4",
   "react-dom": "^15.5.4",
   "react-router": "^4.1.1",
   "react-router-dom": "^4.1.1"
}
```

`antd`：`ant design`，基于react实现，蚂蚁金服开源的react的UI库。做中后台管理非常方便。

`axios`：JS中用于异步请求支持的基础库。

`polyfill`：解决浏览器api不支持的问题。写好polyfil就让你的浏览器支持，帮你抹平差异化。

`react`：开发的主框架。

`react-dom`：支持DOM

`react-router`：支持路由

`react-router-dom`：DOM绑定路由

`mobx`：状态管理库，透明化。

`mobx-react`, `mobx-react-devtools`：mobx和react结合的模块。

react和mobx是一个强强联合。

<br />

#### 3.2、babel配置

&emsp;&emsp;`.babelrc`：babel转译的配置文件

```javascript
{
  "presets": [
    "react",
    "es2017",
    "es2015",
    "env",
    "stage-0"
  ],
  "plugins": ["transform-decorators-legacy", "transform-runtime"]
}
```

<br />

#### 3.3、webpack配置

##### 3.3.1、webpack.config.dev.js

&emsp;&emsp;这是一个符合Commonjs语法的模块，下面将对该文件的各部分进行详细解释。

<br />

###### 3.3.1.1、module.exports

&emsp;&emsp;模块导出：

```javascript
module.exports = {
    ....
}
```

<br />

###### 3.3.1.2、devtool: 'source-map'

&emsp;&emsp;定义生成及如何生成source-map文件。source-map适合生成环境使用，会生成完整的Sourcemap独立文件。由于在bundle（压缩后的JS文件）中添加了引用注释，所以开发工具知道如何在Sourcemap中找到对应的代码片段。

```javascript
devtool: 'source-map',
```

<br />

###### 3.3.1.3、entry入口

&emsp;&emsp;描述入口。webpack会从入口开始，找出直接或间接的模块和库，然后输出到bundles文件中。entry如果是一个字符串，定义就是入口文件。如果是一个数组，数组中每一项都会执行，里面包含入口文件，另一个参数可以用来配置一个服务器，我们这里配置的是热载插件，可以自动刷新。

```javascript
entry: {
    'app': [
        'react-hot-loader/patch',
        './src/index'
    ]
},
```

<br />

###### 3.3.1.4、output输出

&emsp;&emsp;告诉webpack输出bundles到哪里去以及如何命名。filename定义输出的bundle的名称，path输出目录是`__dirname+'dist'`，名字叫做`bundle.js`，publicPath可以是绝对路径或者相对路径，一般会以`/`结尾。`/assets/`表示网站根目录下assets目录下。

```javascript
output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js',
    publicPath: '/assets/'
},
```

<br />

###### 3.3.1.5、resolve解析

&emsp;&emsp;设置模块如何被解析。extensions自动解析扩展名。`.js`的意思是，如果用户导入模块的时候不带扩展名，它尝试补全。

```javascript
resolve: {
    extensions: ['.js']
},
```

<br />

###### 3.3.1.6、module模块

&emsp;&emsp;定义如何处理不同的模块，rules匹配请求的规则，以应用不同的加载器、解析器等。

```javascript
module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: [
                    { loader: 'react-hot-loader/webpack' },
                    { loader: 'babel-loader' }
                ]
            },
            {
                test: /\.less$/,
                use: [
                    { loader: "style-loader" },
                    { loader: "css-loader" },
                    { loader: "less-loader" }
                ]
            }
        ]
},
```

以上参数解释如下：

*   `test`：匹配条件，JS使用两个斜线代表模式匹配。

*   `exclude`：排除选项，打包时排除`/node_modules/`目录。这一句一定要有，否则，编译时就把这个目录下的所有文件也打包进来了，巨大无比。

*   `use`：使用模块的`UseEntries`列表中的loader。

合起来的意思就是对`.js`结尾的但不在`node_modules`目录的文件使用转译`babel-loader`和热加载loader。

<br />

CSS和LESS加载器

*   `style-loader`：通过`<style>`标签把css添加到DOM中。
*   `css-loader`：加载css。
*   `less-loader`：对less的支持。

<br />

###### 3.3.1.7、less

&emsp;&emsp;CSS好处是简单易学，但是坏处是没有模块化、复用的概念，因为它不是语言。LESS是一门CSS的预处理语言，扩展了CSS ，增加了变量、Mixin、函数等开发语言的特性。

&emsp;&emsp;LESS本身是一套语法规则及解析器，将写好的LESS解析成CSS。LESS可以使用在浏览器端和服务器端。

```less
@color: #4D926F;

#header {
	color: @color;
}

h2 {
	color: @color;
}
```

&emsp;&emsp;可以使用解析成如下的CSS

```css
#header {
	color: #4D926F;
}

h2 {
	color: #4D926F;
}
```

&emsp;&emsp;LESS在服务器端使用，需要使用LESS编译器，`npm install less`，本项目目录已经安装过了。

```shell
$ node_modules/.bin/lessc test.less
$ node_modules/.bin/lessc test.less test.css
```

<br />

###### 3.3.1.8、plugins

&emsp;&emsp;webpack的插件，`HotModuleReplacementPlugin`：表示开启HMR，`DefinePlugin`：全局常量配置。

```javascript
plugins: [
    new webpack.optimize.OccurrenceOrderPlugin(true),
    new webpack.HotModuleReplacementPlugin(),
    new webpack.DefinePlugin({'process.env': {NODE_ENV: JSON.stringify('development')}})
],
```

<br />

###### 3.3.1.9、devServer 

&emsp;&emsp;开发用的web server，`compress`：启动gzip，`port`：启动的访问端口为3000，`hot`：启用HMR，`proxy`：指定访问`/api`开头路径都代理到`http://127.0.0.1:8080`去。

```javascript
devServer: {
        compress: true,
        port: 3000,
        publicPath: '/assets/',
        hot: true,
        inline: true,
        historyApiFallback: true,
        stats: {
            chunks: false
        },
        proxy: {
            '/api': {
                target: 'http://127.0.0.1:8080',
                changeOrigin: true
            }
        }
}
```

<br />

#### 3.4、vscode配置

&emsp;&emsp;`jsconfig.json`是vscode的配置文件，覆盖当前配置。

&emsp;&emsp;以上是所有配置文件的解释。拿到这个文件夹后，需要修改**name, version, description ,repository,author和license**信息。这些信息修改好之后，就可以开始开发了。

<br />

### 4、启动项目

&emsp;&emsp;在项目根目录运行`npm start`：

```shell
jason@DESKTOP-4MCCUTP MINGW64 /e/react-mobx-starter-master
$ npm start

> react-mobx-starter@1.0.0 start
> webpack-dev-server --config webpack.config.dev.js --hot --inline

Project is running at http://localhost:3000/
webpack output is served from /assets/
404s will fallback to /index.html
Hash: 717d4c182a54af1f7ddb
Version: webpack 2.7.0
Time: 1941ms
        Asset     Size  Chunks                    Chunk Names
    bundle.js  1.57 MB       0  [emitted]  [big]  app
bundle.js.map  1.93 MB       0  [emitted]         app
webpack: Compiled successfully.
```

&emsp;&emsp;启动成功应该就可以访问了，webpack使用babel转译、打包，较为耗时，需要等一会儿。

![image-20211231100511257](https://gitee.com/jasonzhao86/blog-pics/raw/master/images/image-20211231100511257.png)

