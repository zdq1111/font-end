# webpack# webpack

## 什么是webpack

### 模块化是指解决一个复杂问题时自顶向下逐层把系统划分成若干模块的过程，有多种属性，分别反映其内部特性。

模块化一般指的是可以被抽象封装的最小/最优代码集合，模块化解决的是功能耦合问题；组件化则更像是模块化进一步封装，根据业务特点或者不同的场景封装出具有一定功能特性的独立整体；

前端提到组件化更多的是具有模板、样式和 js 交互的 UI 组件。

### 模块化规范

- CommonJS

	- Nodejs 同步加载模块依赖 require、exports、module
	- 浏览器环境没法使用，后来通过Browserify和webpack处理

- AMD

	- RequireJS 浏览器内支持，异步加载模块 id、dependence、factory、require
	- npm包管理机制导致其可能逐步淘汰
	- 模块依赖前置加大开发难度

- ES6 Module

	- ES6 import、export
	- Node.js 9+原生支持 --experimental-modules

- CMD(SeaJS) UMD(兼容CommonJS和AMD)

### 工程化

1. 模块多了，依赖管理怎么做；
2. 页面复杂度提升之后，多页面、多系统、多状态怎么办；
3. 团队扩大之后，团队合作怎么做；
4. 怎么解决多人研发中的性能、代码风格等问题；
5. 权衡研发效率和产品迭代的问题。

### 对比grunt、gulp

- 遍历文件 --》匹配规则 --》 打包
- 入口文件 --》 模块依赖加载、分析 --》打包

### 解决哪些问题

- 模块化打包，一切皆模块，JS 是模块，CSS 等也是模块
- 语法糖转换：比如 ES6 转 ES5、TypeScript；
- 预处理器编译：比如 Less、Sass 等；
- 项目优化：比如压缩、CDN；
- 解决方案封装：通过强大的 Loader 和插件机制，可以完成解决方案的封装，比如 PWA；
- 流程对接：比如测试流程、语法检测等。

## 开发环境搭建

### Node.js

- nvm版本管理 推荐8.9以上版本

### npm

- 规范:  主版本号.次版本号.修订号  MAJRO.MINOR.PAHCH

	- 主版本号：当你做了不兼容的 API 修改；
	- 次版本号：当你做了向下兼容的功能性新增；
	- 修订号：当你做了向下兼容的问题修正

- package.json

	- name、@scope/name作用域包命名
	- dependencies
	- devdependencies

		- webpack构建工具

## webpack配置

### webpack.config.js

- webpack 是一个模块打包工具，能够从一个需要处理的 JavaScript 文件开始，构建一个依赖关系图（dependency graph），该图映射到了项目中每个模块，然后将这个依赖关系图输出到一个或者多个 bundle 中。


### 配置类型

- 对象
- 函数 （env, argv） => {}

	- 环境对象
	- Webpack-CLI 的命令行选项

- promise
- 多配置数组 打包多次

### 配置项

- entry 项目入口
- module 开发中每一个文件都可以看做 module，模块不局限于 js，也包含 css、图片等 
- chunk 代码块，一个 chunk 可以由多个模块组成
- loader 模块转化器，模块的处理器，对模块进行转换处理
- plugin  扩展插件，插件可以处理 chunk，也可以对最后的打包结果进行处理，可以完成 loader 完不成的任务
- bundle  最终打包完成的文件，一般就是和 chunk 一一对应的关系，bundle 就是对 chunk 进行便意压缩打包等处理后的产出

### context

- 项目打包的相对路径上下文
- 是一个绝对路径，默认为 process.cwd() 即工作目录

### entry

- 单文件入口

	- string  该 string 指定的模块（文件）作为入口模块
	- 数组 [string]  自动生成另外一个入口模块，并将数组中每个元素指定的模块（文件）加载进来，并将最后一个模块的 module.exports 作为入口模块的 module.exports 导出 

- 多文件入口

	- 多页应用、页面模块分离优化

### output

- path  此选项制定了输出的 bundle 存放的路径，比如 dist 、 output 等
- filename 这个是 bundle 的名称

	- [name].js 占位符 对应entry的key

- publicPath 指定了一个在浏览器中被引用的 URL 地址

	- 将静态文件放在不同的域名或者 CDN 上面

- 不指定 output 的时候，默认输出到 dist/main.js 
- library

	- 供别人使用的库

- libraryTarget

	- 指定库打包出来的规范 var 、 assign 、 this 、 window 、 global 、 commonjs 、 commonjs2 、 commonjs-module 、 amd 、 umd 、 umd2 、 jsonp

- 相关配置项

	- externals

		- 去除输出的打包文件中依赖的某些第三方 js 模块（例如 jquery ， vue 等等）常用于自定义开发库

	- target

		- 指定构建的目标（target）宿主环境不同

	- devtool

		- 控制怎么显示sourcemap（快速还原代码的错误位置）
		- 生产环境不使用或者使用 source-map（有 Sentry 这类错误跟踪系统）
		- 开发环境使用 cheap-module-eval-source-map

### resolve

- 帮助 Webpack 查找依赖模块
- extensions  解析扩展名的配置，默认值： ['.wasm', '.mjs', '.js', '.json']
- alias  以帮助 webpack 更快查找模块依赖

	- src: path.resolve(__dirname, 'src')
'@lib': path.resolve(__dirname, 'src/lib')
	- alias 的名字可以使用 @ ! ~ 等这些特殊字符，不要跟 npm 包的scope冲突

		-  vscode 中会导致我们检测不到 utils 中的内容

		  //jsconfig.json
		  {
		   	"compilerOptions": {
		   		"baseUrl": "./src",
		   		"paths": {
		   			"@lib/": ["src/lib"]
		   		}
		   	}
		  }

	- 给生产环境和开发环境配置不同的 lib 库

		- san: process.env.NODE_ENV === 'production' ? 'san/dist/san.min.js' : 'san/dist/san.dev.js'

	- 在名称末尾添加 $ 符号来缩小范围只命中以关键字结尾的导入语句，这样可以做精准匹配

		- react$: '/path/to/react.min.js'

- mainFields

	- 在不同的 target 下去决定使用不同版本的模块代码

### module

- 不同的模块需要使用不同类型的模块处理器来处理
- noParse 忽略对部分没采用模块化的文件的递归解析和处理

	- noParse: /jquery|lodash/

- rules

	- parser 来控制模块化语法更细粒度的配置哪些模块语法要解析，哪些不解析 语法层面的限制
	- 条件匹配

		- 通过 test 、 include 、 exclude 等配置来命中可以应用规则的模块文件

			- resoucce

				- 请求文件的绝对路径

			- resourceQuery
			- issuer

				- 被请求资源（requested the resource）的模块文件的绝对路径，即导入时的位置

	- 应用规则

		- 对匹配条件通过后的模块，使用 use 配置项来应用 loader ，可以应用一个 loader 或者按照从后往前
的顺序应用一组 loader，当然我们还可以分别给对应 loader 传入不同参数
		- js中使用loader

			- const html = require('html-loader!./loader.html');   ! 隔开的命令是从右到左解析的

		- loader传参 options query

	- 重置顺序

		- 一组 loader 的执行顺序默认是**从后到前（或者从右到左）**执行，通过 enforce 选项可以让其中一个 loader 的执行顺序放到最前（pre）或者是最后（post）

### plugin

- loader 面向的是解决某个或者某类模块的问题，而 plugin 面向的是项目整体
- 内置plugin、NPM包引入

## webpack模块化

### 一切皆模块

### Module的增强

- import()

	- 返回的promise对象
	- 动态打包语法，模块异步加载，代码分割

- 神奇注释

  import(
  	 /*
   	webpackChunkName: 'lazy-name'
   	*/
   	'./lazy'
  ).then(lazy => {
  	 console.log(lazy);
  });

	- webpackInclude

		- 如果是 import 的一个目录，则可以指定需要引入的文件特性

	- webpackExclude

		- 如果是 import 的一个目录，则可以指定需要过滤的文件

	- webpackChunkName

		- 这是 chunk 文件的名称

	- webpackPrefetch

		- 是否预取模块，及其优先级，可选值 true 、或者整数优先级别，0 相当于 true，webpack4.6+支持

	- webpackPreload

		- 是否预加载模块，及其优先级，可选值 true 、或者整数优先级别，0 相当于 true，webpack4.6+支持

	- webpackMode

		- lazy

			- 为每个 import() 导入的模块，生成一个可延迟加载 chunk

		- lazy-once

			- 生成一个可以满足所有 import() 调用的单个可延迟加载 chunk，此 chunk 将在第一次 import() 调用时获取，随后的 import() 调用将使用相同的网络响应

		- eager

			- 不会生成额外的 chunk，所有模块都被当前 chunk 引入，并且没有额外的网络请求。仍然会返回Promise，但是是 resolved 状态。和静态导入相对比，在调用 import() 完成之前，该模块不会被执行。

		- weak

			- 尝试加载模块，如果该模块函数已经以其他方式加载（即，另一个 chunk 导入过此模块，或包含模块的脚本被加载）。仍然会返回 Promise，但是只有在客户端上已经有该 chunk 时才成功解析。如果该模块不可用，Promise 将会是 rejected 状态，并且网络请求永远不会执行

### require.include(dependency)

- 引入某个依赖，但是并不执行它，可以用于优化 chunk

### require.resolve() 和 require.resolveWeak()

### require.context()

### webpack对Node.js模块的polyfill

- 在 JavaScript 中表示一些可以抹平浏览器实现差异的代码，比如某浏览器不支持 Promise，可以引入 es6-promise-polyfill 等库来解决
- Node.js 的 querystring 模块，引入的是node-libs-browser来对 Node.js 核心库 polyfill

### 资源的模块化处理

- 样式文件的 @import 和JavaScript 中的 import
- CSS Modules 语法 配置 loader

## Babel 转换 JavaScript 代码

### Babel 是 JavaScript 的编译器，通过 Babel 可以将我们写的最新 ES 语法的代码轻松转换成任意版本的 JavaScript语法

### Babel CLI

- @babel/cli 、@babel/core 和 @babel/preset-env

### 配置文件

- 使用 package.json 的 babel 属性

  "babel": {
   	"presets": ["@babel/preset-env"]
   }

- 在项目根目录单独创建 .babelrc 或者 .babelrc.js 文件

### Babel 的插件和 Preset

- 转换插件

	- 箭头函数转换为 ES5 函数， 安装 @babel/plugin-transform-arrow-functions 插件

- 解析插件

	- 解析 jsx 这类 React 设计的特殊语法，则需要对应的 jsx插件

- preset

	- 插件预设，插件组合更加好理解一些

		- @babel/preset-env

### Babel polyfill

- ES5 中，有些对象、方法实际在浏览器中可能是不支持  Promise 、 Array.prototype.includes
- @babel/polyfill  在对应的文件内显性的引入

	- 直接修改内置的原型，造成全局污染
	- 无法按需引入，Webpack 打包时，会把所有的 Polyfill 都加载进来，导致产出文件过大

### Bable runtime

- @babel/polyfill 会产生一个 window.Promise 对象，@babel/runtime 则会生成一个 _Promise 来替换掉我们代码中用到的 Promise
- 安装依赖 @babel/runtime
- 安装 npm i -D @babel/plugin-transform-runtime 作为 Babel 插件
- 安装需要转换 Object.assign 的插件： npm i -D @babel/plugin-transform-object-assign
- 由于 @babel/runtime 不修改原型，所以类似 [].includes() 这类使用直接使用原型方法的语法是不能被转换的

### @babel/preset-env精细化的使用

- useBuiltIns设置浏览器 polyfill

	- ["@babel/preset-env", {"useBuiltIns": "usage|entry|false"}]
	- usage 表示明确使用到的 Polyfill 引用,每个需要用到Polyfill 的引用时，会自动加上
	- entry 表示替换 import "@babel/polyfill"

- target为了目标浏览器或者对应的环境（browser/node）

### @babel/plugin-transform-runtime

- 每个需要转换的代码前面都会插入一些 helpers 代码，这可能会导致多个文件都会有重复的 helpers 代码

### Browserslist帮助我们来设置目标浏览器的工具

- 声明了一段浏览器的集合，我们的工具可以根据这段集合描述，针对性的输出兼容性代码。
- 配置package.json/.browserslistrc

	- "browserslist": ["last 2 version", "> 1%", "maintained node versions", "not ie < 11"]

### 最佳实践

// .babelrc
{
 "plugins": [
 [
 "@babel/plugin-transform-runtime",
 {
 "corejs": false, // 默认值，可以不写
 "helpers": true, // 默认，可以不写
 "regenerator": false, // 通过 preset-env 已经使用了全局的 regeneratorRuntime, 不再需要 transform-runtime 提供
的 不污染全局的 regeneratorRuntime
 "useESModules": true // 使用 es modules helpers, 减少 commonJS 语法代码
 }
 ]
 ],
 "presets": [
 [
 "@babel/preset-env",
 {
 "targets": {}, // 这里是targets的配置，根据实际browserslist设置
 "corejs": 3, // 添加core-js版本
 "modules": false, // 模块使用 es modules ，不使用 commonJS 规范
 "useBuiltIns": "usage" // 默认 false, 可选 entry , usage
 }
 ]
 ]

## Babel原理

###  JavaScript 的静态分析编译器，不需要执行代码的前提下对代码进行分析和处理的过程

### 解析：指的是首先将代码经过词法解析和语法解析，最终生成一颗 AST（抽象语法树），在 Babel 中，语法解析器是 Babylon

// index.js
const fs = require('fs');
const babel = require('@babel/core');
const traverse = require('@babel/traverse').default;
// 读取 source.js内容
let source = fs.readFileSync('./source.js');
// 使用 babel.parse方法
babel.parse(source, (err, ast) => {
 	// ast就是树
 	console.log(ast);
});

### 转换：得到 AST 之后，可以对其进行遍历，在此过程中对节点进行添加、更新及移除等操作，Babel 中 AST 遍历工具是 @babel/traverse

const fs = require('fs');
const babel = require('@babel/core');
const traverse = require('@babel/traverse').default;
let source = fs.readFileSync('./source.js');
babel.parse(source, (err, ast) => {
 // console.log(ast)
 let indent = '';
 traverse(ast, {
 // 进入节点
 enter(path) {
 console.log(indent + '<' + path.node.type + '>');
 indent += ' ';
 },
 // 退出节点
 exit(path) {
 indent = indent.slice(0, -2);
 console.log(indent + '<' + '/' + path.node.type + '>');
 }
 });
});

### 生成：经过一系列转换之后得到的一颗新树，要将树转换成代码，就是生成的过程，Babel 用到的是 @babel/generator

const fs = require('fs');
const babel = require('@babel/core');
const traverse = require('@babel/traverse').default;
const gen = require('@babel/generator').default;
let source = fs.readFileSync('./source.js');
babel.parse(source, (err, ast) => {
 // console.log(err, ast)
 let indent = '';
 traverse(ast, {
 // 一顿操作猛如虎。。
 });
 // 生成新的 ast，然后使用generator生成 code
 console.log(gen(ast).code);
});

### AST语法树

- 抽象语法树（Abstract Syntax Tree，AST）源代码语法结构的一种抽象表示，不会表示出真实语法中出现的每个细节
- AST 是经过词法解析和语法解析两个步骤解析出来 https://astexplorer.net/

## webpack样式配置

### css-loader

### style-loader

- CSS 代码以 <style> 标签的形式插入到 HTML 文件中

### mini-css-extract-plugin

- CSS 以 <link> 的方式通过 URL 的方式引入
- 需要分别配置 loader 和 plugin

### CSS Modules

- 所有的 CSS 类名及其动画名都只是局部作用域的 CSS 文件

	- 解决 CSS 类都是全局的，容易造成全局污染（样式冲突）
	- JS 和 CSS 共享类名, JS 可以直接使用 CSS 的类名作为对象值
	- 可以方便的编写出更加健壮和扩展方便的 CSS

- CSS 命名约定的BEM、OOCSS;  React 中使用的用 JavaScript 来写 CSS 规则的 CSS in JS 方案
-  css-loader 增加 modules 的选项

### CSS 预处理器

- Less，Sass 及其语法变种 Scss和Stylus
- 使用预处理器 loader

	-  sass-loader，需要同时安装 node-sass

### PostCSS：CSS 后处理器

- 包括添加前缀、最新语法转义、压缩等，甚至可以扩展 CSS 的语言特性

	- 根据适配的浏览器使用 Autoprefixer 插件自动添加不同浏览器的适配

- 使用 JavaScript 插件来转换 CSS 的工具，PostCSS 核心是将 CSS 解析成 AST，然后通过各种插件做各种转换，最终生成处理后的新 CSS
- postcss-loader在 css-loader 之前
- 配置

	- 通过配置文件 postcss.config.js ，一般放置在项目的根目录下
	- 通过 loader 的配置项 options
	- 直接在 package.json 中添加个 postcss 属性

- postcss插件

	- Autoprefixer

		- 是给 css 补齐各种浏览器私有的前缀，主要参数就是 browserslist

	- postcss-preset-env

		- 通过它可以安心的使用最新的 CSS 语法来写样式，不用关心浏览器兼容性

	- PreCSS

		- 变量定义方式

	- cssnano

		-  CSS 压缩优化

## webpack lint

### ESLint

- Airbnb代码规范
- JavaScript Standard Style Guide
- Google JavaScript代码规范
- 百度FECS
- CLI 工具 eslint 初始化.eslintrc.json

### eslint-loader

{
test: /\.js$/,
loader: 'eslint-loader',
enforce: 'pre',
include: [path.resolve(__dirname, 'src')], // 指定检查的目录
options: { // 这里的配置项参数将会被传递到 eslint 的 CLIEngine
formatter: require('eslint-friendly-formatter') // 指定错误报告的格式规范
}
}

- enforce: 'pre' 来调整了 loader 加载顺序，保证先检测代码风格

### StyleLint

- stylelint-config-recommended
- stylelint-config-standard
- stylelint-order 插件

	- 强制我们在写 CSS 的时候按照某个顺序来编写

- .stylelintrc.json

### StyleLint-webpack-plugin

- emitErrors ：默认是 true ，将遇见的错误信息发送给 webpack 的编辑器处理
- failOnError ：默认是 false ，如果是 true 遇见 StyleLint 报错则终止 Webpack 编译

## webpack管理静态资源

### loader加载图片

- file-loader

	- 能够根据配置项复制使用到的资源（不局限于图片）到构建之后的文件夹，并且能够更改对应的链接

- url-loader

	- 包含 file-loader 的全部功能，并且能够根据配置将符合配置的文件转换成 Base64 方式引入,以减少 http 请求
	- 优先是将资源 Base64 引入, limit 选项来控制

### 配置 CDN 域名

- output.publicPath

### HTML 和 CSS 中使用 alias

- 使用 alias 必须要前面添加 ~
- <img> 引入图片等静态资源的时候，需要添加 html-loader 配置

### svg-url-loader

- 利用 URL encoding编码

### 图片优化

- img-webpack-loader

	- enforce: 'pre' 提高 img-webpack-loader 的优先级

### CSS Sprite 雪碧图

- 将页面用到的小图片合并到一张大图中，然后使用 background-position 重新定位
- postcss-sprites

	- 安装需要安装phantomjs可能需要正确上网

### 其他资源处理

- 字体、富媒体等静态资源，可以直接使用 url-loader 或者 file-loader 进行配置即可
- 数据

	- 使用csv-loader和xml-loader

## webpack打包HTML 和多页面

### HTML插件

- html-webpack-plugin

	- Template

		- 自定义index.html
		- JavaScript模板引擎

			- html-loader
			- pug-html-loader

### 多页项目配置

- 多页面问题

	- 多次实例化 html-webpack-plugin

- 多入口问题

	-  html-webpack-plugin

		- chunks
		- excludeChunks

### 多页应用最佳实践

- 目录结构规范

	- template和entry名字对应

- Node.js遍历出entry和html-webpack-plugn内容

## webpack dev server本地开发

### 基于express的本地开发服务器， webpack-dev-middle中间件为通过 Webpack 打包生成的资源文件提供 Web 服务，通过 Socket IO 连接着 webpack-dev-server 服务器的小型运行时程序

### 后将文件打包到内存中，Webpack 会打包到硬盘上

### 自动刷新，支持模块热替换

- iframe 模式

	- 页面被放到一个 iframe 内，当发生变化时，会重新加载

- inline 模式

	- 将 webpack-dev-server 的重载代码添加到产出的 bundle 中

- webpack-dev-server --hot --inline 是开启 inline 模式的自动刷新

### webpack内置插件 devServer

### Hot Module Replacement

- webpack.HotModuleReplacementPlugin
- 设置 devServer.hot=true 

	- entry 添加 webpack/hot/dev-serve 或者 webpack/hot/only-dev-serve 实现 HMR 的服务端代码

- 设置 devServer.inline=true

	- 会给 entry 添加 webpack-dev-server/client ，这是通信客户端

-  在 webpack.config.js 中添加 plugins： new webpack.HotModuleReplacementPlugin()
- 修改入口文件添加 HMR 支持代码

  /
  // 在入口文件index.js最后添加如下代码
  if (module.hot) {
  // 通知 webpack 该模块接受 hmr
  module.hot.accept(err => {
  if (err) {
  console.error('Cannot apply HMR update.', err);
  }
  })

### proxy

- 解决本地开发跨域的问题

	- http-proxy-middleware中间件

### 自定义中间件

- 在 devServer.before 和 devServer.after 两个时机
- mock server

### 服务开启 Gzip 压缩

- devServer.compress

## 配置Vue开发环境

### vue-cli

### 创建目录、初始化 package.json、安装 Vue、安装 Webpack 和安装 Babel

### vue-loader html-webpack-plugin

### 开启HMR

### 开发环境/生产环境

- 区别

	- 生产环境可能需要分离 CSS 成单独的文件，以便多个页面共享同一个 CSS 文件
	- 生产环境需要压缩 HTML/CSS/JS 代码
	- 生产环境需要压缩图片
	- 开发环境需要生成 SourceMap 文件
	- 开发环境需要打印 debug 信息
	- 开发环境需要 HMR、devServer 等功能

- 按环境划分 Webpack 配置文件

	- webpack.config.js ：所有环境的默认入口配置文件
	- webpack.base.js ：基础部分，即多个文件中共享的配置

		- 环境变量

			- 使用环境变量，例如 cross-env + NODE_ENV
			-  使用 Webpack 配置文件的 function 方式

	- webpack.development.js ：开发环境使用的配置

		- hmr、devServer、devtool

	- webpack.production.js ：生产环境使用的配置

		- optimization 、 devtool

	- webpack.parts.js： 将零件配置进行拆分

		- 获取 css-loader 配置

		  exports.getCssLoader = ({mode, test = /\.css$/, include, exclude, uses = []} = {}) => ({
		  test,
		  include,
		  exclude,
		  use: [
		  {
		  loader: mode === 'production' ? MiniCssExtractPlugin.loader : 'style-loader'
		  },
		  {
		  loader: 'css-loader'
		  }
		  ].concat(uses)
		  });

		- 获取 devServer 配置

		  exports.getDevServerConfig = ({host = '0.0.0.0', port = 8888} = {}) => ({
		  stats: 'errors-only',
		  host,
		  port,
		  open: true,
		  overlay: true
		  });

		- 获取 url-loader 配置

		  exports.getUrlLoader = ({test, largeAssetSize = 1000, assetsDir, dir} = {}) => ({
		  test,
		  use: {
		  loader: 'url-loader',
		  options: {
		  limit: largeAssetSize,
		  name: getAssetPath(assetsDir, `${dir}/[name]${isProduction ? '.[hash:8]' : ''}.[ext]`)
		  }
		  }
		  });

## 体积优化

### JavaScript 压缩

- terser-webpack-plugin

	- UglifyJS 在 ES6 代码压缩上做的不够好
	- Tree-Shaking 也是依赖这个插件
	- 配置删除无用的代码
	- 多线程压缩

- 合理划分代码职责，适当使用按需加载方案
- 善用 webpack-bundle-analyzer 插件，帮助分析 Webpack 打包后的模块依赖关系
- 设置合理的 SplitChunks 分组
- 对于一些 UI 组件库，例如 AntDesign、ElementUI 等，可以使用bable-plugin-import这类工具进行优化
- 使用 lodash、momentjs 这类库，不要一股脑引入，要按需引入，momentjs 可以用 date-fns 库来代替
- 合理使用 hash 占位符，防止 hash 重复出现，导致文件名变化从而 HTTP 缓存过期
- 合理使用 polyfill，防止多余的代码
- 使用 ES6 语法，尽量不使用具有副作用的代码，以加强 Tree-Shaking 的效果
- 使用 Webpack 的 Scope Hoisting（作用域提升）功能

	- 作用域提升（Scope Hoisting）是指 webpack 通过 ES6 语法的静态分析，分析出模块之间的依赖关系，尽可能地把模块放到同一个函数中。
	- optimization.concatenateModules=true

### css

- CSS 导出

	- 单独文件，通过 style-loader 的 addStyles导入js中
	- mini-css-extract-plugin

- CSS 压缩：cssnano

	- 删除空格和最后一个分号；删除注释；优化字体权重；丢弃重复的样式规则；压缩选择器；减少手写属性；合并规则
	- css-loader 已经集成了 cssnano
	- optimize-css-assets-webpack-plugin

### 图片资源优化

- url-loader、svg-url-loader和 image-webpack-loader 来优化
- 雪碧图

## 优化之增强缓存命中率

### 浏览器缓存策略和 Webpack 缓存相关配置

- 将静态资源（JavaScript 、CSS、图片字体文件等）不经常变动的文件寻找更合适的服务器（CDN）存放
- 保证动静分离，将动态页面和静态资源分开部署有利于服务的更好维护
- CDN 可以更加接近用户的终端，提供加速服务，详细可以查看 CDN 的原理
-  静态资源不会具有动态逻辑，单独的域名可以减少页面请求中的 Cookie 等不必要字段，减少 HTTP 带宽
- HTTP 缓存相关协议

	- Cache-Control: max-age=31536000

- 针对发生了变更的静态资源进行重命名

	- 哈希值（hash）

		- chunkhash根据不同的 chunk 及其包含的模块计算出来的 hash，chunk 中包含的任意模块发生变化，则 chunkhash 发生变化
		- contenthashCSS 发生变化则其值发生变化

- 动态加载的 JavaScript 文件 使用 preload 方式来提前预取
- 包含所有内容的清单文件类似 Application Cache或 者 PWA方案

	- webpack-manifest-plugin
	- 包含了原文件名和带哈希文件名的映射

### 将依赖和 Runtime 提取到单独的文件中

- 将变更频率低的这些依赖模块移到单独的文件

	- 将输出文件名替换为 [name].[chunkname].js
	- 我们要将 entry 的值改为对象
	- 设置 optimization.splitChunks 来划分不同的代码

- Webpack 的 Runtime

	- optimization.runtimeChunk
	- 把 Webpack 的 Runtime 代码内联到 HTML节省 HTTP 请求

		- html-webpack-inline-source-plugin

### 合理使用动态加载功能来拆分代码

- 通过 import() 或者 require.ensure() 方式来引入模块
- 不是重要的逻辑或者更加深入操作才能触发的代码逻辑拆分开，保证优先加载首页相关的代码

### 多页面项目按照路由拆分代码

- Vue、React 这类多路由页面，也可以通过 import() 方式来动态加载模块

## splitChunks 功能来拆分代码

### Webpack4 之前使用 CommonsChunkPlugin

### webpack代码拆分方式

- entry 配置：通过多个 entry 文件来实现
- 动态加载（按需加载）：通过写代码时主动使用 import() 或者 require.ensure 来动态加载
- 抽取公共代码：使用 splitChunks 配置来抽取公共代码

### module：就是 JavaScript 的模块，简单来说就是你通过 import 、 require 语句引入的代码，也包括 css、图片等资源

### chunk: chunk 是 webpack 根据功能拆分出来的，chunk 包含着 module，可能是一对多也可能是一对一

### bundle：bundle 是 webpack 打包之后的各个文件，一般就是和 chunk 是一对一的关系，bundle 就是对 chunk 进行编译压缩打包等处理之后的产出。

### Webpack 做到了开箱即用

module.exports = {
// ...
optimization: {
splitChunks: {
chunks: 'async', // 三选一： "initial" | "all" | "async" (默认)
minSize: 30000, // 最小尺寸，30K，development 下是10k，越大那么单个文件越大，chunk 数就会变少（针对于提取公共 chunk
的时候，不管再大也不会把动态加载的模块合并到初始化模块中）当这个值很大的时候就不会做公共部分的抽取了
maxSize: 0, // 文件的最大尺寸，0为不限制，优先级：maxInitialRequest/maxAsyncRequests < maxSize < minSize
minChunks: 1, // 默认1，被提取的一个模块至少需要在几个 chunk 中被引用，这个值越大，抽取出来的文件就越小
maxAsyncRequests: 5, // 在做一次按需加载的时候最多有多少个异步请求，为 1 的时候就不会抽取公共 chunk 了
maxInitialRequests: 3, // 针对一个 entry 做初始化模块分隔的时候的最大文件数，优先级高于 cacheGroup，所以为 1 的时候
就不会抽取 initial common 了
automaticNameDelimiter: '~', // 打包文件名分隔符
name: true, // 拆分出来文件的名字，默认为 true，表示自动生成文件名，如果设置为固定的字符串那么所有的 chunk 都会被合
并成一个
cacheGroups: {
vendors: {
test: /[\\/]node_modules[\\/]/, // 正则规则，如果符合就提取 chunk
priority: -10 // 缓存组优先级，当一个模块可能属于多个 chunkGroup，这里是优先级
},
default: {
minChunks: 2,
priority: -20, // 优先级
reuseExistingChunk: true // 如果该chunk包含的modules都已经另一个被分割的chunk中存在，那么直接引用已存在的c
hunk，不会再重新产生一个
}
}
}
}
}

- 默认配置对应第二种拆分方式
- chunks
- 使用 cacheGroups

	- 分组vendors

		- minSize、minChunks、name
		- test

			- 满足这个条件的才会被缓存组命中，取值可以是正则、字符串和函数
			- test为函数，参数module、chunks

				- module 对象包含了模块的基本信息，例如类型、路径、文件 hash 等
				- 当前模块被分到哪些 chunks 使用

		- priority

			- 有一个模块满足了多个缓存组的条件就会去按照权重划分

		- reuseExistingChunk

			- 是否使用已有的 chunk

	- 分组default

### splitChunks.chunks

- async

  splitChunks: {
    cacheGroups: {
      vendors: {
       chunks: 'async', // 这里是我们修改的地方，async|initial|all
       test: /[\\/]node_modules[\\/]/
      }
    }
  }

	- 只会针对动态引入的的代码进行拆分  imortt()或者require.ensure

- initail

	- 动态引入的模块不受影响，它是无论如何都会被拆分出去的
	- 同步引入的代码，如果有多处都在使用，则拆分出来共用， 次数通过minChunks配置

- all

	- 最大程度的生成复用代码，推荐

### splitChunks 也适用于使用mini-css-extract-plugin插件的 css 配置

- CSS 设置自己的 cacheGroup 规则

	- 设置更高权重的 cacheGroup
	- 使用 test 函数针对类型为 js 和 css 分别设置各自cacheGroup

## webpack速度优化

### loader和plugin构建、压缩

### 单纯升级 Webpack 版本、区分开发和生产环境两套配置，各司其职

### 减少查找过程

- resolve.alias别名
- resolve.extensions 优先查找

	- 常用到的文件后缀写在前面
	- 导入模块时，尽量带上文件后缀名

### 排除不需要解析的模块

- module.noParse

	- jQuery、ChartJS，它们体积庞大又没有采用模块化标准
	- 被忽略掉的文件里不应该包含 import 、 require 、 define 等模块化语句

### 合理配置 rule 的查找范围

{
// test 使用正则
test: /\.js$/,
loader: 'babel-loader',
// 排除路径使用数组
exclude: [path.resolve(__dirname, './node_modules')],
// 查找路径使用数组
include: [path.resolve(__dirname, './src')]
}

- 只在 test 和 文件名匹配中使用正则表达式
- 在 include 和 exclude 中使用绝对路径数组
- 尽量避免 exclude ，更倾向于使用 include

### 利用多线程提升构建速度

- Node.js 之上的 Webpack 是单线程模型，加入插件利用多核CPU
- 简单的项目会因为多线程打包浪费更多的 CPU 资源
- thread-loader

	- 将 loader 放置在一个 worker 池里面运行

- HappyPack

	- 多进程模型，来加速代码构建
	- 给 loader 配置使用 HappyPack 需要对应的 loader 支持

### 使用 webpack.DllPlugin 来预先编译

DLL 是动态链接库（Dynamic-link library）的英文缩写，最早是微软提出来的一种共享函数库概念，实
际就是将一些经常会共享的代码制作成 DLL 文档，当其他代码需要使用这些 DLL 中的代码时，Windows 操
作系统会将 DLL 文档加载到内存中。这里借用了 DLL 的概念，帮助 Webpack 使用者理解用途

- 日常业务代码开发的时候并不会升级版本或者修改内部代码
- 在一个额外独立的 Webpack 设置中创建一个只有公共库的 dll 文件

  // webpack.config.dll.js
  const webpack = require('webpack');
  // 这里是第三方依赖库
  const vendors = ['react', 'react-dom'];
  module.exports = {
  mode: 'production',
  entry: {
  // 定义程序中打包公共文件的入口文件vendor.js
  vendor: vendors
  },
  output: {
  filename: '[name].[chunkhash].js',
  // 这里是使用将 verdor 作为 library 导出，并且指定全局变量名字是[name]_[chunkhash]
  library: '[name]_[chunkhash]'
  },
  plugins: [
  new webpack.DllPlugin({
  // 这里是设置 mainifest.json 路径
  path: 'manifest.json',
  name: '[name]_[chunkhash]',
  context: __dirname
  })
  ]
  };

	- webpack.config.dll.js
	- 第三方库依赖打包到一个 bundle 的 dll 文件
	- manifest.json

		- 让 DllReferencePlugin 在 webpack.config.js 配置中映射到相关的依赖上

- 配合 webpack.DllReferencePlugin

  plugins: [
  new webpack.DllReferencePlugin({
  context: __dirname,
  // 这里导入 manifest配置内容
  manifest: require('./manifest.json')
  })
  ]

	- 在 webpack.config.js 中使用，读取manifest.json

- HTML 中不会主动引入 dll 的 vendor.js 文件

	- 使用add-asset-html-webpack-plugin插件
	- 在 dll 打包的时候就修改一下 html-webpack-plugin的 template 文件

### 缓存（Cache）相关

- babel-loader 配置

	- cacheDirectory 配置给 Babel 编译时给定的目录，并且将用于缓存加载器的结果，设置为 true
	- 用 exclude 和 include 来尽可能准确的指定要转换内容的范畴

### sourceMap 生成耗时严重，根据之前 sourceMap[TODO](sourcemap 表格链接)表格选择合适的 devtool 值

### 切换一些 loader 或者插件，比如：fast-sass-loader可以并行处理 sass 文件，要比 sass-loader 快 5~10 倍

### 压缩速度优化

- 生产环境打包才会做

	- 添加 cache 和多线程支持
	- terser-webpack-plugin配置

	  optimization: {
	  minimizer: [
	  new TerserPlugin({
	  cache: true, // 开启缓存
	  parallel: true // 多线程
	  })
	  ]
	  }

## webpack的Tree-Shaking

### 移除 JavaScript 上下文中没用的代码，这样可以有效地缩减打包体积

你可以将应用程序想象成一棵树。绿色表示实际用到的源码和 library，是树上活的树叶。灰色表示无用的代码，是秋天树上枯萎的树叶。为了除去死去的树叶，你必须摇动这棵树，使它们落下。

### 无用代码消除（Dead Code Elimination）

- 程序中没有执行的代码，如不可能进入的分支， return 之后的语句等
- 导致 dead variable 的代码，写入变量之后不再读取的代码

### Tree-Shaking 则更关注于消除没有用到的代码

- ES6 Modules 静态语法解析

	- 不执行代码，从字面量上对代码进行分析

- ES6 中 import 和 export 是显性声明的
- import 的模块名只能是字符串常量
- ES6 模块的依赖关系是可以根据 import 引用关系推导出来的
- ES6 模块的依赖关系与运行时状态无关

### 是需要配合 mode=production 来使用的

- Webpack 自己来分析 ES6 Modules 的引入和使用情况，去除不使用的 import 引入

	- 去掉了无用的引用

-  借助工具（如 uglifyjs-webpack-plugin 和 terser-webpack-plugin ）进行删除，这些工具只在 mode=production 中会被使用

	- 删除无用的代码

### Tree-Shaking 并不是银弹

- 代码必须遵循 ES6 的模块规范

	- 库或者类库都提供了 ES6 语法的库

- 只处理顶层内容，例如类和对象内部都不会再被分别处理

	- class内无用的方法没去掉

- 副作用（Side Effect）代码不会被 Tree-Shaking 删除

	- 纯函数：对于相同的输入就有相同的输出，不依赖外部环境，也不改变外部环境
	- 函数内调用外部方法、直接使用全局对象、直接修改原型

- 代码中消除副作用
- package.json配置 sideEffects

  // package.json
  {
  "name": "tree-shaking-side-effect",
  "sideEffects": ["./src/utils.js"]
  }

- 按需引入模块，避免「一把梭」

	- 使用 lodash 的 isNumber ，可以使用 import isNumber from 'lodash-es/isNumber'; ，而不是 import {isNumber} from 'lodash-es'
	- AntDesign 和 ElementUI 这些组件库，本身自己开发了 Babel 的插件，通过插件的方式来按需引入模块，避免一股脑的引入全部组件

## 占位符

[hash] ：是整个项目的 hash 值，其根据每次编译内容计算得到，每次编译之后都会生成新的 hash，即修改任
何文件都会导致所有文件的 hash 发生改变；在一个项目中虽然入口不同，但是 hash 是相同的；hash 无法实现
前端静态资源在浏览器上长缓存，这时候应该使用 chunkhash；

[chunkhash] ：根据不同的入口文件（entry）进行依赖文件解析，构建对应的 chunk，生成相应的 hash；只要组
成 entry 的模块文件没有变化，则对应的 hash 也是不变的，所以一般项目优化时，会将公共库代码拆分到一
起，因为公共库代码变动较少的，使用 chunkhash 可以发挥最长缓存的作用；

[contenthash] ：使用 chunkhash 存在一个问题，当在一个 JS 文件中引入了 CSS 文件，编译后它们的 hash 是
相同的。而且，只要 JS 文件内容发生改变，与其关联的 CSS 文件 hash 也会改变，针对这种情况，可以把 CSS
从 JS 中使用mini-css-extract-plugin 或 extract-text-webpack-plugin抽离出来并使用 contenthash。

### [hash] ：模块标识符的 hash

### [chunkhash] chunk 内容的 hash

### [name] 模块名称

### [id] 模块标识符

### [query] 模块的 query，例如，文件名 ? 后面的字符串

### [function] 一个 return 出一个 string 作为 filename 的函数

*XMind: ZEN - Trial Version*
