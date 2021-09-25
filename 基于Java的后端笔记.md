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
  
- Windows 上存在多个版本的 JDK的切换方法: 在环境变量中编辑 ` Path`  中指向 Java 的路径即可. 也可以存在两个路径, 需要时调整一下上下顺序即可, 在前面的会起作用.

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

- Windows 直接官网下载InterlliJ IDEA community版本. InterlliJ IDEA内置 Java 构建工具Maven和Gradle.

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
- class中定义了一个函数, (官方叫method , 方法, 但是我一般用函数, 方法这个词极其容易引起歧义, 如果说"这个方法", 到底是在说这个函数还是在说这个编程的手法呢, 放在上下文中更加有可能让人迷糊), 他是静态的, 而且不需要返回值(输出).  此函数需要一个输入, 输入的数据类型是字符串阵列(也叫数组, 但这又是一个翻译问题, 这里面明显没有数), 这个阵列的代称在这里叫args. 官方规定程序开始的函数必须是是静态函数，方法名必须为`main`，括号内的参数必须是String阵列. 

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
  - 十六进制数用0x开头,例如: ` 0xff0000` 表示十进制的16711680.
  - 单精度浮点数` float`的后面要加上f, 例如: ` 3.14f`.
  - 定义变量要初始化, 不然会报错, 不报错程序也容易出问题.

- 除了基本数据类型, 剩下的都为引用类型, 它存储的是数据起始地址, 而不是数据本身. 最常见的有字符串类型 ` String`

- 定义变量时加上关键词 ` final`, 那么这就是一个常量, 他的值不可变. 常量的作用是用有意义的变量名来避免Magic number，例如，不要在代码中到处写`3.14`，而是定义一个常量。如果将来需要提高计算精度，我们只需要在常量的定义处修改，例如，改成`3.1416`，而不必在所有地方替换`3.14`。

  根据习惯，常量名通常全部大写。

- var 关键词的作用. 例如以下语句太长` StringBuilder sb = new StringBuilder()`, 可以简化为 ` var sb= new StringBuilder`, 程序会根据后面的数据类型判断出前面的数据类型.

- 尽量不要定义全局变量. 一般的变量都在它定义时的大括号内可用, 超出定义它的大括号将不可用. 但是也尽量不要因此定义很多同名的变量, 引起阅读困难.

#### 整数

- 整数的运算结果也是整数, 小数部分会被丢弃. 整数除以0程序会出错, 但是编译不会报错或者无法检查出来.(有可能某个变量在运行过程中变成0) ` int`这个数据类型只有四个字节, 数字太大就会溢出, 溢出不会让程序崩溃但是计算结果会奇怪. 整数求余数的运算符为%.  简写运算符: ` n=n+1` 可简写为 ` n+=1`, 对加减乘除都适用. 还有 ` n++, n--`表示加一减一.

- 整数移位运算符 ` <<, >>`. 移位总是在4字节2进制的格式下进行, 不足4字节会在高位补0变成4字节. 

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

- 运算的优先级, ++ 和--  比普通的加减乘除优先级高, 会先处理. 乘除有比加减高, 求余数是一种除法. 其他的优先级太多记不住就用括号来保证不会错, 括号的优先级最高.

- 运算的自动转型和强制转型. 两个长度不一样的整数运算, 结果会变成较长的那个类型. 可以给变量改变类型, 命令:

  > int i =5;
  >
  > short x = (short) i;

  - 强制将一个大尺寸的数据转为小尺寸的, 就是直接丢弃它高位上的数据, 所以数据并不能保持完整.

#### 浮点数

- 浮点数表示范围很大, 但是有误差, 例如0.1在二进制下是一个无限循环小数.  由于浮点数的误差, 直接比较两个数的大小往往会出现错误的结果, 正确方法是两个数相减后取绝对值, 这个绝对值很小的话可以认为他们相等. 代码:

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

## Maven 的安装

- Maven必须要跟JDK一起工作.

- 在官网下载 https://maven.apache.org/download.cgi 应该是压缩包, Linux可以下载gz格式, Windows下载zip格式.
- Maven不需要安装程序, 解压即可使用, 将文件夹解压放在合适的地方即可. (C:\Program Files\) 
- Linux下的解压命令:  ` tar xzvf apache-maven-3.8.2-bin.tar.gz`
- 将解压的文件夹中的bin文件夹路径添加至系统环境变量.  在Windows上, 右键点击开始- 系统- 高级系统设置- 环境变量, 窗口下部为系统变量, 选中PATH- 编辑- 新建- 写入"C:\Program Files\apache-maven-3.8.2\bin"(我的情况). 然后打开powershell或者其他控制台, 使用命令 ` mvn -v`来验证安装, 如果成功会显示版本信息. 
  - 在Linux上要简单些:  ` export PATH= ....../apache-maven-3.8.2/bin:$PATH` 将文件的摆放路径添加上去即可.

## Maven的使用

- Maven解决的问题是一个Java工程, 在不同的电脑上编译出来结果不一样. 一般行业内的做法是在需要部署的服务器上编译, 运行. 这样出问题的概率最小. 这个过程中需要管理很多随之而来的文件打包和配套软件和组件版本问题, 这时Maven只用一个pom.xml 配置文件来记录管理一切项目信息就要容易得多. 对于Java来说, 服务器上并没有图形界面的IDE, 如何编译? 最后全部需要命令行, 手动或者只用脚本来编译都是不够直观而且麻烦的. 那么由Maven就可以一键生成项目需要的输出. Maven还提供了一种标准化的项目文件结构, 让你的项目结构直接与业内标准相同.
- Maven的本质是将整个项目当成一个object来对待, 使用这个项目就是创建这个项目的一个class. 他的目的就是解决 Java 工程复杂的组件依赖和自动编译打包. 除了项目配置信息, 依赖管理的资源都来自中央服务器. 所以构建时必须有网络.

- 新建一个放置工程的文件夹, 并且在文件夹位置打开控制台, 执行命令: ` mvn archetype:generate `, 然后程序会让你选择一个工程的模板, 直接按回车接受默认1823或者再单独选一个输入都可以(我选了747, 一个默认的Java工程), 程序会开始下载一系列文件, 如果还有选项按照提示操作即可. 然后要求你给出groupId(公司名), 随便起名字. 然后会问artifactId(程序的名字),随便起. 然后问你版本号, 随便起, 最后是package, 暂时随便起. 然后按y确认. 然后就会构建一个新的空白工程文件夹.

  > 如果我按照官网的方法构建反而会失败, 官网给出的命令是 ` mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false`, 程序报错说没有工程也没有pom文件, 直到我把所有参数删掉才可以了.

- 文件夹的结构如下

  ```
  my-app
  |-- pom.xml
  `-- src
      |-- main
      |   `-- java
      |       `-- com
      |           `-- mycompany
      |               `-- app
      |                   `-- App.java
      `-- test
          `-- java
              `-- com
                  `-- mycompany
                      `-- app
                          `-- AppTest.java
  ```
  
  - 其中 src/main/java 这个目录包含着工程的源代码. 而在 src/test/java中存放了测试源代码, pom.xml中pom意思是Project Object Model, 这个文件是整个工程的设置.
  
- 控制台中进入刚刚生成的app文件夹, 也就是存放pom的地方, 运行命令:` mvn package`, 就会自动开始下载文件和构建(导出)工程. 刚刚的命令是一个goal, 只给出了目标, 程序来执行. 这个命令是一个phase, 一个阶段: 打包. Maven有很多阶段, 比如给出命令编译compile, 实际会执行下面一系列动作:

	1. validate
	2. generate-sources
	3. process-sources
	4. generate-resources
	5. process-resources
	6. compile
	
- 执行完了打包命令后,  工程就被打包成一个程序文件可供使用了, 这个文件放在工程文件夹下的target目录, 以你的程序命名和版本号为文件名, 后缀是jar. 在控制台中用java命令调用这个文件就会执行,  一般示范工程会输出Hello World!  命令: ` java -jar appID.jar`. 

  - 但是我的构建完成后运行并不成功, 运行jar文件错误是找不到主类, 显示 ` java.lang.ClassNotFoundException: /\web-1/0/jar` , 按照官方的提示, 如果失败要检查pom里面两个地方的设置:

    ```  xml
        <properties>
            <maven.compiler.release>11</maven.compiler.release>
        </properties>
    ```
    - 第一个就是编译器的版本要对应 JDK 的版本, 这里我使用了版本11的 JDK. (经过测试, 此工程在目前最新的 JDK 17下也能编译并且 JAR 文件能够成功运行)

    ``` xml
        <build>
            <pluginManagement>
                <plugins>
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-compiler-plugin</artifactId>
                        <version>3.8.1</version>
                    </plugin>
                </plugins>
            </pluginManagement>
        </build>
    ```

    - 第二就是这个编译插件的版本要用最新的才能支持比较新的 JDK.

  - 然后我到源代码文件夹打开源代码的java文件, 源代码里面是空的, 需要手动加上Hello World的代码: ` System.out.println("Hello World");`
  
  - 都完成后再执行 ` mvn package`这次构建出来的jar文件终于可以成功执行. 不仅在这台电脑能执行, 应该在任意电脑都可以执行.(当然要有Java)
  
  - 我已经大概了解工程的构建方式, 就是按照某个模板建立一些文件夹, 然后去pom文件里面设置好一系列参数, 写好源代码文件, 然后用Maven命令打包导出 jar 压缩包就可以在任意电脑上执行了.  我第一次运行不成功可能是其中有些默认设置并不契合我电脑的情况, 换了一个构建模板就好了.

