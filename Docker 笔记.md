# Docker 笔记

- 为什么要用docker? 他的使用类似虚拟机, 但是比虚拟机轻量化, 不需要虚拟整个操作系统, 只虚拟了需要的部分. 由于轻量化, 可以多个容器并用, 构建出复杂的多应用合作的app. 如果用虚拟机或者真实的电脑有可能需要多个电脑, 因为有些多应用环境不能在一个电脑上共存.

## Docker安装(Ubuntu)

- old version: `sudo apt install docker.io`

- new version: `sudo apt install docker-ce docker-ce-cli containerd.io`其中ce指的是community edition

- 我实际安装时提示找不到包, 按照官网提示, 手动下载三个包使用dpkg 安装, 安装步骤比较多, 参考官网文档.

- 有时安装完需要重启才会正常运行.

- 安装包安装完成以后要使用sudo才能调用docker, 要想别的用户也能使用, 要加入新的docker用户:

  1. `sudo groupadd docker` Create the `docker` group.
  2. `sudo usermod -aG docker $USER` Add your user to the `docker` group.
  3. Log out and log back in so that your group membership is re-evaluated. 

  - 然后就可以尝试不使用sudo来调用docker.

- docker 开机启动:

  - 在Ubuntu上面默认是自动启动, 在其他发行版上使用下面的命令:

    ```
     sudo systemctl enable docker.service
     sudo systemctl enable containerd.service
    ```

  - 要关闭自启动, 把 enable 换成 disable. 

## Docker的使用

- 装完以后可以运行常用命令:
  - docker run -d -p 80:80 <容器名> -d ( detached 脱离的 )为后台运行, -p 为端口指定, 80:80 是将容器的端口对应本地主机的端口. 其中前面一个端口号是主机的端口号, 后面的端口号是虚拟容器的端口号.
  - docker ps 显示运行的容器 -a 为所有本机上的容器(包括没有运行的)
  - docker stop <容器名> 停止运行特定的容器
  - docker rm <容器名> 删除特定的容器
  - docker images 显示所有下载的镜像原始文件
  - docker rmi <镜像名> 删除特定镜像文件. 如果这个镜像曾经运行过的话, 应该在系统中有这个镜像的容器, 要先删除关联的容器, 才能删除镜像.
  - docker run <镜像名> <版本号> 运行特定镜像, 如果本地没有就会从网站下载, 如不指定版本号默认用最新版. 
    - 每次使用 run 这个命令都会从镜像建立一个新的容器, 这很可能不是我们需要的. 常见的使用逻辑是容器会退出, 但是下一次还想要使用这个容器而不是新建一个, 不然容器会越来越多. 所以第一次可以使用 run 命令建立新容器, 后面就应该使用 `docker start` 来运行已有的容器. 但是由于两个命令使用的环境有略微区别, 有时 start 以后要加上一些参数 例如 -a -i 才能获得同样的输出.
    - 每次新建立的容器会被系统随机起名字, 可以用命令 ` docker rename <old name> <new name>`给容器命名.
    - 也可以使用 --rm 参数, 在退出容器的时候自动删除容器.
    - -v <本地路径>:<容器路径> 这个参数的作用是把一个本地文件夹和容器内的路径绑定, 这样可以在容器内外传输数据. 意思就是在主机上操作这个文件夹里面的内容, 也会同步操作到容器里面. 容器也可以对主机上的这个文件夹操作,  这个操作叫做 bind mount. 注意如果将系统目录映射到容器里面可能会被不小心删除, 会有安全问题.
  - docker pull <镜像名> <版本号> 下载特定的镜像
  - docker container cp [options] CONTAINER:SRC_PATH DEST_PATH 从镜像里面把文件/目录拷贝到本地文件系统, 也可以把文件拷入镜像. 只要把镜像名和本机路径对调就可以.
  - docker container exec <镜像名> <命令> 对于特定的镜像, 在镜像里面执行特定的命令, 有了这个命令, 就可以让镜像里面执行命令, 例如备份, 查看文件等等
  - docker exec -it <镜像名> bash , 进入镜像的命令行环境去执行命令
- docker container exec 可以被 docker exec 代替, 两者的作用都是在一个运行的镜像中运行命令. 但是docker run 不一样, 他会每次重新打开一个镜像, 然后运行输入的命令.
- 有关于 docker  exec 使用的一些注意事项:
  - docker exec 跟的必须是可执行的命令, 例如一个程序的名字, vim bash 之类的, 如果要执行一系列的程序之间还要用`&&`之类的就不行, 会被docker当成多个参数, 而不是 一个完整的命令. 如果要执行一系列的命令, 最好写成字符串用引号包裹, 由 sh 来调用. 如果命令太多就写成脚本文件.
  - 如果docker exec 执行的命令会进入一个环境进行进一步操作的话, 要带上参数 -i -t, i 是interactive, 系统会传入STDIN 流, 意思就是会传输用户输入进去, 默认没有输入能力就无法在里面的环境里使用. -t的作用是会生成一个虚拟的终端, 没有终端会没有命令提示符等一些环境.
  - 例如 以下命令要调用一个docker 容器里面的mongodb 数据库的shell 后台, 执行成功之后会立即退出: `docker exec mongo_1 mongosh`, 因为没有 `-it`这两个参数. 如果带上这两个参数就可以顺利进入新环境.
  - 如果一个 容器已经退出, 就不能使用 exec 命令, 而要使用 run 命令, 从 image 新建一个容器, 但也可以后面加上 bash 命令进入容器去执行命令. 要记得 -it 参数.
- docker不允许在指定容器内部路径的时候使用相对路径名称.(~,/ 等等)
- docker volume 是一个主机上的数据区域, 用户可以建立和删除, 但是不能在容器外访问, 必须要映射到容器里面才能访问. volume里面的内容不会随着容器退出而消失, volume 的数据是独立于容器而存在的, 这样给多个容器共享数据提供了可能. 
  - volume 的使用和 mount bind 类似, 在 docker run 命令后使用-v 参数, 本地的路径就可以指定为一个volume的名称.
  - volume 的建立: 命令 `docker volume create <名称> `
  - 可以使用 `docker volume inspect <volume name>`来检视 volume的具体信息.
  - `docker volume ls` 列出所有存在的 volume
  - `docker volume rm <name>` 删除指定的 volume

- 执行命令时如果出现提示信息: pid file found, ensure docker is not running or delete /var/run/docker.pid 意思是这个已经在运行了.
- 以拷贝文件的方式得来的 docker 镜像, 要用 docker load 命令加入进去 image 库才能用.
- docker tag <镜像名> <tag名> 可以用来给镜像起名字

### 自建第一个docker容器

- 建立一个任意文件夹, 此处我建立了dockertest, 在文件夹中随意放入一个文本文件, 这里我放入test.txt,  里面有一些任意内容即可.

- 在dockertest文件夹下面建立Dockerfile文件: ` touch Dockerfile`, 记得D要大写. 文件中写入下列内容:

  ```
  FROM ubuntu:20.04
  #意思是建立一个基础虚拟的环境, 安装Ubuntu 20.04
  
  WORKDIR /home/johnson/dockertest/
  #在这个虚拟的环境里, 建立上面的路径作为默认的进入目录
  
  COPY . /home/johnson/dockertest/
  #将本机当前目录的内容copy至虚拟环境的指定路径. 注意参数的写法, 第一个参数是本机目录, 第二个是目标(虚拟 #环境)
  CMD cat ./test.txt
  #容器开始运行的默认的命令是显示test.txt的内容
  
  
  ```

  - 凡是`.`代表的当前目录都是指Dockerfile文件所在的目录, 虚拟环境里面的`. `目录指的就是默认的工作目录. 所以COPY那一行写作 `COPY . .`也可以.
  - 可以看到Dockerfile都是这样一个命令对应一些操作的书写方式, 前面的命令不需要大写, 但是官方推荐大写, 这样容易与命令参数区分开.

- 接下来开始构建我们自己的镜像, 命令为`docker build -t johnson/first .` -t 参数是给构建的镜像命名, 命名方式是斜杠前面为组织公司名, 后面为具体的app名称, 只写一个代表两个名称一样. 最后的`.` 代表Dockerfile的路径, 改成对应的即可.

- 然后应该开始工作, 构建成功, 使用命令 ` docker images` 就可以看到刚才构建好的镜像.

- 然后使用命令 ` docker run johnson/first` 就可以看到 test.txt 的内容被输出了. 说明镜像的构建是成功的, 这个构建出来的镜像应该可以在所有的Linux电脑上运行, 无需安装任何我的电脑上的组件, 因为所有需要的组件理论上来说都被打包构建进入了镜像, 他的里面包含了所有需要的文件. 打包的镜像尺寸是78mb, 只是一个文件, 里面基本包含了一个Ubuntu的环境, 从这个角度来看还是很小的. 

- 可以使用命令 ` docker run -it johnson/first bash` , 进入镜像的环境内部去查看里面的文件结构, 默认进去的目录就应该是我们指定的目录. 此处不能用 `docker exec`因为我们的镜像并没有在运行, 必须要开启新的镜像. 默认的镜像会在运行完默认命令后自动退出. 除非有某些进程会一直保留, 类似服务器或者bash这一类需要手动退出的.

- 在Dockerfile的写法里面, 默认的命令 CMD 应该是一个shell环境, 这样一运行容器就会进入一个shell环境, 而另一种写法就是 CMD 只有参数, 命令被写在 ENTRYPOINT 里面. 对于熟练掌握的人更倾向于这种写法. 因为可以在启动容器的时候替换掉 CMD 里面的参数(当 ENTRYPOINT 存在时, CMD 就会成为 ENTRYPOINT 的参数), 更加灵活. 常见的用法是默认的 ENTRYPOINT 会使用一个命令的 --help 参数输出帮助信息, 用户可以替换为自己的参数. 注意, 只有 docker run 在重新开启一个新的容器时才能附加参数去替代默认 CMD 内容, 已经建立的容器 用 docker start 就不能再去替换 CMD 的内容.

- 向容器内添加文件或数据, 可以使用 ADD 或者 COPY, 但是一般还是 COPY 常见. ADD 用来将一个压缩文件自动解压到容器里面, COPY 不能做到这个. 如果要添加多个文件, 最好写多个 COPY 指令.

- 最简单的Linux镜像是alpine, 一个拥有基本功能的Linux镜像可以作为Dockerfile的起始点, 只有5MB大小.

- 更新后的Dockerfile如下:

  ```
  FROM alpine
  
  WORKDIR /home/johnson/dockertest/
  COPY . .
  ENTRYPOINT ["cat"]
  CMD ["./test.txt"]
  
  ```

  ### docker volume的简单演示

- 还是利用上面的 Dockerfile , 先在主机的 ~/dockertest/ 目录下建立 2nd.txt , 内容随意, 然后使用如下命令运行容器: `docker run -it -v ~/dockertest/:/home/johnson/dockertest johnson ./2nd.txt`

  - 意思是运行时把主机的 ~/dockertest/ 目录映射到容器的 /home/johnson/dockertest 目录. 最后的参数是替代打开的文件为 2nd.txt 那么容器内就可以访问主机里面的内容.

# docker-compose

- 容器启动时会运行默认命令, 对于多数镜像就是没有命令, 所以需要指定命令, 否则镜像运行完毕就会退出
- docker-compose 是一个库, 使用一个 .yaml ( 或者 .yml )文件来组合多个容器, 类似脚本. 
  - .yml 文件要指定里面组件的版本, 可参考官方文档.
- docker registry 是一个镜像托管网站, 收录很多镜像供人使用. pull镜像的时候要写名字为 <用户名>/<镜像名>, 如果只写一个, 代表两个名字一样

- 在存在yaml文件的目录使用 docker-compose up 就可以启动一系列配置好的服务, 关闭时把 up 换成 down .