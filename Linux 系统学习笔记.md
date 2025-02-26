# Linux 系统学习笔记

## 基础知识

- Terminal只是处理输入输出的程序, 并不运算.
- 终端有三种模式, 字符模式, 每按一个字符就发送; 行模式, 按下回车键才发送; 屏幕模式, 进入一个虚拟的界面, 整个屏幕一起发送和更新内容. 屏幕模式只能用于有屏幕的机器, 电传打字机无法使用。屏幕模式将屏幕分成可编辑，不可编辑区并使用方向键控制光标来编辑，最后一次性全部提交处理。
- 终端中分为内部命令和外部命令，系统自带的为内部命令，单独可执行文件为外部命令。
- Linux没有扩展名的概念。也就是说扩展名可以自己随便起，不代表任何意义，除非由其他用户或软件单独规定。
- 操作系统的启动顺序. bootloader(引导扇区内) -- kernel 内核 -- 其他 . 所有的程序进程都由内核来启动。
- 一组进程串接起来可视为一个组或一个作业(job)。
- 同步和异步执行(前台和后台)。输入命令默认为前台同步执行，如果需在后台执行，要在命令末尾加入&符号。
- 文件名和目录名前面带点 `.` 的是隐藏的。
- `/usr`用户文件夹用来存放用户程序。
- 动态链接库的后缀名为`.so`。
- 安装包的格式为rpm和deb，可通过包管理器安装或dpkg命令安装。
  - 使用方法` sudo dpkg -i <deb路径>`, 用来安装软件包
  - 删除安装包 `sudo dpkg -r <安装包名称>`
  - 查看所有安装的包 `sudo dpkg -l`, 可以 加 `| grep '关键词'`来查找
- 默认的包管理器为apt。
  - 安装命令：sudo apt install <name> 
  - 删除:  sudo apt remove --purge <name>
- Ubuntu自己的包管理器是snap, 安装的文件和配置文件在 `/var/snap/`里面。
- 命令用分号分开，即可以一次输入多个命令(批处理)。尽量不要使用分号执行多个命令，用 `&&`. 这样前面的命令出错，后面的就不会停止。
- `.`代表当前目录。`..`代表上一级目录。`~`代表home目录。`/`代表根目录。
- Linux下的转义符为正斜杠 `\`即为windows上的路径分隔符。
- 系统设置存放在 `/etc/profile/ `. 用户设置存放在 `~/.profile`. 命令行工具的设置存放在 `~/.bashrc`
- 几个常用的系统变量。终端的类型 `$TERM`. 多个系统目录 `$PATH`.  当前用户的文件夹。`$HOME`. 当前语言。`$LANG`.  产生一个随机数。`$RANDOM`. 系统的时区。系统所在的时区。`$TZ`.  用户ID。`$UID`. 命令提示符的格式。`$PS1`.
- 在图形界面中 `sudo init 3` 则关闭图形界面. ` sudo init 5`进入图形界面。
- 常用的命令 `Ctrl+c`, 强制退出当前任务.
- Linux区分大小写.

## Linux的安装

- 有些电脑不能直接从U盘启动安装, 要启用旧的启动模式(Legacy mode)才行, 有些要关掉安全启动功能才行.(例如Surface和某些品牌机)
- 发行版可以直接装在U盘里面。从U盘启动以后用图形界面安装。
-  WSL的安装参考官方文档。

## 常用命令

- `ls` (list)当前文件夹内容。可以加路径显示某个文件夹内容。`-a`参数可显示隐藏文件。`-l`参数可以显示更多信息。`ls -a -l`在很多时候可以简化为`ll`。`-h`, 文件大小以更大的单位显示。

  - `ls -l`显示出前面10个字符为权限。例如: `-rwxr-xr--`以三个为一组一共9个，第1位为文件属性。`d`就是文件夹。`l`就是链接或者快捷方式。后面三个显示的为创建者，中间三个为创建者的同一用户组, 最后三个为其他用户。
  - 改变权限: `chmod`. 第1组用户代号为u(user)第2组为g(group)，第3组为o(other)。
  - 例如给第1组用户加读写和执行权限。`chmod u+rxw <文件名>`; 给第2组减去权限的命令: `chmod g-rw <文件名>`.依此类推。如果省略用户的话，默认就是给文件所有者的用户更改权限。
  - 常用的给某一个文件加上执行权限(往往是脚本文件)。`chmod +x <file>`

- `lspci`可以列出PCI设备, `lscpu`列出CPU参数, `lsblk`列出所有存储设备.

- 改变文件的所有者。`chown <目标用户名或者ID> <文件名>`

- 改变文件的用户组。`chgrp <目标用户名或者ID> <文件名>`

- 当前路径。`pwd`

- 显示文件内容。`cat` 参数`-b`, 显示行号。如果不加任何文件路径就会自动回显输入的内容。参数 `-A` 列出所有隐藏符号. `-n`列出行号. `-b` 行号只对非空的行有效.
  - 将两个文件合成第三个 `cat <file1> <file2> > <file3>`

- 可以用 ` more`和`less`来代替`cat`查看文件, 他们提供了翻页功能. 使用空格翻下一页, 回车下一行, d 向下半页, b向上一页, q 退出. `/+<正则表达式>`来进行搜索. 其中less 可以加参数 `-N`来显示行号.

- 使用 `tail -f` 会一直刷新显示文件末尾. 用来监听某个文件的修改. 退出只能用 `Ctrl+c`. 参数 `-<n>`会显示末尾的 n 行.

- 命令行音乐播放器, 需要安装. ` sudo apt install cmus`, 装好可以直接使用cmus. 进去以后 `:set <path>`添加路径. `+-`控制音量, 左右是快进/快退, `< >`也是快进快退. `q`是退出, `Tab`是光标寻址.

- 回到上一级文件夹。`cd ..`  后退到上一个访问的目录`cd -` . `cd <path>`  改变到路径。`cd` 回到home目录。回到上两级目录: ` cd ../../`

- 输入的内容不可见。`Ctrl+s`  重新显示输入的内容`Crtl+q`

- 为某个命令设置别名, 用来简化输入`alias <命令> = '别名'`, 关机即失效, 要想保留就要写入用户配置文件`~/.profile`里面

- 历史命令`<上箭头>`

- 显示进程树 `pstree` 参数`-p`显示进程编号(uid)。

- 关闭当前进程`Crtl+c`

- 退出某个进程`kill <进程编号>` 强制退出 `killall -SIGKILL`. kill后面需要加进程编号。killall后面需要加的是进程名称，所以不防重名。

- 暂停进程`kill -TSTP <pid>`

- 强制暂停进程 `kill -STOP <pid>`

- 继续进程 `kill -cont <pid>`

- 查看环境变量 `env`

- 设置环境变量(关机失效) `export <名称>=<值>` 不可以设置重名变量(重名会覆盖原先的设置)

- 复制`cp <source> <destination>` 

- 移动 /重命名 `mv <source> <destination>`如果两个路径一样，但是文件名不一样，就是重命名。可以重命名目录.

- 删除 `rm <file>` `-r`参数用来删除目录。这种方式只是标记为空白，并不实际擦除。如果要实际擦除 `shred <file>`

- 新建文件夹 `mkdir <name>`. 命名时可使用变量。{1..10}意思就是建立从1到第10共10个文件夹。或者使用{a..z}/{1..10}他们的组合。`-p` 建立一整条路径，包括子目录。`-m` 可以赋予不同的权限。注意大括号里面只能使用两个点。

- 删除文件夹` rmdir`. 同样可以使用上面的大括号里面的变量。

- 在命令的结果中查找 `<命令> | grep "keyword"`在前面命令输出中查找关键字并返回所在的行。

- 显示所有监听的网络端口 `netstat -an | grep "LISTENING"` . `netstat`命令用来显示网络状况。类似的命令还有`ss`, 可以用来查看端口信息常用的参数为: `-n` 不解析服务名称只显示端口号。`-l` 显示监听。`-t`仅显示TCP。`-u` 仅显示udp。`-p` 显示使用端口的进程。

- 显示CPU信息。`cat /proc/cpuinfo`. 显示内存信息。`cat /proc/meminfo`

- PCI总线的设备信息, 网卡声卡，显卡等。`lspci` .  USB总线的信息。`lsusb`

- 获取管理员权限(切换当前用户为管理员) `sudo -i`,然后输入密码。如果要退出管理员 `Crtl+d`

- 远程登录其他Linux主机 `ssh -i <private key> <host name>@<address>`, 如果不需要使用密钥就去掉参数 `-i <private key>`，只写主机&地址，用密码登录即可。默认端口22。可使用参数`-P <端口号>`来更改登录的默认端口(在已知服务器端默认端口被更改的情况下)。
  - 生成新的密钥对 ` ssh -keygen -t rsa` . rsa为指定的加密方式。
  - 将自己的公钥写入远程主机 `ssh-copy-id <host>@<address>`

- 显示所有已安装的包`dpkg -l`, 或者 `sudo apt list --installed`

- 显示所有进程`ps -e`参数 `-f` 为显示所有信息。

- 退出登录 `Crtl+d`或者 `exit`, `logout`都可以。

- 清除屏幕 `Crtl+l`或者` clear`

- 移除不再需要的软件包 `sudo apt autoremove`

- 查找程序的位置 ` which <程序>` . 也可以使用` command -v <程序>` 或者 `type <程序>`

- 查询内核的版本 `uname -r` 命令本身是用来显示系统信息的。可以使用参数 `-a`来查看所有。常见的参数还有 `-m` 硬件信息， `  -p`处理器信息,  `-i`硬件平台，`-o`操作系统。

- 查找文件的位置 `whereis <文件>`

- 磁盘剩余空间 `df`. `-k`用KB为单位, `-h`使用更易读的格式, `-a`显示所有文件系统, `-T`会显示文件系统

- 查看内存情况`free`. 查看系统运行状况, 类似于资源管理器` htop`, 如果一些老机器或者精简过的机器没有此命令则用` top`.

- 读取并执行文件中的命令。`source <file>`. 可省略为点`.`。

- 常见的文件编辑器` nano`. 在里面编辑完以后保存`Crtl+o` 然后重命名，无需重命名直接回车。退出`Crtl+x`。

- 常见命令的说明书 `man <命令>`

- 测试某个地址的路由` traceroute <address>`

- 端口扫描器。`nmap <address>`

- 修改当前用户的密码 ` passwd`. 显示当前用户的用户名 ` whoami`

- 显示当前已登录这台电脑的所有用户` users`/ `who`/`w`

- 重启 `sudo init 6`/`reboot`. 安全关闭 ` shutdown`/ `poweroff`

- 统计文件的行数 词数和字节数, 可跟多个文件。` wc <file>`

- 将文件在屏幕上打印出来，但是对阅读进行优化，加入自动间距和换行等。`pr <file>`

- 登录FTP主机进行上传下载。`ftp <server address>`

- 查看文件类型和文件编码。`file <file>`

- 创建一个新的空白文件，如果文件已存在，则改变文件的最后修改时间。`touch <file>`

- 查看系统上挂载的所有磁盘。`mount`

- 查看文件的信息。`stat <file>`

- 域名和地址的互相查找。`host <address>`

- 查找文件。`find`. 例如要从根目录下查找名为<XX>的所有文件并包含子目录。`fine / -name xx`

- 建立到某一个文件的软链接。并且此链接不存在。`ln -s <source> <link>`

- 将光标移动到行首。`Crtl+a`/`home` . 将光标移动到行尾。`Crtl+e`/`end`

- 光标以词为单位移动。`Alt+f`, `Alt+b`

- 显示命令历史。` history` . 在历史命令中搜索 `Crtl+r`. 退出搜索。` Crtl+g`

- 以系统管理员权限执行上一个命令。`sudo !!`, 也就是说 `!!` 就是执行上一个命令。

- 从当前光标剪切至行尾。` Crtl+k`. 从当前光标剪切至行首。`Crtl+u`.  粘贴 `Crtl+y`.

- 进入编辑缓冲区, 可以写入多行命令，保存退出即可执行，会调用默认的编辑器。$EDITOR. ` Crtl+x+e`. 常见的默认编辑器有vim和nano。

- 路由持续追踪一个地址，可以探测链路的节点。` mtr <address>`

- 在本地和远程主机之间传文件。需要使用ssh(命令里面的主机必须是可以进行ssh连接的)。` scp <source> <username@hostname>:<path>` . 如果远程路径里面也包含了文件名，就可以在复制的过程中重命名. `-r` 参数复制整个目录。把源和目标调换也可从远程复制到本地。`-P` (大写) 可以指定端口。`-C` (大写) 可以在复制的过程中启用压缩。也可以复制远端的文件到另一个远端机器. 不过两台机器要配置好ssh互相访问才行. 如果没有配置可以使用`-3`参数, 流量就会绕行发出命令的这台机器, 则无需配置两台远端机器互相的ssh.

- 使用`rsync`传文件, 两台机器都要安装。Ubuntu默认包含这个软件。`rsync -a <source> <host>:<path>`. 参数 `-r` 表示复制文件夹。使用这个命令复制文件到远程机器，默认会跳过重复的文件，而scp不会，这个软件也要使用ssh。

- 添加新用户, 需要root权限。`adduser <name>`

- 测试系统的性能。要使用snap安装。`unixbench`

- 查找占用80端口的进程。`lsof -i tcp:80`

- 查看系统服务的状态。`service <service> status`. 重启某个系统服务。` sudo service network-manager restart` 

- 查看所有服务。`service --status-all`

- 列出所有硬件。`lshw` 列出所有模块。`lsmod`

- 在比较新的系统上，system control ( systemctl ) 已经取代了service命令。启动, 停止和重启服务: ` systemctl start/stop/restart/status <service>`. 如果只输入命令不带参数，会列出系统运行的设备,组件和服务。

- 显示目录占用的空间。`du -sh <path>`. 如果要查看子目录。` du -h --max-depth=<想要查看的层数> <path>`. 以上命令中层数为子目录的层数。如果设为0就只看目录本身。`--max-depth`参数的简化形式是 `-d`。以上命令可以简化为 `du -hd <层数> <path>`

- 如果要查看文件大小 `ls -htla`

- 查看系统服务的日志 `sudo journalctl -fu <service>`

- 关闭蓝牙。`bluetoothctl power off`. 需要安装。

- 重启控制台, 乱码的时候使用。`reset`

- 显示一个软件包是否安装等信息。`apt show <package>`. 查看软件包安装位置。`dpkg -L <package>`

- 使用命令行时内容太多可以在末尾加上 `|more` 或者 `|less` 用来分屏。

- 如果需要开机自动运行某些命令, 可以将命令加入 ~/.profile 文件, 用户登录就会自动调用这个文件. ~/.bashrc 中保存了bash的设置, 包括命令提示符的内容和颜色等.

- 可以直接定义变量: ` x=9998`, 使用的时候直接用 x 即可. 这样定义的变量是局部变量, 除了当前的bash程序以外别的程序无法访问, 需要使用 export 命令导出为全局变量才可以. 在使用变量的时候要加上 $ 符号。

- 压缩文件并生成同样后缀的压缩包。`tar ` `gzip` `bzip` `zip` 这几个都是压缩和解压缩软件。具体使用方法可以参考说明文件。

- 查看移动设备的电量，例如笔记本电脑。先安装acpi。 `sudo apt install acpi ` . 然后使用 `acpi` 命令即可。

- 文字界面下的声卡和音量控制。安装 ` alsa-utils `. ` alsamixer gui`, 是文字界面的混音台，可以用来控制音量。要用命令控制音量: ` amixer -c 1 set Speaker x%`. 控制耳机音量: ` amixer -c 1 get Speaker(Headphone)`. 命令里面的数字1是声卡的编号。可使用命令 `alsamixer` 来查看。

- 查看IP地址。`ip addr`

- 查看某个进程的进程号。或者验证该进程是否存在。` pidof <name of programme>`, 例如 `pidof sshd` 查看ssh进程的进程号。

- 查看无线网和WiFi的信息。使用 `iwconfig`. 需要安装。

- 把命令运行的结果输出到文件, 使用 `>` 号。` cat name > <file>` . 如果文件已存在，会覆盖。如果使用 `>>` 是追加在文件末尾。

- 如果需要把一个命令留在历史记录里面，但是却不执行，可以用 `#` 把它注释起来。`#XXXXXXX`

- 通配符. `?` 代表任意一位字符。`*` 代表任意长度的任意字符。`[]` 代表括号里面的任意一个字符。`[1-9]` 代表1~9范围内的任意一个。

  - 下面一些举例。

    ```
    a*
    #以a开头的任意长度字符串。
    g*t
    #以g开头t结尾的任意长度字符串。
    *e*
    #包含字母e的任意长度字符串。
    [abw]*
    #以a,b或w开头的任意长度字符串。
    [a-g]*
    #以a到g之间的字母开头的任意长度字符串。
    ```

- wget 默认下载到当前目录, 可以输入多个地址用空格隔开. 要制定输出目录要用 -O 参数.

- `!$`指代上一个命令的最后一个参数, 在复用上一个命令的参数但是主命令要改变时很有用, 例如 `ls .ssh/` `cd .ssh`, 后一个就可以写成`cd !$`.

## 一些应用专题

### ls 应用案例

- ` ls -l | grep "Aug" | sort +4n | more`. 符号 `|` 表示将前一个命令的结果传递到下一个命令. 这个命令的意思是, 列出当前目录的文件, 并且显示详细信息. 然后只显示其中含有 'Aug' 的行. 并且以第4列来排序 (文件大小). 并且对结果自动分页. 空格键为下一页, b 为上一页.

### 无需使用sudo的密码

- `echo "%sudo ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/sudo`

### 修改电源键的行为

- 打开 `/etc/systemd/logind.conf` 配置文件
- 查找有 `HandlePowerKey=poweroff` 的行
- 如果行以 `#` 符号开头，请将其删除以启用设置
- 使用以下选项之一替换 `poweroff` ： 					
  - `poweroff`关闭计算机
  - `reboot`重启系统
  - `halt`启动系统停止

### 移动硬盘或新加硬盘的挂载

- 首先使用`fdisk -l`查看磁盘是否存在, 以及地址.

- 然后使用`sudo mount <path> <mounting point>`来挂载, mounting point一般为一个空文件夹.

- 如果挂载失败, 可能是没有格式化. 使用`sudo mkfs.ext4 <path>`来格式化. 然后再挂载.

- 如果需要开机自动挂载而不是每次开机手动挂载, 就要将挂载命令写入`/etc/fstab`文件. 格式为`UUID=xxxxxxxxx <mounting point> <file system(ext4,fat32,ntfs)> defaults 1 1`, 每个参数之间用空格分开, 然后就会在开机的时候自动挂载.

- 第一次手动挂载以后使用命令`blkid`来查看`UUID`, 也可以在`fstab`里面使用`fdisk`查看到的设备地址来代替`UUID`, 但是这个地址可能会变, 尤其是磁盘顺序发生变化的时候. 其中一个解决办法是在fstab里面加入延时参数:`UUID="962093AD209392BB" /home/jo ntfs x-systemd.device-timeout=30 0 0`, 这样就只会等待30秒, 超时就会放弃挂载.
- 在一次断电以后, 系统提示外置硬盘无法挂载. 手动挂载提示`can’t read superblock`. 首先使用windows上的DiskGenius对磁盘错误扫描和尝试修复. 然后放在Linux上使用命令`mount -o ro,noload /dev/sda1 /home/........`来进行挂载就可以成功. 原因是superblock损坏. 但是这样损坏并未修复.
	- 修复方法: superblock一般是有备份的, 使用命令`sudo mke2fs -n /dev/xxx`来查看
	- 然后就会产生类似如下的信息:
	``` 
	mke2fs 1.45.5 (07-Jan-2020)
	/dev/sdb1 contains a ext4 file system labelled 'Stretch'
	last mounted on /media/linuxbabe/b43e4eea-9796-4ac6-9c48-2bcaa46353732 on Thu Jan 28 02:43:43 2021
	Proceed anyway? (y,N) y
	Creating filesystem with 7864320 4k blocks and 1966080 inodes
	Filesystem UUID: fcae3dc8-ee11-412c-97f0-27106601314e
	Superblock backups stored on blocks: 
		32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
		4096000
	```
	- 使用命令从第一个备份地址(32768)来恢复superblock. `sudo e2fsck -b 32768 /dev/sda1`. 所有的修复选项都选yes.
	- 然后就可以使用了.

- 第二种方式是使用`systemd`来自动挂载, 这种方式更加灵活.

  - 在`/usr/lib/systemd/system/`新建以`.mount`结尾的文件.

  - 写入以下内容

  - ```bash
    [linux]# cat /usr/lib/systemd/system/home-disk.mount
    
    [Unit]
    Description=mount my disk
    After=home.mount
    
    [Mount]
    #What=UUID=xxxxxxxxxxxxxxxxxxx
    #What=/dev/sdb2
    #以上地址或者UUID二选一
    Where=/home/user/Videos
    #where就是挂载点, 自行修改
    Type=ext4
    Options=defaults
    
    [Install]
    WantedBy=multi-user.target
    ```

  - 然后使用命令`sudo systemctl start home-disk.mount`测试, 成功以后应该能看到挂载成功的目录.

  - 然后加入开机自启动`sudo systemctl enable home-disk.mount`

  - 这样挂载失败了也不会有问题.
### 修复/屏蔽坏道
- 使用命令`badblocks -sv -o badblocks.log /dev/sda`来扫描坏道

- 扫描结束以后，再用e2fsck把坏道屏蔽

- 命令: `e2fsck -l badblocks.log /dev/sda` 这个方案比较软，就是把扫描出来的坏道数据添加到文件系统的黑名单里，适合硬盘上已经有数据的情况。还有个方案比较硬，用badblocks往指定范围的区块上写入数据，写入失败时硬盘会自动重新映射，这个方案适合硬盘上没有数据的情况。
	```
	badblocks -wsv /dev/sda [END] [START]
	```
	
- 注意`[END]`是结束区块编号，`[START]`是开始区块编号。

### Linux下使用NTFS文件系统

- 使用apt安装一个叫`ntfs-3g`的包, 即可以像使用其他文件系统一样使用ntfs.

### 关于把一些需要长时间运行的程序放入后台

- 方法一:(最简单)

  - 将正在运行的程序(前台进程)转后台并暂停 `Crtl+z` , 后台进程继续运行 `bg %<job number>` , 后台进程转回前台进程 `fg`

  - 显示后台进程的job number`jobs`

- 方法二:

  - 正常的命令都会让程序挂起(hang up), 也就是会显示程序运行的所有输出信息到屏幕. 在命令的开头加上参数`nohup`, 意为 no hang up, 就不会显示程序的输出信息. 所有的输出会被保存在当前路径`nohup.out`里面.
  - 然后还要在命令末尾加上一个`&`符号, 作用是让程序在后台运行.
  - 注意这只是让进程跑在了后台, 并不是系统服务, 退出登录(关闭终端)依然会将程序退出.
  - 在运行命令时, 在末尾加入disown, 这样进程就不属于当前用户, 那么退出终端也不会终止进程, 但是这样的缺点是放弃了用户所有权也就再也无法管理这个进程, 不能查看进程状态, 故障也不会通知.

- 方法三:(传统方法, 有些过时)

  - 使用screen. 它的作用是虚拟终端, 可以将多个进程放入后台并方便管理.
  - 首先安装`sudo apt install screen`
  - 创建一个虚拟终端:`screen -R <name>`此命令会进入名为<name>的虚拟终端, 如果不存在则会创建.
  - 进入以后就可以按常规方法运行程序, 程序开始运行后使用`ctrl+a`, 然后按`d`就可以保留信息, 并将程序放入后台运行. 这时就算断开ssh连接也不会终止程序. 按`k`则会退出当前的虚拟终端.
  - 查看所有后台的虚拟终端`screen -ls`. 显示所有的虚拟终端格式为`pid.name`, 操作的时候可以指定pid 或者name.
  - 这时候使用创建命令就可以回到终端. `screen -R <name>`
  - 使用完毕, 在虚拟终端内可以使用`crtl+a, k`退出, 也可以用`exit`或者`crtl+d`命令来退出. 或者在虚拟终端外使用`screen -R <name/pid> -X quit`来退出某个虚拟终端.
  - 对于出现错误的终端, 会丢失状态, 这时候强制它detach即可, 命令:`screen -d <name/pid>`

- 方法四: 将程序注册为服务, 用systemd来管理.

- 方法五: 使用tumx.
### 压缩与解压缩
- 最常见的压缩解压缩命令为`tar`
- 他的常用参数:tar 的第一个选项参数必须是下列之一：
  - -c, --create 创建一个新的压缩文件
  - -x, --extract 从中提取文件
  - -t, --list 列出文件内容
- 剩下的常见参数和命令为: `tar -czvf <archive_name> <file_list>`, 参数c为创建新文件, z为使用压缩, 如果没有z参数就不压缩只是打包, v是显示处理过程, f是要指定压缩文件名. 最后的文件列表可以加入多个文件用空格分开, 也可以写入整个目录, 就会压缩整个目录. 压缩文件名一般后缀是`tar.gz`, 或者`tgz`.
- 解压的命令是 `tar -xvf <file>`, 参数x为提取, v是显示过程信息, f为指定压缩包. 会解压在当前目录, 如果要解压到别的目录, 要用`-C` 参数附上路径. 例如 `tar -xvf archive.tar -C /opt/files`
- 列出压缩包内容 `tar -tvf archive.tar`
- 解压部分文件, 只需在解压命令后面加上需要解压的文件列表: `tar -xf archive.tar file1 file2`. 也可以解压目录.
- 增加一个文件: `tar -rvf archive.tar newfile`
- 删除一个文件: `tar --delete -f archive.tar file1`

### nano的一些使用方法

- 保存:`ctrl+s`, 与Windows一样. 另存为才是`ctrl+o`
- `ctrl+6`进入选择模式, 选择以后可以复制或者剪切 `(ctrl+k)`.
- `alt+u`, undo
- `alt+6`, 如果已经选中了内容就复制选中的, 没有选中就复制当前行.
- `alt+/`跳到文件末尾, `alt+\`跳到文件开头. 
- `ctrl+a`跳到行首, `ctrl+e`跳到行尾.
- `alt+n`行号开关
- `ctrl+r`, 然后`alt+f`在不关闭当前文件的情况下打开新文件. 可以在界面里面查找文件, 打开以后可以复制然后退出新文件, 粘贴到原来的文件.
- `ctrl+u`粘贴
- `ctrl+x`退出.

### 查看系统信息

- 使用neofetch. 安装: ` sudo apt install neofetch`
- 使用: `neofetch`

### 修改电脑名称

- 先使用命令`hostnamectl status`来查看电脑的名称, 第一个static hostname就是电脑的主机名称
- 使用命令`hostnamectl set-hostname <new name>`来设置新的名字, 要退出当前用户再登录才能看见

### 设置系统时间

- 一般有网络的情况下时间都是自动同步的, 使用`date`命令显示, 参数`-R`会显示具体的时区信息.
- 如果时区错误就会导致时间错误, 修改时区使用命令` ln -sf /usr/share/zoneinfo/<你的时区> /etc/localtime`. 所处的时区可以在上面的路径看到, 填上去即可.

### 查看系统温度

-  安装 `sudo apt install lm-sensors`
- 第一次运行之前要检测有哪些传感器`sudo sensors-detect`, 一路yes即可

- 然后运行`sensors`即可给出所有温度传感器当前读数
- 如果要持续观察, 配合watch命令: `watch sensors`, 会持续给出各个温度传感器的读数.

###  nmcli 命令(Network-manager command line interface (Ubuntu上已经废止)
- 要先安装 `Network manager`
- 跟网络用相关的可以全部用此命令。
- 还有一个nmtui提供了一个图形界面。
- 显示目前的网络状况。`nmcli general status` 或者 `nmcli dev status`
- 显示存在过的连接。`nmcli connection show`
- 关闭WiFi连接 `nmcli radio wifi off`

### 网络测速

- 安装 `sudo apt install speedtest-cli`
- 测速使用多线程模式 ` speedtest-cli`
- 测速使用单线 ` speedtest-cli --single`
- 使用有线网会测试到比wifi更高的网速
### Ubuntu下配置静态IP地址
- Ubuntu默认使用`netplan`
- 使用命令`sudo nano /etc/netplan/00-installer-config.yaml` 编辑网卡的配置文件:
```yaml
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp3s0:
      routes:
      - to: default
        via: 192.168.3.1
# this is the gateway
      dhcp4: no
      addresses: [192.168.3.156/24]
      nameservers:
        addresses: [192.168.3.1]
        #this is dns
  version: 2
```
- 根据上面的需要更改具体的地址, 注意网卡的名称要用`ip add`命令查看
### New Zealand Ubuntu 下载镜像源, 解决下载失败问题

- 使用文字编辑器编辑 ` sudo vim /etc/apt/sources.list`. 除了最后三个以外, 在服务器域名前加`nz.`。最后三个前面加 `security.`
- 如果是非Ubuntu系统, 可以找找对应的网址, 也是同样的文件修改方法.

### 纯文字命令行下面的一些用法(非SSH或者Terminal)

- 设置文字界面的字体大小. 安装 ` sudo apt install console-setup`. 然后运行 ` sudo dpkg-reconfigure console-setup`. 然后选 UTF-8, 字符集自动, 字体可以用 Terminus, 尺寸选最大.
- 纯文字界面下也可以切换多个命令行屏幕, 最多可以用6个. ` Crtl+Alt+(F1-F6)`

###  ssh的配置

-  apt安装sshd。装完一般会自动启动，如果没有自动启动，手动启动的路径为 `/usr/sbin/sshd`

-  安装完以后就可以在别的电脑使用账号密码登录。 `ssh <user>@<server>` 主机地址使用IP地址就可以。

-  登录后在`~/.ssh`目录下建立一个文件 `touch authorized_keys`, 然后用vim或者nano等编辑器将公钥粘贴进去即可无密码登录。这样服务端的配置就完成了。如果需要更多的配置可以去配置文件。里面可以禁用密码登录, 和更改登录的端口。建立客户机白名单黑名单等等。进行此类操作时，建议留一个自己备用的登录方法，以防无法登录的时候，作为补救手段，如果因为意外禁用掉了所有的登录方式，自己也无法登录到机器。

-  初次在客户端无密码登录使用命令 `ssh -i <private_keyfile> <user>@<server>`

-  在客户端进行配置，可以快速登录。在客户端的 `~/.ssh` 文件夹下建立 `config`文件，填写服务器信息。可以将服务器的名称和IP地址等存储在里面。

  ``` bash
  Host <host nick name>
  HostName <host address>
  User <user name>
  Port <22> #默认端口22，如果没有修改服务器端的设置，此处可以省略。
  ```

  保存完成以后，使用命令 `ssh <nick name>` 就可以登录。

- 服务器端的配置可以使用命令`sudo nano /etc/ssh/sshd_config`修改.

### Oracle服务器防火墙(iptables)设定

- Ubuntu或者CentOS有可能内置了`iptables`, 新装好可能会阻挡一切非常见的网络流量. 使用命令` sudo iptables -L -n -v`查看当前所有过滤规则.

- 命令` sudo iptables -A INPUT -p udp --dport 8387 -j ACCEPT`打开某个端口的输入网络流量, 其中协议udp可以更换为其他常用的, 例如tcp.

- 还有一个比较危险的操作, flush网络`sudo iptables -F`, 删除所有防火墙规则.

- 更改完毕以后要使用命令`sudo iptables-save > /etc/iptables/rules.v4` 来保存所有的规则, 不然重启后会消失.

- 以上不管用的话使用下面的方法:`sudo iptables-save | sudo tee /etc/iptables/iptables.rules`, 不管用是因为sudo没有对第二个命令生效.

- 编辑文件`nano /etc/iptables/rules.v4`

- 添加需要的规则, 可以根据上下文来复制, 以下为tcp写法, 然后保存.

  ```bash
  -A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
  -A INPUT -p tcp -m state --state NEW -m tcp --dport 443 -j ACCEPT
  ```

- 运行`iptables-restore < /etc/iptables/rules.v4`可以保存.

### 配置内网固定IP

- 在图形界面里按照提示操作即可，只对单个无线信号起作用。
- 在命令行下配置文件在 `/etc/NetworkManager/system-connctions`. 每一个无线SSID会有单独的一个配置文件。
- 一些旧的系统中用来管理无线网的是 `netplan`。在Ubuntu 20:04下已经交由Network Manager管理，所以配置文件位置改变了。
- 进入配置文件以后，在 [ipv4] 的字段:

```bash
[ipv4]
address1 = 192.168.x.xxx/24,192.168.x.x
#前面一个IP地址为自己想要的固定IP，24子网掩码，后面一个IP地址为网关地址。
dns = 192.168.x.x
method = manual
```

​	然后保存退出即可。

- 还有一种办法是在路由器上面设置 IP 绑定设备。
- 如果要设置有线网络，要用IP命令。` ip a add <ip_address/mask> dev <interface>`. interface 就是网卡的设备名称。可以使用 `ip a` 命令来查看所有的网络设备名称和状态。默认的子网掩码是24。如果要删除一个绑定的固定IP地址。`ip a del <ip address> dev <interface>`

### 图形界面下的使用(Terminal)

- 关闭和打开图形界面。关闭 ` sudo init 3` ( 即退出图形界面，进入纯文字环境 ). 进入图形界面。` sudo init 5`
- 删除桌面环境 ( DE )。`sudo apt remove ubuntu-desktop gnome`. 每个发行版的图形界面名称不一样，需要查找。然后运行自动清理。`sudo apt autoremove, purge <DE> , autoclean, clean` 等. 删除了也可以安装回来，只要记住桌面环境的名称。
- 开机不进入图形界面，自动进入文字环境。(ubuntu). 进入修改以下文件。` sudo vim /etc/default grub/`. 把这一行改为这样: `GRUB_CMDLINE_LINUX_DEFAULT = "TEXT"`. 原本的默认应该为 `QUIET SLASH`。保存退出，然后运行 `sudo update-grub` 命令。 然后设置系统服务, 在进入系统时进入服务器模式。` sudo systemctl set-default multi-user.target`. 如果要恢复进入图形界面就把最后一个参数改成 `graphical.target`. 然后重启 (命令 `sudo reboot`)。

###   Linux下用GCC编译C语言单个文件

- 命令是 `gcc -o <输出可执行文件名> <代码文件>`

### Linux 在笔记本上, 关上盖子不休眠并且关闭屏幕

- 首先编辑以下文件

- ```bash
  sudo nano /etc/systemd/logind.conf
  ```

  - 将这一行 `#HandleLidSwitch=suspend` 改为`HandleLidSwitch=ignore`
  - 然后运行 ` setterm --blank 1 --powerdown 2`
  - 再运行 ` sudo systemctl restart systemd-logind.service`

### 在移动磁盘上存在LVM, 如何挂载到Linux

- 首先要确保系统安装了LVM `sudo apt install lvm2`
- 运行 `sudo vgscan`来找到所有的LVM, 看看需要的磁盘在不在这里 
- 然后 `sudo vgchange -ay`来激活所有的LVM
- 使用命令`sudo lvdisplay `来查看所有的LVM, 把需要用到的盘的 `lv path`记录下来
- 如果有需要, 新建一个文件夹来当作挂载点
- 使用命令来挂载 `sudo mount <LV_PATH> <path to mount point>`
- 如果想要永久性的自动挂载, 就要更新 `/etc/fstab`文件, 将挂载点信息写入其中
- 移除挂载: `sudo umount <LV_PATH>`

### 安装zsh,或者类似的shell

- 首先安装 `sudo apt install zsh`
- 完成以后运行 `zsh`, 会出现配置助手, 按照提示配置即可.
- 然后要想更好用, 就安装`Oh-my-zsh`. 这是一个用来管理zsh的开源框架. 直接在GitHub上找到安装链接, 记得要有下载软件wget或者curl, 命令`sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`.但这个并不是必要的, oh-my-zsh也会拖慢shell运行的速度.
- 安装完以后并不会自动运行, 要手动将系统默认的shell切换到zsh, 命令是 `chsh -s $(which zsh)`. `chsh`命令就是切换shell用的, 后面的参数是zsh的路径. 
- 可以查看电脑上已经安装的shell `cat /etc/shells`
- 切换以后可以在 `~/.zshrc`文件中指定theme, 具体方法在Oh-my-zsh的GitHub文档里面. Theme放在 `~/.Oh-my-zsh/theme/`里面.
- Oh-my-zsh 有自己的命令 omz, 很多命令以这个为开始. 要更新设定, 命令是`omz reload`.
- 它还带有很多快捷命令, 使用`alias`来查看.

### 为Linux或者wsl更改dns

- 有时会遇到域名无法解析的问题, 可能是Windows自动给wsl生成无效的dns, 可以手动更改dns的ip地址.

- 先 `ping`一下目标网址, 如果不通就进行更改

- 配置文件是 `sudo nano /etc/resolv.conf`

- 在其中添加 需要的dns服务器如下: `nameserver 8.8.8.8`. 这个地址是Google的dns服务器, 比较可靠.

- 如果在重启后失效, 说明Windows还在使用无效的dns, 就建立这个文件`/etc/resolv.conf`,在其中写入: 

  ```bash
  [network]
  generateResolvConf = false

​		这样就可以制止Windows自动生成无效的dns
### 快速查看Linux系统状态
- 可以安装 `neofetch`这个包, 安装完成以后 执行命令 `neofetch`即可, 不然要查看系统信息会很麻烦.
### 在WSL2中使用systemd服务

- WSL默认不适用systemd服务, 但是有些东西又依赖这个服务, 所以手动安装.

- 编辑文件 `sudo nano /etc/wsl.conf`, 在其中加入语句:
	```bash
	[boot]
	systemd=true
	```
	
- 然后在Windows命令行使用命令 `wsl.exe --shutdown`, 关闭并且重启wsl,这样再开就会带有systemd了.

### 端口被占用的简单解决办法

- 使用命令`lsof -i :<port No.>`来查看是什么在占用端口, 同样会显示占用的程序名字和系统pid, 可以用`kill <pid>`命令关闭掉占用端口的程序. 

### 在新部署的电脑上导入ssh key

- 首先要安装一个软件: `sudo apt install -y ssh-import-id`. 这个软件在Ubuntu 22以后应该是自带的.
- 然后将自己的public key放在GitHub的账户里.
- 然后使用命令`ssh-import-id gh:<GitHub_username>`就可以导入public key, 下次就可以避免输密码.

## Linux 脚本

- 脚本就是将一系列命令写在一个文件里执行。一般的脚本使用 .sh 扩展名, 但任意名字并不影响执行。
- 脚本有两种运行方式，一文件直接执行，二将文件传递给 bash 执行。
- 文件直接执行，在文件第一行要指定shell : `#!/bin/bash` .然后再写命令，写完以后要用 `chmod +x <file>` 给文件赋予执行权限。
- 第2种将文件传递给bash执行的。文件是当做bash的一个参数，这样开头就无需指定shell，即便指定了也无效。用法是 ` bash <file>` 或者 `source <file>` 或者缩写为 `. <file>`
- 可以在文件内定义变量。`x=999`. 如果变量里面没有空格可以不加引号。等号的两边也不能有空格。在使用变量时要加 "$" 符号, 这是告诉系统后面跟的是一个变量。变量的边界可以由大括号来规定({})。在容易引起误会的情况下，可以用大括号来告诉系统变量到底叫什么，名字排除很多空格的干扰。例如: `x_1="I like to eat food."`, ` echo $x_1--said by me. `. 以上这种情况就容易引起误会，让人看不懂变量到底是哪一个名字。加上大括号以后就会清晰很多。`echo ${x_1}--said by me.`
- 在定义变量时，单引号中不可以引用其他变量, 也不支持转义符，双引号中可引用其他变量和转义符。，即单引号中的一切都视为字符串。
- 将变量改为只读变量。`readonly <变量>`. 指读变量不能被修改和删除。
- 删除变量。`unset <变量>`. 以上两种情况都不需要加 "$" 符号。
- 获取字符串的长度。`${#<变量>}`
- 一个简单的脚本例子:

```bash
#!/bin/bash
x='Hello World!'
y='I am the King of the world!'

echo $x $y
echo "The length of the variavle x is ${#x}.  The length of variable y is ${#y}."

unset x y
echo $x $y

```

​	以上会输出两个字符串，并且输出他们的长度，然后删除这两个变量，然后再尝试输出会发现什么都输出不了。将代码保存为一个文件。test.sh  然后用命令 `chmod +x test.sh` 给他赋予执行权限。用第1种方式执行就直接输它的路径和文件名就可以执行。一定要输路径，因为它并不是系统自带的命令。如果就在当前文件夹也要输 ` ./test.sh` . 如果用第2种方式执行，就直接使用命令 `source test.sh` 或者 `. test.sh`

- 如果要获取用户的输入，就要使用read命令。常见的用法是将输入直接赋予一个变量。`read <name>`

- 在一个文件里 `$1`, 就表示第1个参数，在调用这个文件的时候可以同时传递参数。

  - 例如

    ```
    name=$1
    echo $name
    
    #然后在调用这个文件的时候。输入 source ./test.sh Nick, 这个参数就会被赋值。如果有多个参数会依次被赋值。
    ```

    

- 如果用 `#` 开头的句子。在脚本里就是注释。

- 这样定义的变量都是局部变量，关机后会消失。

- 将命令的结果赋值给变量。`var=$(<command>)` . 例如 `user=$(whoami)` , `date=$(date)`, 或者 `whereami=$(pwd)`.

- 如果想要每次开机直接调用一个函数，将函数定义在 `~/.profile` 文件中。

- 子字符串。`${<string>:1:4} ` 代表了字符串的第二到第5个字符，因为是从0开始数的。

- 查找字符串。 `$(expr index "$string" <xx>)` 从1开始查找xx出现的位置，如果指定了多个字母，先出现的字母为准。返回查找到的位置。

- 一些系统内置的变量。`$RANDOM` 一个随机数。`$SHELL` 当前使用的shell。`$USER` 当前用户。`$HOSTNAME` 当前的主机(电脑)名称。`$PWD` 当前的工作路径。

- 用户自定义变量一定要使用export命令。这样的命令仍然会在关机或退出登录以后失效。如果需要他永久性生效，就要把变量定义在 `.bashrc ` 里面 。

- 一个使用随机数的脚本案例。

  ```bash
  #!/bin/bash
  echo "Welcome to the fortune teller."
  echo "What's your name?"
  read name
  sleep 1
  echo "How old are you?"
  read age
  sleep 2
  echo "Hello, $name. You are $age years old."
  sleep 2
  echo "Calculating....."
  sleep 1
  echo "You will be rich at age of $((($RANDOM%20)+$age)). Wow, good!"
  ```

  上面的例子中用到了一个新的方法可以把数学表达式写在脚本中，并由电脑运算。`$(( ))`.  其中 `%` 的作用是, 计算随机数字除以20以后的余数。也就是说将运算结果限制在了0~19的范围内。

- 接下来要使用条件语句。语法如下。

  ```bash
  if [[ condition ]]; then
  statement
  elif [[ condition ]]; then
  statement
  else
  statement
  fi
  ```

  需要注意的是两个方括号里面前后都要有空格，后面要有分号。还要加上关键词then. 如果要判断某个变量是否等于另外一个数值，两个等号 `==` 前后也要空格。还可以进行逻辑运算。OR `||`, AND `&&`

- 比较运算符。在很多情况下不能使用传统的运算符。要使用这样的。

- | -eq  | 检测两个数是否相等，相等返回 true。                   | [ $a -eq $b ] 返回 false。 |
  | ---- | ----------------------------------------------------- | -------------------------- |
  | -ne  | 检测两个数是否不相等，不相等返回 true。               | [ $a -ne $b ] 返回 true。  |
  | -gt  | 检测左边的数是否大于右边的，如果是，则返回 true。     | [ $a -gt $b ] 返回 false。 |
  | -lt  | 检测左边的数是否小于右边的，如果是，则返回 true。     | [ $a -lt $b ] 返回 true。  |
  | -ge  | 检测左边的数是否大于等于右边的，如果是，则返回 true。 | [ $a -ge $b ] 返回 false。 |
  | -le  | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | [ $a -le $b ] 返回 true。  |

- 循环 while 循环:

  ```bash
  while(( condition ))
  do
  	statements
  done
  ```

  

## 注意事项

- 远程控制服务器时, 本机的程序过多会引起网络阻塞, 现象看起来像是服务器不定时卡死. ping 的时候每隔几秒就会出现巨大的延迟.