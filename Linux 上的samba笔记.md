# Linux 上的samba笔记

- 先用apt安装samba。
- 打开配置文件。` sudo vim /etc/samba/smb.conf`. 
- 在[Global]里面做常用的系统设置. `Network`里面填上网卡的名字, 使用`ip add`命令来查看要使用的网卡. 后面的选项总是使用上面的网卡要设置为yes.
- 确保`server role = standalone server`这一句存在, 将samba设置为独立服务器.
- 添加共享目录. 在文件末尾加上如下

```bash
[sambashare]
comment=<description of sharing>
path=<path to the sharing folder> #不需要引号
read only = yes/no
browsable = yes
```

​	路径最好写绝对路径，不要写相对路径。

​	保存退出。

- 配置完以后要重新启动服务，配置才会生效。` sudo service smbd restart` 或者 ` sudo systemctl restart smbd.service`. 每次修改配置文件都需要运行此命令。

- 系统防火墙为文件服务器开启端口。` sudo ufw allow samba`

- 设置文件服务器的密码。用下面的命令设置对应用户名的密码，用户名必须为Linux的用户名。` sudo smbpasswd -a <username> `, 系统会要求输入密码. 如果输入的用户名不是Linux的系统用户名, 要用`sudo adduser <new user name>`命令新建一个系统用户, 再用`sudo setfacl -R -m "u:<new user name>:rwx" <path>`给新用户赋予某个路径的读写权限.

-  Windows下访问samba文件服务器。在资源管理器而不是浏览器地址栏填入 `\\<address>`。

- 也可使用苹果手机和电脑访问samba文件服务器。但是新的系统对samba支持不好，可能会有访问问题或者是无法在上面保存文件。解决方法是引入一份苹果的配置。首先安装 VFS 模块。`sudo apt install --no-install-recommends samba-vfs-modules`. 此操作有可能会覆盖配置文件，最好先备份。安装完成后在配置文件的共享文件夹设置 [sharing] 中加上如下语句: 

  ```
  min protocol = SMB2
  fruit:delete_empty_adfiles = yes
  vfs objects = fruit streams_xattr
  fruit:metadata = stream
  fruit:model = MacSamba
  fruit:aapl = yes
  ```

  然后重启服务。