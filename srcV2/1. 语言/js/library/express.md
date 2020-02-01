#### 1. 使用

``` `$ npm install express --save```



##### 2. Hello world 例子

```javascript
const express = require('express')
const app = express()

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(3000, () => console.log('Example app listening on port 3000!'))
```



#### 3. Express 应用程序生成器

##### 3.1 安装

`npm install express-generator -g`

- 命令行参数

```javascript
$ express -h

  Usage: express [options] [dir]

  Options:

    -h, --help          输出使用方法
        --version       输出版本号
    -e, --ejs           添加对 ejs 模板引擎的支持
        --hbs           添加对 handlebars 模板引擎的支持
        --pug           添加对 pug 模板引擎的支持
    -H, --hogan         添加对 hogan.js 模板引擎的支持
        --no-view       创建不带视图引擎的项目
    -v, --view <engine> 添加对视图引擎（view） <engine> 的支持 (ejs|hbs|hjs|jade|pug|twig|vash) （默认是 jade 模板引擎）
    -c, --css <engine>  添加样式表引擎 <engine> 的支持 (less|stylus|compass|sass) （默认是普通的 css 文件）
        --git           添加 .gitignore
    -f, --force         强制在非空目录下创建
```



##### 4. 路由

- Route definition takes the following structure:

  ```javascript
  app.METHOD(PATH, HANDLER)
  ```

Where:

- `app` is an instance of `express`.
- `METHOD` is an [HTTP request method](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods), in lowercase.
- `PATH` is a path on the server.
- `HANDLER` is the function executed when the route is matched.



##### 4.1请求

- Respond with `Hello World!` on the homepage:

```javascript
app.get('/', function (req, res) {
  res.send('Hello World!')
})
```

- Respond to POST request on the root route (`/`), the application’s home page:

```javascript
app.post('/', function (req, res) {
  res.send('Got a POST request')
})
```





#### 5. 利用 Express 托管静态文件

为了提供诸如图像、CSS 文件和 JavaScript 文件之类的静态文件，请使用 Express 中的 `express.static` 内置中间件函数。

此函数特征如下：

```javascript
express.static(root, [options])
```
- 通过如下代码就可以将 public 目录下的图片、CSS 文件、JavaScript 文件对外开放访问了

```javascript
app.use(express.static('public'))
```

- 如果要使用多个静态资源目录，请多次调用 `express.static` 中间件函数：

```javascript
app.use(express.static('public'))
app.use(express.static('files'))
```

- To create a **virtual path prefix** (where the path does not actually exist in the file system) for files that are served by the `express.static` function, [specify a mount path](http://www.expressjs.com.cn/4x/api.html#app.use) for the static directory, as shown below:

```javascript
app.use('/static', express.static('public'))
```





#### 6. 路由

*路由*是指应用程序的端点（URI）如何响应客户端请求。

- 可以使用[app.all（）](http://www.expressjs.com.cn/en/4x/api.html#app.all)处理所有HTTP方法，并使用[app.use（）](http://www.expressjs.com.cn/en/4x/api.html#app.use)将中间件指定为回调函数
- 路径： 字符`?`，`+`，`*`，和`()`是他们的正则表达式的对应的子集。连字符（`-`）和点（`.`）由基于字符串的路径按字面意义进行解释

##### 6.1 route

- 您可以使用来为路由路径创建可链接的路由处理程序`app.route()`
- 路径是在单个位置指定的，因此创建模块化路由非常有帮助，减少冗余和错别字也很有帮助

这是使用定义的链式路由处理程序的示例`app.route()`。

```javascript
app.route('/book')
  .get(function (req, res) {
    res.send('Get a random book')
  })
  .post(function (req, res) {
    res.send('Add a book')
  })
  .put(function (req, res) {
    res.send('Update the book')
  })
```



##### 6.2 快速路由器

使用`express.Router`该类创建模块化的，可安装的路由处理程序。一个`Router`实例是一个完整的中间件和路由系统; 因此，它通常被称为“迷你应用程序”

```javascript
var express = require('express')
var router = express.Router()

// middleware that is specific to this router
router.use(function timeLog (req, res, next) {
  console.log('Time: ', Date.now())
  next()
})
// define the home page route
router.get('/', function (req, res) {
  res.send('Birds home page')
})
// define the about route
router.get('/about', function (req, res) {
  res.send('About birds')
})

module.exports = router
```

然后，在应用程序中加载路由器模块：

```javascript
var birds = require('./birds')

// ...

app.use('/birds', birds)
```

该应用程序现在将能够处理对`/birds`和的请求`/birds/about`，以及调用`timeLog`特定于该路线的中间件功能。







#### 7. 中间件

- *中间件*功能是可以访问[请求对象](http://www.expressjs.com.cn/en/4x/api.html#req) （`req`），[响应对象](http://www.expressjs.com.cn/en/4x/api.html#res)（`res`）和`next`应用程序的请求-响应周期中的功能的功能。该`next`功能是Express路由器中的功能，当调用该功能时，将在当前中间件之后执行中间件。
- 中间件功能可以执行以下任务：
  - 执行任何代码。
  - 更改请求和响应对象。
  - 结束请求-响应周期。
  - 调用堆栈中的下一个中间件。
- 如果当前的中间件功能没有结束请求-响应周期，则必须调用`next()`将控制权传递给下一个中间件功能。否则，该请求将被挂起。
- ![image-20200201115730912](https://tva1.sinaimg.cn/large/006tNbRwgy1gbgrkt8x4yj31gg0i846v.jpg)

### 中间件功能myLogger

这是一个称为“ myLogger”的中间件功能的简单示例。当对应用程序的请求通过时，此功能仅显示“已记录”。中间件功能已分配给名为的变量`myLogger`。

```javascript
var myLogger = function (req, res, next) {
  console.log('LOGGED')
  next()
}
```

要加载中间件功能，请调用`app.use()`，指定中间件功能。例如，以下代码`myLogger`在路由到根路径（/）之前加载中间件功能。

```javascript
var express = require('express')
var app = express()

var myLogger = function (req, res, next) {
  console.log('LOGGED')
  next()
}

app.use(myLogger)

app.get('/', function (req, res) {
  res.send('Hello World!')
})

app.listen(3000)
```

- 中间件的加载顺序很重要：首先加载的中间件功能也将首先执行。

## 可配置中间件

如果您需要可配置的中间件，请导出一个接受选项对象或其他参数的函数，然后再根据输入参数返回中间件实现。

文件： `my-middleware.js`

```javascript
module.exports = function(options) {
  return function(req, res, next) {
    // Implement the middleware function based on the options object
    next()
  }
}
```

现在可以如下所示使用中间件。

```javascript
var mw = require('./my-middleware.js')

app.use(mw({ option1: '1', option2: '2' }))
```





#### 8 使用中间件

Express应用程序可以使用以下类型的中间件：

- [应用层中间件](http://www.expressjs.com.cn/guide/using-middleware.html#middleware.application)
- [路由器级中间件](http://www.expressjs.com.cn/guide/using-middleware.html#middleware.router)
- [错误处理中间件](http://www.expressjs.com.cn/guide/using-middleware.html#middleware.error-handling)
- [内置中间件](http://www.expressjs.com.cn/guide/using-middleware.html#middleware.built-in)
- [第三方中间件](http://www.expressjs.com.cn/guide/using-middleware.html#middleware.third-party)



##### 应用层中间件

使用和函数将[应用](http://www.expressjs.com.cn/en/4x/api.html#app)程序级中间件绑定到[应用程序对象](http://www.expressjs.com.cn/en/4x/api.html#app)的实例

- 示例显示了没有安装路径的中间件功能。每次应用收到请求时，都会执行该功能。

```javascript
var app = express()

app.use(function (req, res, next) {
  console.log('Time:', Date.now())
  next()
}
```

- 要从路由器中间件堆栈中跳过其余中间件功能，请调用`next('route')`将控制权传递给下一条路由。 注意： `next('route')`仅适用于使用`app.METHOD()`或`router.METHOD()`函数加载的中间件函数。

- `- /user/:id`路径上安装的中间件功能。该函数针对`/user/:id`路径上的任何类型的HTTP请求执行。

```javascript
app.use('/user/:id', function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})
```

​	此示例显示了路由及其处理程序功能（中间件系统）。该函数处理对`/user/:id`路径的GET请求。

```javascript
app.get('/user/:id', function (req, res, next) {
  res.send('USER')
})
```

##### 路由器级中间件

路由器级中间件与应用程序级中间件的工作方式相同，只不过它绑定到的实例`express.Router()`。

##### 错误处理中间件

以与其他中间件函数相同的方式定义错误处理中间件函数，除了使用四个参数而不是三个参数（特别是使用签名`(err, req, res, next)`

```javascript
app.use(function (err, req, res, next) {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```