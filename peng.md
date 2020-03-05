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
function MyPromise(constru) {
 this.status = 'pednging'
 this.reson = undefined
 this.result = undefined
 this.resonarr = []
 this.resultarr = []
 let resolvefn = function(value) {
    this.status = 'resolve'
    this.reson = value
 }
  let rejectfn = function(value) {
    this.status = 'reject'
    this.result = value
 }
 constru(resolvefn, rejectfn)
}
MyPromise.prototype.then=function(resolve, reject) {
  if(this.status = 'pednging') {
    this.resonarr.push(() => {
      resolve(this.reson)
    })
     this.resultarr.push(() => {
        reject(this.result)
    })
  }
  if(this.status == 'resolve') {
    resolve(this.reson)
  }
  if(this.status == 'resolve') {
    resolve(this
  }
}
```


4. 自定义事件 / DOM / BFC(块级格式化上下文)
5. 浏览器渲染过程 ｜｜各种状态码 ｜｜  缓存机制
6. setTimeout 和 setImmedate 区别
7. vue中 v-for  key 原理和作用
8. 虚拟dom 作用， diff算法
9. vuex  使用
10. js 宏任务 微任务
11. vue nextTick使用场景
### 12. vue 中 mounted create 有什么区别
* create在vue实例化结束对data进行oberver以及watch/copmputed钩子加载
* mountedcpmplite创建的vm.$el挂载到$el上。

13. webpack 怎么打包优化
14. 事件队列 Event Loop
15. 手写 判断对象是否相等
16. 手写深度拷贝


1. 公司上下班（8:30 - 6:00）
2. 公司公积金 ()
3. 年终奖(0.5-1)/绩效()/项目提成()
4. 团建
5. 公司对我这个职位的期望是什么？
6. 这个职位未来几年的职业发展是怎样的？
7. 为了胜任这个岗位我还需要学习哪些技术知识？