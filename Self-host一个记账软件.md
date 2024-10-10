# Firefly 记账软件

作为使用最广泛的一个记账软件, 支持多用户, 各种功能足够完备, 支持多语言, 多货币, 各个平台都有客户端. 支持第三方客户端. 使用docker即可部署.

## 安装

- 从`https://raw.githubusercontent.com/firefly-iii/firefly-iii/main/.env.example`下载环境变量文件, 重命名为`.env`
- 下载`https://raw.githubusercontent.com/firefly-iii/docker/main/database.env`, 里面包含了一些数据库的变量. 重命名为`.db.env`
- 必须要按要求重命名, 因为docker-compose文件里面已经写好了要引用这两个文件, 否则就要去修改让docker-compose能找到对应的文件.
- 打开`.env`, 修改 `DB_PASSWORD`变量, 自己选一个数据库的密码.
- 打开`.db.env`, 修改`MYSQL_PASSWORD`为刚才选定的密码.
- 要在ARM机器上装的话要把数据库改为PostgreSQL, 因为ARM上缺少对应的模块.具体看官网.
- 运行命令`docker compose -f docker-compose.yml up -d --pull=always`开始安装
- 安装完成会显示可以访问的网址, 就可以在局域网使用ip和端口访问网页端了. 端口应该在安装信息里面, 如果没有, docker-compose yaml文件里面也有.

## 第一次使用

- 使用网址打开就可以在内网中使用, 按照网页提示就可以一步一步建立账号.
- 然后自己建立账户, 余额, 开始记录每一笔交易.

## 备份

- 所有的Docker变量, 就是compose 文件.
- 你自己设定的数据库密码, 在`.env`里面的那个
- 两个docker的数据文件夹`upload`和`db`:
- 命令用来备份数据库
```bash
    docker run --rm -v "firefly_iii_db:/tmp" \
    -v "$HOME/backups/firefly:/backup" \
    ubuntu tar -czvf /backup/firefly_db.tar /tmp
```

- 上面的命令调用一个Ubuntu把db映射到/tmp文件夹, 然后把firefly映射到backup文件夹, 然后把db压缩到backup文件夹, 然后删除这个Ubuntu的容器.

- 用这个命令来恢复备份的数据库:

  ```bash
  docker run --rm -v "firefly_iii_db:/recover" \
      -v "$HOME/backups/firefly:/backup" \
      ubuntu tar -xvf /backup/firefly_db.tar -C /recover --strip 1
  ```

  - 这个命令把

- 完成备份以后, 在别处使用恢复功能恢复一次, 检验备份是否成功.