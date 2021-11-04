



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

	- 保存为后缀名为 HelloWorldApp.java的文件.
	
	- 在命令行进入保存java文件的目录, 编译文件, 命令: ` javac HelloWorldApp.java`, 这样会生成一个HelloWorldApp.class文件, 此文件为java的可执行文件. 文件名一定是这个除非另外指定, 因为这是代码中写好的.
	
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
  - 测试代码解释: 首先定义了一个class, Hello. 命名一般使用驼峰命名法, 且以字母开头. 这个class是公共的, 如果不是公共就无法用命令行直接访问. class 一般以大写字母开始. 
  
- class中定义了一个函数, (官方叫method , 方法, 但是我一般用函数, 方法这个词极其容易引起歧义, 如果说"这个方法", 到底是在说这个函数还是在说这个编程的手法呢, 放在上下文中更加有可能让人迷糊), 他是静态的, 而且不需要返回值(输出).  此函数需要一个输入, 输入的数据类型是字符串阵列(也叫数组, 但这又是一个翻译问题, 这里面明显没有数), 这个阵列的代称在这里叫args. 官方规定程序开始的函数必须是是静态函数，方法名必须为`main`，括号内的参数必须是String阵列. 

- class名称必须和文件名一致, Java区分大小写. 每个Java必须有一个main函数.

- sysytem.out.println() 函数用于在控制台输出一行字符.

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
  - 浮点数类型：float，double. 他们可用科学计数法, 例如 34.54e4f表示345400.00
  - 字符类型：char, 在Java中, 字符不仅可以存储ASCII字符(即标准字母和数字), 还可以存储Unicode字符(即支持中文和其他语言字符). char只表示单个字符, 用单引号包围. 当然char也接受ASCII数值.
  - 布尔类型：boolean .  boolean只有两个值 ` true`和` false`.
  - 二进制数用0b开头,例如:  ` 0b1000000000` 表示十进制的512.
  - 十六进制数用0x开头,例如: ` 0xff0000` 表示十进制的16711680.
  - 单精度浮点数` float`的后面要加上f, 例如: ` 3.14f`.
  - 定义变量要初始化, 不然会报错, 不报错程序也容易出问题.

- 除了基本数据类型, 剩下的都为引用类型, 它存储的是数据起始地址, 而不是数据本身. 最常见的有字符串类型 ` String`. String类型要使用双引号包围. 

- 基本类型和引用类型的区别: 基本类型只是储存了数据本身. 引用类型并不是一个数据类型, 而是一个class. 也就是一套预定义好的函数, 他内部除了储存数据本身还预定义了很多对数据的操作, 那么无需定义就可以直接使用, 在定义这些变量时会一起被建立, 有时可以简化代码. 基本数据类型永远都有值, 引用类型则可以为空 ` null`.  基本类型以小写字母开头, 引用类以大写字母开头. 同一基本类型的占用空间大小都一样, 例如int都是4字节. 引用类型的占用空间则不一定. 常见的引用类型有 `String` `Array` `Class` `Interface`.

- 定义变量时加上关键词 ` final`, 那么这就是一个常量, 他的值不可变. 常量的作用是用有意义的变量名来避免Magic number，例如，不要在代码中到处写`3.14`，而是定义一个常量。如果将来需要提高计算精度，我们只需要在常量的定义处修改，例如，改成`3.1416`，而不必在所有地方替换`3.14`。

  根据习惯，常量名通常全部大写。

- var 关键词的作用. 例如以下语句太长` StringBuilder sb = new StringBuilder()`, 可以简化为 ` var sb= new StringBuilder`, 程序会根据后面的数据类型判断出前面的数据类型.

- 尽量不要定义全局变量. 一般的变量都在它定义时的大括号内可用, 超出定义它的大括号将不可用. 但是也尽量不要因此定义很多同名的变量, 引起阅读困难.

- 定义变量最好不要用无意义的字, 而是尽量让人能知道意思. 例如变量a, b, c, 就不好, 而 age, name, id等就要容易读得多.

- 命名变量只能用字母数字下划线和美元符号, 不能用其他特殊字符和空格.

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

- 运算的自动转型和强制转型. 两个长度不一样的整数运算, 结果会自动变成较长的那个类型. 可以手动给变量改变类型, 命令:

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

#### String类

- 定义多个字符串之间要用逗号隔开.

- 一个string实际上是一个对象, 其中包含一些操作自身数据的函数. 例如可以检查自身长度的函数: string.length();

- string.toUpperCase() 可以将字符串中的英文字母全部转为大写, 已经是大写的不变. 同样, string.toLowerCase()可以将字符串中的英文字母全部转为小写. 如果里面有其他非字母的字符则不受影响. 
  - 注意, 这个函数并没有改变字符串的内容, 只是将内容输出时转换了而已, 不影响字符串的本身.
- string.indexof("string") 在字符串中查找给出的内容, 返回查找到的内容在字符串中的起始位置(index), 从0 算起.
- 可以用加号连结两个字符串. ` a=string1+string2;`. 也可以用String内置的函数concat()来连接两个字符串. 例如: string1.concat( string2 );
- 转义符为\, 如果字符串中需要用到 "  ' 或者\等符号, 要用\来转义, 不然会被编译器当作编程的操作符号. 例如 ` "It's "alright"."`要写为 `news="It's \"alright\"."`才能正确显示.
- 如果用加号 + 连接一个数字和字符串, 数字会被当做字符串处理. 其中浮点数中的 d, f字母会被省略. 科学计数法也会被转换成电脑认为方便的格式.

#### Math 类

- Math 类(class) 内置了一些跟数学有关的函数.
- Math.max(x,y) 用来比较大小, 他会返回两个数中较大的. Math.min(x,y)则会返回较小的. 这两个函数只能比较两个数, 可以比较整数和浮点数.
- Math.sqrt(x) 用来开平方. Math.abs(*x*) 用来返回一个数的绝对值. Math.random()会给出一个0-1之间的随机数,  包括0但是不包括1. 如果需要随机数的范围变化的话就需要自己写一些算法. 例如需要一个0-100之间的随机整数就要写成:  ` int randomNum = (int)(Math.random() * 101); `

#### 布尔值

- 布尔变量只有true和false两种值
- 布尔表达式也会返回布尔值, 例如 ` 9>10`就会返回false.

#### 条件语句

- 基本用法 , 其中第二 , 三 句可以省略.

```java
  if(condition){
      //code if condition is true
  } else if(condition2) {
      //code if the condition is false and condition2 is true
  } else{
      //code if none is true
  }
```

- if else 语句的缩写: ` variable = (condition) ? stringIfTrue :  StringIfFalse; ` 注意以上简写的方法只能输出字符串.
- Switch语句, 用法:

```java
switch(){
    case 1:
        //code
        break;
    case 2:
        //code
        break;
    default:
        //code
}
```

- break和default语句是可选的.

#### 循环语句

- while loop: 

  ```java
  while(condition){
      //code
  }
  ```

  ```java
  do{
      //code
  }
  while(condition);
  ```

  do/while 循环的特点是, 第一次进入循环并不会进行条件判断, 总是先执行一次代码, 然后再判断.

- for loop. 和C语言的一样, 省略.

- for-each loop. 这个循环专为历遍一个阵列用的.

  ```java
  String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};
  for (String i : cars) {
    System.out.println(i);
  }
  
  ```

- break语句. break除了能跳出switch语句, 还可以跳出循环. 例如

  ```java
  for (int i = 0; i < 10; i++) {
    if (i == 4) {
      break;
    }
  }
  ```

  - 当 i = 4 的时候, 循环被终止, 不会继续执行.

- continue语句. continue的作用和break类似, 区别是continue只会终止一次循环, 不会停止整个循环. 
- break和continue都可以使用在for loop 和while loop.

#### 阵列

- 阵列用来存储多个同类数据, 而不用建立多个数据. 要定义一个阵列, 只需在定义的数据类型后面加上方括号. 例如一个字符串阵列: ` String[] names;`, 也可以定义时赋值:  ` String[] names = {"Bill", "Newton", "Hamilton", "Miku"};`

- 要访问阵列中的元素, 使用序号, 例如 names[0], 序号从零开始数.

- 阵列也是引用数据类型, 带有函数. 常用的有 array.lenth, 这回返回一个int. 历遍阵列的元素, 前面说过用for-each loop. 这个loop更容易写, 容易读. 而且不需要计数的变量.

- 多维阵列. 建立多维阵列就是给每一个维度单独的括号. 例如:

  ```java
  int[][] myNumbers = { {1, 2, 3, 4}, {5, 6, 7} };
  ```

  第一个序号代表阵列的序号, 第二个序号是阵列内部元素的序号. 例如 `myNumbers[1][2]`意思就是第二个阵列中的第三个元素, 就是 7. (序号从零开始数)

  所谓多维阵列就是阵列的阵列.

  - 要历遍多维阵列, loop就要两层. 如果只写一层loop, 输出的就是一个个阵列对象而不是具体的数据. 以上面的`int[][]`为例, 要历遍其中的整数要这样写:

    ```java
    //for each loop
    int[][] my={{1,2,3,4},{5,6,7,8},{9,10,11,12}};
            for (int[] i:my
                 ) {
                for (int j:i
                     ) {
                    System.out.println(j);
                }
            }
    //for loop
    for (int i = 0; i < my.length; ++i) {
          for(int j = 0; j < my[i].length; ++j) {
            System.out.println(my[i][j]);
          }
    ```

### 函数(或者method 方法)

- 一个函数必须定义在一个class内部,  先给出函数的一些性质(static, public, private 等等), 返回类型(输出的数据类型), 名称, 参数, 最后是代码. 
- 调用函数就是使用函数名和括号, 如果有参数就写在括号内.
- 函数的overloading, 指的是多个函数拥有同样的名称, 但是功能不同. 例如两个同名函数, 一个处理整数, 一个处理小数, 可以使用同一个名称, 但是返回值类型或者参数列表必须不一样.

#### 代码块

- 在Java中, 变量只有先定义才能被使用, 如果先使用, 再定义是不行的, 哪怕是相邻的两行也不行.
- 被大括号{}包裹的代码就是代码块, 代码块内部的变量是不能被代码块外部的变量看到或者访问的. 除了循坏可以使用代码块以外, 代码块也可以独立存在. 对于for 循环, 在循环判断语句中定义的变量也是可以被底下的代码块访问的.

#### 递归

- 递归的意思就是让一个函数调用自己本身. 例如从 1  加到 10 , 一般的方法要调用loop 运算 10 次. 而使用递归则只需要写一个函数, 然后自动运算即可.  例:

  ```java
  public class Main {
    public static void main(String[] args) {
      int result = sum(10);
      System.out.println(result);
    }
    public static int sum(int k) {
      if (k > 0) {
        return k + sum(k - 1);
      } else {
        return 0;
      }
    }
  }
  ```

  以上函数写在main class内, 但是写在main 函数外, 也就是说一个函数内不能定义另一个函数.

  在使用循环, 递归这类功能时要小心进入无限循环, 一般要设置停止条件. 上面代码的停止条件比较开放, 不够安全., 最好是设定成 start, end这样的数字范围才足够安全.

### OOP 面向对象编程

- 面向过程编程就是只处理数据, 面向对象就是处理的几乎都是对象, 除了数据本身, 往往还附加绑定了很多功能(函数). OOP的好处, 结构清晰, 减少重复代码, 容易维护和管理. 重复使用代码很方便. 将需要重复使用的代码提取出来单独放置, 这样可以尽量减少重写.

#### class 类

- class 和 object的区别. class就是一个模板,或者一个抽象的概念, 这个模板的具体例子就是object. 所以创建一个对象(object)时, 他会从模板那里继承所有变量和函数. 变量和函数也叫属性(attributes)和方法(method).

- 创建一个class, 要使用关键词class, 命名的第一个字母要大写, 文件名一定要和class名一致. 起始一个class可以理解为一个新的数据格式, 类似int.

- 创建一个object, 首先指定object的类型, 也就是class 名, 然后命名, 然后等号后面要用关键字 new. 例如建立一个class 叫Main, 然后建立一个object属于这个class的代码如下:

  ```java
  public class Main {
    int x = 5;
  
    public static void main(String[] args) {
      Main Obj = new Main();
      System.out.println(myObj.x);
    }
  }
  ```

- 既然class是模板, 那么当然一个模板可以创建无数个实例, 只要每一个命名不一样即可. 例如:

  ```java
      Main myObj1 = new Main();  
      Main myObj2 = new Main(); 
  ```

- 一般每个class都存放在单独的文件中, 然后用Main class去建立所有的object和调用这些object里面的数据和函数. 代码如下:

  ```java
  MyClass.java
  public class MyClass {
    int x = 5;
      public int cal(int a){
          return a+x;
  	}
  }
  ```

  ```java
  Main.java
  class Main {
    public static void main(String[] args) {
      MyClass myObj = new MyClass();
      System.out.println(myObj.x);
      System.out.println(myObj.cal(89));
    }
  }
  ```

  - 上面两个java文件放在同一文件夹下面就可以互相访问, 无需引用.

#### class attributes 类的属性

- 属性(attribute)也叫变量(variables), 还有些地方叫域(field). 无法访问一个class, 因为他只是一个模板, 我们要将他实例化, 将他变成一个对象才能访问. 访问的方法是对象名称后面加点. 在上个例子中我们就访问了myObj这个对象里面的x变量, 写法是myObj.x
- 我们可以随意改变变量的值, 例如 `myObj.x=10`, 如果我们不希望变量被改变, 那么就在定义的时候加上 `final`关键词: ` final int x=10;` 这个用法是为了保存一个常量, 例如一个系统的参数, 这样它就不会被随意修改, 当我们要修改的时候也只要修改一处即可.
- 如果一个class建立了多个对象, 要注意区分每个对象里面的同名变量. 例如 myObj.x 和myObj2.x

#### class method 类的函数

- 一个class里面可以写无数个变量, 也可以写任意数量的函数. 调用函数就是函数名加括号, 如果有参数就在括号内写入.
- static 函数与 非 static函数. 如果在定义class时, 定义了static函数, 意思就是这个函数不需要建立一个实例的对象也可使用, 而 非static的函数, 就必须要通过建立一个对象才可以使用.  
  - 需要注意的是, 要使用static函数, 那么就没有建立对象的步骤, 那么其中函数以外的变量很可能还是不能访问的, 因为变量也需要建立对象才会存在.
  - 要使用static函数, 调用的时候对象名称就要改成class名称. 例如 ` myObj1.x();`这样的调用就是使用了对象的名称, 而static函数不需要建立对象, 那调用的话就是: ` myClass.x();`

#### java constructor 构造函数

- 构造函数就是用来初始化对象的函数. 当创建对象的时候就会调用构造函数. 构造函数的名称必须要和class名称一致, 而且不能有输出类型(就是不能写void, int等等).

- 所有的class默认都有构造函数, 如果没有手动写, 系统会给你自动创建. 但是你就不能给他加入你想要的初始数据了.

- 构造函数也是可以接收参数的, 用来设定初始值. 例如:

  ```java
  public class Main {
    int x;
  
    public Main(int y) {
      x = y;
    }
  
    public static void main(String[] args) {
      Main myObj = new Main(5);
    }
  }
  ```

  这个例子中, Main class就有一个没有返回类型的函数, 与class 同名. 系统就会把它认作构造函数. 这个构造函数要求了一个整数的输入,用来初始化变量x, 在建立对象时, 也传入了一个整数.

- 当然, 也可以传入多个参数, 同时初始化多个变量.

#### Java modifier 修饰词

- 常用的一个修饰词时public, 这是一个访问修饰, 控制由哪些权限可以访问或使用后面定义的东西.
- 权限控制的修饰词:
  -  修饰class的: 
    - public 可以被另一个class 访问, 意思就是别的class能不能创建这个class的对象.
    - default 只能被处在同一个package(文件夹)的class访问,如果没有修饰词就会应用这个默认修饰词.
  - 修饰class成员的: 
    - public 可以被所有class访问, 基本上相当于开放了
    - private 只能被自己的class访问
    - default 只能被同一个package内访问, 如果没有指定, 就会应用这个默认的.
    - protected 可以被同一个package和子类(subclass, 就是继承类)访问.
- 非权限控制的修饰词:
  - 修饰class的
    - final 这个class不能被继承
    - abstract 抽象, 这个class不能用来创建对象. 要访问抽象class, 只能从别的class继承.
  - 修饰class成员的
    - final 成员变量不能被修改, 函数不能override
    - static 变量和函数属于class, 不属于对象. 即是建立对象时不会建立static的成员, static成员只有一份存在class里面.
    - abstract 只能在abstract class里面用, 而且只能在函数上用. 而且函数没有代码只有名称, 例如: ` abstract void run();` 函数代码会在继承时提供.
    - transient 成员在序列化成对象时会被跳过
    - synchronized 函数一次只能被一个线程访问.
    - volatile 变量的值不在线程内部缓存, 永远从内存中读取.

#### java encapsulation 封装

- 封装的意思是, 有一些敏感的数据是对用户不可见的. 要封装数据, 要把变量定义为 ` private`, 要操纵这些变量就要用 get 函数(读取) 和 set 函数(赋值).
- getter 和 setter 的写法也是有标准的, 首先要用 public 来修饰, 函数名称要以get和set开头, 后跟变量名, 但是首字母要大写. 例如:

```java
public class Person {
  private String id; // private = restricted access

  // Getter
  public String getId() {
    return id;
  }

  // Setter
  public void setId(String newId) {
    this.id = newId;
  }
}
```

需要注意的是, 这里使用了 ` this`关键词,  意思是这个对象的意思. 因为使用的时候 class 会被实例化成对象. 当然, 变量id是 private 的, 外部也无法访问.

- 一个使用 setter 和 getter 的例子:

  Person class

  ```java
  public class Person {
     private String name;
  
     // Getter
     public String getName() {
       return name;
     }
  
     // Setter
     public void setName(String newName) {
       this.name = newName;
     }
  }
  ```

  Main class:

  ```java
  public class Main {
    public static void main(String[] args) {
      Person myObj = new Person();
      myObj.setName("John");
      System.out.println(myObj.getName());
    }
  }
  ```

  - 以上例子省略了构造函数, 因为并不需要设定初始值, 现实工作中, 建立一个class, 构造函数, getter, setter 都是默认要写的.
  - IDE中会提供自动生成这些常用函数的功能, 可以节省大量时间. 在IntelliJ IDEA上就可以右键, 生成这些常用的 重复代码, 无需手打. 

- 使用封装的好处: 更好的控制这些变量和函数.  可以使变量成为只读的, 只要不写setter就可以. 管理代码也更容易, 可以只修改一部分而不影响其他. 关键数据对用户不可见更加安全.

#### Java package 包

- 一个package是用来给相关的class文件分组的, 类似一个文件夹. 用包来避免命名冲突, 方便管理代码. 包分为自带的和用户定义的. JDK 的Java API自带了很多包,  ` https://docs.oracle.com/javase/8/docs/api/.`具体在线文档可以看细节. 一个库(Library)可以分割成package 和 class , 就是说你可以引入一个class 也可以引入一个package.

- 引入一个class, 需要用import关键词. 例如需要 ` Scanner` class, 用来处理用户输入, 代码就是: ` import java.util.Scanner;`, 记得结尾也要分号. ` java.util`就是一个内置的package, 而 ` Scanner`则是一个class. 以下的例子里面我们要用 ` Scanner` class 的一个函数, ` nextLine()`, 用来读取一行用户输入(以回车键结尾算一行).

  ```java
  import java.util.Scanner;
  
  class MyClass {
    public static void main(String[] args) {
      Scanner myObj = new Scanner(System.in);
      System.out.println("Enter username");
  
      String userName = myObj.nextLine();
      System.out.println("Username is: " + userName);
    }
  }
  ```

- 引入一个package,  ` import java.util.*;`用星号表示所有的class就可以引入整个package.
- 如果想自定义package, 要用 ` package`关键词. 代码: ` package mypackage;` 然后再写下代码即可. 一般在现代的IDE中, 无需再手动编译package. 只要在新建菜单中选择package即可, 编译的时候会自动生成package名字的文件夹. 同一个package下的文件一般是可以互相看见的, 不需要引用.

#### Java inheritance 继承

- Java可以用一个class 来继承另一个class 的变量和函数 , 被继承的就是父类(superclass), 继承的就是子类(subclass).

- 如果要继承, 就使用extends 关键词.  例如我们建立一个class 叫做People, 然后用一个class Student去继承他, 在继承的时候除了会继承所有的变量和函数, 还能添加原本父class不存在的属性. 这也是继承的意义, 即不用重复写已经存在的部分.

  ```java
  class People {
      protected int id=1;
      public void outputId(){
          System.out.println(id);
      }
  }
  
  class Student extends People{
      //
  }
  ```

  - 当使用Student模板建立一个新的object时, 这个对象也具有id这个父class的属性.
  - People class的id用了protected来修饰, 因为这样才能被子类访问, 如果是private就不能被子类访问了.
  - 如果要让一个class 不能被继承, 就要加上final修饰词.

#### Java的多态 Polymorphism

- 多态的意思是从一个class继承出来的多个class的关系. 类似上一个例子中 People的子类是Student, 那么其实还可以有多个继承, 例如Man, Woman, Child. 他们可以有相同的属性, 但是也可以有自己独特的属性. 

#### Inner class 内部类

- 内部类的意思就是一个class 里面还有一个class, 这样的用处是让类似的class都放在一起, 方便阅读. 

```java
class OuterClass {
  int x = 10;

  class InnerClass {
    int y = 5;
  }
}

public class Main {
  public static void main(String[] args) {
    OuterClass myOuter = new OuterClass();
    OuterClass.InnerClass myInner = myOuter.new InnerClass();
    System.out.println(myInner.y + myOuter.x);
  }
}
```

- 使用内部类的时候要先创建一个外部类的对象, 再创建一个内部类的对象.  要注意这里的写法, 有点绕.
- 如果将内部类定义为 private 或者 protected, 外部类就不能访问内部类.
- 如果将内部类定义为 static, 就不需要创建外部的对象就可以使用内部类创建对象. 但是如果静态类使用了外部类的数据, 那么就还是要实例化外部类才行, 否则外部类是不存在的.
- 创建了外部类对象和内部类对象以后要注意, 内部类对象是可以访问外部对象的数据和函数的.

#### Java的抽象 Abstraction

- 数据抽象的意思就是只给用户显示核心和必须的信息, 其他的信息全部隐藏. 抽象可以通过建立抽象class或者界面(interface)的方式来完成.

- abstract class 就是不能用来建立对象的,  要访问他, 只能用另外一个class继承他.  abstract method 抽象函数就是只存在于抽象class中, 抽象函数没有内容, 内容要在被继承的时候赋予.

- 抽象class 也可以包含非抽象的函数. 例:

  ```java
  abstract class Animal {
    public abstract void animalSound();
    public void sleep() {
      System.out.println("Zzz");
    }
  }
  ```

  以上这个class如果创建对象就会报错. 其中的第一个函数也没有代码只有名称.

  要使用这个class, 要先继承他:

  ```java
  class Pig extends Animal {
    public void animalSound() {
      // The body of animalSound() is provided here
      System.out.println("The pig says: wee wee");
    }
  }
  
  class Main {
    public static void main(String[] args) {
      Pig myPig = new Pig(); // Create a Pig object
      myPig.animalSound();
      myPig.sleep();
    }
  }
  ```

  - 抽象的作用就是隐藏信息, 只开放特定的信息给用户.

#### Interface 接口

- 另外一种抽象的方法就是使用接口. 接口就是一种完全抽象的class, 把相关的函数都放在一起, 但是没有代码, 只有名称. 代码都存放在另外一处地方, 以达到保密的目的. 要使用这些函数, 就要使用另外一个class, 配合implemants关键词. 函数的代码都由这个implements class提供.

- 示例代码:

  ``` java
  interface Animal {
    public void animalSound(); 
    public void sleep(); //只有名称没有代码的函数
  }
  
  class Pig implements Animal {
    public void animalSound() {
  //函数代码在这里提供
      System.out.println("The pig says: wee wee");
    }
    public void sleep() {
  
      System.out.println("Zzz");
    }
  }
  
  class Main {
    public static void main(String[] args) {
      Pig myPig = new Pig();  // 创建对象
      myPig.animalSound();
      myPig.sleep();
    }
  }
  ```

  - 界面和抽象class 类似, 都不能当作对象的创建模板.
  - 界面的函数也都没有代码, 代码都放在implements class里面, 实施的时候全部函数都要被override(重写).
  - 界面的函数默认就是abstract 和public的, 所以无需写出来.
  - 界面的变量默认就是public static 还有final的, 即无需对象化就存在, 也不能被更改的.
  - 界面不能对象化, 当然也就没有构造函数.

- interface 界面的用途, 除了加密需求外, 因为Java不支持多重继承, 即一个class只能从一个父class继承, 例如学生这个class就可以继承儿童和人这两个class, 要做到这样, 就要使用界面.  因为一个class可以实现多个界面. 要实现多个界面, 用逗号分隔开. 示例如下:

  ```java
  interface FirstInterface {
    public void myMethod(); // interface method
  }
  
  interface SecondInterface {
    public void myOtherMethod(); // interface method
  }
  
  class DemoClass implements FirstInterface, SecondInterface {
    public void myMethod() {
      System.out.println("Some text..");
    }
    public void myOtherMethod() {
      System.out.println("Some other text...");
    }
  }
  
  class Main {
    public static void main(String[] args) {
      DemoClass myObj = new DemoClass();
      myObj.myMethod();
      myObj.myOtherMethod();
    }
  }
  ```

  - 这其中interface的函数前面不需要public, 因为这个代码是教程中的. 还有要implement这些interface, 可以自动生成他们的函数名称返回类型等等, 不需要重新写这些重复的代码.

#### Java Enums 枚举

- 枚举包含了一组常量. 

- 要创建一个 enum, 用 ` enum`关键词来建立, 然后常量用逗号分开, 一般都是用大写字母. 例如:

  ```java
  enum Level {
    LOW,
    MEDIUM,
    HIGH
  }
  ```

  - 使用的时候就写 Level.LOW
  - enum可以放在class外面和class同级也可以放在class里面. 放在class里面 enum 默认是static的, 无需对象化. 可以直接用. 示例:

  ```java
  public class MyClass {
  
  enum namelist{
      Jonny,Ive,Teddy,Helen
  	}
  }
  
  然后在mian()中调用:
  public class Main {
  
      public static void main(String[] args) {
           MyClass.namelist myname= MyClass.namelist.Jonny;
          System.out.println(myname);
      }
  }
  ```

  - 注意调用的写法比较绕.

- enum在switch语句中的使用, enum在switch中常常用来做数值判断. 代码: 

  ```java
   MyClass.namelist myname= MyClass.namelist.Jonny;
  
          switch (myname){
              case Jonny:System.out.println("My name is "+myname+". ");break;
              case Ive:System.out.println("My name is "+myname+". ");break;
              case Helen:System.out.println("My name is "+myname+". ");break;
              case Teddy:System.out.println("My name is "+myname+". ");break;
              default:System.out.println("My name is null. ");
          }
  ```

- enum自带一个values() 函数, 会给出所有的值的阵列, 用来历遍这个里面所有的值很有用. 还是上面的例子, 结合一个for each loop:

  ```java
   for (MyClass.namelist myname:MyClass.namelist.values()
               ) {
              switch (myname){
                  case Jonny:System.out.println("My name is "+myname+". ");break;
                  case Ive:System.out.println("My name is "+myname+". ");break;
                  case Helen:System.out.println("My name is "+myname+". ");break;
                  case Teddy:System.out.println("My name is "+myname+". ");break;
                  default:System.out.println("My name is null. ");
              }
          }
  ```

- enum和class的区别: enum也可以有变量和函数, 但是他们都是public和static和final的. 不能改变, 不能改写(override). enum不能创建对象, 不能继承其他class, 但是可以implement 界面. 使用enum的原因是里面有很多值是无需改变的, 例如年月日, 颜色, 一组卡片等.

#### Java 的用户输入

- 在java.util package里面有一个class Scanner用来处理用户输入. 要使用他就创建一个Scanner的对象并传入System.in这个变量. 前面已经用过. 然后用 ` nextLine()`函数来读取字符串. 除此以外还可以读取其他类型:

  - 布尔值
  - 字节
  - double 小数
  - float 小数
  - int 整数
  - long 整数
  - short 整数

- 如果输入的类型与期待的类型不同, 就会产生系统错误, 处理方法在后面讲到.

- 读取的代码如下:

  ``` java
  import java.util.Scanner;
  
  class Main {
    public static void main(String[] args) {
      Scanner myObj = new Scanner(System.in);
  
      System.out.println("Enter name, age and salary:");
  
      // String input
      String name = myObj.nextLine();
  
      // Numerical input
      int age = myObj.nextInt();
      double salary = myObj.nextDouble();
  
      // Output input by user
      System.out.println("Name: " + name);
      System.out.println("Age: " + age);
      System.out.println("Salary: " + salary);
    }
  }
  ```

  每次输入完都要按回车, 一定要按要求的顺序输入.

#### Java的日期与时间

- Java没有内置日期class, 但是可以引入 ` java.time`package来处理日期和时间. 例如

  - LocalDate 用来指示一个日期
  - LocalTime 用来指示时间
  - LocalDateTime 指示日期和时间
  - DateTimeFormatter 用来转换日期和时间的格式

- 显示当前日期: 引入 ` java.time.LocalDate`class, 然后用 now()函数.

- 显示当前时间: 引入 ` java.time.LocalTime`class, 用 now()函数.

- 显示当前日期和时间: 引入 ` java.time.LocalDateTime`class, 用now()函数.

- 日期和时间格式: 引入 ` java.time.format.DateTimeFormatter`其中的ofPattern()函数来指定输出格式. 以上功能的代码如下:

  ```java
  import java.time.LocalDate;
  import java.time.LocalTime; 
  import java.time.LocalDateTime;
  import java.time.format.DateTimeFormatter; 
  
  public class Main {
  
      public static void main(String[] args) {
          
          LocalDate myDate=new LocalDate.now();
          LocalTime myTime=new LocalTime.now();
          LocalDateTime myDateTime=new LocalDateTime.now();
          DateTimeFormatter myFormat=new DateTimeFormatter.ofPattern("dd-MM-yyyy HH:mm:ss");
          String dateTime=myDateTime.format(myFormat);
          System.out.println(myDateTime);
          System.out.println(dateTime);
      }
  }
  ```

  - 在我写入代码测试的时候会报错, 说找不到对象. IDE 提示我删除所有的new关键词, 然后就可以了. 可能是新版的Java有了一些更改.
  - 没有格式化时输出的日期时间是 ` 2021-10-30T15:59:50.163223800` 中间的T是分隔日期和时间的, 后面的数字是纳秒, 为了精确, 但是一般都用不上这样的格式, 所以一般都要自己格式化. 经过格式化的输出是 ` 30-10-2021 15:59:50`, 注意里面不包含时区信息, 他读取的是电脑主机的时钟而已.

  - ofPattern()接受的常见格式有:
    - *yyyy-MM-dd*
    - *dd/MM/yyyy*
    - *dd-MMM-yyyy*
    - *E, MMM dd yyyy* 其中E是星期几, MMM指的是月份的英文简写.

#### Java的阵列列表 ArrayList

- 一个 ` ArrayList`是一个可改变大小的阵列, 在 ` java.util`里面定义.

- 他和标准的array的区别是, array的尺寸是不能更改的. 要想增加或减少元素, 就要重新建立array. ` ArrayList`可以随时添加元素. 语法稍微有区别.

- 建立一个` ArrayList`对象的语法: 

```java
ArrayList<String> cars = new ArrayList<String>(); 
```

-   `ArrayList`的一些函数:

  - 加入元素:

  ```java
      ArrayList<String> cars = new ArrayList<String>();
      cars.add("Volvo");
      cars.add("BMW");
      cars.add("Ford");
      cars.add("Mazda");
      System.out.println(cars);
  ```

  - 最后一句会把所有的元素在方括号中输出, 没有双引号.
  - IDE推荐的写法是: `ArrayList<String> cars = new ArrayList<>(); `, 因为后面一个String很明显, 不用强调.
  - 使用元素, 不能直接使用下标, 要使用get()函数. 例如 ` cars.get(0)`, 要修改元素就要使用set()函数: ` cars.set(0, "Honda")`, 其中0 是index, 后面跟你要更新的值. 要删除元素, 就用remove()函数, ` cars.remove(0)`, 要清空这个阵列列表, 用 ` cars.clear()` 函数. 还有一个size()函数会返回列表的元素个数. 删除掉一个元素, 后面的元素全部会前移.

- 历遍列表: 使用 ` for-each loop`

  ```java
  for (String i:cars
        ) {
       System.out.println(i);
  }
  ```

- 在阵列列表里面存储的实际都是对象, 就像String全部都是对象, 而不是数据格式一样, 如果要在列表中存放别的数据格式, 也要用一个封装过的格式, int 要用 Integer, boolean 要用 Boolean, char 要用 Character,  double 要用Double. 例如 ` ArrayList<Integer> myNumbers = new ArrayList<Integer>();`

- ` java.util`里面还有个常用的class是 Collection, 有个sort()函数, 用来给列表排序. 这是一个static函数, 无需建立对象. 代码: ` Collections.sort(cars);`. 如果是字符串就按字母顺序排, 数字就按数字排. 字母数字都有就是先数字后字母.

#### LinkedList

- 连接表 跟 ArrayList 非常类似, 都可以用类似阵列的方式存储数据(其实是对象), 只要将代码中的ArrayList替换成LinkedList就可以继续使用. 所有的函数也有是一样的, 因为其他们都是 实现了 List 这个界面. 
- 区别. Array List包含一个阵列, 也就是不能随意加减元素, 添加元素的原理实际上是删除旧的, 新建一个新的. 而Linked List 的所有成员对象都放在单独的容器, 只不过互相链接起来了, 添加元素的时候就是新加了一个容器, 再跟原来的容器链接起来.
- 所以 ArrayList 更适合存储和统计数据, 可能会运行效率更高, LinkedList 用来操纵数据(增删改), 有些函数更方便. 例如, 看名字就知道意思了, 这个就不解释了.
  - addFirst()
  - addLast()
  - removeFirst()
  - removeLast()
  - getFirst()
  - getLast()

#### Java HashMap 哈希映射

- 不像 ArrayList , 它是把数据按照编号排列的, 一个哈希映射使用成对的 "名称/值" 来存储数据, 要找到他们的时候不用记编号, 而是直接使用他们的名称.  而且名称和值不一定是同一种数据, 名称可以是字符, 值是数字.

- 例子, 建立一个存贮城市名称的映射, 所以名称和值都是字符串:

  ```java
  import java.util.HashMap; //要引入这个功能
  
  public class Main {
    public static void main(String[] args) {
  	HashMap<String, String> cities = new HashMap<String, String>();
  	
        // Add keys and values (Country, City)
      cities.put("England", "London");
      cities.put("Germany", "Berlin");
      cities.put("Norway", "Oslo");
      cities.put("USA", "Washington DC");
      System.out.println(cities);
        
    	}
  }
  ```

  - 注意增加元素用的是put()函数.
  
- 要访问元素用get("England")函数, 括号中要输入请求的数据名称, 删除也是同理, remove()函数括号中要输入数据名称, String也一样要用引号. 只有删除所有元素的clear()函数不需要参数.

- 同样也可以用size()函数返回元素对数量.

- 同样可以用for-each 循环历遍所有元素, 但是有两个函数, 一个是历遍名称, 一个是历遍值. 示例:

  ```java
  for (String i : cities.keySet()) {
    System.out.println(i);
  }
  
  for (String i : cities.values()) {
    System.out.println(i);
  }
  ```

  - 如果需要两个都显示, 用下面的方法:

  ```java
  for (String i : cities.keySet()) {
    System.out.println("key: " + i + " value: " + cities.get(i));
  }
  ```

- 和前面一样, HashMap 存储的都是对象, 要使用其他数据格式, 要使用 wrapper class, 例如Integer Boolean Character 和Double. 建立HashMap的语句: ` HashMap<String, Integer> people = new HashMap<String, Integer>();`

#### HashSet 哈希集合

- 哈希集合也是一种类似的数据, 但是就像数学中的集合一样, 里面的元素是不能重复的. 同样需要 ` java.util.HashSet`这个包. 创建语句为: ` HashSet<String> cars = new HashSet<String>();` 这个语句创建了一个存储String的HashSet.

- Hash Set里面不存在重复的元素, 所以不需要数字或者名称来定位元素, 直接写出元素的值就可以定位. 

- 要增加元素直接用add()函数即可.

- 由于不存在重复元素, 所以提供了函数来检查某个元素是否存在: contains(), 他的返回值是boolean的. 其实前面的几个列表数据格式也有这个函数.

- remove()函数也可以直接删除, 不需要提供序号. 同样有clear()函数. 也可以查看大小size().

- 也可以历遍所有的元素, 写法都一样.

- 也可以存储各种数据, 也要用wrapper class.

  示范代码:

  ```java
  import java.util.HashSet;
  
  public class Main {
    public static void main(String[] args) {
          HashSet<String> car = new HashSet<String>();
          car.add("Volvo");
          car.add("BMW");
          car.add("Ford");
          car.add("BMW");
          car.add("Mazda");
  
          System.out.println(car.contains("Mazda"));//检查是否存在元素
          for (String i : car) {
              System.out.println(i);
          }
    }
  }
  ```

#### Iterator 历遍 class

- Iterator class 也是 ` java.util`里面的一个class, 他的意思就是历遍的意思. 用来历遍ArrayList 或者HashSet之类的东西. 列表类的的对象都内置了一个函数 iterator(), 他的返回值就是所有列表里面的对象. 返回的对象们就要用iterator 对象来接收, 以前面的 ArrayList 为例, 代码:

  ```java
  Iterator<String> it = cars.iterator();
  ```

  - 这个新建的 it 对象有一些函数来处理这些元素.

  - next()会返回第一个__对象__.  也就是说如果返回的数据是整数的话, 要用 Integer 对象来接收, 而不是 int. 代码: ` System.out.println(it.next());`, 但是重复执行这个代码会输出下一个元素, 并不会输出同一个元素.

  - 函数hasNext()会返回一个boolean值指示是否还存在下一个元素, 用这个就可以历遍所有的元素, 代码:

    ```java
    while(it.hasNext()) {
      System.out.println(it.next());
    }
    ```

- Iterator的特性可以让他在历遍时删除特定的元素, 代码:

  ``` java
  import java.util.ArrayList;
  import java.util.Iterator;
  
  public class Main {
    public static void main(String[] args) {
      ArrayList<Integer> numbers = new ArrayList<Integer>();
      numbers.add(12);
      numbers.add(8);
      numbers.add(2);
      numbers.add(23);
      Iterator<Integer> it = numbers.iterator();
      while(it.hasNext()) {
        Integer i = it.next();
        if(i < 10) {
          it.remove();
        }
      }
      System.out.println(numbers);
    }
  }
  ```

  - 在这个例子里面, 如果用for loop或者for-each loop 去删除小于10 的整数是不能正常运行的, 因为for-each loop 需要的历遍源必须是一个阵列(array), iterator并不是array, 而是一个单独的类型. 要使用for loop来历遍这个iterator, IDE给出的方法是这样:

    ```java
            for (Iterator<Integer> iterator = iter; iterator.hasNext(); ) {
                Integer i = iterator.next();
            }
    ```

    这样看起来并不简洁, 阅读也不如上一种容易.

#### Java wrapper classes 包装类

- 包装类的意义是要把基础的数据类型变成对象类型来使用. 用法就是将类型的第一个字母大写, 除了 int 变成 Integer, 还有 char 变成 Character.

- 有时必须要使用包装类型, 因为有些函数只接受对象, 不能接受纯数据. 例如集合类的对象, ArrayLiat, HashSet等等, 他们只能储存对象.

- 包装类对象的定义方法和基础类型是一样的, 要获取他们的数据, 使用方法也是一样的. 例如:

  ```java
      Integer myInt = 5;
      Double myDouble = 5.99; //这里不需要d在结尾.
      Character myChar = 'A';
      System.out.println(myInt);
      System.out.println(myDouble);
      System.out.println(myChar);
  ```

- 这些毕竟是对象不是数据, 所以也自带了一些函数. 可以使用这些函数去获取他们的值: `intValue()`, `byteValue()`, `shortValue()`, `longValue()`,  `floatValue()`, `doubleValue()`, `charValue()`, ` booleanValue()`

- 还有个很有用的函数是 `toString()`函数, 可以让输出的数据转成字符串, 比较常用. 代码: ` Integer myInt= 10000;  myInt.toString();`

#### Java的异常处理 exception

- 程序运行是会出错的, 常见的错误就是输入类型错误. 需要整数的时候输入了字母就会出错.

- try and catch 语句就是为了处理这种情况的.  分为两块代码, 一块正常处理, 一块处理异常, Exception e 就是异常的信息存储的对象. 例如:

  ```java
  try{
      //正常的代码
        int[] myNumbers = {1, 2, 3};
        System.out.println(myNumbers[10]);
  }
  catch(Exception e) {
      //出错的解决方法
      System.out.println("Something went wrong.");
  } finally{
      System.out.println("The 'try catch' is finished.");
  }
  ```

  - 以上代码不用 try - catch 语句会报错, 因为阵列不存在10号元素, 使用了异常处理语句就会输出一个错误信息.
  - 然后还可以加入 ` finally`语句, 意思是不管是否出错, 都执行里面的语句.

- 对于输入的错误处理, 当读取输入时经常会输错信息, 例如需要数字而输入了字母, 在catch语句中的写法如下:

  ```java
  import java.util.Scanner;
  public class Main {
  
      public static void main(String[] args) {
          // input handle
          Scanner myObj = new Scanner(System.in);
          System.out.println("Enter name, age and salary:");
          String name = myObj.nextLine();
          int age = 0;
          try { //Exception handle
              age = myObj.nextInt();
          } catch (Exception e) {
              myObj.next();//empty the input buffer
              System.out.println("This is not a whole number. Try again:");
              age = myObj.nextInt();
          }
          double salary = 0;
          try {
              salary = myObj.nextDouble();
          } catch (Exception e) {
              myObj.next();//empty the input buffer
              System.out.println("This is not a number. Try again:");
              salary = myObj.nextDouble();
          }
          System.out.println("Name: " + name);
          System.out.println("Age: " + age);
          System.out.println("Salary: " + salary);
      }
  }
  ```

  - 以上代码就是要求输入名字, 年龄(整数), 工资(浮点数). 名字可以任意输入, 年龄如果输了小数或者字母, 就会执行catch语句, 其中有一句 ` myObj.next();`作用是清空输入缓冲区, 不然后面的再次读取输入就会读取到先前的输入而再次产生错误.

- throw 关键词可以生成你想要输出的错误信息. 错误信息有很多种, 例如 `ArithmeticException`,  `FileNotFoundException`, `ArrayIndexOutOfBoundsException`, `SecurityException`等. 使用throw时要提供错误种类和自订的错误信息. 代码:

  ```java
  public class Main {
    static void checkAge(int age) {
      if (age < 18) {
        throw new ArithmeticException("Access denied - You must be at least 18 years old.");
      }
    }
  }
  ```

#### Java  Regular Expressions 正则表达式

- 正则表达式是用类似通配符的形式来表达一些文字的通用形式, 在搜索, 替换字符时很常用.  缩写为regex.

- 正则表达式可以是一个字符, 也可以多个字符组合来表达复杂的意思.

- 使用正则表达式要引入 ` java.util.regex`包. 里面包含三个class:

  - Pattern class, 用来定义一个样式
  - Matcher class, 用来根据样式寻找文字.
  - PatternSyntaxException class, 用来产生错误信息.

- 例子, 检查一个句子里是否含有特定的字符串"year": 

  ```java
  import java.util.regex.Matcher;
  import java.util.regex.Pattern;
  
  public class Main {
    public static void main(String[] args) {
      Pattern pattern = Pattern.compile("year", Pattern.CASE_INSENSITIVE);
      Matcher matcher = pattern.matcher("1963 is a very good year!");
      boolean matchFound = matcher.find();
      if(matchFound) {
        System.out.println("Match found");
      } else {
        System.out.println("Match not found");
      }
    }
  }
  ```

  - 函数 ` Pattern.compile()`用来创建一个新的正则表达式, 第一个参数是表达式, 第二个参数是可选的, 意思为是不区分大小写. 不写的话默认区分大小写. 这个地方还可以传入其他的flag, 常见的有` Pattern.LITERAL`,  特殊字符当作字符串的一部分, 而不是当作通配符或者控制符来解释. ` Pattern.UNICODE_CASE`  这个flag和 ` CASE_INSENSITIVE`一起用, 就可以忽略其他非用于字母的大小写.
  - 函数 ` matcher()`用来搜索一个字符串是否包含指定的表达式内容, 他的返回值并不是一个boolean类型, 而是一个Matcher 对象, 里面不仅包含了搜索的结果的真假, 还有一些其他的信息.
  - 搜索结果的真假存放在 Matcher对象的find()函数中, 调用这个函数就会返回boolean值告诉你搜索是否成功.

- 正则表达式的写法: 注意全部要由双引号包裹.

  - [abc] 含有abc任意一个字符就可以匹配成功
  - [^abc] 不含有abc中的任意一个就能匹配成功
  - [0-9] 含有0-9的任意一个就能匹配成功

- 通配符:

  - | 用来分隔多个正则表达式, 只要有一个匹配成功就算是匹配成功
  - .  匹配任意单个字符, 除了控制符号
  - ^ 表示开头, ^hello 表达式意思就是以hello开头, 注意要和方括号里面的用法区分开
  - $ 表示结尾, id$, 意思就是以id结尾
  - \d 表示一位数字
  - \s 表示一个空格
  - \b 表示开头或者结尾, 比如 \bnew 就表示 ^new 或者new$
  - \uxxxxx 表示unicode的十六进制编码

- 数量符号

  - n+ 任意数量的字符串, 最起码含有一个n
  - n* 任意数量的字符串, 含有0个或以上n
  - n? 任意数量的字符串, 含有0 个或1一个n
  - n{x} 任意数量的字符串, 含有连续的x个n
  - n{x,y} 任意数量的字符串, 含有连续的 x-y个n
  - n{x,} 任意数量的字符串, 含有最少x个连续的n

- \ 转义符, 如果要在字符中区别控制符和特殊字符本身, 要使用转义符. 例如要搜索\ 符号, 就要用\\ \表示, 不然就会引起歧义, 电脑以为\后面是控制符. 

#### Java threads 线程

- 线程可以让程序同时进行多项任务, 更加有效率

- 线程可以把复杂的任务放在后台而不用打断主程序.

- 要创建一个线程, 可以继承(extend) 一个Thread class 然后 改写他的run()函数

- 第二种方法创建线程 是 实现(implement)一个 Runnable 的界面(interface)

  两种方法的代码如下:

  ```java
  //第一种方法
  public class Main extends Thread {
    public void run() {
      System.out.println("This code is running in a thread");
    }
  }
  //第二种方法
  public class Main implements Runnable {
    public void run() {
      System.out.println("This code is running in a thread");
    }
  }
  ```

- 运行一个线程的方式也有两种:

  ```java
  MyThread.java
  public class MyThread extends Thread {
      public void run() {
          System.out.println("This code is running in a thread");
      }
  }
  
  class ThreadRun implements Runnable {
      public void run() {
          System.out.println("This code is running in a thread");
      }
  }
  ```

  ```java
  Main.java  
  package com.company;
  
  public class Main {
  
      public static void main(String[] args) {
  		// 第一种, 建立新的thread 对象
          MyThread thread = new MyThread();
          thread.start();
          System.out.println("This code is outside of the thread.");
          // 第二种, 实现runnable接口, 然后将这个接口传入一个空的thread. use the thread from runnable interface
          ThreadRun obj = new ThreadRun();
          Thread threadRun = new Thread(obj);
          threadRun.start();
          System.out.println("This code is outside of the thread");
      }
  }
  ```

  - 这两种本质是一样的, 因为Java的thread class本来就是runnable的一个实现.
  - 运行的结果来看, 线程是后台运行, 优先级似乎比较低, 在主程序输出了以后, 线程才运行.
  - 这两种方法的区别在于, 如果是实现Runnable接口的方式,  你还可以从别的class继承代码, 而如果你是继承Theard class, 你就不能再继承别的class了, 因为Java不允许多重继承. 但是接口类允许在继承一次.

- 同时读写的问题(concurrency problem). 因为系统如果同时运行多个线程, 无从得知哪个代码会先运行, 如果线程和主程序在同时写入同一个变量, 结果是无法预料的. 例如下面的这个例子:

  ```java
  public class Main extends Thread {
    public static int amount = 0;
  
    public static void main(String[] args) {
      Main thread = new Main();
      thread.start();
      System.out.println(amount);
      amount++;
      System.out.println(amount);
    }
  
    public void run() {
      amount++;
    }
  }
  ```

  - 这个例子在我的环境下运行的结果是, 两次输出分别输出了 0 和 1 . 也就是说输出的时候只有主线程在运行, 后台线程的运行结果根本没有反映出来. 加上后台线程的运算结果应该是2才对. 但是不同环境下这个结果可能不一样.
  - 为了避免这种无法预料的情况, 应该尽可能避免在不同线程之间共享变量. 如果一定要共用变量, 一种方式是用线程的 ` isAlive()` 函数检查线程是不是已经运行完毕, 运行完毕以后再读写.

#### Java Lambda expression `λ`表达式(匿名函数)

- λ 表达式就是一段代码, 可以输入参数, 也可以返回数据, 跟函数很相似, 但是他们不需要命名, 所以也叫匿名函数. 他们也可以在其他的函数中间使用.
- 最简单的 λ 表达式是 `parameter -> expression `, 如果有多个参数, 用括号包裹: ` (parameter1, parameter2) -> expression`
- 表达式本身有一些限制, 他们必须能马上返回数据, 而不能包含变量, 也不能赋值, 也不能用条件语句和循环. 如果语句比较多也可以用大括号, 如果有输出, 那么必须要有 return 语句. (因为在定义的适合不需要指定返回类型)


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
  
  - 我已经大概了解工程的构建方式, 就是按照某个模板建立一些文件夹, 然后去pom文件里面设置好一系列参数, 写好源代码文件, 然后用Maven命令打包导出 jar 压缩包就可以在任意电脑上执行了.  我第一次运行不成功是因为pom.xml中没有加入<build>字段, 加入构建模板就好了.
  
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
  
  - 网址的写法, 根目录("/")就是localhost, 或者ip地址或者域名, 后面的网址就是shoppinglist, 在代码中定义过了. 网址后面跟参数的写法就是加"?"问号, 然后用字符串对的形式给出参数, 用等号来表示一对属性参数, 多个参数用"&"隔开. 例如 ` ?date=1/21/2020&item=bread`
  
- 这样我们的web程序就成功运行了. 程序由三个部分组成, 分别对应三个class, 数据库, 具体的程序功能, 主程序负责调用功能, 我们甚至不需要在主程序中写其他的模块在哪里, 就会由spring boot自动扫描出来. 自动扫描的前提是主程序和模块要在同一级或同一个package里面, 模块可以放入子目录, 但是不能放在主程序的上一级目录.

- 接下来是程序的打包发布, 如果这时候在IDE右侧的Maven标签页中直接执行package命令, 就在target文件夹下会生成一个jar包文件, 如果在命令行下面试图部署的话:  `java -jar *.jar`, 会出错. 错误是jar中没有主清单属性. 因为我们的pom.xml文件少了一个打包的工具, 在pom.xml中加入这个包即可:

  ```xml
  <build>
      <plugins>
          <plugin>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-maven-plugin</artifactId>
          </plugin>
      </plugins>
  </build>
  ```

  然后再一次运行mvn package, 就会生成一个jar包, 比刚才的体积要大得多. 然后将jar文件放置到任意电脑上, 只要安装了java 运行环境 JRE 或者JDK, 使用命令 ` java -jar *.jar`就可以部署这个网络服务. 因为java的特性, 无需为了不同的操作系统编译单独的版本, 一个jar包可以运行在几乎所有的平台. 在我的例子中, 我用Windows下的IntelliJ IDEA打包好jar文件, 使用JDK版本为11, 将jar文件 copy到Linux的电脑, 使用JRE版本为17去运行这个jar包, 也可以成功运行.

