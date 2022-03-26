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
- 容器启动时会运行默认命令, 对于多数镜像就是没有命令, 所以需要指定命令, 否则镜像运行完毕就会退出
- docker-compose 是一个库, 使用一个 .yaml ( 或者 .yml )文件来组合多个容器, 类似脚本. 
  - .yml 文件要指定里面组件的版本, 可参考官方文档.
- docker registry 是一个镜像托管网站, 收录很多镜像供人使用. pull镜像的时候要写名字为 <用户名>/<镜像名>, 如果只写一个, 代表两个名字一样