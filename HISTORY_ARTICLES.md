# 好文共欣赏
    （👍 赞） （⚡ 重要）（⭐ 我的博客文章）（💎 待深入）
- 💎
    - 事件循环
        - [Node.js Event Loop 的理解 Timers，process.nextTick()](https://cnodejs.org/topic/57d68794cb6f605d360105bf)
        - [Node.js 事件循环机制](https://www.cnblogs.com/onepixel/p/7143769.html)
        - [Node.js的event loop及timer/setImmediate/nextTick](https://github.com/creeperyang/blog/issues/26)
        - [node中的Event模块(上）](https://zhuanlan.zhihu.com/p/31043667?utm_source=qq&utm_medium=social&utm_oi=709122276448047104)
- 2018.10.07
    - babel
      - babel 编译时只转换语法，几乎可以编译所有新的 JavaScript 语法，但并不会转化BOM里面不兼容的API
      - babel-plugin-transform-runtime 对浏览器的一些新的 api 进行转换（Map、Set、Promise），其依赖于 babel-runtime
        - babel-runtime 与 babel-plugin-transform-runtime 的区别是：plugin 会由工具自动添加，主要的功能是为api提供沙箱的垫片方案，不会污染全局的api，因此适合用在第三方的开发产品中
      - babel-polyfill 通过改写全局prototype的方式实现，比较适合单独运行的项目
      - babel-preset-env 对语法进行转换
      - [对 babel-transform-runtime，babel-polyfill 的一些理解](https://www.jianshu.com/p/7bc7b0fadfc2)
    - webpack
        - `entry` 入口文件，使用对象表示会比较清晰
            ```js
            entry: {
              sha： './src/sha.js',
            }
            ```
        - `output` 打包生成的文件，对应 `entry`
            ```js
            output: {
                // name 对应 sha
              filename: '[name]/min.[hash:5].js',
            }
            ``` 
        - `loader`
            ```js
              module: {
                rule: [
                  {
                    test: /\.css$/,
                    use: 'css-loader',
                  },
                ],
              }
            ```
        - `plugin`
          - CommonsChunkPlugin 将不同 chunk 中相同的 chunk
          - UglifyJsPlugin 混淆、压缩、生成 source-map
          - ExtractTextWebpackPlugin 将 CSS 提取成单独的文件
          - HtmlWebpackPlugin 生成 html 页面
          - HotModuleReplacementPlugin 模块热更新插件
          ```js
            // const webpack = require('webpack'); 
            plugins: [
              new webpack.optimize.UglifyJsPlugin(),
            ],
          ```
        - bundle、chunk、module
            - bundle 是由 webpack 打包出来的文件，chunk 是指 webpack 在进行模块依赖分析的时候，代码分割出来的代码块，module 是开发中的单个模块
        - Loader、Plugin
            - Loader 是用来告诉 webpack 如何处理某一类型的文件，并且引入到打包出的文件中
            - Plugin 是用来**自定义 webpack 打包过程的方式**，一个插件是一个含有 apply 方法的对象，通过这个方法可以参与到整个 webpack 打包的流程（生命周期）
        - Tree-shaking
            - Tree-shaking 是指在打包的时候去除文件中引入的但是没有使用到的死代码。wepback 中使用 UglifyJsPlugin 来实现 Tree-shaking JS。CSS 使用 Purify-CSS 实现。
        - 资源
            - [Webpack4之SplitChunksPlugin规则](https://blog.csdn.net/Napoleonxxx/article/details/81975186)
    - [webpack 与 gulp 的区别](https://www.cnblogs.com/lovesong/p/6413546.html)
        - gulp 强调的是前端开发的工作流程，让 gulp 执行这些 task ，从而构建项目的整个前端开发流程
        - webpack 是一个前端模块化方案，更侧重模块打包，可以递归地打包项目中的所有模块，最终生成几个打包后的文件。它和其他工具最大的不同在于它支持 code-splitting、模块化（AMD、ESM、CommonJS）、全局分析
        - 总结：gulp 严格上讲，模块化不是它强调的东西，它旨在规范前端开发流程。webpack 更是明显强调模块化开发，而那些文件压缩合并、预处理等功能，不过是他附带的功能
   
- 2018.10.06
    - [js模块化编程之彻底弄懂CommonJS和AMD/CMD！](https://www.cnblogs.com/chenguangliang/p/5856701.html)
        - CommonJS：浏览器不兼容 CommonJS 的根本原因，在于缺少四个 Node.js 环境的变量，`module`、`exports`、`require`、`global`，只要提供这四个变量，浏览器就能加载 CommonJS 模块。
            - Browserify 是目前最常用的 CommonJS 转换工具， 其编译之后，将所有模块放入一个数组，id 是模块的编号，source 是模块的源码，deps 是模块的依赖
        - AMD：采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行
            - `require([module], fn)` 
            - 模块的定义
                ```js
                define(function() {
                  var add = function () {};
                  return {
                    add,
                  };
                });
                ```
            - 正常script标签加载的时候，浏览器会停止网页渲染
            - RequireJS 实现 js 文件的异步加载，避免网页失去响应，管理模块之间的依赖性，便于代码的编写和维护
        - AMD、CMD 区别
          - AMD 先加载后执行
          - CMD 懒加载
        - ESM（EcmaScript Module）
          - import、export
        - [RequireJS demo code](./democode/requirejs-demo)
    - [什么是事件循环](https://github.com/creeperyang/blog/issues/26)
        - 所有任务都在主线程上执行，形成一个执行栈（ecs）
        - 主线程之外还存在一个任务队列（task queue），系统把异步任务放到任务队列中，然后主线程继续执行后续的任务
        - 当 ECS 中的所有任务执行完毕，系统就会读取任务队列，如果这个时候，异步任务已经结束了等待状态，将回调函数推入任务队列，就会从任务队列进入执行栈，恢复执行
- 2018.10.05
    - [迷宫问题 广度优先寻找最短路径](mypost/base/democode/迷宫问题/maze-problem-bfs-minimum-length.js)
    - [迷宫问题 normal 版本](mypost/base/democode/迷宫问题/maze-problem.js)

- 2018.10.04
    - [尾调用及其优化](http://es6.ruanyifeng.com/#docs/function#%E5%B0%BE%E8%B0%83%E7%94%A8%E4%BC%98%E5%8C%96)
        - 尾调用（Tail Call）：某个函数的最后一步是调用另一个函数（调用之后不能再赋值，不能再进行运算）
        - 调用栈：函数调用会在内存形成一个“调用记录”，又称“调用帧”（call frame），保存调用位置和内部变量等信息。如果在函数 `A` 的内部调用函数 `B`，那么在 `A` 的调用帧上方，还会形成一个 `B` 的调用帧。等到 `B` 结束，将结果返回到 `A`，`B` 的调用帧才会消失。如果函数 `B` 内部还调用函数 `C` ，那就还有一个 `C` 的调用帧，以此类推。所有的调用帧，就形成一个“调用栈”（call stack）。
        - **尾调用优化**
            - 如果所有的函数都是尾调用，那么完全可以做到每次执行时调用帧只有一项，这将大大节省内存
            - 尾调用由于是函数的最后一步操作，不需要保留外层函数的调用帧，因为调用位置、内部变量等信息不会再用到了，只要直接用内层函数的调用帧，取代外层函数的调用帧就可以了
            - 注意：只有不再用到外层函数的内部变量，内层函数的调用帧才会取代外层函数的调用帧，否则就无法进行“尾调用优化”
    - [阮一峰ES6 函数的扩展篇](http://es6.ruanyifeng.com/#docs/function)
        - 函数的 length 属性，表示预期传入的参数个数，某个参数指定默认值以后，预期传入的参数个数就不包含这个参数了（只计入默认值之前的参数个数），rest 参数也不计算在内
        - 箭头函数
            - 函数体内的 `this` 对象，就是定义时所在的对象，而不是使用时所在的对象（在箭头函数中，this 是固定的，不会变化）。`this` 指向的固定化，并不是因为箭头函数有绑定 `this` 的机制，实际原因是箭头函数根本没有自己的 `this`，导致内部的 `this` 就是外层代码块的 `this`。
            - 正式因为它没有 `this`，所以也就不能当做构造函数，即不能使用 `new` 命令，否则会报错
            - `super`、`new.target`、`arguments` 在箭头函数中是不存在的（指向外层函数的对应变量）
                - `super keyword unexpected here`
                - `new.target expression is not allowed here`
                    ```js
                    function foo() {
                      setTimeout(() => {
                        console.log('args:', arguments);
                      }, 100);
                    }
                    
                    foo(2, 4, 6, 8)
                    // 指向了外层函数的对应属性
                    // args: [2, 4, 6, 8]
                    ```
            - 不可以使用 `arguments` 对象，该对象在函数体内不存在。可使用 `rest` 替代
            - 不可以使用 `yield` 命令，即箭头函数不能当做 Generator 函数
            - 不适用箭头函数的场景
                - 定义函数的方法
                    ```js
                    // 调用 cat.jumps() ，如果是普通函数，则正常执行
                    // 但是箭头函数默认固定为创建的时候的 this（全局对象），会报错
                    const cat = {
                      lives: 9,
                      jumps: () => {
                        this.lives--;
                      }
                    }
                    ```
                - DOM 中添加事件监听，动态绑定 `this`
                    ```js
                    button.addEventListener('click', () => {
                      // 这个 this 并没有指向 button 本身
                      this.classList.toggle('on');
                    });
                    ```
    - 斐波拉契数列
        - js 尾调用实现
            ```js
            function fibonacci(n, prev = 1, next = 1) {
                if (n <= 2) return next;
                return  fibonacci(n - 1, next, prev + next);
            }
            ``` 
        - 循环实现
            ```js
            function fibonacci1(n) {
            
                if (n <= 2) return 1;
                var arr  = [1, 1];
                var temp;
                for(var i = 2; i < n; i++){
                    temp = arr[0];
                    arr[0] = arr[1];
                    arr[1] = arr[1] + temp;
                }
                
                return arr[1];
            }
            ```
    - 事件冒泡、事件捕捉、事件委托（事件代理）
        - **事件捕捉**是指从 document 一直到触发事件的那个节点，事件冒泡则相反，指的是触发事件的节点到 document
        - [js中的事件委托或是事件代理详解](https://www.cnblogs.com/liugang-vip/p/5616484.html)
        - `e.stopPropagation` 可以阻止事件冒泡或者事件捕捉
        - 事件委托能减少由于大量绑定事件带来的性能损耗，同时对新加入的结点不需要额外绑定事件
        - 事件委托基于事件冒泡，所以不支持冒泡的事件无法进行委托
        - `addEventListener('click', func, bool)` `addEventListener` 的第三个参数，默认为false，表示使用冒泡，true表示事件捕捉
        
- 2018.10.03
    - [页面重排与重绘（Reflow & Repaint)](https://zhuanlan.zhihu.com/p/35184404)
        ![intro](http://qiniu1.lxfriday.xyz/common/v2-b03158856ef36b4668d101e13ea949ed_hd.jpg)
        - reflow（重排）：当涉及到 DOM 节点的布局属性发生变化时，就会重新计算该属性，浏览器会重新描绘相应的元素
        - repaint（重绘）：当影响 DOM 元素可见性的属性发生变化（color、visibility等），浏览器会重新描绘相应的元素。重排必然会引起重绘
                             - 浏览器渲染的大致流程：
            1. 渲染 HTML 文档，构建 DOM 树
            1. 解析 CSS 属性，构建 CSSOM 树
            1. 结合 DOM 树和 CSSOM 树，构建 render 树
            1. 在 render 树的基础上布局，计算每个节点的几何结构
            1. 把每个节点绘制在屏幕上
        - 一个页面可以简单的看成由两部分构成
            - DOM 节点，描述页面的结构
            - DOM 节点的属性，描述 DOM 节点如何呈现
        - reflow 发生在第4步， repaint 发生在第5步
        - **如何减少reflow、repaint**
            - 避免 js 逐条更改样式，使用 className
            - 避免频繁操作 dom ，创建 documentFragment 或 div，在它上面应用 DOM 操作之后，添加到文档中
            - 在设置为 `display: none` 的元素上操作，最后显示出来
            - 避免频繁读取元素集合属性（scrollTop等）
            - 绝对定位具有复杂动画的元素。使其脱离文档流，避免引起父元素及其后续元素大量重排
    - [对 DOM 树进行深度优先和广度优先遍历](./mypost/2018/10/03/bfs-dfs-on-dom.md)
    - [querySelectorAll 方法相比 getElementsBy 系列方法有什么区别？](https://www.zhihu.com/question/24702250)
        - w3c 标准：`querySelectorAll` 属于 W3C 中的 Selectors API 规范，而 `getElementsBy*` 系列则是属于 W3C DOM 规范
        - 接收参数： `querySelectorAll` 接收的参数是一个 CSS 选择符（必须严格符合 CSS 选择器命名规范，否则会抛出异常 DOMException），`getElementsBy*` 的参数只能是单一的 className、tagName、name、id
        - 返回值：`querySelectorAll` 返回的是一个 Static Node List（页面 DOM 变动不会影响之前已经获取到的返回值），`getElementsBy*` 返回的是 Live Node List（之前的返回值会受到页面的 DOM 变动影响）
        - chrome 中的效果
            - `document.querySelectorAll('a').toString();    // return "[object NodeList]"`
            - `document.getElementsByTagName('a').toString();    // return "[object HTMLCollection]"`
            - `Note: Collections in the HTML DOM are assumed to be live meaning that they are automatically updated when the underlying document is changed.`
        - HTMLCollection 是属于 **Document Object Model HTML** 规范，而 NodeList 属于 **Document Object Model Core** 规范。
            ```js
                var ul = document.getElementsByTagName('ul')[0],
                    lis1 = ul.childNodes,
                    lis2 = ul.children;
                console.log(lis1.toString(), lis1.length);    // "[object NodeList]" 11
                console.log(lis2.toString(), lis2.length);    // "[object HTMLCollection]" 4

            ```
        - NodeList 对象会包含文档中的所有节点，如 Element、Text 和 Comment 等。
        - HTMLCollection 对象只会包含文档中的 Element 节点。


- 2018.10.01
    - [图 JS实现](mypost/base/democode/Graph.js)
        - 图的实现：邻接矩阵、邻接表、关联矩阵
    - [集合（百度百科）](https://baike.baidu.com/item/%E9%9B%86%E5%90%88/2908117?fr=aladdin)
        - [JS 实现](mypost/base/democode/set的模拟实现/Set.js)
        - 集合的表示方法有：列举、描述、图像、符号法
        - 列举法：N = {1,2,3,4,5} （这也是计算机中常用的表示法）
     - [二叉搜索树的 JS 实现](mypost/base/democode/BinarySearchTree.js)
    - [红黑树](https://baike.baidu.com/item/%E7%BA%A2%E9%BB%91%E6%A0%91/2413209?fr=aladdin#reference-[1]-133754-wrap)
        - 红黑树的节点是红色或黑色的
        - 根节点是黑色的
        - 每个叶节点（NIL、空节点）是黑色的
        - 每个红色节点的两个子节点都是黑色的（从每个叶子节点到根的所有路径上不能有两个连续的红色节点）
        - 从任一节点到其每个叶子节点的所有路径都包含相同数目的黑色节点
    - [理解红黑树左旋和右旋](http://www.cnblogs.com/skywang12345/p/3245399.html)
        - 左旋意味着将节点变成其右子节点的左子节点，然后其原有右子节点的左子节点变成节点的右节点（右旋类推）

- 2018.09.23
    - [理解事件循环二(macrotask和microtask)](https://github.com/ccforward/cc/issues/48)
        - Microtasks: process.nextTick, promise
        - Macrotasks: setTimeout, setInterval, setImmediate, I/O
    - [理解 JavaScript 中的 macrotask 和 microtask](https://juejin.im/entry/58d4df3b5c497d0057eb99ff)
        - 如果我的某个 microtask 任务又推入了一个任务进入 microtasks 队列，那么在主线程完成该任务之后，仍然会继续运行 microtasks 任务直到任务队列耗尽。
        - 而事件循环每次只会入栈一个 macrotask ，主线程执行完该任务后又会先检查 microtasks 队列并完成里面的所有任务后再执行 macrotask
    - [Stack的三种含义 -- 阮一峰](http://www.ruanyifeng.com/blog/2013/11/stack.html)
        - 栈中的数据占用空间大小是确定的，堆中数据占用的大小是不确定的，堆需要GC进行空间回收，栈在当前执行上下文结束之后进行回收
    - ⚡ [JavaScript 运行机制详解：再谈Event Loop -- 阮一峰](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)
    - [从setTimeout-setInterval看JS线程](https://segmentfault.com/a/1190000013702430)
    - 👍 [你不曾察觉的隐患：危险的 target="_blank" 与 “opener”](https://segmentfault.com/a/1190000016421263)

- 2018.09.22
    - ⚡ [深入剖析 JavaScript 的深复制](http://jerryzou.com/posts/dive-into-deep-clone-in-javascript/)
    - ⚡ [JavaScript中的缓冲数组和强类型数组 ArrayBuffer、TypedArray](https://zhuanlan.zhihu.com/p/30938992)

- 2018.08.13
    - ⚡ [setTimeout、setImmediate、process.nextTik 的区别](https://www.cnblogs.com/onepixel/articles/7605465.html)
        - idle观察者(process.nextTik) >> io观察者(setTimeout) > check观察者(setImmediate)
    - ⚡ [排序算法详解](https://www.cnblogs.com/onepixel/articles/7674659.html)

- 2018.08.10
    - ⚡ [Promises/A+ 规范](https://promisesaplus.com)

- 2018.08.03
    - 👍 [继承的多种方式及优缺点](https://github.com/mqyqingfeng/Blog/issues/16)
    - ⭐ [为什么表达式语句不能以大括号或 function 开头({}.toString() 报错原因)](./mypost/2018/08/03/why-expression-cannot-start-with-function-or-curly-braces.md)


- 2018.08.02
    - [JavaScript深入之创建对象的多种方式以及优缺点](https://github.com/mqyqingfeng/Blog/issues/15#issue-227556285)
    - 👍 [JS中运算符的优先级 运算符的优先级决定了表达式中运算执行的先后顺序](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)
        - 结合性决定了拥有相同优先级的运算符的执行顺序(左结合、右关联)
        - 20 圆括号(最大)
        - 19 成员访问、需要计算的成员访问、new
        - 18 后置++、后置--
        - 3 赋值运算符(从右到左) =、+=、-=、*=
        - 0 ,

    - [emoji符号 🌹🍀🍎💰📱🌙🍁🍂🍃🌷💎🔪🔫🏀⚽⚡👄👍🔥](http://www.fhdq.net/emoji.html)

- 2018.07.31
    - 👍 [乐意黎 JS根据useAgent来判断edge, ie, firefox, chrome, opera, safari 等浏览器的类型及版本](https://blog.csdn.net/aerchi/article/details/51697592)
