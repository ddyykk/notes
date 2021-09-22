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

- Windows 直接官网下载InterlliJ IDEA community版本. InterlliJ IDEA内置 Java 构建工具Marven和Gradle.

## 第一个Java工程(IntelliJ IDEA)

- Java语言对大小写敏感.

- 在路径 C:\Program Files\Java\jdk-17\bin 下存放了Java的几个主要程序.
  - java.exe：这个可执行程序其实就是JVM，运行Java程序，就是启动JVM，然后让JVM执行指定的编译后的代码.
  - javac.exe：这是Java的编译器compiler，它用于把Java源码文件 (以`.java`后缀结尾) 编译为Java字节码文件 (以`.class`后缀结尾).
  - jar.exe：用于把一组`.class`文件打包成一个`.jar`文件，便于发布.
  - jdb：Java调试器 debugger，用于开发阶段的运行调试.

- 在IntelliJ IDEA中新建工程, 选择Java, 其他无需选择, 一路下一步. 指定工程路径和文件名.

- 在工程的src (source code)文件夹下新建文件, 命名为Hello.java, 要和代码中的class名严格一致, 粘贴入测试代码:

  ```java
  /**
   * The HelloWorldApp class implements an application that
   * simply prints "Hello World!" to standard output.
   */
  public class Hello {
      public static void main(String[] args) {
          System.out.println("Hello, world!");
      }
  }
  ```

  - 在Run菜单中点击 Run 'Hello', 在下部的Run窗口检查运行结果.
- 如果IDE版本过于旧, 或者JDK版本太旧有可能出错, 可以尝试升级解决.
  - 测试代码解释: 首先定义了一个class, Hello. 命名一般使用驼峰命名法, 且以字母开头. 这个class是公共的, 如果不是公共就无法用命令行直接访问.
- class中定义了一个函数, (官方叫method , 方法, 但是我一般用函数, 方法这个词极其容易引起歧义, 如果说"这个方法", 到底是在说这个函数还是在说这个编程的手法呢), 他是静态的, 而且不需要返回值(输出).  此函数会需要一个输入, 输入的数据类型是字符串阵列(也叫数组, 但这又是一个翻译问题, 这里面明显没有数), 这个阵列的代称在这里叫args. 官方规定程序开始的函数必须是是静态函数，方法名必须为`main`，括号内的参数必须是String阵列. 

## Java 语法

- 函数的首字母必须小写.

- 每一个语句以` ;` 分号结尾.

- 乘号不能省略.

- 注释的格式有三种:

  1. // 

  2. /*

     words

     */

	3. /**
	
	   *
	
	   *
	
	   */
	
- Java对空格和回车不敏感.
	
- IntelliJ 自动格式化代码: ` Ctrl + Alt + L`

- 基本数据类型: 

  - 整数类型：byte(单字节整数)，short(双字节)，int(四字节)，long(八字节)
  - 浮点数类型：float，double
  - 字符类型：char, 在Java中, 字符不仅可以存储ASCII字符(即标准字母和数字), 还可以存储Unicode字符(即支持中文和其他语言字符).
  - 布尔类型：boolean .  boolean只有两个值 ` true`和` false`.
  - 二进制数用0b开头,例如:  ` 0b1000000000` 表示十进制的512.
  - 十进制数用0x开头,例如: ` 0xff0000` 表示十进制的16711680.
  - 单精度浮点数` float`的后面要加上f, 例如: ` 3.14f`.
  - 定义变量要初始化, 不然会报错, 不报错程序也容易出问题.

- 除了基本数据类型, 剩下的都为引用类型, 它存储的是数据地址, 而不是数据本身. 最常见的有字符串类型 ` String`

- 定义变量时加上关键词 ` final`, 那么这就是一个常量, 他的值不可变. 常量的作用是用有意义的变量名来避免Magic number，例如，不要在代码中到处写`3.14`，而是定义一个常量。如果将来需要提高计算精度，我们只需要在常量的定义处修改，例如，改成`3.1416`，而不必在所有地方替换`3.14`。

  根据习惯，常量名通常全部大写。

- var 关键词的作用. 例如以下语句太长` StringBuilder sb = new StringBuilder()`, 可以简化为 ` var sb= new StringBuilder`, 程序会根据后面的数据类型判断出前面的数据类型.

- 尽量不要定义全局变量. 一般的变量都在它定义时的大括号内可用, 超出定义它的大括号将不可用. 但是也尽量不要因此定义很多同名的变量, 引起阅读困难.

#### 整数

- 整数的运算结果也是整数, 小数部分会被丢弃. 整数除以0程序会出错, 但是编译不会报错或者无法检查出来.(有可能某个变量在运行过程中变成0) ` int`这个数据类型只有四个字节, 数字太大就会溢出, 溢出不会让程序崩溃但是计算结果会奇怪. 整数求余数的运算符为%.  简写运算符: ` n=n+1` 可简写为 ` n+=1`, 对加减乘除都适用. 还有 ` n++, n--`表示加一减一.

- 移位运算符 ` <<, >>`. 移位总是在4字节2进制的格式下进行, 不足4字节会在高位补0变成4字节. 

  - 负数的最高位是1, 右移的时候不会移动最高位的1, 所以还是负数. 
  - 除此以外, 负数向左移空位是补0, 向右移空位是补1. 如果要向右移补0, 那么右移使用 ` >>>`( 但是没有<<<运算), 但是负数的最高位(符号位)依然不会被影响, 即向右位移操作不会将负数变成正数. 
  - 但是向左移有可能将负数变成正数, 也有可能把正数移成负数, 因为他是补0的.
  - 左移实际就是不断地×2，右移实际就是不断地÷2. 
  - 只有整数类型可以做位移运算, 其他类型都不行.

- 逻辑运算符: ` & and运算`, `| or运算 `, ` ~ not运算`, ` ^ nor运算`. 位运算就是将两个数对齐每一位之间进行指定的逻辑运算. 只有整数类型可以进行位运算, 其他类型都不行. 

  >  例:  将以下两个数按位进行and运算: 167776589 & 167776512
  >  转换为2进制后 167776589 = 00001010 00000000 00010001 01001101
  >  而                       167776512 = 00001010 00000000 00010001 00000000    		
  >  运算的结果就是第二个数167776512.	

  > 此运算的意义为, 如果将转为二进制的数字不看成一个数而是四个8位二进制数的话, 用点隔开就是一个IP地址, 转成十进制就是
  > ` 10.0.17.77`	和 ` 10.0.10.0` , 对他们进行逐位and运算可以判断出IP是否在某个网段中. 

  > 将整数转成二进制输出 String s=Integer.toBinaryString(x);

- 运算的优先级, ++ 和--  比普通的加减乘除优先级高, 会先处理. 乘除有比加减高, 求余数算是一种除法. 其他的优先级太多记不住就用括号来保证不会错, 括号的优先级最高.

- 运算的自动转型和强制转型. 两个长度不一样的类型运算, 结果会变成较长的那个类型. 可以给变量改变类型, 命令:

  > int i =5;
  >
  > short x = (short) i;

  - 强制将一个大尺寸的数据转为小尺寸的, 就是直接丢弃它高位上的数据, 往往数据并不能保持完整.

#### 浮点数

- 浮点数表示范围很大, 但是有误差, 例如0.1在二进制下是一个无限循环小数.  由于浮点数的误差, 比较两个数的大小往往会出现错误的结果, 方法是两个数相减后取绝对值, 这个绝对值很小的话可以认为他们相等. 代码:

  ```java
  float i = Math.abs(a - b); //函数Math.abs()用于提取绝对值
  if (i < 0.0001) {
  //可以认为他们相等
  }
  ```

- 如果一个整数和浮点数运算, 整数会被转成浮点数, 因为浮点数不能转成整数. 但是需要注意的是, 如果涉及到一系列运算, 其中有整数又有小数, 那么整数和整数运算的部分并不会自动转成浮点数, 而是会抛弃小数部分, 然后在和小数运算的时候才会转换, 这样会带来很大的误差, 所以最好手动进行转换.

- 整数除0会出错, 浮点数除0时会返回特殊值, NaN(not a number), Infinity 或者 -Infinity.

- 强制给浮点数转换为整数会丢掉小数部分, 例如 ` int n = (int) 14.3`就会等于14, 如果数字过大超过整数类型所能表达的范围, 就会保留最大值.

  >  四舍五入的一种做法: 给浮点数加0.5然后转成整数型.
- 把正数变成负数或者相反的办法是乘 -1, 也可以直接加上负号.

