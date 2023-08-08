---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
transition: slide-left
title: Welcome to Slidev
---

# 现代化前端开发

modernized fontend development

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
---
# 什么是前端开发
<v-clicks>

- HTML/CSS/JavaScript
- jQuery
- Web浏览器
- 切图仔
- 一定会写CSS
- ......

</v-clicks>
--- 

# jQuery时期

<v-clicks>

- 前端编写HTML,CSS
- 前端通过script标签在html中引入jQuery类库以及jQuery插件实现功能
- 前端将HTML，CSS，JS文件给到后端，后端将对应的HTML转成PHP/ASP/JSP, 并且将对应的静态资源存储在后端项目中
- 有些激进的团队，会将所有数据都通过ajax进行加载，将HTML,CSS,JS等静态资源发布到独立的服务器上，实现前后端的分离

</v-clicks>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
# jQuery的影响
- 降低了前端门槛，让更多人进入了前端
- 促进了浏览器的发展(document.querySelectAll/promise等)
- 提供了开发效率,节省了开发时间 
---

# jQuery问题
<v-clicks>

- 全局污染. jQuery或其它前端类库都并没有提供一套模块化规范
  - 各类库通过暴露全局变量的方式，供其它类库依赖
  - 依赖关系靠JS文件加载顺序维护
  - 在大型项目中难以维护和理解
- 性能. 一个页面，可能会塞满script标签. script的加载，会影响页面的解析与呈现，导致白屏问题
- 可维护性. jQuery命令式的编程范式，对于编写可维护的大型前端应用,有一定的难度

</v-clicks>

---
transition: slide-up
---

# NodeJS出现
<v-clicks>

- 08年V8引擎开源
- 09年NodeJS
- 10年npm

</v-clicks>
---

# NodeJS对于前端的意义

<v-clicks>

- 让JavaScript可以脱离浏览器运行
- 前端可以使用JS构建前端工具链

</v-clicks>
---

# 命运的齿轮⚙️开始转动
<v-clicks>

- 移动互联网高速发展
  - 前端需求增多
  - 前端业务复杂度增加
- jQuery等类库降低了开发门槛
  - 吸引了大量人员从事前端
- NodeJS出现
  - 前端可以使用JS构建前端工具链

</v-clicks>

---
layout: image-right
image: /unsplash_code.jpg
---
# 模块化
- <Link to="8" title="CommonJS" />
- <Link to="12" title="AMD" />
- <Link to="" title="UMD" />
- <Link to="" title="ESM" />

<style>
  img {
    borde-radius: 8px;
  }
</style>
---

# CommonJS - 规范定义
<v-clicks>

  - 每个文件就是一个模块，有自己的作用域. 在一个文件里面定义的变量，函数，类，都是私有的，对其它文件不可见

  - 每个模块内部，module 变量代表当前模块。这个变量是一个对象，它的exports 属性（即module.exports）是对外的接口。加载某个模块，其实是加载该模块的module.exports 属性
    ```javascript
    // example.js
    var x = 5;
    var addX = function (value) {
      return value + x;
    };
    module.exports.x = x;
    module.exports.addX = addX;
    ```  
  - require 方法用于加载模块
    ```javascript
    var example = require('./example.js');
    console.log(example.x); // 5
    console.log(example.addX(1)); // 6
    ```

</v-clicks>

---

# CommonJS - 特点
<v-clicks>

  - 所有代码都运行在模块作用域，不会污染全局作用域
  - 模块可以多次加载，但是只会在第一个加载时运行一次，然后运行结果就被缓存，以后再加载，就直接读取缓存结果
  - 模块加载的顺序，按照其再代码中出现的顺序，是同步加载，只有加载完成，才能执行后面的操作
  - 可以动态加载
    ```javascript
    let m;
    if (x > 1) {
      m = require('./m2.js');
    } else {
      m = require('./m1.js');
    }
    ```

</v-clicks>
---

# 局限性
- 同步加载
- 不适合浏览器环境

<v-click>
```javascript {1|2|3|4|all}
  var math = require('./math');       // 加载远端math模块并执行
  var console = require('./console'); // 加载远端console模块并执行
  math.add(2, 3);                     // 执行add
  console.log('hello');               // 执行log
```
</v-click>

---

# AMD
- AMD是"Asynchronous Module Definition"的缩写，意思是"异步模块定义"
<v-clicks>

- 专门为浏览器环境设计。
- require.js库是AMD规范的具体实现。

</v-clicks>
---

# AMD - 规范定义

  - 每个模块都必须通过define函数定义.  define函数签名如下:
    ```js
      /**
        *  id 模块名称
        *  dependencies 模块依赖项
        *  factory 模块工厂，可以是一个函数或者对象。函数的参数与前面的依赖项一一对应。如果是函数，
        *  那么它的返回值就是模块对外暴露的成员
        **/
      define(id?: String, dependencies?: String[], factory: Function | Object); 
    ```
  - 一个例子
  ```js
  define(['dep1', 'dep2'], function (dep1, dep2) {
    dep1.log();
    dep2.log();
    return {
      myLog: function () { console.log('log'); }
    }
  })
  ```
  更多内容详见：[AMD规范](https://requirejs.org/docs/whyamd.html)    
---

# UMD
- UMD是通用“Universal Module Definition”的缩写，意思是通用模块定义规范。
<v-clicks>

- 适用于浏览器端,服务器端, 和其它任何JavaScript运行时
- 聚合了AMD和CommonJS规范，同时还兼容全局变量的方式

</v-clicks>
---

# UMD - 定义模块
```js
(function (root, factory) {
  // 如果当前环境支持AMD
  if (typeof define === 'function' && define.amd) {
    define(['dep1'], factory);
  } else if (typeof exports === 'object') {
    //如果当前环境支持CommonJS
    module.exports = factory(require('dep1'));
  } else {
    // 挂载到全局变量 (root 即 window)
    root.returnExports = factory(root.dep1);
  }
}(this, function (dep1) {
  //方法
  function myLog(){
		dep1.print();
		console.log('log');
	}
  //暴露公共方法
  return {
		myLog,
	}
}));
```

---

# ESM
ESM是ECMAScript Modules的缩写。
<v-clicks>

- ES6带来的新语法，是JavaScript语言层面提供的模块化规范
- 适用于浏览器端,服务器端, 和其它任何JavaScript运行时

</v-clicks>

---

---
layout: two-cols
---

# ESM-规范定义
<v-clicks at="0">

- 一个文件就是一个模块

</v-clicks>
<v-clicks at="1">

- 通过import导入模块，export导出模块, 通过export default 导出默认输出

</v-clicks>
<v-clicks at="3">

- 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存，以后再加载，就直接读取缓存结果

</v-clicks>
<v-clicks at="4">

- import语句是静态加载。无法像CommonJS模块那样，在if语句里面进行import

</v-clicks>
<v-clicks at="5">

- ES2020提案引入import()函数，支持动态加载
  ```js
  if (true) {
    import('./dep1').then((dep1) => dep1.log());
  }
  ```
</v-clicks>

::right::

<v-clicks at="2">

<div class="mt-15 ml-8">
```js
// module1.js
export const m1 = () => { console.log('m1') };

// module2.js
import { m1 } from './module1'

const m2 = () => {
  m1();
  console.log('m2');
}
export default m2;

// main.js
import module2 from './module2'
module2.m2();
```
</div>

</v-clicks>

---

# 小结
<v-clicks>

  1. AMD曾经普遍使用的浏览器端模块化规范，目前很少单独使用
  2. UMD是结合了AMD，CommonJS, 全局变量的模块化方式，目前还在普遍使用
  3. CommonJS适用于服务器端，是NodeJS的模块化规范。
      - 在浏览器端使用，需要通过browserify或者webpack等打包工具进行打包
      - 主流的模块化规范
  4. ESM是JavaScript语言标准提供的模块化规
      - 既适用于浏览器，也适用于服务器端。
      - 由于浏览器兼容性等考虑，现在还是结合webpack等打包工具使用
      - 主流并且面向未来的模块化规范

</v-clicks>

---
layout: image-right
image: /unsplash_electron.avif
---

# 包管理器
- <Link to="20" title="npm" />
- <Link to="" title="yarn" />
- <Link to="" title="pnpm" />

---
# npm
- NodeJS官方默认的包管理器，安装NodeJS时，会自动安装
- 创建新项目时
  - npm init  // 将当前目录作为项目的根目录，并创建package.json
- 运行别人的前端项目
  - npm install
  - npm run \<CMD\>

---
---
layout: two-cols
---

# npm - package.json

<v-click>

- 前端项目的配置文件, 定义了项目的配置信息

</v-click>

<v-click>

- 一些字段含义
  - name: 项目名称
  - scripts: 自定义脚本. 可通过`npm run <CMD>`运行
  - dependencies: 项目运行所依赖的模块
  - devDependencies: 项目开发所需要的模块
  - peerDependencies: 要求宿主环境提供peerDependencies中的包
  - 更多字段详见: [npm docs](https://docs.npmjs.com/cli/v9/configuring-npm/package-json)

</v-click>

::right::

<v-click>

<div class="mt-15 ml-8">
```json
  // package.json
  {
    "name": "modernized-front-end-development",
    "scripts": {
      "build": "slidev build",
      "dev": "slidev --open",
      "export": "slidev export"
    },
    "dependencies": {
      "@mrdrogdrog/optional": "^1.2.1",
      "@slidev/cli": "^0.42.5",
      "@slidev/theme-default": "latest",
      "@slidev/theme-seriph": "latest"
    }
  }
```
</div>

</v-click>

---

# npm - 常用命令
<v-clicks>

- 
  ```bash
  npm install # 根据package.json安装依赖

  npm install <--save-dev> <pkg-name> # 安装特定依赖

  npm uninstall <--save-dev> <pkg-name> # 卸载特定依赖

  npm run <cmd> # 执行package.json中配置的scripts

  npm config set registry <registry_addr> # 设置镜像仓库地址
  ```
- 快速切换npm仓库地址
  ```bash
  npm i -g nrm

  nrm add safeheron https://safeheron.xxxx.com

  nrm use safeheron
  ```

</v-clicks>

---

# yarn
yarn是facebook推出的另一个包管理器。之所以推出yarn，主要是为了解决早期npm的一些问题:
- 可靠性。不同机器和用户安装依赖项时安装的最终依赖版本可能不一致
  - yarn提供了yarn-lock.json文件，用来锁定依赖的版本
- 性能
  - yarn提供离线缓存，多次获取相同的依赖会从缓存中取，安装速度更快

---
layout: two-cols
--- 
# yarn - 常用命令
- 
  ```bash
  yarn # 根据package.json安装依赖
  yarn add <--dev> <pkg_name>  # 安装特定依赖
  yarn remove <--dev> <pkg_name> # 安装特定依赖
  yarn config set registry <registry_addr> # 设置镜像仓库地址
  ```
::right::
<div class="ml-8">

# yarn - 其它功能
  - workspace。可以在一个项目下，创建和管理多个包
  - plugin。可以通过编写plugin，添加新的依赖解析器，接收器，连接器，指令

</div>

---

# pnpm
pnpm是另一个包管理器。比起npm和yarn, 它有如下的一些优势:
- 节省磁盘空间
- 安装速度比其它包管理器更快
- 可以管理Node.js环境

---

# 三者功能对比
![pnpm-vs-yarn-vs-npm](/pnpm_vs_yarn_vs_npm.png)

<style>
  .slidev-layout h1 + p {
    opacity: 1;
    height: 90%;
  }  
  img {
    height: 100%;
  }
</style>

---
layout: image-right 
image: /unsplash_es6.avif
class: my-content
---

# ES6与TypeScript
- <Link to="" title="ES6(ES2015)" />
- <Link to="" title="TypeScript" />

<style>
  .my-content {
    background-color: red;
  }
  .my-content + .w-full {
    background-color: red;
    background-position: 0 center !important;
  }
</style>

---

# ES6(ES2015)
ES6就是ECMAScript 6， 是新一代的JavaScript标准，由于是2015年发布的，也称ES2015。  
之后每年发布的新标准，ES2016, ES2017等也称为ES6.

<v-click>

#### ES6提供了一系列新的语法和API
- let, const
- 变量解构
- 新的数据类型，set和map
- promise, async await
- class
- ...

</v-click>
<v-click>

[ES6入门教程](https://es6.ruanyifeng.com/)

</v-click>

---

# Babel
Babel是一个JavaScript编译器，主要用于将采用 ECMAScript 2015+ 语法编写的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中。
<v-click>

```bash
babel src --out-dir lib
```
</v-click>

<v-click>


<div class="mt-4 mb-2">

#### 支持的功能 

</div>

- 语法转换
- 通过polyfill(core-js库)在目标环境添加缺失的API
- 源码转换
- 自定义插件

</v-click>

---

# TypeScript
TypeScript是微软开发的编程语言，是JavaScript的一个超集，扩展了JavaScript，提供了静态类型检测。

<v-click>

#### 优点:

- 静态类型 
  - 在编译期，就能发现类型相关的问题
  - 类型可以增加代码的可读性，通过类型信息可以帮助我们快速理解变量，函数的作用
  - 编辑器等工具可以通过类型信息，生成代码提示，帮助我们理解代码，增强开发体验。

</v-click>

<v-click>

- 完全兼容JavaScript
  -  一个TS应用能够直接使用已经存在的JS脚本。编译后的TS脚本也可以在JavaScript应用中使用

</v-click>
---

# TSC
由于现在的浏览器，NodeJS运行时不能直接运行TS，所以需要将TS编译成JS。  
TSC就是TypeScript的编译器，将TS编译成JS

```bash
tsc src/**/*.ts --outDir lib
```

---
layout: two-cols
---

# .d.ts文件
TypeScript类型定义文件
- 由于JavaScript没有类型信息，ts导入js文件时，会获取不到js的类型信息。  
- 为了让ts使用js库时，依旧能够获得静态类型带来的优势，就需要为js库提供类型定义文件(.d.ts文件)。
- TODO: 补充几张无类型提示的图片

::right::

<v-click>

<div class="ml-4">

# 获取.d.ts文件
如何?
- .d.ts文件可以手工编写，如果是由ts编译成的js库，可以在ts编译时，自动生成.d.ts文件
- 现在npm库基本都会在包中提供.d.ts文件，如果没有提供。可以通过npm安装对应的@types库
  ```bash
  # 安装lodash库的类型定义文件
  npm i -D @types/lodash
  ```

</div>

</v-click>

---

# 打包 

为什么需要打包?

<v-clicks>

- 可以将多个资源文件合并成一个或多个文件
  - 支持CommonJS等模块
  - 减少网络请求次数, 提高网站的加载速度
- 打包工具都会提供插件能力。打包的过程也是构建的过程
  - 编译代码至目标代码, 使用最新的JS语法, 兼容低版本浏览器
  - 可以在构建流程中进行一些额外的资源处理
    - 压缩资源文件
    - lint代码规范
    - 其它自动化功能
- 本质还是浏览器发展速度跟不上前端技术发展速度
  - 为了开发大型复杂项目，前端需要利用工具链支持一些浏览器原生没有支持的新功能

</v-clicks>

---

# 打包工具 - Webpack
Webpack是一个用于现代JavaScript应用程序的静态模块打包工具。它可以将你项目中的每一个模块组合成一个或多个bundle。
<v-click>

- Loader
- Plugin
- webpack-dev-server

</v-click>

---
layout: two-cols
---

# Loader
Webpack默认本身只能处理JavaScript和JSON文件。如果需要处理图片，css文件等其它类型的文件，就需要安装对应的loader进行转换。
<v-click>

- 图片文件 
  - `url-loader`
  - `file-loader`
- 样式文件 
  - `css-loader`
  - `style-loader`
- ts文件 
  - `babel-loader`
  - `ts-loader`

</v-click>

::right::

<v-click>

<div class="mt-16 ml-4">

  ```js
  // webpack.config.js
  const path = require('path');

  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist'),
    },
    module: {
      rules: [
        {
          test: /\.css$/i,
          use: ['style-loader', 'css-loader'],
        },
      ],
    },
  };
  ```

</div>

</v-click>
---

# Plugin
loader用于转换某些类型的模块，而plugin则可以扩展webpack的能力
<v-click>

- 打包优化
- 资源管理
- 注入环境变量
- ...

</v-click>

<v-click>

```js
// webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack'); 

module.exports = {
  entry: './src/index.js',
  output: {
	  filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })],
};
```

</v-click>

---

# webpack-dev-server
基于express的本地开发服务器
- 为webpack打包生成的资源文件提供Web服务
- 实时刷新页面, 模块热替换(HMR)

<v-click>

```bash
npm install --save-dev webpack-dev-server
```
```json
// package.json
{
  "scripts": {
    "dev1": "webpack serve",
    "dev2": "webpack-dev-server"
  }
}
```

```js
// webpack.config.js
module.exports = {
  devServer: {
    // 一些dev-server的配置项
  },
};
```

</v-click>

---

# Rollup
Rollup另一个JS模块打包工具
#### 与Webpack的区别
- Rollup推荐使用ESM模块，并且默认只能导入ESM模块，需要通过安装Plugin来支持导入现有的CommonJS模块
  ```js
  // rollup.config.js
  import commonjs from '@rollup/plugin-commonjs';

  export default {
    input: 'src/index.js',
    output: {
      dir: 'output',
      format: 'cjs'
    },
    plugins: [commonjs()]
  };
  ```
- Rollup更适合打包库，Webpack则更适合打包应用。
  - Rollup打包出的产物比Webpack小；Webpack打包时，会注入Webpack相关的工具函数

---

# 其它工具
![bundler](/bundler.png)

<style>
  .slidev-layout h1 + p {
    opacity: 1;
    height: 90%;
  }  
  img {
    height: 100%;
  }
</style>

---

# 前端框架
- React
- Vue
- Angular
- Svelte

---

# React

React是facebook开源的用来构建前端界面的JavaScript库

<v-click>

- 组件化开发
- 声明式编程
- 虚拟DOM
- JSX（JavaScript XML）

</v-click>

--- 

# 相比JQuery的优势
- 数据驱动。 `UI = render(state)`
  - 只需关注组件和状态, 无需再关注如何操作DOM
- 灵活。
  - JSX, 把HTML与JS结合，操作起来更加灵活
- 性能。
  - 开发大型应用时，React虚拟DOM能减少DOM的操作，优化应用的性能

---

# React-Router
React只关注View层。一个应用一般会有多个页面，所以需要React-Router库来实现页面路由
```js
import ReactDOM from "react-dom/client";
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Layout from "./pages/Layout";
import Home from "./pages/Home";
import Contact from "./pages/Contact";

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Layout />}>
          <Route index element={<Home />} />
          <Route path="contact" element={<Contact />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

---
layout: two-cols
---

# Redux
Redux是JavaScript应用的状态容器，提供可预测的状态管理。  

<v-click>

#### 使用场景
- 应用中有很多state在多个组件中需要使用
- 应用state会随着时间的推移而频繁更新
- 更新state的逻辑复杂
- 中型和大型代码量的应用，很多人协同开发

</v-click>

::right::

<v-click>

<div class="mt-13">

![redux](/redux.png)

</div>

</v-click>

---
layout: two-cols
---

# React问题 - CSR
<v-click>

CSR(client-side rendering/客户端渲染)

</v-click>

<v-click>

- 不利于SEO
- 首屏渲染时间（FCP）长, 出现短暂白屏

</v-click>

::right::
<v-click>

# 解决方案 - SSR
SSR(server-side rendering/服务端渲染)

![csr_ssr](/csr_ssr.png)

</v-click>

---

# NextJS
