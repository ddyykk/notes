# node 部署一个完整网站的笔记

1. 新建项目文件夹, 例如 `test`
2. 安装node.js 在系统里
3. 在文件夹里面安装需要的package, 例如express, express-generator, 等
4. 将所有的html静态文件放入`public`文件夹, 不然就要在主程序中指出单独的目录, 例如所有文件都在public中时, 只需一句代码`app.use(express.static(path.join(__dirname, 'public')));`, 如果CSS 文件不在public文件夹里面, 就要再加一句`app.use('/css',express.static(path.join(__dirname, 'stylesheets')));`
5. 使用 `npx express-generator`命令初始化项目文件夹, 这一步操作可以替代原来的初始化`node init`
6. 
7. 