# React 简介

- React是一个node.js的框架, 所以要依赖node的一系列功能, 在此基础上开发.
- 所以首先要安装node. (一般都是在Linux环境下进行开发.)
- 跟node类似, 要新建一个文件夹来存放React的工程.
- 进入这个目录以后使用命令`npx create-next-app`来初始化, 会自动安装一些基本的库, 例如React, TypeScript, TailwindCSS等常用的, 都可以一次性配置好. 其中这个next是一个React的框架, 可以用来开发前端和后端的程序. 根据提示输入即可.
- 创建安装结束以后就可以在文件夹中看到一个node程序的目录结构, 其中有`package.json`这个是项目的所有配置信息, 在部署到新电脑上以后, 使用命令`npm i`就会根据这个文件安装所有的依赖程序. `tsconfig.json`是typescript的设置文件.
- 然后就使用VS Code来打开文件夹, 可以在里面修改代码.
- 要运行这个程序, 在命令行进入项目的目录, 使用命令`npm run dev`, 第一次运行会进行编译. 将程序代码变成html语言, 会需要一定的时间, 取决于项目大小. 然后在修改保存代码以后会自动应用, 无需重新加载(刷新). 需要关闭网页服务或者退出可以在terminal使用ctrl+c或者关闭terminal.

# React的第一个简单项目

- 使用上面的方式新建好一个项目以后, 文件夹内会有app目录, 里面存放了所有的app内容, 如果需要添加其他的元件可以另外新建文件夹叫做`component`. app文件夹内有`layout.tsx`为默认的页面模板, `page.tsx`为第一个页面. 后续的页面需要自己添加. 除了app文件夹, 还有public文件夹可以存放一些静态内容. 根目录下还有一些项目的配置文件.

- 要开始写第一个页面, 可以在app目录中的`page.tsx`直接写即可. 也可以建立其他的文件或子目录, 以后可以学习如果把他们加入到模板里面来渲染.

- tsx文件一般用来写React和Typescript结合的文件, ts就是Typescript文件.

- React里面大部分页面元素都是以组件(component)的形式写的, 一个组件一般是一个函数. 称作 function based component. 这也是官方推荐的写作方式. 组件化的形式更方便重复调用. 相对应的就是用对象或者类(class)的形式写的组件, 称作class component, 现在基本淘汰了, 只存在于一些老的项目中.

- 在项目根目录中新建文件夹`components`, 在里面新建一个文件, 命名为`Hello World.tsx`, 代码如下:

  ```tsx
  function Hello() {
    return (
      <div>
        <h1>Hello World!</h1>
      </div>
    );
  }
  export default Hello;
  ```

  - 注意, React的函数都要使用帕斯卡命名法, 即每一个单词的首字母要大写.
  - 可以看到一个组件就是一个函数. 其中返回的内容就是html代码. 但是只能返回单个元素, 如果需要多个元素, 就要用容器包裹. 这个语法叫做 JSX (JavaSrcipt XML). 虽然这里看起来是html语言, 但实际会被编译成Javascript来执行.
  - 最后将函数导出为默认函数, 并且命名. 这样就可以在主页面调用.

- 然后在主页面文件(`page.tsx`)写入下面的代码, 可以将原来的任意代码删掉.

  ```tsx
  import Hello from "@/components/Hello_World"
  
  export default function Home() {
  
    return (
      <div>
  		<Hello />
      </div>
    );
  }
  ```

  - 第一句的意思就是导入刚才写好的组件.
  - 然后在terminal里面进入项目文件夹, 使用命令`npm run dev`开始运行服务器程序. 如果页面显示出来`Hello World!`就说明执行是成功的.

- 接下来在这个组件中加入动态内容(使用变量). 将`Hello_World.tsx`的内容修改成这样

  ```tsx
  function Hello() {
    const name = "John";
    return (
      <div>
        <h1>Hello {name}!</h1>
      </div>
    );
  }
  export default Hello;
  ```

  - 保存以后应该可以看到页面已经变化了, 变成了`Hello John!`. 这里的语法就是在字符串中使用变量或者Javascript代码就用大括号将其包裹.

- 然后加入条件渲染语句, 更贴近现实中的情况, 有时用户名是存在的, 有时用户名是未知的, 在用户名未知的时候就要显示默认语句. 这时就要使用if语句. 代码如下.

  ```tsx
  function Hello() {
    const name = "";
    if (name) {
      return <h1 className="text-4xl">Hello {name}!</h1>;
    }
    return <h1 className="text-4xl">Hello World!</h1>;
  }
  export default Hello;
  ```

  - 这时候name变量内容为空, 就会显示`Hello World!`, 如果赋予名字一个任意字符串, 就会显示出来这个名字, 实现了条件语句.

- 至此, 第一个Hello World 项目就完成了.

