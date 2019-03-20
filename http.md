## http (超文本传输协议)
### 状态吗
- 100 临时相应，客户端继续请求
- **成功**
- 200 OK
- 201 请求已经响应并且创建一个新的资源
- 202 请求收到但是没有相应
- **重定向**
- 300 请求资源有一部分提供反馈信息
- 301 永久重定向
- 302 暂时重定向
- 303 请求方式出错
- 304 请求成功，浏览器通过缓存请求
- **客户端**
- 400 语义有错
- 401 需要验证尤其是头部信息
- 403 Forbidden禁止访问
- 404 找不到
- 405 方法不被允许
- **服务器响应**
- 500 服务器报错
- 501此方法不被服务器支持

### HTTP 缓存有哪几种？
先说说MDN上面对缓存的解释[MDN缓存](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Caching_FAQ)
缓存分为： 
> (私有)浏览器缓存

私有缓存只能用于单独用户。你可能已经见过浏览器设置中的“缓存”选项。浏览器缓存拥有用户通过 HTTP 下载的所有文档。这些缓存为浏览过的文档提供向后/向前导航，保存网页，查看源码等功能，可以避免再次向服务器发起多余的请求。它同样可以提供缓存内容的离线浏览。
> (共享)代理缓存

共享缓存可以被多个用户使用。例如，ISP 或你所在的公司可能会架设一个 web 代理来作为本地网络基础的一部分提供给用户。这样热门的资源就会被重复使用，减少网络拥堵与延迟。

> http缓存
- Cache-control: max-age=31536000
- ETags: time

### get/post区别
> 先说明一下这个问题是一点水平都没有，这个接口的请求方式不是后端定的吗？他想咋样就咋样，不然还和你闹情绪，我特么....
> 说下区别吧
- **get获取资源**  **post请求提交资源** ***翻译下就OK***
- get的参数可以直接在url上看到，post的参数写在requsetbody
- get参数的长度有限制，post没有
- get请求浏览器会主动cache，而Post没有当然可以
- get只能url编码，post支持多种
- get不如post安全
  
### Cookie V.S. LocalStorage V.S. SessionStorage V.S. Session
> cookie localstroage （在我看来localstroage就是浏览器的数据库呀）
- cookie也是存储在浏览器中的
- 这两个东西一个会发送到服务端，一个存在客户端
- cookie大小4k左右，localstroage大小5M
> LocalStorage V.S. SessionStorage
- 都是本地数据存储，localstroage一般不会清除除非手动，SessionStorage关闭就关闭
> SessionStorage V.S. Session
- Session存在于服务端的文件中，而SessionStorage在浏览器存储中
- session是基于cookie实现的，把sessionid存在cookie中