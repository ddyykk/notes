# Docker 笔记

## Docker安装(Ubuntu)

- old version: `sudo apt install docker.io`
- new version: `sudo apt install docker-ce docker-ce-cli containerd.io`其中ce指的是community edition
- 我实际安装时提示找不到包, 按照官网提示, 手动下载三个包使用dpkg 安装

## Docker的使用

- 装完以后可以运行常用命令:
  - docker ps 显示运行的容器 -a 为所有本机上的容器(包括没有运行的)
  - docker stop <容器名> 停止运行特定的容器
  - docker rm <容器名> 删除特定的容器
  - docker images 显示所有下载的镜像原始文件
  - docker rmi <镜像名> 删除特定镜像文件
  - docker run <镜像名> <版本号> 运行特定镜像, 如果本地没有就会从网站下载, 如不指定版本号默认用最新版
  - docker pull <镜像名> <版本号> 下载特定的镜像
  - docker container cp [options] CONTAINER:SRC_PATH DEST_PATH 从镜像里面把文件/目录拷贝到本地文件系统
  - docker container exec <镜像名> <命令> 对于特定的镜像, 在镜像里面执行特定的命令, 有了这个命令, 就可以让镜像里面执行命令, 例如备份, 查看文件等等
  - docker exec -it <镜像名> bash , 进入镜像的命令行环境去执行命令
- docker container exec 可以被 docker exec 代替, 两者的作用都是在一个运行的镜像中运行命令. 但是docker run 不一样, 他会每次重新打开一个镜像, 然后运行输入的命令.
- 有关于 docker  exec 使用的一些注意事项:
  - docker exec 跟的必须是可执行的命令, 例如一个程序的名字, vim bash 之类的, 如果要执行一系列的程序之间还要用`&&`之类的就不行, 会被docker当成多个参数, 而不是 一个完整的命令. 如果要执行一系列的命令, 最好写成字符串用引号包裹, 由 sh 来调用. 如果命令太多就写成脚本文件.
  - 如果docker exec 执行的命令会进入一个环境进行进一步操作的话, 要带上参数 -i -t, i 是interactive, 系统会传入STDIN 流, 意思就是会传输用户输入进去, 默认没有输入能力就无法在里面的环境里使用. -t的作用是会生成一个虚拟的终端, 没有终端会没有命令提示符等一些环境.
  - 例如 以下命令要调用一个docker 容器里面的mongodb 数据库的shell 后台, 执行成功之后会立即退出: `docker exec mongo_1 mongosh`, 因为没有 `-it`这两个参数. 如果带上这两个参数就可以顺利进入新环境.

# docker-compose

- 容器启动时会运行默认命令, 对于多数镜像就是没有命令, 所以需要指定命令, 否则镜像运行完毕就会退出
- docker-compose 是一个库, 使用一个 .yaml ( 或者 .yml )文件来组合多个容器, 类似脚本. 
  - .yml 文件要指定里面组件的版本, 可参考官方文档.
- docker registry 是一个镜像托管网站, 收录很多镜像供人使用. pull镜像的时候要写名字为 <用户名>/<镜像名>, 如果只写一个, 代表两个名字一样