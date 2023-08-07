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
image: ./assets/unsplash_code.jpg
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
image: ./assets/unsplash_electron.avif
---

# 包管理器
- <Link to="20" title="npm" />
- <Link to="" title="yarn" />
- <Link to="" title="pnpm" />

---
# npm
- NodeJS官方默认的包管理器，安装NodeJS时，会自动安装
- 一般进行前端项目开发时，都会先通过npm init 进行初始化，将当前目录作为项目的更目录，并创建package.json文件
- 运行别人的前端项目
  - npm install
  - npm run <CMD>, 一般



