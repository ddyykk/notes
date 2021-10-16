



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
  - java.exe：这个可执行程序其实就是JVM，运行Java程序，就是启动JVM，然后让JVM执行指定的编译后的代码. VM就是virtue machine, 虚拟机. Java所有的程序都运行在虚拟机中, 用来保证可移植性.
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

### Maven的测试工程

- 新建一个放置工程的文件夹, 并且在文件夹位置打开控制台, 执行命令: ` mvn archetype:generate `, 然后程序会让你选择一个工程的模板, 直接按回车接受默认1823或者再单独选一个输入都可以(我选了747, 一个默认的Java工程), 程序会开始下载一系列文件, 如果还有选项按照提示操作即可. 然后要求你给出groupId(公司名), 随便起名字. 然后会问artifactId(产品的名字),随便起. 然后问你版本号, 随便起, 最后是package, 暂时随便起. 然后按y确认. 然后就会构建一个新的空白工程文件夹.

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
  
### POM的使用

- Maven使用pom.xml文件来定位和管理所有的文件包, 以下一个例子是其中一个文件包的代码:

	```xml
	<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
      <version>1.4.2.RELEASE</version>
      <scope>test</scope>
	</dependency>
	```
	
	dependency说明了字段的性质, 是一个项目的依赖(包). groupId是发布这个包的公司或机构的名称. artifactId是具体的产品名字, version即为版本号, 带有RELEASE字样说明是发布的最终版本. 还有可能是BETA或者SNAPSHOT版本, 意为开发中的版本. 此段代码请求了一个spring-boot-starter-web的包, Maven会自动分析这个包需要的另外多个包并会自动下载.
	
- Maven的依赖管理还有个字段 ` <scope>test</scope>`, 其中的值有可能是compile, test, runtime, provided. 顾名思义, 它指明了这个包在什么阶段会被用到, 分别是编译时用到, 测试时用到, 运行时用到, 编译时用到但运行时由其他地方提供.

- Maven维护了一个中央的仓库, 管理所有的包可供下载. 一旦被下载过, 这个包就会在本地缓存.

- 如果官方仓库下载缓慢, 可以使用镜像仓库. 方法是, 在用户主目录下的.m2文件夹创建settings.xml文件, 写入以下代码添加一个国内的镜像仓库:

	 ```xml
  <settings>
      <mirrors>
          <mirror>
              <id>aliyun</id>
              <name>aliyun</name>
              <mirrorOf>central</mirrorOf>
              <url>https://maven.aliyun.com/repository/central</url>
          </mirror>
      </mirrors>
  </settings>
  ```
  
- 如果需要使用一个包, 可以在官网提供的搜索页面搜索包的信息填入pom文件:search.maven.org

- 进入到pom文件的目录, 在命令行使用命令: ` mvn clean package`, 正确的话就会在target文件夹看到编译结果

- 在主流IDE中都对maven有支持, 可以导入或新建maven工程.

## Spring Boot

- Spring Boot是一个基于Spring的快速开发工具, 不需编写复杂的配置文件. 内嵌了Tomcat和JSON支持, 开箱即用, 即使要配置也是大幅简化的.

### 	REST API

- 以前的网站前后端不分家, 例如 PHP  JSP 等, 如果只为PC端服务, 问题不大. 但是现在要求一个网站既能输出电脑网页, 更重要的是手机页面, 平板页面等等, 这样的架构就会很困难, 要想适配好就要弄三套页面设计才行. 还有一些平台化网站, 页面不太重要, 重要的是一条条的数据, 例如微博 FB, 他并没有网页的概念. 用户看到的不过是后端发来的一条条数据而已. 所以新的web设计逻辑应运而生. 简言之就是用URL定位资源，用HTTP动词（GET,POST,DELETE,DETC）描述操作。前后端传递信息主要用 JSON 格式 (成对的字符串组成的阵列). 

- 如此一来, 前端发送数据请求, 后端响应纯数据, 没有网页代码, 那么任何硬件平台都可以读取这些数据再按照需求加工, 无需为了不同的硬件做适配. 后端程序可以使用Spring MVC 或者 Jersey 或者 Play Framework, 前端可以使用重量级的AngularJS，也可以用轻量级 Backbone + jQuery 等. 这样建立起来的网站或者叫API就是REST API.

### 第一个SpringBoot项目

- 准备好Maven, IntelliJ IDEA 免费版, 在Spring boot的官网工程生成器页面 ` https://start.spring.io/`可以自动生成一个工程.

- 在生成器页面选择Maven作为项目管理工具, 语言是Java, Spring boot版本暂时无所谓. 后面的项目信息公司名称项目名称等等随意即可. Packaging打包方法选择Jar. 最后一个Java版本选择电脑上对应的版本, 由于我的Java 17网页上不支持, 网页只支持到15, 所以我选择了11. (电脑上也要设置成11)

- 页面右边的依赖, Dependencies 里面就是spring boot帮我们已经集成好的各种组件, 我先选择Spring Web这个基于Spring 和Tomcat服务端的包即可.

- 全部选好以后点击生成, 会自动下载工程模板到本地, 将之解压到需要的文件夹即可.

- 然后在IntelliJ IDEA里面点击打开工程所在的文件夹就会自动导入, 或者打开pom.xml文件就会出现导入工程选项. (2020版, 没有2019版的导入工程按钮). 导入完成以后会自动给我们加上一堆外部插件, 就在 ` External Libraries`下面, 注意看里面的Java 版本要和我们之前选择的匹配.

- 这里导入的工程由于选择了Maven作为管理工具, 目录结构包含了Maven的结构. 包括src就是源代码, target就是成品等等. 还多了两个文件夹.idea是IDE管理设置的文件, .mvn是Maven的文件. 然后工程目录里面还有 .gitignore这个文件说明也直接帮我们配置了git的支持方便以后使用.

- 导入工程成功以后这个工程应该是可以运行的, 在 ` /src/main/java`路径下面应该有一个空白的class, 右键点击文件, run这个文件, 系统就会自动开始运行这个空白工程, 但是虽然我们没有写代码, 但是框架还是可以运行的. 运行以后如果没有问题, 会自动显示Terminal控制台窗口, 里面能看到Spring的文字LOGO, 其中有一行写着 `Tomcat started on port(s): 8080 (http) with context path ''`, 这就基本成功了. 打开浏览器, 输入网址: ` localhost:8080`, 回车就能看到这个服务器的错误信息, 说明服务器成功运行. 然后在IntelliJ IDEA里面点停止按钮就可以停止服务器运行.

-  在` /src/main/java`路径下新增一个class, 起名叫HelloWorld, 默认的代码如下:

    ```java
    package com.example.johnson.web.backend;
    
    public class HelloWorld {
    }
    ```
    
    将代码修改成如下:
    
    ``` java
    @RestController
    public class HelloWorld {
        @RequestMapping("/")
        public String getString(){
            return "Hello world from my first web app.";
        }
    }
    ```
    
    然后IDE应该自动补上引用的模块在代码最前面, 如果没有自动出现, 手动写入也可以:
    
    ```java
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    ```
    
    然后保存, 运行 (run), 和上次一样的Terminal信息说明成功, 同样在浏览器打入网址 ` localhost:8080`就可以看到代码中输出的一句话. 输出内容可以随意修改.
    
- 代码解释 @RestController就是调用了一个REST API的控制器. @RequestMapping 就是调用了一个请求映射功能, 代码中给出的地址是"/", 代表网页的根目录或者默认的页面. 代码的意思就是服务器对于客户机根目录的请求就会运行下面的函数. 如果把"/"改成"/page", 那么想要运行下面的函数, 对应的网址就要改成 ` localhost:8080/page`. 如果在新网址能看到输出的信息, 那么工程就算运行成功. 但是现在只能在IDE中运行, 以后还涉及打包发布等.

- 所以Spring boot就是使用一个个的@XXXX这样的标记符来引入功能, 而无需担心代码, 常见的模块都已经被配置完毕可以直接使用, 大大简化了编程.

### 第二个Spring boot项目, 一个简单的后端

- 如果使用收费版IntelliJ IDEA, 新建工程的页面就可以直接选择spring boot工程, IDE会直接调用Spring boot的工程模板API, 免费版则需要每次在官网下载工程模板, 网址 `https://start.spring.io/`

  - 如果不想使用网站模板, 也可以自己建立.

- 如果要自己建立工程不使用模板方法是:

  - 新建一个Maven工程, 输入好GorupId( 公司名称 ), Artifact (产品名称), 版本, 等. 命名时不能有空格, 有时会产生问题.

  - 在pom文件中写入以下依赖即可:

    ```xml
    <parent>
        <artifactId>spring-boot-starter-parent</artifactId>
        <groupId>org.springframework.boot</groupId>
        <version>2.5.5</version>
    </parent>
    
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>
        </dependencies>
    ```

  - 这里引入了两个包, 一个是spring boot的基础包(版本2.5.5是我目前最新的稳定版， 也是IDE自动帮我填写的), 一个是spring boot的web包, 就可以提供一个基本的网站后端功能. 当然也可以引入别的包, 但是这个是最基础最简单的. 这里就是spring boot大幅简化编程的地方, 这两个组件不是真的两个组件, 他们包含了一大堆组件, 每一个都需要配置和编写, 在这里被几行代码全部搞定了.

  - 记得要写在原pom的groupId等属性的后面.  当然要在<project></project>里面. 这样就可引入spring boot的组件.

- 还要写一个主程序的入口, 也就是application class. 如果使用了spring boot模板来创建工程是自动生成好的, 但是从Maven开始的话就不会自动生成. 先在 ` src/main/java/`中建立你的group Id的package, 这个class也要位于这个package里面. 我给class一个名字叫application, 在第一行package名称后面引入: ` @SpringBootApplication`即可完成一个空白spring boot的程序.

  - 在我尝试建立application时, 输入@SpringBootApplication系统无法解析, 解决方法是在编辑器右侧的maven标签菜单中点击刷新按钮, 这样maven会把spring boot下载到工程中, 因为我们并没有使用模板, 仅仅在pom文件引入spring boot并不会直接生效. 所以需要这样手动加载一下. 但是下一次其他工程应该就不需要, 因为文件只会被下载一次.
  - 接下来添加一些固定的执行代码, 完成后文件如下:

  ```java
  package com.yourgroupId;
  
  import org.springframework.boot.SpringApplication;
  import org.springframework.boot.autoconfigure.SpringBootApplication;
  
  @SpringBootApplication
  public class application {
      public static void main(String[] args){
          SpringApplication.run(application.class, args);
      }
  }
  ```

  - main函数中那个函数调用的东西就是你的application同名的class.

  - 此时我们的工程就已经可以运行了, 右键点击run即可.

  - 在浏览器输入网址 ` http://localhost:8080/`可以看到一个网页就是成功了, 也可以在命令行中使用curl来读取, 命令: ` curl http://localhost:8080/` (需要安装curl). 如果返回结果: `{"timestamp":"2021-10-16T08:22:32.284+00:00","status":404,"error":"Not Found","path":"/"}`

     就说明服务器在运行了. 如果服务器不在运行应该返回结果: `curl: (7) Failed to connect to localhost port 8081: Connection refused`

  - 然后在IDE中停止服务.

- 然后在src/main/java路径下这个package里面新加一个class, 就是我们的数据库. 因为要使用真正的MySQL数据库还有很多配置, 这里就使用一个Java的class当作数据库. 这里我写了一个数据库叫shoppingList, 新建完成后在空白的class里面建立数据库. 我定义了几个私有的属性, id, date, item, shop, purchased. 然后右键生成他们的构造函数和getter. 完成后代码如下:

  ```java
  package yourgroupId;
  
  
  public class shoppingList {
      private long id;
      private String date, item, shop;
      private boolean purchased;
  
      public shoppingList(long id, String date, String item, String shop, boolean purchased) {
          this.id = id;
          this.date = date;
          this.item = item;
          this.shop = shop;
          this.purchased = purchased;
      }
  
      public long getId() {
          return id;
      }
  
      public String getDate() {
          return date;
      }
  
      public String getItem() {
          return item;
      }
  
      public String getShop() {
          return shop;
      }
  
      public boolean getPurchased() {
          return purchased;
      }
  }
  ```

  - 需要注意的是boolean要小写, get函数也要手动修改一下, 原来的名称会出错.

- 然后再新增一个class, 作为这个shoppingList的controller. 名称可以就叫shoppingController. 在class定义之前引入一个组件 ` @RESTController`, 然后对应的引用也会由IDE自动完成. 这个控制器就是给前端返回shoppingList数据用的, 由他来操作后端数据库再返回数据. 

  - class里面要定义一个变量，作为计数器使用来生成id号: ` private final AtomicInteger counter= new AtomicInteger();`, 这个功能需要的引用IDE也会自动加上.

  - 在class里面引入一个GetMapping组件, 用来把内部的程序功能映射到网址上, 这样就能用网址访问程序功能. 代码 ` @GetMapping()`, 括号内写入想要映射的相对网址. 例如("/shoppinglist"), 这个网址一看就是访问购物清单的.

  - Map组件的函数要从用户端传递参数, 需要传递的参数也要在这里定义好. 需要注意的是最后一个参数是boolean类型, 不能直接由JSON方式传递, 因为JSON传递的都是String类型, 所以只能先接受一个int类型的变量purchased, 再用一个函数转换成boolean值. 最后使用接收到的数据建立一个数据库的对象, 再原样输出一份证明接收正确.

  - 还有在比较变量purchased和1的时候, 如果purchased变量是int这种基础变量, 可以使用==来比较, 如果purchased是String这种高级类型, 比较的是两个数据是否指向同一个地址. 如果purchased定义为了String, 那么要用equals()这个函数来比较, 语法: ` yourstring.equals("targetString")`. class完整代码如下:

    ```java
    package yourgroupId;
    
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;
    
    import java.util.concurrent.atomic.AtomicInteger;
    
    @RestController
    public class shoppingController {
        
        private final AtomicInteger counter = new AtomicInteger();
    
        @GetMapping("/shoppinglist")
        shoppingList instance1(@RequestParam(defaultValue = "") String date, String item, String shop, int purchased) {
            
            boolean a = false;
    
            if (purchased==1){
                a = true;
            }
            return new shoppingList(counter.incrementAndGet(), date, item, shop, a);
        }
    }
    ```

    写完后点击run, 运行成功后在浏览器输入这个带有很多参数的网址: ` http://localhost:8080/shoppinglist?date=2021-2-2&shop=supermarket&item=beef&purchased=1`, 如果运行正常的话会返回这些参数的 JSON格式. 以上网址也可以在curl中测试, 需要注意的是如果使用Windows powershell要在命令中使用转义符不然不能识别为一个命令: `  curl http://localhost:8080/shoppinglist?date=2021-2-2&shop=supermarket&item=beef&purchased=1` , Windows的转义符为反引号`.
  
- 这样我们的web程序就成功运行了. 程序由三个部分组成, 分别对应三个class, 数据库, 具体的程序功能, 主程序负责调用功能, 我们甚至不需要在主程序中写其他的模块在哪里, 就会由spring boot自动扫描出来. 自动扫描的前提是主程序和模块要在同一级或同一个package里面, 模块可以放入子目录, 但是不能放在主程序的上一级目录
