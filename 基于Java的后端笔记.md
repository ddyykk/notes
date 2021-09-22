# 基于Java的后端笔记

网上大部分讲Java后端的教程都只是在讲Java语言, 如何使用各类变量和数据, 而不是真正的网页后端, 所以我记录下我的学习笔记. 

## Java环境配置  

- Java主要部分是API和JVM. API将指令翻译成机器语言,  JVM则是一个虚拟机, 保证同样的代码可以运行在不同的环境(Windows, Linux, Mac).

- 要运行Java代码, 需要开发包, JDK (Developer Kits).  而不是JRE, JRE是面向普通用户的Java运行环境. 从JDK17开始, Java对所有用户完全免费了, 以前的JDK版本只允许学习和开发用户免费使用JDK, 商用需要付费. 
- JDK的安装.
  - Windows: https://www.oracle.com/java/technologies/downloads/#jdk17-windows 下载后直接安装, 当前版本为 JDK 17.
  
  - Linux: https://www.oracle.com/java/technologies/downloads/#jdk17-linux 下载x64 Debian Package (for Ubuntu). 
  
  - 我是在Windows上下载的, 然后使用rsync传到Linux上. 下载后在文件位置执行命令 ` sudo dpkg -i jdk*.deb`  或者 `sudo apt install jdk*.deb` 来进行安装. 我在安装时遇到问题说我的dependency没有安装, 所以用apt去装需要的包. 代码: ` sudo apt install libc6-i386 libc6-x32`.  再执行一次dpkg安装命令就会自动完成安装.  如果需要卸载, 执行命令 ` sudo apt remove package-name`.
  
  - 安装完后使用命令 ` java -version` 来检查版本验证安装是否成功. 结果运行失败说找不到java命令.  运行 ` apt show jdk-17 ` 来检查, 显示已经安装. 再运行 ` dpkg -L jdk-17` 查看安装位置, 看来确实是安装成功了. 但是程序的目录没有添加到系统默认环境变量中, 需要手动添加. 新建一个临时文件, 123: ` nano 123`. 写入如下代码:
  
    ```shell
    cat <<EOF | sudo tee /etc/profile.d/jdk.sh
    export JAVA_HOME=/usr/lib/jvm/jdk-17/
    export PATH=\$PATH:\$JAVA_HOME/bin
    EOF
    ```
  
	  保存退出然后执行保存的代码, 命令: ` . 123` , 然后再执行脚本: ` . /etc/profile.d/jdk.sh`, 完成后再一次使用 ` java -version`验证, 终于返回版本, 安装成功. 然后可以删除123临时文件和deb安装包.
  
  - 不得不说, 这个新发布的  JDK 的 deb 包也太简陋了, 基本上就是复制了文件, 什么也不管, 官方文档页面也没有提怎么使用, 非常浪费时间.
  
- 尝试写一个Java程序: 

   - 在文本编辑器写下代码:

     ```java
     /**
      * The HelloWorldApp class implements an application that
      * simply prints "Hello World!" to standard output.
      */
     class HelloWorldApp {
         public static void main(String[] args) {
             System.out.println("Hello World!"); // Display the string.
         }
     }
     ```

	- 保存为后缀名为 java的文件.
	
	- 在命令行进入保存java文件的目录, 编译文件, 命令: ` javac a.java`, 这样会生成一个HelloWorldApp.class文件, 此文件为java的可执行文件. 文件名一定是这个除非另外指定, 因为这是代码中写好的.
	
	- 执行 HelloWorldApp.class 文件, 命令: ` java -cp . HelloWorldApp`  命令中` .`的意思为当前目录, 如果执行文件不在当前目录要改为具体路径. 输出Hello World! 就说明程序成功执行, JDK的安装也没有问题.

## 其他工具安装

- Windows 直接官网下载InterlliJ IDEA community版本. InterlliJ IDEA内置 Java 构建工具Marvin和Gradle.
