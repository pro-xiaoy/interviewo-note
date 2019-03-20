## vue
### 说一下watch和computed的区别
> 翻一下，watch监听属性， computed计算属性
- computed计算属性有缓存属性，在他属性依赖没有变化的时候，他是不会计算的; watch属性则看到就会计算一次
- watch属性可以做除了计算的额外的属性，比如子父组建的通讯也就是上报属性
  
### Vue 有哪些生命周期钩子函数？分别有什么用？
- beforeCreate
- create
- beforemount
- mounted  **数据交互** 
- beforeUpdata
- updata
- beforedestory
- destory

### Vue 如何实现组件间通信？
- 父子： $on('name', method(value))  $emit('name', data)
- 爷孙就多用几个$on
- 多组间通信就vuex/eventBus

### VUE响应式原理
- new Vue的时候我们会对data里面的值通关object.defineProperty全部转为get/set属性
- **当你把一个普通的 JavaScript 对象传给 Vue 实例的 data 选项，Vue 将遍历此对象所有的属性，并使用 Object.defineProperty 把这些属性全部转为 getter/setter。（vue官网）**
- **Vue 不能检测到对象属性的添加或删除**
- Vue 不允许在已经创建的实例上动态添加新的根级响应式属性 (root-level reactive property)。然而它可以使用 Vue.set(object, key, value) 方法将响应属性添加到嵌套的对象上

### vue.set该怎么用
- 这个是vue实例增加新的根级响应式属性，Vue.set(object, key, value) 
- 根级别的array改变不了某个下标一列的值，我们用到set来改变根元素

### Vuex 你怎么用的？
> vuex应用于vue的状态管理器
- state: 地址存放数据 
- getter: 类似计算属性，提取属性(获取值)
- mutations: 修改state值 每个mutation 都有一个字符串的 事件类型 (type) 和 一个 回调函数 (handler)。默认state是第一个参数；调用的时候不是直接调用，而是通过commit来调用（类似订阅发布模式）**必须同步**
- action: 类似于mutation不同的在于他也是提交给mutations **异步操作**
- Modules: 状态树 就是分为多个Modul,分别掉用吧。

### VueRouter 你怎么用的？
> router，vue官方的路由管理器
- 导航守卫（beforeRouteUpdate），重定向好方法
- history 需要额外的配置去除#
- 懒加载 import（compentent
- router-link/router-view/this.$route.push/this.$route.parma

### 路由守卫
- vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。有多种机会植入路由导航过程中：全局的, 单个路由独享的, 或者组件级的。
~~~js const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
// 主要操作是重定向
~~~