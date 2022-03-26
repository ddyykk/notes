# MongoDB的笔记

MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组.

## MongoDB在WSL2 上的安装

- https://docs.microsoft.com/zh-cn/windows/wsl/tutorials/wsl-database#install-mongodb 根据微软的官方指导经行安装.

- 按照微软的指示, 安装完的包名称叫 mongodb-org , MongoDB官网上说只有这个包才是官方发布和维护的. Ubuntu 提供的包名字是 mongodb 并不是官方提供和维护的, 并且和官方的程序有冲突  (https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/)

- 安装完成会有几个默认的设定, 数据放在 /data/db , 默认的数据库服务器端口为 27017.  守护程序为mongod, shell 程序为 mongosh, 老版为mongo.

  ## MongoDB的使用

- 进入shell后用db命令查看当前使用的数据库, use 命令指定使用的数据库, 如果不存在会新建, 但是要插入数据才会真正建立.

- 一些常用的命令: 
  - show dbs 显示所有非空数据库
  - show collections 显示当前数据库里面的表格
  - db.dropDatabase() 删除当前数据库
  - db 显示当前数据库
  - use 指定当前数据库 (不存在会新建)
  - db.creatCollection(name, options) 建立新的表
  - mongodump --archive > mongodump-$(date -I).dump 将当前的数据库备份到 `mongodump-(当前日期).dump`的文件
  - mongorestore --archive < /path 将path路径的文件恢复到数据库里面
  - 如果在docker中运行着的mongodb, 就用`docker exec -i <mongo镜像名> sh -c 'mongorestore --archive' < ~/data/mongodump-2020-04-04.dump` 注意, 这里面的dump 文件的路径是存在于容器以外的.