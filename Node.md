# 什么是Node
1. <font style="background-color:rgb(100.000000%, 100.000000%, 100.000000%);">Node.js是一个基于V8 JavaScript引擎的JavaScript运行时环境。 </font>
2. Node.js和浏览器的差异：![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1711612240148-350000ef-b5de-47b8-b214-fcc9803e4b0d.png)
3. Node.js架构：<font style="background-color:rgb(100.000000%, 100.000000%, 100.000000%);">		</font>

<font style="background-color:rgb(100.000000%, 100.000000%, 100.000000%);">我们编写的JavaScript代码会经过V8引擎，再通过Node.js的Bindings，将任务放到Libuv的事件循环中; Libuv是使用C语言编写的库，其提供了事件循环，文件系统读写，网络IO，线程池等等内容</font>

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1711612291707-728327d8-ce23-4b3a-9436-7dee95d8e7e8.png)

4. Node.js的REPL：REPL是read-eval-print Loop的简称，翻译为读取-求值-输出 循环，REPL是一个简单的，交互式的编程环境

# node的输出
+ console.log：最常用的输入内容的方式
+ console.clear：清空控制台
+ console.trace：打印函数的调用战

# 全局对象
## 特殊的全局对象
在模块中任意使用，但是在命令行交互中是不可以使用的。

__dirname、__filename、exports、module、require()

+ __dirName：获取当前文件所在的路径，不包括后面的文件名
+ __fileName：获取当前文件所在的路径和文件名称，包括后面的文件名称

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1712130934633-82784fbd-9afb-43cd-a0ff-4199841b26c6.png)

## 常见的全局对象
+ process对象：process提供了node进程中相关的信息，比如node的运行环境，参数信息等
+ console对象：提供了简单的调试控制台
+ 定时器函数：
    - setTimeout(callback,delay[, ...args])：callback在delay毫秒后执行一次
    - setInterval(callback,delay[, ...args])：callback每delay毫秒重复执行一次
    - setImmediate(callback[, ...args])：callback I/O事件后的回调的立即执行
    - process.nextTick(callback[, ...args])：添加到下一次tick队列中

# 模块化
## commonjs
1. 核心变量：exports，module.exports，require
2. commonjs中是没有module.exports的概念的，为了实现模块的导出，node中使用的是module的类，每一个模块都是Module的一个实例，也就是module，所以在node中真正导出的不是exports而是module.exports，**module.exports = exports **
3. require是一个函数，帮助我们引入一个文件（模块）中导入的对象。导入格式为require(X)。其查找规则：
    1. X是一个核心模块，比如path，http：直接返回核心模块，并且停止查找
    2. X是以./或../或/（根目录）开始的：证明查找的是本地文件
        1. 将X当作一个文件在对应的目录下查找：
            + 如果有后缀名，则按照后缀名的格式查找对应的文件
            + 如果没有后缀名，会按照以下顺序：
                1. 直接查找文件X
                2. 查找X.js文件
                3. 查找X.json文件
                4. 查找X.node文件
        2. 没有找到对应的文件，将X作为一个目录，查找目录下面的index文件
            + 查找X/index.js文件
            + 查找X/index.json文件
            + 查找X/index.node文件
        3. 如果没有找到，则报错：not found
    3. 直接是一个X（没有路径），并且X不是一个核心模块

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1712483228519-4223adbd-9060-4a64-844a-1ad5bd813e45.png)

### commonjs缺点
+ commonjs加载模块是同步的：只有等到对应的模块加载完毕，当前模块中的内容才能被运行，这个在服务器不会有什么问题，因为服务器加载的js文件都是本地文件，加载速度非常快，**但是将commonjs应用于浏览器的话，浏览器加载js文件需要先从服务器将文件下载下来，之后再加载运行，采用同步的话就意味着后续的js代码都无法正常运行，即使是一些简单的dom操作，所以在浏览器中，通常不实用commonjs规范（可以结合webpack使用）**

### 模块加载过程
+ 模块在被第一次引入时，模块中的js代码会被运行一次；
+ 模块被多次引入时，会缓存，最终只加载（运行）一次；
+ 如果有循环引入的话，，引用关系可以看成是一种图结构，图结构在遍历的过程中有深度优先搜索和广度优先搜索两种方式，node采用的是深度优先算法

下图所示的加载顺序为main -> aaa -> ccc -> ddd -> eee -> bbb

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1712545765547-450d9dde-c7bc-40b7-9dae-8f4fb3df532a.png)

## AMD（Asynchronous Module Definition）异步模块定义
### require.js
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1712557731707-3a8a3d8a-a26a-428c-a706-3cc64ddd8139.png)

## CMD（Common Module Definition）通用模块定义
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1712557929613-9c2f71ab-0023-4458-8b1d-2510b5c44341.png)

## ESmodule
### 加载过程
esmodule家在js文件的过程是编译时加载的，并且是异步的，也就是说设置了type="module"的代码，相当于在script标签上也加上了async属性

### 与commonjs的不同：
+ 使用了import和export关键字
+ 采用编译期的静态分析，并且也加入了动态引用的方式

### export关键字——三种导出方式
1. 直接在语句声明的前面直接加上export关键字

```javascript
export const name = "tyx";
export const age = 18;
export const sayHello = funciton (name) {
  console.log("Hello");
}
```

2. 将所有需要导出的标识符，放到export后面的{}中

这里的{}里面不是ES6的对象字面量的增强写法，不是对象

```javascript
export {
  name,
  age,
  sexy
}
```

3. 导出时给标识符起一个别名

### import关键字——三种导入方式
1. import {标识符列表} from '模块'

```javascript
import { age, name, sayHello } from './foo.js';
```

2. 导入时给标识符起别名

```javascript
import { name as myName } from './foo.js';
```

3. 通过*将模块功能放到一个模块功能对象上

### export和import结合使用
```javascript
export { sum as barSum } from './foo.js';
```

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1712741844426-ce3ecc0e-af76-467a-97be-95c80deebf9a.png)

## default用法
默认导出export时不需要指定名字，在导入时不需要使用{}并且可以自己指定名字，在一个模块中，只能有一个默认导出

## import函数
通过import加载一个模块，是不可以将其放到逻辑代码中的，是因为esmodule在被js引擎解析的时候，就必须知道它的依赖关系。如果我们希望动态来选择加载模块的路径，则需要使用import()函数来动态加载。import函数会返回一个promise

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713171555158-72b09a48-e9b8-4000-a01b-ded03282d162.png)

# 内置模块
## path——处理文件和路径
在MacOS、Linux、和windows上的路径是不一样的

+ window上会使用\或者\\来作为文件路径的分隔符，目前也支持/
+ 在Mac OS、Linux的Unix操作系统上使用/来作为文件路径的分隔符
+ 为了屏蔽windows和MacOS或Linux上的差异，我们可以使用path模块

常见的API：

+ 从路径中获取信息：
    - dirname：获取文件的父文件夹
    - basename：获取文件名
    - extname：获取文件扩展名

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713235604637-1cdc2957-3f22-436d-b289-15912b5d4a94.png)

+ path.join：路径的拼接，如果我们希望将多个路径进行拼接，但是不同的操作系统可能是用的是不同的分隔符，这个时候可以使用path.join函数![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713236280106-ee450a5b-3c05-4e47-aafb-4e452fb2a6a3.png)
+ path.resolve：如果我们希望将某个文件和文件夹拼接，可以使用path.resolve，resolve函数会判断我们拼接的路径前面是否有/或./或../，如果有则表示是一个绝对路径，会返回对应的拼接路径，如果没有，那么回合当前执行文件所在的文件夹进行路径的拼接![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713249109212-597ff77d-15a2-41f2-89e8-27f169d6cd71.png)

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713249242635-3cd6e4df-bbf7-4d7f-9a96-e0d48e888ad8.png)

## fs——文件系统
借助于node帮我们封装的文件系统，我们可以在任何的操作系统windows、macos、linux上面直接去操作文件

fs文件系统的API非常多，但是这些API大多数都提供三种操作方式：

1. 同步操作文件：代码会被阻塞，不会继续执行；
2. 异步回调函数操作文件：代码不会被阻塞，需要传入回调函数，当获取到结果时，回调函数被执行
3. 异步promise操作文件：代码不会被阻塞，通过fs.promises调用方法操作，会返回一个Promise，可以通过then、catch进行处理

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713253862126-2ab91a65-b480-49b1-b4cb-6c6316ffdb20.png)

### 文件描述符
在POSIX系统上，对于每个进程，内核都维护者一张当前打开着的文件和资源的表哥，每个打开的文件都分配了一个称为文件描述符的简单的数字标识符。在系统层，所有文件系统操作都使用这些文件描述符来标识和跟踪每个特定的文件，windows系统使用了一个虽然不同但是类似的机制来跟踪资源。**为了简化用户的工作，node.js抽象出操作系统之间的特定差异，并为所有打开的文件分配一个数字型的文件描述符**

+ fs.open()方法用于分配新的文件描述符：一旦被分配，则文件描述符可用于从文件读取数据，向文件写入数据，或请求关于文件的信息

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713254658391-8d5fd479-5914-4214-9f94-db3784e15a53.png)

### 文件的读写
+ fs.readFile(path[,options],callback)：读取文件的内容
+ fs.writeFile(file,data[,options],callback)：在文件中写入内容；

options中可以传入两个参数：flag  encoding

+ flag：[https://nodejs.org/dist/latest-v14.x/docs/api/fs.html#fs_file_system_flags](https://nodejs.org/dist/latest-v14.x/docs/api/fs.html#fs_file_system_flags)

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713255109219-60b9188c-50e2-4e04-874c-3a92819df2c1.png)

+ [encoding](https://www.jianshu.com/p/899e749be47c)：如果不填写encoding，返回的结果是Buffer。**目前基本上用的就是UTF-8**

### 文件夹操作
+ fs.mkdir()或fs.mkdirSync()：创建一个文件夹

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713256446300-a6db1601-a5ff-44b1-a634-4591c9d82e49.png)

+ 读取文件夹中的所有文件

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713256563433-156d940c-a06f-4db6-b222-a56490edfd39.png)

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713257117904-31d1a7b2-43a9-499c-814b-261ae910e8db.png)

+ 文件重命名：

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713257188432-759aa6d6-9c5f-4eb5-9d6b-8ff9193fb2b3.png)

+ 文件夹复制

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713257942583-bcdde4b5-e9a4-473e-ad62-e18f753b8f81.png)

## events模块
Node中的核心API都是基于异步事件驱动的，在这个体系中，某些对象（发射器（Emmitters））发出某一个事件，我们可以监听这个事件（监听器Listeners），并且传入的回调函数，这个回调函数会在监听到事件时调用

发出事件和监听事件都是通过EventEmmitter类来完成的，他们都属于events对象

+ emitter.on(eventName, listener)：监听事件，也可以使用addListener
+ emitter.off(eventName, listener)：移除事件监听，也可以使用removeListener
+ emitter.emit(eventName[,args])：发出事件，可以携带一些参数

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713259063722-d03d3e5b-762a-4c7b-ae4e-23a9f8752030.png)![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713259108698-9544a035-156c-4bd3-b950-7ce5fcc48192.png)

+ emitter.eventNames()：返回当前EventEmitter对象注册的事件字符串数组
+ emitter.getMaxListeners()：返回当前EventEmitter对象的最大监听器数量，可以通过setMaxListeners()来修改，默认是10
+ emitter.listenerCount(事件名称)：返回当前EventEmitter对象某一个事件名称，监听器的个数
+ emitter.listerners(事件名称)：返回当前EventEmitter对象某个事件监听器上所有的监听器数组
+ emitter.once(eventName, listener)：事件监听一次
+ emitter.prependListener()：将监听事件添加到最前面
+ emitter.prependOnceListener()：将监听事件添加到最前面，但是只监听一次
+ emitter.removeAllListeners([eventName])：移除所有的监听器

# npm install
## [命令](https://docs.npmjs.com/cli/v10/commands)
+ 全局安装：npm install yarn -g

全局安装一般用于安装一些工具包：yarn webpack等而不是axios，express等库文件

+ 局部安装（项目安装）：npm install

局部安装分为开发时依赖和生产时依赖：

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1714983988199-d7cb977e-b586-4e31-92de-07fbea304b08.png)

+ 强制重新build：npm rebuild
+ 清除缓存：npm cache clean
+ 获取缓存路径：npm config get cache
+ 卸载某个依赖包：

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1714986367272-ce444940-6277-4182-96b3-1398dd9f8a30.png)

+ 查看npm镜像：npm config get registry
+ 设置npm镜像：npm config set registry https://registry.npm.taobao.org

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1714988030827-df09625b-806d-4b71-8df7-e024ef829f93.png)

## 原理
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1713337767904-3cf6b02e-63f7-41f8-bfa4-13efce6eb51f.png)

npm install会检测是有package-lock.json文件：

+ 如果没有loack文件
    - 分析依赖关系：<font style="background-color:rgb(100.000000%, 100.000000%, 100.000000%);">这是因为我们可能包会依赖其他的包，并且多个包之间会产生相同依赖的情况; </font>
    - <font style="background-color:rgb(100.000000%, 100.000000%, 100.000000%);">从registry仓库中下载压缩包（如果我们设置了镜像，那么会从奖项服务器下载压缩包）；</font>
    - <font style="background-color:rgb(100.000000%, 100.000000%, 100.000000%);">获取到压缩包后会对压缩包进行缓存（从npm5开始有的）</font>
    - <font style="background-color:rgb(100.000000%, 100.000000%, 100.000000%);">将压缩包解压到项目的node_modules文件夹中</font>
+ <font style="background-color:rgb(100.000000%, 100.000000%, 100.000000%);">有lock文件</font>
    - <font style="background-color:rgb(100.000000%, 100.000000%, 100.000000%);">检测loack中包的版本是否和package.json中一致，会按照semver版本规范检测</font>
        * <font style="background-color:rgb(100.000000%, 100.000000%, 100.000000%);">如果不一致，则会重新构建依赖关系，直接走顶层的流程</font>
        * <font style="background-color:rgb(100.000000%, 100.000000%, 100.000000%);">一致的话，会优先查找缓存，</font>
            + <font style="background-color:rgb(100.000000%, 100.000000%, 100.000000%);">没有找到：会从registry残酷下载，直接走顶层流程</font>
            + <font style="background-color:rgb(100.000000%, 100.000000%, 100.000000%);">找到：获取缓存中的压缩文件，并且将压缩文件解压到node_modules文件夹中</font>

## <font style="background-color:rgb(100.000000%, 100.000000%, 100.000000%);">package.json</font>
+ name：项目名称
+ version：项目的版本
+ lockfileVersion：loack文件的版本
+ requires：使用requires来跟着模块的依赖关系
+ dependencies：项目的依赖
    - 当前项目依赖axios但是axios依赖follow-redireacts
    - axios中的属性如下：
        * version：表示实际安装的axios版本
        * resolved用来记录下载的地址，registry仓库中的位置
        * requires：记录当前模块的依赖
        * integrity：用来从缓存中获取索引，再通过索引去获取压缩包文件

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1714986275161-69b1b86a-3752-4515-a8f2-8d7c528cd339.png)

# 脚手架工具
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1715672537975-49e29e19-7664-49a3-8fa5-45c63d615a65.png)

[Commander.js](https://github.com/tj/commander.js/blob/master/Readme_zh-CN.md#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1715674680316-54f71329-40eb-49cd-8fcb-d213b0d7ed25.png)

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716197407194-89ac1e96-329a-497a-a1ae-d4605e7948fd.png)

## buffer
### buffer的创建过程
创建buffer时，并不会频繁向操作系统申请内存，会默认先申请一个8*1024个字节大小的内存，也就是8kb

### buffer和二进制
buffer中存储的是二进制数据，我们可以将buffer看成是一个存储二进制的数组，这个数组中的每一项，可以保存8位二进制

> 为什么是8位？
>
> 在计算机中，很少的情况我们会直接操作一位二进制，因为一位二进制存储的数据十分有限，通常会将8位合在一起作为一个单元，称之为一个字节（byte），即1byte=8bit，1kb=1024byte，1M=1024kb
>

### buffer和字符串
buffer相当于是一个字节的数组，数组中的每一项对应一个字节的大小

### buffer和文件读取
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716434271957-49b35829-2ce6-4398-8aae-4863e3949b06.png)

# stream
流：连续字节的一种表现形式和抽象概念，流应该是可读并且可写的

node中很多对象是基于流实现的，所有的流都是eventEmitter的实例：

+ http模块的request和response对象
+ process.stdout对象

Node.js中有四种基本流类型：

+ writable：可以向其写入数据的流
+ readable：可以从中读取数据的流
+ duplex：同时为readable和writable的流
+ transform：duplex可以在写入和读取数据时修改或转换数据的流

eg1：

readable：当我们一次性将一个文件中所有的内容都读取到程序中可能会出现文件过大、读取的位置、结束的位置、一次读取的大小等问题

我们可以使用createReadStream(start, end, hignWaterMark)

+ start: 文件读取开始的位置
+ end：文件读取结束的位置
+ hignWaterMark：一次性读取字节的长度，默认是64kb

```javascript
const read = fs.createReadStream('./foo.txt',{
  start: 3,
  end: 8,
  highWaterMark: 4
})
read.on("data", (data) => {
  console.log(data);
})
read.on("open", (data) => {
  console.log("文件被打开");
})
read.on("end", (data) => {
  console.log("文件读取结束");
})
read.on("close", (data) => {
  console.log("文件被关闭");
})
read.pause();
setTimeout(() => {
  read.resume();
},2000)
```

pipe方法

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716453246422-0d8841af-b6cb-49b9-8cc7-c584b8cb4eba.png)

# http模块
## web服务器
当应用程序需要某一个资源时，可以向一个台服务器，通过http请求获取到这个资源，提供资源的这个服务器就是一个web服务器

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716533536162-990c2338-abfa-4dd5-8668-b302647fc524.png)

目前有很多开元的web服务器：Nginx、Apache（静态）、Apache Tomcat（静态、动态）、Node.js

## 创建服务器
createServer：

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716534135742-ee788f52-74dc-4946-b15e-c3f3978a8707.png)

http.server底层其实使用直接new Server对象

```javascript
function createServer(opts, requestListener) {
  return new Server(opts, requestListener);
}
```

我们也可以自己来创建这个对象

```javascript
const server2 = new http.server((req, res) => {
  res.end("hello server2");
})
server2.listen(9000, () => {
  console.log("服务器启动成功～")；
})
```

## 监听主机和端口号
listen函数有三个参数：

+ 端口port：可以不传，系统会默认分配端，后续项目中我们会写入到环境变量中
+ 主机host：通常可以穿入localhost、IP地址127.0.0.1、或者ip地址0.0.0.0，默认是0.0.0.0
    - localhost：本质上是一个域名，通常情况下会被解析成127.0.0.1
    - 127.0.0.1：回环地址，表达的意思其实是我们主机自己发出去的包，直接被自己接收

> 正常的数据库包经常  应用层 - 传输层 - 网络层 - 数据链路层 - 物理层
>
> 而回环地址是在网络层直接就被获取到了，是不会经常数据链路层和物理层的
>

    - 0.0.0.0：监听IPV4上所有的地址，再根据端口找到不同的应用程序
+ 回调函数：服务器启动成功时的回调函数

## request对象
### URl的处理
客户端在发送请求时，会请求不同的数据，那么会传入不同的请求地址，服务器需要根据不同的请求地址，作出不同的响应

```javascript
const server = http.createServer((req, res) => {
  const url = req.url;
  if(url === '/login'){
    res.end("welcome back～")；
  }else if(url ==== "/products"){
    res.end("products");
  }else{
    res.end("error message");
  }
})
```

### URL的解析
```javascript
// http://localhost:8000/login?name=why&password=123;
// 这个时候url的值时/login?name=why&password=123;
const parseInfo = url.parse(req.url);
console.log(parseInfo);
const { pathname, query } = url.parse(req.url);
const queryObj = qs.parse(query);
```

### 创建用户接口
获取body携带的数据，我们需要通过监听req的data事件来获取

```javascript
req.setEncoding('utf-8');
req.on('data', (data) => {
  const {username, password} = JSON.parse(data);
})
req.on("end", () => {
  console.log("传输结束");
})
res.end("create user success");
```

## 返回响应结果
如果我们希望给客户端响应的结果数据，可以通过两种方式

+ write方法：这种方式是直接写出数据，但是并没有关闭流
+ end方法：这种方式是写出最后的数据，并且写出后会关闭流

```javascript
// 响应数据的方式有两个
res.write("Hello world");
res.write("Hello Response");
res.end("message end");
```

如果我们没有调用end和close，客户端将会一直等待结果，所以客户端在发送网络请求时，都会设置超时时间

## 返回状态码
设置状态码常见的有两种方式

```javascript
res.statusCode = 400;
res.writeHead(200);
```

## 响应头文件
+ res.setHeader：一次写入一个头部信息
+ res.writeHead：同时写入header和status

```javascript
res.setHeader("Content-Type", "application/json;charset=utf8");

res.writeHead(200, {
  "Content-Type": "application/json;charset=utf8"
})
```

## http请求
axios库可以在浏览器中使用，也可以在Node中使用：

+ 在浏览器中，axios使用的是封装xhr
+ 在Node中，使用的是http内置模块

```javascript
// get请求
http.get("http://localhost:8000", (res) => {
  res.on('data', data => {
    console.log(1);
  })
})
// post请求
const req = http.request({
  method: 'POST',
  hostname: "localhost",
  port: 8000
}, (res) => {
  res.on("data", data => {
    console.log(1);
  })
})
req.end();
```

## 文件上传
**错误示范**

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716866928213-26133d6b-064d-49ff-a6a3-f724a1959a89.png)

**正确做法**

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716867539698-bd62e12c-e003-4aaa-aac7-87476d73bd17.png)

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716867324398-9a1e45ff-290b-4bfa-a82f-1cc669889ce7.png)

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716867344823-44d46076-5cb0-4abf-96b6-cdd76e040e10.png)

# express
> [前端Express框架必学之：Node.js项目搭建与接口开发实战-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/2411958)
>
> [Express 中文网](https://express.nodejs.cn/)
>

**express整个框架的核心就是中间件**

**express应用程序本质上是一系列中间件函数的调用**

**中间件的本质是传递给express的一个回调函数，该回调函数接受三个参数：**

+ 请求对象（request对象）
+ 响应对象（response对象）
+ next函数（在express中定义的用于执行下一个中间件的函数）

中间件的作用：

+ 执行任何代码
+ 更改请求（request）和响应（response）对象
+ 结束请求-响应周期（返回数据）
+ 调用栈中的下一个中间件

如果当前中间件功能没有结束请求-响应周期，则必须调用next()将控制权传递给下一个中间件功能，否则，请求将被挂起。

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716889997222-509b1f3d-c0af-4bca-b932-95cf797f1e65.png)

## 安装
+ 通过express提供的脚手架，直接创建一个应用的骨架
    1. 安装express-generator  

```javascript
npm install -g express-generator
```

    2. 创建项目

```javascript
express demo
```

    3. 安装依赖

```javascript
npm install
```

    4. 启动项目

```javascript
node bin/www
```

+ 从零搭建自己的express应用结构

```javascript
npm init -y
```

## 基本使用
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716889734374-7e7f2ccb-116d-44b3-a65e-59826b7d02c5.png)

## 应用中间件
### 自己编写
express主要提供了两种方式：app/router.use和app/router.methods

```javascript
const express = require('express');
const app = express();

// app.use 示例
app.use((req, res, next) => {
  console.log('这是一个全局中间件');
  next();
});

// app.get 示例
app.get('/hello', (req, res) => {
  res.send('Hello World!');
});

app.listen(3000, () => {
  console.log('服务器启动在 3000 端口');
});
```

### body解析
express有内置一些帮助我们完成对request解析的中间件，registry仓库中也有很多可以辅助我们开发的中间件

在客户端发送post请求时，会将数据放到body中，客户端可以通过json的方式传奇，也可以通过form表单的方式传递

### request body中间件
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716890349975-17b74321-fe34-4cb4-a6c5-1e46cbb107a9.png)

### express提供
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716948944746-d2e78fb8-c5f9-4782-9fc5-6fc962439389.png)

使用express内置的中间件或者使用body-parser来完成

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716890394287-fc118e27-8b74-4c25-9d0a-d5e4aa59e8bb.png)

如果我们解析的是application/x-www-form-urlencoded：

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716890428300-62317af7-7328-4815-919e-7640374302de.png)

### 第三方中间件
我们可以使用express官网开发的第三方库：morgan

```javascript
const loggerWriter = fs.createWriteStream('./log/access.log',{
  flags: 'a+'
})
app.use(morgan('combined', {stream: loggerWriter}));
```

上传文件，我们可以使用express提供的multer来完成

```javascript
const upload = multer({
  dest: "uploads/"
})
app.post('upload',upload.single('file'), (req, res, next) => {
  res.end("文件上传成功～")；
})
```

上传文件时，添加后缀名

```javascript
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, "uploads");
  },
  filename: (req, file, cb) => {
    cb(null, Date.now() + path.extname(file.originalname));
  }
})

const upload = multer({
  storage
})
app.post('/upload', upload.single('file'), (req, res, next) => {
  res.end("文件上传成功～");
})
```

如果我们希望借助multer帮助我们解析一些form-data中的普通数据，那么我们可以使用any：

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716964095562-8083e5ff-fce3-437f-9250-4e7e633d3211.png)

```javascript
app.use(upload.any());
app.use('/login', (req, res, next) => {
  console.log(req.body);
})
```

## 客户端发送请求的方式
1. 通过get请求中的URL的params

```javascript
app.use('/login/:id/:name', (req, res, next) => {
  res.json("请求成功");
})
```

2. 通过get请求中的URL的query

```javascript
app.use('/login', (req, res, next) => {
  res.json("请求成功～");
})
```

3. 通过post请求中的body的json格式
4. 通过post请求中的body中的x-www-form-urlencoded格式
5. 通过post请求中的form-data格式

## 服务器响应数据
+ end方法：类似于http中的response.end方法，用法是一致的
+ json方法：可以传入很多的类型，object、array、string、boolean、number、null等，他们会被转成json格式返回
+ status：用于设置状态码

## 路由
我们可以使用express.Router来创建一个路由处理程序：一个Router实例拥有完整的中间件和路由系统，也被称为mini-app![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716965810006-23719a6e-a234-4781-b2dd-f6d64b4a205d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716965827067-cecd8854-3a26-4a55-aa30-990fef799984.png)

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716967229201-b2ee0ebf-13c3-45ab-9301-65609cece702.png)

## 静态资源服务器
部署静态资源我们可以选择很多方式：node也可以作为静态资源服务器，并且express给我们提供了方便部署静态资源的方法

```javascript
const express = require("express");
const app = express();
app.use(express.static('./build));
app.listen(8000, () => {
 console.log("静态服务器启动成功")
})
```

## 服务端的错误处理
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716968969409-0bd1ab6f-71a1-4016-873b-7e049362b1e3.png)

```javascript
app.use((err, req, res, next) => {
  const message = err.message;
  switch (message) {
    case "USER DOES NOT EXISTS":
      res.status(400).json({message});
  }
  res.status(500);
})
```

## express源码分析
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716969183086-5c92b964-13e6-48eb-a2bc-3abd3d003aff.png)

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716973311102-e1fbd200-0683-425f-9117-56cbf259b164.png)

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716973322421-2d3e7ddc-9eca-47ed-bd94-7ab20438abfc.png)

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716973333517-d38a9fc9-1df7-4155-9a3c-166886888dbb.png)

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1716973343503-d8936f8d-2ad6-4a6c-85ee-b8f1bd9b9366.png)

# koa
koa注册的中间件提供了两个参数：

+ ctx：上下文（context）对象；
    - koa中req和res不是分开的，而是作为ctx的属性；
    - ctx代表一次请求的上下文对象
    - ctx.request：获取请求的对象
    - ctx.response：获取响应对象
+ next：本质上是一个dispatch，类似于之前的next

```javascript
const koa = require('koa');
const app = new Koa();
app.use((ctx, next) => {
  next();
})
app.use((ctx, next) => {
  ctx.response.body = "Hello World";
})
app.listen(8000, () => {
  console.log("服务器启动成功");
})
```

## 中间件
koa通过创建的app对象，注册中间件只能通过use方法：

+ koa并没有提供methods的方式来注册中间件，也没有提供path中间件来匹配路径

分离路径和method：

1. 根据request自己来判断
2. 使用第三方路由中间件

```javascript
app.use((ctx, next) => {
  if(ctx.request.path === '/users'){
    if(ctx.request)
  }
})
```

## 路由
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1720508037980-01ce9c27-aa86-4969-a7a8-72108bcb2132.png)

## 参数解析
### params-query
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1720509419052-473fcdb9-2332-404d-9575-95141f42315a.png)

### json
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1720510738133-0e027632-aed4-499f-bf1b-ae92f419046f.png)

### form-data
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1720511255000-f2f3f899-5573-4f54-ba31-2dab489373ac.png)

multer上传文件

![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1720511278402-3d539e52-bc76-4737-938a-33df88229231.png)

## 数据的响应
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1720512438332-93d9bd74-c8e9-4f6c-84cb-33eb754ec652.png)

## 静态服务器
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1720513915571-9a97e740-2ba4-4552-a093-2d8e045b6571.png)

## 错误处理
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1720514852931-6757ca92-1cb0-4cc7-b6ba-16ba4fa80849.png)

## koa洋葱模型
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1720525333064-64175375-a301-4316-b3c1-4e144e57abf9.png)

# koa与express的区别
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1720514950445-dc68d0ff-054b-47f9-ad2c-78fc5a772b17.png)

# MySQL
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1729062863457-4fff7a1c-c070-40d9-bbaf-f4ad673ba351.png)

