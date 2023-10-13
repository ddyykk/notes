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