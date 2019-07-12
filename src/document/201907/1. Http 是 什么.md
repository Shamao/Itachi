#### Http是什么

- HTTP协议是Hyper Text Transfer Protocol（超文本传输协议)

- 一个基于TCP/IP通信协议来传递数据

- 一个属于应用层的面向对象的协议, 有Http1.0、Http1.1、Http2.0

  


#### URL 是什么
- 全称是UniformResourceLocator, 中文叫统一资源定位符,是互联网上用来标识某一处资源的地址. HTTP使用URL来传输数据和建立连接。
- 组成部分 : protocol://host: port/path
  - protocol : 协议
  - host: 主机名
  - port: 端口
  - path : 路径



#### 报文
##### 请求报文

- 请求行 : 用来说明请求类型,要访问的资源以及所使用的HTTP版本
- 请求头 : 请求头部，紧接着请求行（即第一行）之后的部分，用来说明服务器要使用的附加信息
- 空    行 :  空行，请求头部后面的空行是必须的
- 请求体 : 请求数据也叫主体，可以添加任意的其他数据。

![image-20190709014409705](/Users/saidengliu/Library/Application Support/typora-user-images/image-20190709014409705.png)

![image-20190709014452851](https://upload-images.jianshu.io/upload_images/2964446-fdfb1a8fce8de946.png)


##### 响应报文

- 状  态  行 : 由HTTP协议版本号， 状态码， 状态消息 三部分组成
- 消息报头 : 用来说明客户端要使用的一些附加信息
- 空        行 : 消息报头后面的空行是必须的
- 响  应  体 : 响应正文，服务器返回给客户端的文本信息

![image-20190709014452851](/Users/saidengliu/Library/Application Support/typora-user-images/image-20190709014452851.png)


#### 状态码

- 1xx：指示信息--表示请求已接收，继续处理
- 2xx：成功--表示请求已被成功接收、理解、接受
- 3xx：重定向--要完成请求必须进行更进一步的操作
- 4xx：客户端错误--请求有语法错误或请求无法实现
- 5xx：服务器端错误--服务器未能实现合法的请求

##### 常见状态码
- 200 OK                                //客户端请求成功
- 400 Bad Request               //客户端请求有语法错误，不能被服务器所理解
- 401 Unauthorized              //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用 
- 403 Forbidden                    //服务器收到请求，但是拒绝提供服务
- 404 Not Found                    //请求资源不存在  
- 500 Internal Server Error   //服务器发生不可预期的错误
- 503 Server Unavailable       //服务器当前不能处理客户端的请求，一段时间后可能恢复正常



#### Http请求方法

- HTTP1.0定义了三种请求方法： GET, POST 和 HEAD方法
- HTTP1.1新增了五种请求方法：OPTIONS, PUT, DELETE, TRACE 和 CONNECT 方法。

#### Http 工作原理

- **建立TCP连接**, 客户端连接到Web服务器
- **发送HTTP请求**,发送一个文本的请求报文
- **服务器接受请求并返回HTTP响应报文**, 
- **释放连接TCP连接**
- **客户端浏览器解析HTML内容**

