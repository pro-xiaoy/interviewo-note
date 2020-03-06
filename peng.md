### 1. vue 源码相关知识 包括 vuex vue-router
* vue2.x 实现基本上是通过传入的data进行object.defineprotype来对数据进行get/set,在数据变化的实话进行订阅发布，在compile编译的时候进行监听。
* vuex 一个new vue()的实例，创建一个已经数据双向绑定的全局的data[简书-vuex实现原理](https://www.jianshu.com/p/87db06d5b84e)
* vue-router,一个也是定一个new vue()但是并不完全是，走的是数据劫持，观察路径的变化



### 2. webpack 相关知识， 编译原理，路径问题，自定义plugin /load 如何编写，webpack 性能优化
* 编译原理是把内部的文件读取成字符串转化为ast，根据特定的语法将文件输出成文件，
* 路径resolve： alias自己定义路径
* load（加载器）【'css-loader','less-loader'】/plugins（插件）['HtmlWebpackPlugin'， 'CleanWebpackPlugin']基本上插件都需要new一下实成构造函数
> load编写
```js
loader从本质上来说其实就是一个node模块。相当于一台榨汁机(loader)将相关类型的文件代码(code)给它。根据我们设置的规则，经过它的一系列加工后还给我们加工好的果汁(code)。
单一原则: 每个 Loader 只做一件事；
链式调用: Webpack 会按顺序链式调用每个 Loader；
统一原则: 遵循 Webpack 制定的设计规则和结构，输入与输出均为字符串，各个 Loader 完全独立，即插即用；
```
> plugin编写
```js
在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过Webpack提供的API改变输出结果。通俗来说：一盘美味的 盐豆炒鸡蛋 需要经历烧油 炒制 调味到最后的装盘等过程，而plugin相当于可以监控每个环节并进行操作，比如可以写一个少放胡椒粉plugin,监控webpack暴露出的生命周期事件(调味)，在调味的时候执行少放胡椒粉操作。那么它与loader的区别是什么呢？上面我们也提到了loader的单一原则,loader只能一件事，比如说less-loader,只能解析less文件，plugin则是针对整个流程执行广泛的任务。

```
> webpack 优化
```js
1. mode模式一定要设计好。 production模式下会进行tree shaking(去除无用代码)和uglifyjs(代码压缩混淆)
2. 设置路径alia,这个很有帮助，如果都是import引入的话只会在整个node_modules里面寻找会占内存的
3. 使用happypack，多加进程，虽然js是单线程，但是happypack会将这些转到一个个子进程中。
4. 走cdn但是有风险以前和同事用百度的jquery cdn库直接崩了。
```

3. 基础的js  手写promise, generator， call , bind , apply ，filter, map, reduce等等
```js
1. promise
function MyPromise(constru) {
 this.status = 'pednging'
 this.reson = undefined
 this.result = undefined
 this.resonarr = []
 this.resultarr = []
 let resolvefn = function(value) {
    this.status = 'resolve'
    this.reson = value
    this.resonarr.forEach(cb=> cb())
 }
  let rejectfn = function(value) {
    this.status = 'reject'
    this.result = value
     this.resultarr.forEach(cb=> cb())
 }
 constru(resolvefn, rejectfn)
}
MyPromise.prototype.then=function(resolve, reject) {
  if(this.status = 'pednging') {
    if(resolve) {
      this.resonarr.push(() => {
        resolve(this.reson)
      })
    }
    if(reject) {
      this.resultarr.push(() => {
        reject(this.result)
      })
    } 
  }
  if(this.status == 'resolve') {
    resolve(this.reson)
  }
  if(this.status == 'resolve') {
    resolve(this
  }
}
2. generator(暂存性)

3. call , bind , apply ，

Function.prototype.mycall = function(content) {
  content._fn = this
  content._fn(content)
}
Function.prototype.myapply = function(content, arr) {
  content._fn = this
  content._fn(...arr)
  delete content._fn 
}
// bind的区别就是bind返回值是一个函数必须在次调用一下
Function.prototype.mybind = function(content) {
  let _this = this
  // return content()
  var args = Array.prototype.slice.call(arguments,1); // 后参数
  var temp = function(){}
  var result = function() {
    return _this.call(temp instanceof this ?this :content ,args.concat(Array.prototype.slice.call(arguments)) ) 
  }
  temp.prototype = this.prototype
  result.prototype = new temp();
  return result
}

4. filter, map, reduce
filter过滤属性和find区别就是，find找到了就return

Array.prototype.myfilter = function (fn) {
  let newArr = [];
  for (var i = 0; i < this.length; i++) {
    fn(i) && newArr.push(this[i]);
  }
  return newArr;
}
map:
Array.prototype.mymap = function(fn) {
  let newArr = [];
  for (var i = 0; i < this.length; i++) {
    newArr.push(this[i]);
  }
  return newArr;
}
```



### 4. 自定义事件 / DOM / BFC(块级格式化上下文)
```js
CustomEvent
DOM
```

5. 浏览器渲染过程 ｜｜各种状态码 ｜｜  缓存机制
```js
  浏览器拿到render tree的时候计算节点的位置（回流），对各个位置进行渲染（重绘）
  触发回流： 首次渲染，dom节点的的位置改变，删除添加dom‘
  触发重绘制： 颜色改变呀，文字改变呀，位置不变就行
```
6. setTimeout 和 setImmedate 区别
```js
setImmedate MDN 不推荐基本属于node那一层，
说一下我理解的chrome范畴的宏观任务和微观任务
基本上同步的代码执行完就会执行微观任务（promise.then）
宏观任务最后执行（setTimeout）

```
7. vue中 v-for  key 原理和作用
8. 虚拟dom 作用， diff算法
9. vuex  使用
10. js 宏任务 微任务
11. vue nextTick使用场景
```js
dom加载完后续的回掉函数/更新循环结束之后执行延迟回调
```
### 12. vue 中 mounted create 有什么区别
* create在vue实例化结束对data进行oberver以及watch/copmputed钩子加载
* mountedcpmplite创建的vm.$el挂载到$el上。

13. webpack 怎么打包优化
14. 事件队列 Event Loop
```js
js语言在内存中的以栈堆的形式出现，栈存储引用类型的值，堆存储基础数据类型和引用类型的指向
在js引擎加载的时候，因为js是单线程的，会从上往下的执行下去，当他发现一个异步操作的时候会丢到栈队列中
在同步执行完毕后，走异步的栈堆
异步这里面分为宏观任务和微观任务
先执行微观任务(promise)在走宏观任务(计时器)

```
15. 手写 判断对象是否相等
```js

```
16. 手写深度拷贝
```js
浅拷贝直接赋值
1、基本数据类型的特点：直接存储在栈(stack)中的数据
2、引用数据类型的特点：存储的是该对象在栈中引用，真实的数据存放在堆内存里
所以你一般指向的基本上都是
```
