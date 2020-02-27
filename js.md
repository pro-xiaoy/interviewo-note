## 原生JS
### 知道哪些es6语法
- 箭头函数 **箭头函数不会创建自己的this,它只会从自己的作用域链的上一层继承this**
- 解构赋值
- 块级作用域
- 块级变量let/块级常量const
- 模板字符串
- 展开运算符
- import
- export
- class

### Promise, Promise.all, Promise.race
-  对象用于表示一个异步操作的最终状态（完成或失败），以及该异步操作的结果值。
-  接下来的JS都是可以在MDN找到[Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
~~~js
    // promise
    function fn(){
        return new Promise((resolve, reject)=> {
            setTimeout(()=> {
                resolve('123')
            }, 1000)
        })
    }
    fn().then((res) => {console.log(res)}, (rej) => {console.log(rej)})

    // Promise.all(iterable) 方法返回一个 Promise 实例

    // 个人理解。把pormise对象包裹为Array，当作参数传入，反馈的值也是Array反馈
    Promise.all(['1',
                promise.resolve(2), 
                new Pormise((resolve, reject) => { 
                    setTimeout(resolve, 100, 'foo')})]).the(res=> {console.log(res)})
    // [1, 2, 'foo']
    // 再说明一个东西， setTimeout第三个参数是传入resolve的参数

    // Promise.race(iterable) 方法返回一个 promise，一旦迭代器中的某个promise解决或拒绝，返回的 promise就会解决或拒绝。

    // 个人理解race内置promise对象哪个先完成
    var p1 = new Promise(function(resolve, reject) { 
        setTimeout(resolve, 500, "one"); 
    });
    var p2 = new Promise(function(resolve, reject) { 
        setTimeout(resolve, 100, "two"); 
    });

    Promise.race([p1, p2]).then(function(value) {
    console.log(value); // "two"
    // 两个都完成，但 p2 更快
    });

    var p3 = new Promise(function(resolve, reject) { 
        setTimeout(resolve, 100, "three");
    });
    var p4 = new Promise(function(resolve, reject) { 
        setTimeout(reject, 500, "four"); 
    });

    Promise.race([p3, p4]).then(function(value) {
    console.log(value); // "three"
    // p3 更快，所以它完成了              
    }, function(reason) {
    // 未被调用
    });

    var p5 = new Promise(function(resolve, reject) { 
        setTimeout(resolve, 500, "five"); 
    });
    var p6 = new Promise(function(resolve, reject) { 
        setTimeout(reject, 100, "six");
    });

    Promise.race([p5, p6]).then(function(value) {
    // 未被调用             
    }, function(reason) {
    console.log(reason); // "six"
    // p6 更快，所以它失败了
    });
~~~
### 手写函数防抖和函数节流
先说下个人理解把，节流（throttle）相当于游戏里面的技能CD，那么我们稍微写下这个函数
~~~js
    function throttle(fn, delay) {
        let use = true;
        return function(){
            if(use) {
                fn.call(this, arguments);
                use = false
                setTimeout(function() {
                    use = true
                }, delay)
            }
        }
    }
~~~
防抖（debounce）理解为输入框多少秒以后查询，你输入一个查询重新开始，上一个查询结束，一直这样下去.
~~~ js
    function debounce(fn, delay){
        let timer;
        return function(){
            clear(timer)
            let _this = this;
            timer = setTimeout(function(){
                fn.call(_this, arguments);
                clear(timer);
            }, delay)
        }
    }
~~~

### 手写ajax
~~~js
let xhr = new XMLHttpRequest();
xhr.open(methods, url)
xhr.onreadystatechange = function(){
    if(xhr.readyState === 4) {
        if(xhr.status >= 200 && xhr.status < 300) {
            console.log(xhr.responseText)
        }
    }
}
~~~

### 代码中this的指向问题
- fn()          // window/glob
- obj.fn()      // obj
- fn.call(xx)   // xx
- fn.bind(xx)   // xx
- new Fn()      // 新构成的对象
- ()=>{}        // 外面的this

### 闭包/立即执行函数是什么？
- 闭包
  - 闭包是什么
    - 「函数」和「函数内部能访问到的变量」（也叫环境）的总和，就是一个闭包。
  - 闭包的作用
    - 闭包常常用来「间接访问一个变量」。换句话说，「隐藏一个变量」。
- 立即执行函数
  - 立即执行函数是什么？
    - 声明一个匿名函数
    - 马上调用这个匿名函数
  - 立即执行函数有什么用？
    - 只有一个作用：创建一个独立的作用域。
  
### 什么是 JSONP，什么是 CORS，什么是跨域？
- JSONP
> JSONP就是js引用script标签，利用script没有跨域限制这个黑洞来达到与第三方数据交互的目的
- cors
> CORS(cross-origin sharing standard)跨域资源共享,这个东西应该后端了解的比前端深吧，就是Http请求接口的头部里面写道('Access-Control-Allow-Origin': '*'),目的就是防止跨域；
- 跨域
> 浏览器对于javascript的同源策略的限制,例如a.cn下面的js不能调用b.cn中的js,对象或数据(因为a.cn和b.cn是不同域),所以跨域就出现了.同源策略：同端口同域名同协议；

### 如何实现深拷贝？
- 递归
- 判断类型
- 检查循环类型
~~~ js
    // 第一种 封装函数
    function deepCopy(obj, c){
        var c = c || {};
        for(var i in obj) {
            if(typeof obj[i] === 'object') {
                if(o[i].constructor === Array){
                    c[i] =[]
                } else {
                    c[i] = {}
                }
                deepCopy(o[i],c[i])
            } else {
                c[i] = obj[i]
            }
        }
        return c
    }
    // 第二种 json的函数
    JSON.parse(JSON.stringify(obj))
    // 第三种 之前我在某个项目中用了
    [...obj]
~~~

### 如何用正则实现 trim()？
trim 就是删除写入的字符串的首位空格
~~~js 
    function trim(str) {
        return str.replace(/^\s+|\s+$/g, '')
    }
~~~

### === 
![avatar](./imgs/1.jpg)

### async/await
async function 声明用于定义一个返回 AsyncFunction 对象的异步函数。异步函数是指通过事件循环异步执行的函数，它会通过一个隐式的 Promise 返回其结果。但是如果你的代码使用了异步函数，它的语法和结构会更像是标准的同步函数。
**让异步看上去更像同步**
~~~js
    举个demo吧，MDN的原例子
    function fn1(){
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve('123')
            }, 1000)
        })
    }
    async function callBack(){
        var a = await fn1();
        console.log(a)
    }
    callBack()   // => 123
    // 吐槽一句这种写法看的贼鸡儿难受
~~~
### 如何手写一个pormise
~~~js
   // promise
  function myPromise(constron) {
  let _this = this
  _this.status ='pending'
  _this.result = undefined
  _this.reson = undefined

  // 增加队列处理异步方法
  this.resolveCallBack = []
  this.rejectCallBack = []

  function resolve(val) {
    _this.status = 'resolve'
    _this.result = val
    _this.resolveCallBack.forEach(cb => {
      cb()
    });
  }
  function reject(val) {
    _this.status = 'reject'
    _this.reson = val
    _this.rejectCallBack.forEach(cb => {
      cb()
    });
  }
  constron(resolve, reject)
}
myPromise.prototype.then = function(resolve, reject){
  let _this = this
  if(_this.status == 'pending') {
    if(resolve) {
      _this.resolveCallBack.push(()=> resolve(_this.result));
    }
    if(reject) {
      _this.resolveCallBack.push(reject(_this.reson));
    }
  }
  if(_this.status == 'resolve') {
    resolve(_this.result)
  }
  if(_this.status == 'reject') {
    reject(_this.reson)
  }
}
~~~
