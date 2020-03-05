## vue
### 说一下watch和computed的区别
> 翻一下，watch监听属性， computed计算属性
- computed计算属性有缓存属性，在他属性依赖没有变化的时候，他是不会计算的; watch属性则看到就会计算一次
- watch属性可以做除了计算的额外的属性，比如子父组建的通讯也就是上报属性
  
### Vue 有哪些生命周期钩子函数？分别有什么用？
- beforeCreate
> 在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。在compilte之前 
- create
> 在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测 (data observer)，属性和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前尚不可用。实例化data，但是mouted渲染dom

- beforemount
> 在挂载开始之前被调用：相关的 render 函数首次被调用。
- mounted
> 实例被挂载后调用，这时 el 被新创建的 vm.$el 替换了。 如果根实例挂载到了一个文档内的元素上，当mounted被调用时vm.$el也在文档内。
- beforeUpdata
> 数据更新时调用，发生在虚拟 DOM 打补丁之前。这里适合在更新之前访问现有的 DOM，比如手动移除已添加的事件监听器。
- updata
> 当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用计算属性或 watcher 取而代之。
- beforedestory
> 实例销毁之前调用。在这一步，实例仍然完全可用。
- destory
> 实例销毁后调用。该钩子被调用后，对应 Vue 实例的所有指令都被解绑，所有的事件监听器被移除，所有的子实例也都被销毁。

### Vue 如何实现组件间通信？
- 父子： $on('name', method(value))  $emit('name', data)
- 爷孙就多用几个$on
- 多组间通信就vuex/eventBus
- provide / inject
- $parent/$child

### $nextTick什么时候使用
**在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。**

数据变化以后的要对dom操作重新渲染就应该放到这里

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

### vue/react中的key是干嘛的
个人理解
- key 基本都是挂载在虚拟dom上用于判别是否为sameVnode
- 在不挂载key的时候，key的值一直是underfined，underfined === underfined,在template变化的时候，组建会复用/修改Vnode.
- 在挂载key的时候，在相同的key情况下组建修改的时候，组建会复用。而不是相同的key的时候,组建变化会销毁和创建vnode，在dom中添加移除节点是网页比较大的消耗。所以才会说**不带上key性能更加，反正dom都是复用**
- 使用key的好处： 一般使用组建的时候，每个Vnode都有自己的状态，当组建进行操作的时候，复用了之前的组建，保留之前的状态。当使用key的时候每次都会重新挂载上组建，拥有最正确的状态。

