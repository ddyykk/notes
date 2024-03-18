- node 是一个Javascript 的本地运行环境, 在他出现以前, Javascript只能在浏览器中运行. 然后有人用Chrome 的V8引擎写了一个可以本地运行的Javascript引擎就是node.

## 安装node

- 可以官网下载安装包, linux也可以用apt来安装
- 验证安装 `node --version && npm --version`
- 使用npm安装软件包有两种模式, 全局和当前文件夹, 如果不指定的话, 默认是安装在当前文件夹. 要全局安装要使用 `npm install -g`参数
- 每个项目放在一个文件夹里面
- 项目文件夹里面要包含`package.json`文件, 用来记录项目设置和属性, 项目需要的安装包也会在里面记录, 以后就可以用一个命令安装所有记录的软件包.
- 如果新建项目, 可以在项目文件夹里面使用`npm init`初始化项目文件夹, 主要就是建立`package.json`文件, 过程会要求输入一些项目设置, 可以全部回车, 或者用`npm init -y`来初始化, 就会全部用默认的属性.
- 初始化以后的文件夹, 如果要安装软件包, 并且希望软件包被加入`package.json`的列表的话, 安装就要使用`npm install --save <package name>` 或者 `npm -S <package name>`
- 可以使用一个软件包`nodemon`, 可以在修改服务器文件后自动重启服务, 而无需每次手动操作. 可以全局安装或者本地安装, 这里采用全局安装. 安装以后, 启动服务器的命令就变成`nodemon app.js`. 如果使用本地安装, 那么不能在命令行里直接调用, 要使用脚本. 例如`npm start`
- 使用`express`, 这个是node最受欢迎的后端框架, 使用`node install -S express`安装. 其中常见的函数有`express.get(),express.post(),express.put(),express.set()`

## node 基本知识

- node 默认使用非同步的线程来运行程序, 意味着一个线程可以服务多个程序, 带来更好的性能和更低的延迟. 非常适合于网络服务器后端这样的场景. 但是不适合于高负载大运算的场景, 例如本地大型程序, 需要处理大量数据或者大文件的场景.
- 新建项目文件夹以后开始使用, 首先建立一个js文件: `app.js`, 命名不重要, 但是运行的时候要知道这个是程序的起点. 使用js文件是因为node全部都是围绕Javascript来写的. 默认的编辑器使用VS Code.
- 在`app.js`里面写入如下的代码:
```js
function sayHello(name) {
	console.log('Hello ' + name);
}

sayHello('Johnson');
```
- 保存以后使用命令行, 进入项目文件夹, 使用命令`node app.js` 就可以运行上面的程序.
- node常用的四个模块(module), fs(file system), os, event, http. 在上面的例子中没有任何的引用语句就使用了console.log()这个函数. 说明console是一个全局对象(global object). 他的完整形式是`global.console.log()`. 但是如果在文件中定义一个变量, 那么他就不是全局变量, 在这个文件以外的代码是无法使用这个变量的. 如果要使用的话就要导出(export). 一个文件, 就被称为一个模块(module) .
- 那么在同一路径下新建文件`logger.js` , 在其中写入一些用来模拟登录的代码:
```JavaScript
var url = 'http://mywebsite.io/log';

function log(message) {
	//HTTP request
	console.log(message);
}

module.exports.log = log;
module.exports.engPoint = url;
//注意使用驼峰命名, 然后导出时可以进行重命名
//这样导出的是一个object, 不是一个function. 如果要导出function就写:
//module.exports = log;
//调用的时候也就变为 logger('My message.');
```
- 然后在`app.js` 中, 导入的命令并不是`import`, 而是`require()`. 代码如下:
```js
const logger = require('./logger.js')
//后缀名可省略, 但不省略更加清晰. 因为在同一文件夹下, 路径这样写就可以. 必须要一个变量来接收这个导入的模块. 但最好用const 不用 var, 不然可能会被意外更改.
logger.log('My message.');

```
## express的route

```JavaScript
var express = require('express');
var app = express();

app.get('/hello', function(req, res){
   res.send("Hello World!");
});

app.listen(3000);
```

- 以上是express的基本服务写法, 网址为`/hello`, 还可以对同一个地址有多个http方法:

```JavaScript
app.post('/hello', function(req, res){
   res.send("You just called the post method at '/hello'!\n");
});
```

- 要访问上面的post方法, 不能用浏览器直接访问, 可以使用命令行工具:`curl -X POST "http://localhost:3000/hello"`

- 还有个函数可以处理所有的http方法, `express.all()`

  ```JavaScript
  app.all('/test', function(req, res){
     res.send("HTTP method doesn't have any effect on this route!");
  });
  ```

- 对于不同的路径(在服务器上就不应该叫path了, 应该叫route), 可以设置不同的函数来对应. 例如下面的内容和刚才的代码看似一样, 但是我们把他单独存在一个文件里, 起名`things.js`

  ```JavaScript
  var express = require('express');
  var router = express.Router();
  
  router.get('/', function(req, res){
     res.send('GET route on things.');
  });
  router.post('/', function(req, res){
     res.send('POST route on things.');
  });
  
  //export this router to use in our index.js
  module.exports = router;
  ```

- 在主程序里面写一句话来调用这个文件

  ```JavaScript
  var express = require('Express');
  var app = express();
  
  var things = require('./things.js');
  
  //both index.js and things.js should be in same directory
  app.use('/things', things);
  
  app.listen(3000);
  ```

  这样的作用就是, 要访问上面的内容, 需要的网址变成了`localhost:3000/things/`

- 用这样的方法, 可以把一个子路径的访问都放在一个js文件里面, 便于管理. 主程序`app.js`只负责调用即可.

## 动态路径(route)

- 现代网站中的网址请求路径往往不是预先规定好的, 而是根据模板动态生成的. 一个动态网址请求的例子: 

  ```JavaScript
  var express = require('express');
  var app = express();
  
  app.get('/:id', function(req, res){
     res.send('The id you specified is ' + req.params.id);
  });
  app.listen(3000);
  ```

  以上程序, 用户在网址上输入什么, 就会被当作id这个参数, 然后原样返回出来. 然后还可以写一个更复杂的例子:

  ```JavaScript
  var express = require('express');
  var app = express();
  
  app.get('/things/:name/:id', function(req, res) {
     res.send('id: ' + req.params.id + ' and name: ' + req.params.name);
  });
  app.listen(3000);
  ```

  以上程序, 可以获取到用户输入的两级参数. 使用`req.params`来获取所有的参数. 以上两个例子是不同的路径, 需要两个不同的定义来处理. 如果什么参数都没有, 同样也要单独定义, 否则会返回404错误.

### 路径匹配

- 动态路径的参数可以使用正则表达式来划定范围, 例如:

  ```JavaScript
  var express = require('express');
  var app = express();
  
  app.get('/things/:id([0-9]{5})', function(req, res){
     res.send('id: ' + req.params.id);
  });
  
  app.listen(3000);
  ```

  以上程序限定了id这个参数只能为5位数字.

- 如果用户请求了这个范围以外的路径, 就会返回一个错误信息. 也可以手动写一个错误信息.

  ```JavaScript
  //Other routes here
  app.get('*', function(req, res){
     res.send('Sorry, this is an invalid URL.');
  });
  app.listen(3000);
  ```

  把这个错误信息放在其他所有路径处理的后面, 也就是说前面所有匹配不到的路径会被这个匹配并且给出错误信息.

## middleware(中间件)

- 中间件的一个常见的标志是`next()`函数.

- 中间件是用来处理客户的请求和发送的响应的. 他们可以对响应内容进行更多的处理. 以下是一个例子:

  ```JavaScript
  var express = require('express');
  var app = express();
  
  //Simple request time logger
  app.use(function(req, res, next){
     console.log("A new request received at " + Date.now());
     
     //This function call is very important. It tells that more processing is
     //required for the current request and is in the next middleware
     //function route handler.
     next();
  });
  
  app.listen(3000);
  ```

  这个程序每收到一个http请求就会在服务器上面输出一行字记录收到的时间. 但是并没有写响应客户的内容.

- 然后加上响应客户端的内容, 并且只会对特定的route做出响应, 例子如下:

  ```JavaScript
  var express = require('express');
  var app = express();
  
  //Middleware function to log request protocol
  app.use('/things', function(req, res, next){
     console.log("A request for things received at " + Date.now());
     next();
  });
  
  // Route handler that sends the response
  app.get('/things', function(req, res){
     res.send('Things');
  });
  
  app.listen(3000);
  ```

  - middleware 的作用就是把处理数据的步骤和响应本身分成多步了, 之间用`next()`连接, 如果没有`next()`这个函数, 那么存在两个一样的route都是`/things`就只有第一个会被执行.

  - middleware 的好处是，它可以复用。比如刚才这个打印时间的例子，可以把这个 middleware 用在所有的 url 上，那么每个请求都会打出时间和日志.

- 执行的时候是按照从上到下的顺序执行.

### 一些第三方的常用中间件

#### body-parser

- 这个中间件用来解析客户端请求的主体. 安装: 

  `npm install --save body-parser`

- 使用方法:

  ```JavaScript
  var bodyParser = require('body-parser');
  
  //To parse URL encoded data
  app.use(bodyParser.urlencoded({ extended: false }))
  
  //To parse json data
  app.use(bodyParser.json())
  ```

#### cookie-parser

- 安装 `npm install --save cookie-parser`

- 使用 

  ```JavaScript
  var cookieParser = require('cookie-parser');
  app.use(cookieParser())
  ```

#### express-session

- 

## 模板引擎 Mustache(适应性较广, 对各种后端都有适配)

- 安装命令: `npm install mustache --save`

## 模板引擎 pug (曾经比较受欢迎)

- 现代网页编程一般大量使用模板, 网页的内容都是收到请求以后动态生成的.

- 安装 `npm install --save pug`, 无需要使用require关键词, 使用如下语句:

  ```JavaScript
  app.set('view engine', 'pug');
  app.set('views','./views');
  ```

  可以看到模板被叫做views, 放在 `./views`文件夹

- 在文件夹中新建一个文件` first_view.pug`, 填入下面内容:

  ```pug
  doctype html
  html
     head
        title = "Hello Pug"
     body
        p.greetings#people Hello World!
  ```

  然后使用下面的代码来调用模板:

  ```javascript
  app.get('/first_template', function(req, res){
     res.render('first_view');
  });
  ```

- 在以上写法中, 无需再使用html tag, 直接写tag名称即可. class 用加点的方式来表达, 例如 `p.greetings`, 元素的id用`#`来表示, 例如上面的 `p.greetings#people`, 经过pug引擎渲染后就会输出这样的的语句:` <p class = "greetings" id = "people">Hello World!</p>`

- 上面的模板渲染完成后会变成如下:

  ```html
  <!DOCTYPE html>
  <html>
     <head>
        <title>Hello Pug</title>
     </head>
     
     <body>
        <p class = "greetings" id = "people">Hello World!</p>
     </body>
  </html>
  ```

### pug的一些使用方法

- 元素的递进关系依靠缩进来区分, 同样缩进的元素处于同一级别, 缩进更多的元素是前面元素的子元素.

- 文字加在元素有三种方法, 第一使用空格: `h1 Welcome to Pug`

  第二使用竖线, 或者pipe

  ```pug
  div
     | To insert multiline text, 
     | You can use the pipe operator.
  ```

  第三整段文字:

  ```pug
  div.
     But that gets tedious if you have a lot of text.
     You can use "." at the end of tag to denote block of text.
     To put tags inside this block, simply enter tag in a new line and 
     indent it accordingly.
  ```

- 注释的风格和JavaScript一致, 但是引擎会在渲染的时候转为html风格.

- html元素的属性, 使用括号, 属性之间用逗号分隔, 例子:

  `div.container.column.main#division(width = "100", height = "100")`相当于

  `<div class = "container column main" id = "division" width = "100" height = "100"></div>`

### 向模板中传递变量

- 可以从route handler中向模板传递变量.

  ```JavaScript
  var express = require('express');
  var app = express();
  
  app.get('/dynamic_view', function(req, res){
     res.render('dynamic', {
        name: "TutorialsPoint", 
        url:"http://www.tutorialspoint.com"
     });
  });
  
  app.listen(3000);
  ```

  与之对应的模板是

  ```pug
  html
     head
        title=name
     body
        h1=name
        a(href = url) URL
  ```

  这样就会把程序中变量的值 `name`和`url`传递给模板. 还可以把变量中的文字也传递到网页上显示, 将模板修改成下面这样:

  ```pug
  html
     head
        title = name
     body
        h1 Greetings from #{name}
        a(href = url) URL
  ```

  语法`#{变量名}`就会将变量的内容原样显示出来.

### 模板中使用条件判断. 

- 假设登录完成以后页面会显示用户名和问候信息, 如果不登录就显示登录按钮. 就可以用这样的模板.

  ```pug
  html
     head
        title Simple template
     body
        if(user)
           h1 Hi, #{user.name}
        else
           a(href = "/sign_up") Sign Up
  ```

  在渲染模板的时候, 如果变量name没有被传递, 就不会显示问候内容.

### 引用和包含

- 如果要设计一个网站, 网站所有的页面头部都是一样的, 菜单也是一样的. 那么不用把同样的头部代码复制到每一个页面上去, 只要建立一个模板专门设计头部即可.

  ```pug
  //header.pug
  div.header.
     I'm the header for this website.
  ```

  然后在主页面模板里面引用即可

  ```pug
  //content.pug
  html
     head
        title Simple template
     body
        include ./header.pug
        h3 I'm the main content
        include ./footer.pug
  ```

  然后页脚也有对应的文件, 引用即可

  ```pug
  div.footer.
     I'm the footer for this website.
  ```


## 提供静态文件的响应

- express默认不允许静态文件直接的输出, 要使用以下语句:`app.use(express.static('public'));`, 所有的文件要存在`public`文件夹下面. 需要注意的是请求的路径是以上面设定的静态文件夹作为相对路径的起点的, 但是文件夹名称本身不用包含在请求里面. 
- 网站的根目录现在就被设定为`public`文件夹, 所有的文件会把`public`文件夹当作根目录.

### 设定多个静态文件夹

- 可以设定多个文件夹来提供静态文件访问

  ```JavaScript
  var express = require('express');
  var app = express();
  
  app.use(express.static('public'));
  app.use(express.static('images'));
  
  app.listen(3000);
  ```

### 虚拟的路径前缀

- 虚拟的路径意思是, 客户端请求这个路径, 但是这个路径在服务器上并不存在, 而是被指向了另外的一个文件夹. 例如可以让客户请求这个网址:`/static`, 那么就要在代码中设置下面的语句:

  ```JavaScript
  app.use('/static', express.static('public'));
  ```

  这样当你需要使用一个`public`文件夹里面的文件, 比如`main.js`, 那么在html页面代码里面写这个就可以:

  `<script src = "/static/main.js" />`.  `/static`这个文件夹地址是虚拟的, 并不存在.

- 这个虚拟路径的作用主要是当存在多个静态文件夹的时候, 可以用虚拟路径来做区分, 不用所有的文件都是一个路径.

## 表单

- 几乎所有的网站都会使用表单来存储和提交复杂的数据. 要处理表单类数据, 要使用两个中间件, body-parser和multer
- body-parser用来处理JSON 和 URL相关的数据, multer用来处理多段或者表格类的数据
- 先在项目文件夹进行安装`npm install --save body-parser multer`

### node中的当前目录问题

- 有如下代码

  ```JavaScript
  function serverOn (request,response){  
      const q=url.parse(request.url,true);
      const filename="./html"+q.pathname;
  
      fs.readFile(filename,function(err,data){
          if (err){
                      response.writeHead(404, {"Content-Type": "text/html"});
                      return response.end("404 Not Found");
                  }   
          response.writeHead(200,{"Content-Type":"text/html"});
          response.write(data);
          response.write(uc.upper(str));
          return response.end();
  })
  }
  ```

- 以上代码中, 如果在terminal中执行命令: `node ./server.js`就可以顺利运行, 而如果在`server.js`所在的文件夹外运行文件, 同样可以让服务器开启, 但是会找不到文件, 总是返回404错误. 例如命令`node ./node/server.js`, 就会永远返回404.

- 经过排查, 发现在`server.js`所在的文件夹外运行时, 指代相对路径中的`./html`这个点的意义会发生改变, 因为`html`文件夹与`server.js `放在同一位置, 当在`node`文件夹内运行时, 点就指代`server.js`所在的文件夹, 当在文件夹外运行时, `.`就指代另外一个文件夹, 这样`./html`这个路径当然就不存在了. 也就会永远找不到文件.

- 而在前端代码中这个问题要小得多, 因为前端代码一般都是由html文件开始运行的, 运行环境由浏览器提供, 那么只要把文件夹和html文件放在同一目录内就可以了.

- 解决办法, 使用模块`path`,`const path = require('path');`, 然后`app.use(express.static(path.join(__dirname, 'public')));`. `__dirname`这个变量指代的是当前的绝对路径,  不会再有相对路径产生歧义的问题. 然后用`path.join()`把当前路径和需要附加的文件夹拼合在一起.