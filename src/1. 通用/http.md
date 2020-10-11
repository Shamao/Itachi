- Http 1.0
  - 默认不支持长连接(添加头 **connection: keep-alive)
- http 1.1
  - 支持长连接 (添加头 **connection: keep-alive ** 默认添加)
  - 新增more 请求头和响应头
  - 增加了强缓存cache-control、协商缓存etag\if-none-match 是对http/1 缓存的优化
  - 缺点
    - 明文
    - 传输
  - header-太长
    - server 没有主动push
- http 2.0
  - 多路复用(multiplexing): **允许同时通过单一的 HTTP/2 连接发起多重的请求-响应消息。**
    - `HTTP/1.1 协议中 「浏览器客户端在同一时间，针对同一域名下的请求有一定数量限制。超过限制数目的请求会被阻塞」`
    - `这也是为何一些站点会有多个静态资源 CDN 域名的原因之一`
  - 单连接 + 帧
    - HTTP/2与SPDY一样，将一个TCP连接分为若干个流（Stream），每个流中可以传输若干消息（Message），每个消息由若干最小的二进制帧（Frame）组成
  - 头部压缩
    - HPACK算法
  - 二进制编码传输
  - 服务端推送
    - 允许服务器直接提供浏览器渲染页面所需资源，而无须浏览器在收到、解析页面后再提起一轮请求，节约了加载时间。



#### Cache Control

- http缓存机制主要在http响应头中设定，响应头中相关字段为Expires、Cache-Control、Last-Modified、Etag

- Etag是属于HTTP 1.1属性，它是由服务器（Apache或者其他工具）生成返回给前端，用来帮助服务器控制Web端的缓存验证。 Apache中，ETag的值，默认是对文件的索引节（INode），大小（Size）和最后修改时间（MTime）进行Hash后得到的。

  



疑问: 

- get 和 post 的差别

#### 参考

> [http 2 体验](https://http2.akamai.com/demo)
>
> https://juejin.im/post/6844903838768431118
>
> https://www.zhihu.com/question/34074946
>
> https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/232
>
> 