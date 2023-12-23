# 安装git

- 很多Linux和mac都自带了git, 可以先用命令`git --version`来验证有没有安装.
- Windows下可以在网站下载安装包即可.
- Linux用命令 `sudo apt install git`

## 常用命令

- ` git init` 在当前文件夹初始化git, 开始进行版本管理.
- ` git status`显示文件夹的git状态. 
- `git add <文件>`将文件加入版本追踪, 其中`git add <文件夹>`会把文件夹中的所有内容加入追踪. `git add .`会把当前目录中所有未追踪的文件加入追踪(包括子目录和隐藏文件). `git add -A`会加入仓库文件夹中所有的文件,包括以前指定过无需追踪的文件.(慎用)
- ` git pull`下载/同步当前文件夹和服务器(如果存在git管理的话).
- ` git push `上传/同步当前文件夹的修改到服务器.
- ` git diff` 查看文件夹内被修改过的地方.
- `git commit -m <'commit message'>`提交所有修改的内容为一个commit, 并且附上提交信息 
- `git log`显示提交记录
- `git branch`显示分支情况以及当前分支
- `git checkout <分支名称>`切换到分支
- `git merge <分支名称>`将分支融合到当前所在分支
- `git clone <代码仓库网址>`将远端仓库复制到本地并且会自动关联远端账户, 即以后可以直接提交代码, 无需再设置文件夹. 需要有账户权限.

## 提交代码至远端(remote)

- 确保电脑上存在private ssh-key, 并且把public key 提交到GitHub网页端.
- 设置git的用户名: `git config --global user.name <username>`, 然后可以使用 `git config --global user.name `来查看设置的用户名. 
- 也可以为某个文件夹单独设置用户, 因为有可能一个电脑存在多个github用户, 先打开需要设置的git仓库文件, 然后 `git config user.name <username>`
- 然后确保你的账户名(一般是Email地址)已经在GitHub网站填写过和验证过(收到了验证邮件, 注册时一般都会验证). 然后用命令`git config --global user.email <YOUR_EMAIL>`来设置提交的账户.
- 同样可以为多个用户的不同仓库文件夹设置不同的账户, 先打开需要设置的git仓库文件夹, 使用命令`git config user.email <YOUR_EMAIL>`.
- 然后在网页端开启一个新的仓库, 就拥有了网址, 可以在push的时候填入了.
- 设置了这两项以后, 如果 使用`git push`时没有ssh key, 就会要求输入密码, 有ssh key的话就会直接提交.