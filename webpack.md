# webpack
[知乎webpack](https://zhuanlan.zhihu.com/p/44438844)
### 有哪些常见 loader 和 plugin，你用过哪些？
> loader 加载器主要编译
- babel-loader：把 ES6 转换成 ES5
- css-loader：加载 CSS，支持模块化、压缩、文件导入等特性
- eslint-loader：通过 ESLint 检查 JavaScript 代码
- vue-loader：vue对象的编译
- image-loader：加载并且压缩图片文件
> Plugin 插件 扩展工具
- define-plugin：定义环境变量
- htmlWackpackPlugin 

### webpack构建流程是哪些？
- 初始化数据
- 开始编译
- 确定入口
- 编译开始
- 编译完成
- 输出资源
- 输出完成
  
### webpack的热更新是如何做到的？说明其原理？
> 先了解下什么是热更新，在页面没有刷新的情况下进行数据交互
- 两个插件的了解webpack-dev-server和HotModuleReplacement
- 1. webpack在watch模式下，当监听到文件改变，根本webpack配置对模块重新编译，并将打包后的通关对象保存
- 2. webpack-dev-server与webpack的交互，webpack-dev-server的中间件webpack-dev-middleware调用webpack暴露的API对代码进行监控，并告诉webpack打包到内存中。
- 3. 第三步是 webpack-dev-server 对文件变化的一个监控，这一步不同于第一步，并不是监控代码变化重新打包。当我们在配置文件中配置了devServer.watchContentBase 为 true 的时候，Server 会监听这些配置文件夹中静态文件的变化，变化后会通知浏览器端对应用进行 live reload。注意，这儿是浏览器刷新，和 HMR 是两个概念。
- 4. 第四步也是 webpack-dev-server 代码的工作，该步骤主要是通过 sockjs（webpack-dev-server 的依赖）在浏览器端和服务端之间建立一个 websocket 长连接，将 webpack 编译打包的各个阶段的状态信息告知浏览器端，同时也包括第三步中 Server 监听静态文件变化的信息。浏览器端根据这些 socket 消息进行不同的操作。当然服务端传递的最主要信息还是新模块的 hash 值，后面的步骤根据这一 hash 值来进行模块热替换。
- 5. webpack-dev-server/client 端并不能够请求更新的代码，也不会执行热更模块操作，而把这些工作又交回给了 webpack，webpack/hot/dev-server 的工作就是根据 webpack-dev-server/client 传给它的信息以及 dev-server 的配置决定是刷新浏览器呢还是进行模块热更新。当然如果仅仅是刷新浏览器，也就没有后面那些步骤了。
- 6. HotModuleReplacement.runtime 是客户端 HMR 的中枢，它接收到上一步传递给他的新模块的 hash 值，它通过 JsonpMainTemplate.runtime 向 server 端发送 Ajax 请求，服务端返回一个 json，该 json 包含了所有要更新的模块的 hash 值，获取到更新列表后，该模块再次通过 jsonp 请求，获取到最新的模块代码。
- 7. 而第 10 步是决定 HMR 成功与否的关键步骤，在该步骤中，HotModulePlugin 将会对新旧模块进行对比，决定是否更新模块，在决定更新模块后，检查模块之间的依赖关系，更新模块的同时更新模块间的依赖引用。
- 8. 最后一步，当 HMR 失败后，回退到 live reload 操作，也就是进行浏览器刷新来获取最新打包代码。

### 如何提高webpack的构建速度
- 多入口情况下，使用CommonsChunkPlugin来提取公共代码
- 使用Happypack 实现多线程加速编译
- 利用DllPlugin和DllReferencePlugin预编译资源模块 通过DllPlugin来对那些我们引用但是绝对不会修改的npm包来进行预编译，再通过DllReferencePlugin将预编译的模块加载进来。