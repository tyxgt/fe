> [react进阶，小熊](https://feuux5hgzd.feishu.cn/drive/folder/fldcncoODV3lXEvE6Ba8n0VKmM2)
>
> [「react进阶」一文吃透react-hooks原理 - 掘金](https://juejin.cn/post/6944863057000529933)
>
> [一文彻底搞懂react hooks的原理和实现 - 掘金](https://juejin.cn/post/6844903975838285838?searchId=20231029200059C5BA4D26A9C500550F2F)
>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1701674626900-9c1efa9d-aa60-4654-83e5-617facd2529f.png)

![](https://cdn.nlark.com/yuque/0/2023/jpeg/2366100/1701702657441-492b4e24-7997-4dfe-b7e4-82916f02277f.jpeg)

![](https://cdn.nlark.com/yuque/0/2023/jpeg/2366100/1701702667142-7f85d2ae-9c31-4097-8250-ca466cb681cf.jpeg)

# 宏观包结构
## 基础包结构
1. **react**：react 基础包, 只提供定义 react 组件(ReactElement)的必要函数, 一般来说需要和渲染器(react-dom,react-native)一同使用. 在编写react应用的代码时, 大部分都是调用此包的 api.
2. **react-dom**：react 渲染器之一, 是 react 与 web 平台连接的桥梁(可以在浏览器和 nodejs 环境中使用), 将react-reconciler中的运行结果输出到 web 界面上. 在编写react应用的代码时,大多数场景下, 能用到此包的就是一个入口函数ReactDOM.render(<App/>, document.getElementByID('root')), 其余使用的 api, 基本是react包提供的.
3. **react-reconciler**：react 得以运行的核心包(**综合协调react-dom,react,scheduler各包之间的调用与配合**). 管理 react 应用状态的输入和结果的输出. 将输入信号最终转换成输出信号传递给渲染器.
+ **输入**：暴露api函数(如: scheduleUpdateOnFiber), 供给其他包(如react包)调用。

接受输入(scheduleUpdateOnFiber), 将fiber树生成逻辑封装到一个回调函数中(涉及fiber树形结构, fiber.updateQueue队列, 调和算法等),

+ **注册调度任务**：与调度中心(scheduler包)交互, 注册调度任务task, 等待任务回调.

把此回调函数(performSyncWorkOnRoot或performConcurrentWorkOnRoot)送入scheduler进行调度

+ **执行任务回调**：scheduler会控制回调函数执行的时机, 回调函数执行完成后得到全新的 fiber 树
+ **输出**：再调用渲染器(如react-dom, react-native等)将 fiber 树形结构最终反映到界面上
4. **scheduler**：调度机制的核心实现, 控制由react-reconciler送入的回调函数的执行时机, 在concurrent模式下可以实现任务分片. 在编写react应用的代码时, 同样几乎不会直接用到此包提供的 api.
+ 核心任务就是执行回调(回调函数由react-reconciler提供)
+ 通过控制回调函数的执行时机, 来达到任务分片的目的, 实现可中断渲染(concurrent模式下才有此特性)

# 架构分层
## 接口层
1. 平时在开发过程中使用的绝大部分api均来自此包(不是所有)

## 内核层
### 调度器scheduler
执行回调

+ 把react-reconciler提供的回调函数, 包装到一个任务对象中.
+ 在内部维护一个任务队列, 优先级高的排在最前面.
+ 循环消费任务队列, 直到队列清空.

### 构造器react-reconciler
1. 装载渲染器, 渲染器必须实现[HostConfig协议](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/README.md#practical-examples)(如: react-dom), 保证在需要的时候, 能够正确调用渲染器的 api, 生成实际节点(如: dom节点).
2. 接收react-dom包(初次render)和react包(后续更新setState)发起的更新请求.
3. 将fiber树的构造过程包装在一个回调函数中, 并将此回调函数传入到scheduler包等待调度.

### 渲染器react-dom
1. 引导react应用的启动(通过ReactDOM.render).
2. 实现[HostConfig协议](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/README.md#practical-examples)([源码在 ReactDOMHostConfig.js 中](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/client/ReactDOMHostConfig.js)), 能够将react-reconciler包构造出来的fiber树表现出来, 生成 dom 节点(浏览器中), 生成字符串(ssr).

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698807134957-e1bfac3d-8e64-468b-a3bb-04a5e9aaeaec.png)

# react发展里程碑
1. v15.0架构分为两层：

> [React15和React16的架构比较（一） - 掘金](https://juejin.cn/post/6981488003629711368)
>

+ <font style="color:rgb(37, 41, 51);">Re</font>conciler（协调器）—— 负责找出变化的组件

在React 15 及更早的解决方案中，协调器使用的是stack reconciler ，当React触发更新时，Reconciler会依次完成以下工作：

    - 调用函数组件、class组件的render方法，将返回的JSX转化为虚拟DOM
    - 将虚拟DOM和上次更新时的虚拟DOM对比
    - 通过对比找出本次更新中变化的虚拟DOM
    - 通知Renderer将变化的虚拟DOM渲染到页面上
+ Renderer（渲染器）—— 负责将变化的组件渲染到页面上

渲染器用于管理一棵 React 树，使其根据底层平台进行不同的调用。 由于React支持跨平台，针对不同平台有不同的Renderer：

    - ReactDOM： 负责将 React 组件渲染成 Web环境中的DOM，常用于浏览器开发中
    - ReactNative：负责将 React 组件渲染为 Native 视图，常用于native开发中
    - ReactTest：负责将 React 组件渲染为 JSON 树，常用于 Jest 的快照测试特性
    - ReactArt：负责渲染到Canvas, SVG 或 VML (IE8)

缺点：<font style="color:rgb(37, 41, 51);">在组件初始化或者更新的时候，会递归更新子组件。由于是递归执行，所以更新一旦开始，中途就无法中断。当组件层级很深时，递归更新时间超过了16.6ms（主流浏览器每16.6ms刷新一次），用户交互界面就会卡顿，用户体验较差。</font>

2. v16.0：[React15和React16的架构比较（二） - 掘金](https://juejin.cn/post/6981498660353736734)

**<font style="color:#DF2A3F;">相对于15主要增加了scheduler调度器，架构分为三层：</font>**

+ **<font style="color:#DF2A3F;">Scheduler（调度器）—— 负责调度任务的优先级，高优任务优先进入Reconciler</font>**
+ **<font style="color:#DF2A3F;">Reconciler（协调器）—— 负责找出需要更新的组件</font>**
+ **<font style="color:#DF2A3F;">Renderer（渲染器）—— 负责将需要更新的组件渲染到页面上 可以看到，相较于React15，React16中新增了Scheduler调度器，让我们来了解下它。</font>**
+ 为解决之前大型react应用一次更新遍历大量虚拟dom带来的卡顿问题，react重写了核心模块reconciler，**<font style="color:#DF2A3F;">启用了fiber架构</font>**
+ <font style="color:rgb(37, 41, 51);">基于 expirationTime 的优先级控制</font>
+ 为了在让节点渲染到指定容器内，更好的实现弹窗功能，推出 createPortal API；
+ 为了捕获渲染中的异常，引入 componentDidCatch 钩子，划分了错误边界。
2. v16.2：
+ 推出 Fragment ，解决数组元素问题。
3. v16.3：
+ 增加 React.createRef() API，可以通过 React.createRef 取得 Ref 对象。
+ 增加 React.forwardRef() API，解决高阶组件 ref 传递问题；
+ 推出新版本 context api，迎接Provider / Consumer 时代；
+ 增加 getDerivedStateFromProps 和 getSnapshotBeforeUpdate 生命周期 。
4. v16.6：
+ 增加 React.memo() API，用于控制子组件渲染；
+ 增加 React.lazy() API 实现代码分割；
+ 增加 contextType 让类组件更便捷的使用context；
+ 增加生命周期 getDerivedStateFromError 代替 componentDidCatch 。
5. v16.8：
+ 全新 React-Hooks 支持，使函数组件也能做类组件的一切事情。
6. v17：
+ 事件绑定由 document 变成 container ，移除事件池等。
+ 优先级控制改为基于 lane 模型的控制策略

> 改动原因：基于 expirationTime 的优先级控制不能很好的支撑并发更新。
>
> 不能很好的表达批的概念。比如，现在有5个更新分别是1,2,3,4,5. 我只想更新1,3,5基于 expirationTime 是没办法做到的，expirationTime 只能表达一个连续的范围，比如 expirationTime < 3,或者 expirationTime > 3.做多可以表达 expirationTime > 1或者expirationTime < 3。
>
> I/O 场景的高优先级更新会阻塞低优先级的更新。
>

+ <font style="color:rgb(37, 41, 51);">新的 JSX 转换逻辑，编写 JSX 代码将不再需要手动导入 React 包，编译器会针对 JSX 代码进行自动导入（React/jsx-runtime）和优化</font>

# react 18有哪些更新
## react 18已经放弃了支持ie11；
## setState自动批处理
即多次setState会被合并为1次执行，提高了性能，在数据层，将多个状态更新合并成一次处理，将多次渲染合并成一次渲染

在react 18之前，我们只在react事件处理函数中进行批处理更新。默认情况下，在promise，setTimeout，原生事件处理函数中，或任何其他事件内的更新都不会进行批处理

## render api
+ 引入了新的root api，支持new concurrent renderer（并发模式的渲染）

```javascript
//React 17
import React from "react"
import ReactDOM from "react-dom"
import App from "./App"
const root = document.getElementById("root")
ReactDOM.render(<App/>,root)
// 卸载组件
ReactDOM.unmountComponentAtNode(root)  
// React 18
import React from "react"
import ReactDOM from "react-dom/client"
import App from "./App"
const root = document.getElementById("root")
ReactDOM.createRoot(root).render(<App/>)
// 卸载组件
root.unmount()  
```

+ 如果使用了ssr服务端渲染，需要把hydration升级为hydrateroot

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698327694308-e5ee5134-31a6-4280-a2bb-fb6945b71027.png)

+ React 18从render方法中删除了回调函数，因为当使用**Suspense**时，它通常不会有预期的结果。在新版本中，如果需要在 **render** 方法中使用回调函数，我们可以在组件中通过 **useEffect** 实现
+ 如果需要获取子组件children，需要显示定义

## [flushSync(callback)](https://zh-hans.react.dev/reference/react-dom/flushSync)
批处理是一个破坏性改动，  如果想退出批处理更新，可以使用flushSync。flushSync 函数内部的多个 setState 仍然为批量更新，这样可以精准控制哪些不需要的批量更新。

> 多数时候都不需要使用 flushSync，请将其作为最后的手段使用
>

```javascript
import React,{useState} from "react"
import {flushSync} from "react-dom"
const App=()=>{
  const [count,setCount]=useState(0)
  const [count2,setCount2]=useState(0)
  return (
    <div className="App">
      <button onClick=(()=>{
        // 第一次更新
        flushSync(()=>{
          setCount(count=>count+1)
        })
        // 第二次更新
        flushSync(()=>{
          setCount2(count2=>count2+1)
        })
      })>点击</button>
      <span>count:{count}</span>
      <span>count2:{count2}</span>	
    </div>	
  )
}
export default App
```

## 返回值为undefined问题
在 React 17 中，如果你需要返回一个空组件，React只允许返回null。如果你显式的返回了undefined，控制台则会在运行时抛出一个错误。

在 React 18 中，不再检查因返回 undefined 而导致崩溃。既能返回 null，也能返回 undefined（但是 React 18 的dts文件还是会检查，只允许返回 null，你可以忽略这个类型错误）。

## suspense不再需要fallback来捕获
在 React 18 的 Suspense 组件中，官方对空的fallback属性的处理方式做了改变：不再跳过 缺失值 或 值为null 的 fallback 的 Suspense 边界。相反，会捕获边界并且向外层查找，如果查找不到，将会把 fallback 呈现为 null。

## strict Mode更新
当你使用严格模式时，React会对每个组件返回两次渲染，以便你观察一些意想不到的结果,在react17中去掉了一次渲染的控制台日志，以便让日志容易阅读。react18取消了这个限制，第二次渲染会以浅灰色出现在控制台日志

## 支持useId
在服务器和客户端生成相同的唯一一个id，避免hydrating的不兼容。因为我们的服务器渲染时提供的 **HTML** 是**无序的**，**useId** 的原理就是每个 **id** 代表该组件在组件树中的层级结构。

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698328595776-6dbc3265-ff44-45de-9f03-8182fab32cef.png)

## 并发模式
将同步不可中断更新变成了异步可中断更新

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698328924749-22e4032f-a34d-44e4-98f7-e935b57c8975.png)

并发特性：

1. startTransition  

> [React 18 新特性之 startTransition - 掘金](https://juejin.cn/post/7029120932086022174)
>

startTransition，主要为了能在大量的任务下也能保持 UI 响应。这个新的 API 可以通过将特定更新标记为“过渡”来显著改善用户交互，简单来说，就是被 startTransition 回调包裹的 setState 触发的渲染被标记为不紧急渲染，这些渲染可能被其他紧急渲染所抢占。

2. useDeferredValue

返回一个延迟响应的值，可以让一个**state** 延迟生效，只有当前没有紧急更新时，该值才会变为最新值。**useDeferredValue** 和 **startTransition** 一样，都是标记了一次非紧急更新。

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698329063195-f315d549-1b12-4eee-bc20-b0f594dc99ff.png)

并发模式是一组功能，可帮助 React 应用程序保持响应并平滑地适应用户的设备和网络速度能力。并发模式将其拥有的任务划分为更小的块。 React 的调度程序可以挑选并选择要执行的作业。作业的调度取决于它们的优先级。通过对任务进行优先级排序，它可以停止琐碎或不紧急的事情，或者进一步推动它们。 React 始终将用户界面更新和渲染放在首位。

并发更新的意义就是交替执行不同的任务，当预留的时间不够用时，React 将线程控制权交还给浏览器，等待下一帧时间到来，然后继续被中断的工作

并发模式是实现并发更新的基本前提

时间切片是实现并发更新的具体手段

上面所有的东西都是基于 fiber 架构实现的，fiber为状态更新提供了可中断的能力

## useSyncExternalStore
> [useSyncExternalStore – React 中文文档](https://zh-hans.react.dev/reference/react/useSyncExternalStore#subscribing-to-an-external-store)
>

useSyncExternalStore(subscribe, getSnapshot，getServerSnapshot？)用于解决外部数据撕裂问题，返回值为该store的当前快照

+ subscribe函数应当订阅该store并返回一个取消订阅的函数，接收一个单独的 callback 参数并把它订阅到 store 上。当 store 发生改变，它应当调用被提供的 callback。这会导致组件重新渲染。subscribe 函数会返回清除订阅的函数。
+ getSnapshot函数应当从该store读取数据的快照。在 store 不变的情况下，重复调用 getSnapshot 必须返回同一个值。如果 store 改变，并且返回值也不同了（用 [Object.is](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/is) 比较），React 就会重新渲染组件
+ getServerSnapshot：一个函数，返回 store 中数据的初始快照。它只会在服务端渲染时，以及在客户端进行服务端渲染内容的 hydration 时被用到。快照在服务端与客户端之间必须相同，它通常是从服务端序列化并传到客户端的。如果你忽略此参数，在服务端渲染这个组件会抛出一个错误。

## useInsertionEffect
这个hooks只建议在css in js库中使用，这个hooks执行时机在DOM生成之后，useLayoutEffect执行之前，它的工作原理大致与useLayoutEffect相同，此时无法访问DOM节点的引用，一般用于提前注入脚本

# react和vue的异同？
**相同：**

+ 都使用了虚拟dom
+ 都将注意力集中保持在核心库，而将其他功能如路由和全局状态管理交给相关库
+ 都是单向数据流
+ 都使用组件化思想
+ 都使用了hook的思想：vue3中新增了reactive，ref，watchEffect等

**不同：**

+ Vue的整体思想仍然是拥抱经典的html(结构)+css(表现)+js(行为)的形式，vue鼓励开发者使用template模板；react整体上是函数式的思想，组件使用jsx语法，all in js，将html与css全都融入javaScript
+ React中，当某组件的状态发生改变时，它会以该组件为根，重新渲染整个组件子树，而在Vue中，组件的依赖是在渲染的过程中自动追踪的，所以系统能准确知晓哪个组件确实需要被重新渲染
+ vue中是双向绑定的，通过v-model实现，相当于onchange的语法糖；react需要我们通过setState来更新数据
+ Vue的路由库和状态管理库都由官方维护支持且与核心库同步更新，而React选择把这些问题交给社区维护，因此生态更丰富。

# 函数式组件与类组件的区别
1. 类组件的根基是oop面向对象编程，所以会有继承，内部状态等；

函数组件的根基是FP函数式编程，相同输入必有相同输出

2. 函数式组件没有生命周期，类组件有生命周期
3. 在函数组件的闭包中，捕获的值永远都是确定且安全的，每一次都是新的数据，class会使用之前的数据直到组件销毁
4. 性能优化：类组件是通过 shouldComponentUpdate 生命周期函数去阻断渲染；函数式组件式通过React.Memo函数来优化

**本质区别：**

**对于类组件来说，底层只需要实例化一次，实例中保存了组件的state状态，对于每一次更新只需要调用render方法以及对应的生命周期就可以了，但是在函数组件中，每一次更新都是一次新的函数执行，一次函数组件的更新，里面的变量就会重新声明**

**在编写类组件的时候，开发者编写的逻辑在封装之后是和组件粘连在一起的，比较难以拆分和复用**

# **react应用的启动模式**
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698891730765-e727e083-e5c1-453b-b6c5-9801e1030372.png)

1. legacy模式：ReactDOM.render(<App />, rootNode). 这是当前 React app 使用的方式. 这个模式可能不支持[这些新功能(concurrent 支持的所有功能)](https://zh-hans.reactjs.org/docs/concurrent-mode-patterns.html#the-three-steps)
2. Blocking模式：ReactDOM.createBlockingRoot(rootNode).render(<App />). 目前正在实验中, 它仅提供了 concurrent 模式的小部分功能, 作为迁移到 concurrent 模式的第一个步骤.
3. concurrent模式：ReactDOM.createRoot(rootNode).render(<App />). 目前在实验中, 未来稳定之后，打算作为 React 的默认开发模式. 这个模式开启了所有的新功能.

3 种模式在调用更新时都会执行updateContainer. updateContainer函数串联了react-dom与react-reconciler, 之后的逻辑进入了react-reconciler包

无论是哪种模式，react在初始化的时候都会创建三个全局对象

+ ReactDOM(Blocking)Root

属于react-dom包, 该对象[暴露有render,unmount方法](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/client/ReactDOMRoot.js#L62-L104), 通过调用该实例的render方法, 可以引导 react 应用的启动.

+ fiberRoot

属于react-reconciler包, 作为react-reconciler在运行过程中的全局上下文, 保存 fiber 构建过程中所依赖的全局状态.

其大部分实例变量用来存储fiber 构造循环(详见[两大工作循环](https://react-illustration-series.osrc.com/main/workloop))过程的各种状态.react 应用内部, 可以根据这些实例变量的值, 控制执行逻辑.

> 无论哪种模式下，在ReactDom(Blocking)Root的创建过程中, 都会调用一个相同的函数createRootImpl, 查看后续的函数调用, 最后会创建fiberRoot 对象(在这个过程中, 特别注意RootTag的传递过程):
>
> 三种模式下的tag是各不相同（分别是ConcurrentRoot, BlockingRoot, LegacyRoot）
>

+ HostRootFiber

属于react-reconciler包, 这是 react 应用中的第一个 Fiber 对象, 是 Fiber 树的根节点, 节点的类型是HostRoot.

在创建HostRootFiber时, 其中fiber.mode属性, 会与 3 种RootTag(ConcurrentRoot,BlockingRoot,LegacyRoot)关联起来.

注意:fiber树中所有节点的mode都会和HostRootFiber.mode一致(新建的 fiber 节点, 其 mode 来源于父节点),所以**HostRootFiber.mode**非常重要, 它决定了以后整个 fiber 树构建过程.

3 个对象创建成功, react应用的初始化完毕，此刻内存中各个对象的引用情况：

1. legacy：

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698891356091-86400f44-40e9-4632-80c1-d947837838d2.png)

2. concurrent

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698891386624-e04a4d47-d21a-4a9d-9b7d-fb5d43ecdf11.png)

3. blocking

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698891412195-cdebb4dd-cebe-4330-bbf2-87d527a47913.png)

注意:

1. 3 种模式下,HostRootFiber.mode是不一致的
2. legacy 下, div#root和ReactDOMBlockingRoot之间通过_reactRootContainer关联. 其他模式是没有关联的
3. 此时reactElement(<App/>)还是独立在外的, 还没有和目前创建的 3 个全局对象关联起来

# 生命周期
> react在调和（render）阶段会深度遍历ReactFiber树，目的就是发现不同（diff），不同的地方就是接下来需要更新的地方，对于变化的组件，就会执行render函数，在一次调和过程完毕之后，就到了commit阶段，commit阶段会创建修改真实的dom节点
>

## constructor：**在该阶段进行初始化的工作**
1. 初始化state：可以用来截取路由中的参数，赋值给state
2. 对类组件的事件做一些处理：绑定this，节流，防抖等
3. 对类组件进行一些必要生命周期的劫持，渲染劫持，这个功能更适合反向继承的HOC

## **getDerivedStateFromProps(nextProps,prevState)**
会在render方法之前调用，并且在初始挂载及后续更新时都会被调用，它应返回一个对象来更新 state，如果返回 null 则不更新任何内容。

1. 两个参数：
+ nextProps：父组件新传递的props
+ prevState：传入该方法待合并的state
2. 作为类的静态属性方法执行，内部是访问不到this的，在初始化和更新阶段，接受父组件的props数据，可以对props进行格式化，过滤等操作，返回值将作为新的state合并到state中，供给视图渲染层消费
3. 作用：
+ 代替componentWillMount 和 componentWillReceiveProps
+ 组件初始化或者更新时，将props映射到state
+ 返回值与 state 合并完，可以作为 shouldComponentUpdate 第二个参数 newState ，可以判断是否渲染组件。
4. 简单的替代方案：
+ 如果你需要**执行副作用**（例如，数据提取或动画）以响应 props 中的更改，请改用 [componentDidUpdate](https://zh-hans.reactjs.org/docs/react-component.html#componentdidupdate)。
+ 如果只想在 **prop 更改时重新计算某些数据**，[请使用 memoization helper 代替](https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#what-about-memoization)。
+ 如果你想**在 prop 更改时“重置”某些 state**，请考虑使组件[完全受控](https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#recommendation-fully-controlled-component)或[使用key使组件完全不受控](https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#recommendation-fully-uncontrolled-component-with-a-key)代替。
5. 优点：
+ 该方法是静态方法，在这里不能使用this，是一个纯函数，开发者不能写出副作用的代码
+ 开发者只能通过prevState而不是prevProps来做对比，保证了state和props之间的简单关系以及不需要处理第一次渲染时prevProps为空的情况
+ 基于第一点，将状态变化和昂贵操作区分开，更加便于render和commit阶段操作或者说优化

## componentWillMount 和 UNSAFE_componentWillMount
> 用UNSAFE_componentWillMount替换componentWillMount（注意：这里的UNSAFE并不是指安全性，而是表示使用这些生命周期的代码将更有可能在未来的React版本中存在缺陷，特别是一旦启用了异步渲染）
>

在挂载之前被调用，他在render()之前调用，因此在此方法中同步调用setState()不会触发额外渲染，通常建议使用construtor()来初始化state

**该方法时服务端渲染唯一会调用的生命周期函数**

> componentWillMount ，componentWillReceiveProps ， componentWillUpdate三个生命周期，都是在 render 之前执行的，React 对于执行 render 函数有着像 shouldUpdate 等条件制约，但是对于执行在 render 之前生命周期没有限制，存在一定隐匿风险，如果 updateClassInstance 执行多次，React 开发者滥用这几个生命周期，可能导致生命周期内的上下文多次被执行。
>
> 可以使用componentDidMount和construtor来代替，如果在该生命周期函数中订阅事件，但在服务端这并不会执行该事件，也就是说服务端会导致内存泄漏所以该函数完全可以不使用
>

## componentWillReceiveProps 和 UNSAFE_componentWillReceiveProps
在更新组件阶段，该生命周期执行驱动是因为父组件更新带来的props修改，但是只要父组件触发render函数，调用React.createElement方法，那么props就会被重新创建，生命周期componentWillReceiveProps就会执行了

+ componentWillReceiveProps 可以用来监听父组件是否执行 render
+ componentWillReceiveProps 可以用来接受 props 改变，组件可以根据props改变，来决定是否更新 state ，因为可以访问到 this ， 所以可以在异步成功回调(接口请求数据)改变 state 。这个是 getDerivedStateFromProps 不能实现的

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698334536719-d49ce0cc-b6e9-42d0-a3e0-24c0305e16f8.png)

> 当props不变的前提下，PureComponent 组件能否阻止 componentWillReceiveProps 执行？
>
> componentWillReceiveProps 生命周期的执行，和纯组件没有关系，纯组件是在 componentWillReceiveProps 执行之后浅比较 props 是否发生变化。所以 PureComponent 下不会阻止该生命周期的执行。
>

> **废弃：**
>
> 在老版本的React中，如果组件自身的某个state 跟其props密切相关的话，一直都没有一种很优雅的处理方式去更新state,而是需要在componentWilReceiveProps中判断前后两个props是否相同，如果不同再将新的props更新到相应的state上去。这样做一来会破坏state数据的单一数据源，导致组件状态变得不可预测，另一方面也会增加组件的重绘次数。 
>

## **componentWillUpdate 和 UNSAFE_componentWillUpdate**
在更新之前，此时的 DOM 还没有更新。在这里可以做一些获取 DOM 的操作。但是 React 已经出了新的生命周期 getSnapshotBeforeUpdate 来代替 UNSAFE_componentWillUpdate

> 废弃：
>
> 与componentWillReceiveProps类似，都有可能在一次更新中被调用多次，一次更新中componentDidUpdate只会被调用一次，所以将原先卸载componentWillUpdate中的回调迁移至componentDidUpdate就可以解决这个问题；另一种情况时需要获取dom元素状态，但是由于在fiber中，render可以打断，可能在该函数中获取到的元素状态很可能与实际需要的不同，这个通常可以使用第二个新增的生命周期函数去解决getSnapshotBeforeUpdate(prevProps,preState)
>

## render
返回需要渲染的内容。jsx 的各个元素被 React.createElement 创建成 React element 对象的形式。一次 render 的过程，就是创建 React.element 元素的过程。

通常调用该方法会返回以下类型中一个：

+ React元素：这里包括原生的DOM以及React元素
+ 数组和Fragment(片段)：可以返回多个元素
+ Portals（插槽）：可以将子元素渲染到不同的DOM子树中
+ 字符串和数字：被渲染成DOM中的text节点
+ 布尔值或null：不渲染任何内容

## **getSnapshotBeforeUpdate(prevProps,preState)**
该生命周期是在 commit 阶段的before Mutation ( DOM 修改前)，此时 DOM 还没有更新，但是在接下来的 Mutation 阶段会被替换成真实 DOM 。此时是获取 DOM 信息的最佳时期，getSnapshotBeforeUpdate 将返回一个值作为一个snapShot(快照)，传递给 componentDidUpdate作为第三个参数。

两个参数：

+ prevProps更新前的props
+ preState更新前的state

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698498189531-c8a13e0a-e837-436d-8a40-749dccd1f20f.png)

## **componentDidUpdate(prevProps, prevState, snapshot)**
三个参数：

+ prevProps 更新之前的 props
+ prevState 更新之前的 state
+ snapshot 为 getSnapshotBeforeUpdate 返回的快照，可以是更新前的 DOM 信息

作用：

1. componentDidUpdate 生命周期执行，此时 DOM 已经更新，可以直接获取 DOM 最新状态。**这个函数里面如果想要使用 setState ，一定要加以限制，否则会引起无限循环。**
2. 接受 getSnapshotBeforeUpdate 保存的快照信息

## componentDidMount
此时 DOM 已经创建完，既然 DOM 已经创建挂载，就可以做一些基于 DOM 操作，DOM 事件监听器。是在浏览器刷新屏幕前执行的

作用：

+ 可以做一些关于 DOM 操作，比如基于 DOM 的事件监听器。
+ 对于初始化向服务器请求数据，渲染视图，这个生命周期也是蛮合适的。
+ 添加订阅消息

> componentDidMount方法中的代码，是在组件已经完全挂载到网页上才会调用被执行，所以可以保证数据的加载，在该方法中红调用setState方法，会触发重新渲染，所以建议在该生命周期中获取数据
>

## **shouldComponentUpdate(newProps,newState,nextContext)**
1. 三个参数：
+ 第一个参数新的 props 
+ 第二个参数新的 state 
+ 第三个参数新的 context 
2. 作用： 一般用于**性能优化**，shouldComponentUpdate 返回值决定是否重新渲染的类组件。需要重点关注的是第二个参数 newState ，如果有 getDerivedStateFromProps 生命周期 ，它的返回值将合并到 newState ，供 shouldComponentUpdate 使用。**新旧对比时时前对比，如果比较的数据是引用数据类型，值对比地址**
3. 解决方案：
    1. 使用setState改变数据之前，先采用Object.assign进行拷贝，但是assign只深拷贝数据的第一层，不是最完美的解决办法
    2. Json.parse(Json.stringify())进行深拷贝，但是遇到数据undefined和函数会报错
    3. 使用react官方推荐的第三方库immutable.js进行项目搭建，immutable中讲究数据的不可变性，每次对数据进行操作前，都会自动的对数据进行深拷贝

## **componentWillUnmount**
组件销毁阶段唯一执行的生命周期，主要做一些收尾工作，比如清除一些可能造成内存泄漏的定时器，延时器，或者是一些事件监听器。

作用：

1. 清除延时器，定时器。
2. 一些基于 DOM 的操作，比如事件监听器。

## 生命周期顺序
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698334886728-55ba01ae-95f3-4fea-bb38-80601fad6f2b.png)

React常见生命周期的过程大致如下:

+ 挂载阶段，首先执行constructor构造方法，来创建组件
+ 创建完成之后，就会执行render方法，该方法会返回需要渲染的内容
+ 随后，React会将需要渲染的内容挂载到DOM树上
+ 挂载完成之后就会执行componentDidMount生命周期函数
+ 如果我们给组件创建一个props (用于组件通信) 、调用setstate (更改state中的数据)、调用forceUpdate (强制更新组件) 时，都会重新调用render函数
+ render函数重新执行之后，就会重新进行DOM树的挂载
+ 挂载完成之后就会执行componentDidUpdate生命周期函数
+ 当移除组件时，就会执行componentWillUnmount生命周期函数

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698335105951-30349c07-d61f-4e8f-b695-423dc8a67f7b.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698335128960-8aff7f31-fc29-4925-9476-12468c69ff1f.png)

## React16.0前的生命周期
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698072923750-a0c09c2c-669c-4ad0-907d-60130d526487.png)

1. 挂载阶段：
+ constructor
+ componentWillMount
+ render
+ componentDidMount
2. 更新阶段：state更新或者props更新
+ componentWillReceiveProps（组件props变更）
+ shouldComponentUpdate
+ componentWillUpdate
+ render
+ componentDidUpdate
3. 卸载阶段
+ componentWillUnmount

## react16之后的生命周期
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698073301696-62126f47-e5ee-4b24-9efe-63f12dfafa74.png)

1. 挂载阶段
+ constructor：构造函数
+ getDerivedStateFromProps：派生props。
+ render
+ componentDidMount
2. 更新阶段
+ getDerivedStateFromProps
+ sholdComponentUpdate
+ render
+ getSnapshotBeforeUpdate：获取快照
+ componentDidUpdate
3. 卸载阶段
+ componentWillUnmount

# React 16.0之后为什么要删除will相关生命周期
1. 被删除的生命周器
+ componentWillReceiveProps
+ componentWillMount
+ componentWillUpdate
2. 删除原因
+ 这些生命周期方法经常被误解和巧妙地误用
+ 它们的潜在误用可能会在异步渲染中带来更多问题, 所以如果现有项目中使用了这几个生命周期, 将会在控制台输出如下警告! 大致意思就是这几个生命周期将在 18.x 彻底下线, 如果一定要使用可以带上 UNSAFE_ 前缀
3. **为何移除 componentWillMount？**

因为在异步渲染机制中允许对组件进行中断停止等操作，可能会导致单个组件实例 componentWillMount 被多次调用, 很多开发者目前会将事件绑定、异步请求等写在 componentWillMount 中, 一旦异步渲染时 componentWillMount 被多次调用, 将会导致：

+ 进行重复的事件监听, 无法正常取消重复的事件, 严重点可能会导致内存泄漏
+ 发出重复的异步网络请求, 导致 IO 资源被浪费
+ 现在, React 推荐将原本在 componentWillMount 中的网络请求移到 componentDidMount 中, 至于这样会不会导致请求被延迟发出影响用户体验, React 团队是这么解释的: componentWillMount、render 和 componentDidMount 方法虽然存在调用先后顺序, 但在大多数情况下, 几乎都是在很短的时间内先后执行完毕, 几乎不会对用户体验产生影响。
4. **为何移除 componentWillUpdate？**
+ 大多数开发者使用 componentWillUpdate 的场景是配合 componentDidUpdate, 分别获取 渲染 前后的视图状态, 进行必要的处理, 但随着 React异步渲染 等机制的到来, 渲染 过程可以被分割成多次完成, 还可以被 暂停 甚至 回溯, 这导致 componentWillUpdate 和 componentDidUpdate 执行前后可能会间隔很长时间, 足够使用户进行交互操作更改当前组件的状态, 这样可能会导致难以追踪的 BUG
+ 所以后面新增了 getSnapshotBeforeUpdate 生命周期, 目的就是就是为了解决上述问题并取代 componentWillUpdate, 因为 getSnapshotBeforeUpdate 方法是在 componentWillUpdate 后(如果存在的话), 在 React 真正更改 DOM 前调用的, 它获取到组件状态信息会更加可靠
+ 除此之外, getSnapshotBeforeUpdate 还有一个十分明显的好处: 它的返回结果会作为 componentDidUpdate 的第三个参数进行传递, 从而避免了 componentWillUpdate 和 componentDidUpdate 配合使用时将组件临时的状态数据存在组件实例上引起的浪费内存
+ 同时 getSnapshotBeforeUpdate 返回的数据在 componentDidUpdate 中用完即被销毁, 效率更高

# 为什么需要在 React 类组件中为事件处理程序绑定 this
> [[译] 为什么需要在 React 类组件中为事件处理程序绑定 this - 掘金](https://juejin.cn/post/6844903605984559118?searchId=20231023230922CE2986E8AFB9E5960A08)
>

在使用react时，在遇到受控组件和事件处理程序时，需要使用.bind()来将方法绑定到组件实例上面

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698499110336-a98b21af-cffe-4d2d-a3d2-1b3047e8d599.png)

类声明和类表达式的主体以严格模式执行，主要包括构造函数，静态方法和原型方法，getter和setter函数也在严格模式下执行

**在类中事件处理程序方法会丢失其隐式绑定的上下文，当事件被触发并且处理程序被调用时，，this的值会回退到默认绑定，即值为 undefined，这是因为类声明和原型方法是以严格模式运行。当我们将事件处理程序的 this 绑定到构造函数中的组件实例时，我们可以将它作为回调传递，而不用担心会丢失它的上下文。箭头函数可以免除这种行为，因为它使用的是词法 this 绑定，会将其自动绑定到定义他们的函数上下文。**

# 对react的理解
react是一个用于构建用户界面的js库，主要用于构建UI

特点：

+ 声明式设计：采用声明范式，可以轻松描述应用
+ 高效：React通过对dom的模拟，最大限度减少与DOM的交互
+ 灵活：REACT可以与一直的库或框架很好的配合
+ JSX：jsx是js扩展语法，react开发不一定适用jsx，但建议使用它
+ 组件：通过react构建组件，代码更容易服用，能够很好的应用在大项目的开发中
+ 单向数据流：React实现了单向响应的数据流，从而减少了重复代码，这也是为什么比传统数据绑定更简单

React16.x的三大新特性：

+ Timing Slicing：解决CPU速度问题。使得在执行任务的期间可以随时暂停跑去干别的事情。这个特性使得react在性能极差的机器跑时仍然保持有良好的性能。
+ Suspense（解决网络IO问题）和lazy配合，实现异步加载组件能暂停当前组件的渲染，当完成某件事以后再继续渲染解决。从react出生到现在都存在的异步副作用的问题，而且解决得非常的优雅，使用的是异步，但是是同步的写法，这是最好的解决异步问题的方式。
+ 内置函数componentDidCatch的提供。当有错误发生时，可以有好的展示。fallback组件可以捕捉到它的子组件，包括嵌套子元素抛出的异常可以复用错误组件。

# react的设计思想
+ 组件化

每个组件都符合开放-封闭原则，封闭是针对渲染工作流来说的，指的是组件内部的状态都由自身维护，只处理内部的渲染逻辑。开放是针对组件通信来说的，指的是不同组件可以通过props（单项数据流）进行数据交互

+ 数据驱动视图

如果要渲染界面，不应该直接操作DOM，而是通过修改数据(state或prop)，数据驱动视图更新

+ 虚拟dom

由浏览器的渲染流水线可知，DOM操作是一个昂贵的操作，很耗性能，因此产生了虚拟DOM。虚拟DOM是对真实DOM的映射，React通过新旧虚拟DOM对比，得到需要更新的部分，实现数据的增量更新

# JSX
JSX全称是JavaScript and xml，React发明了JSX，它是一种可以在JS中编写XML的语言。JSX更像一种模板，类似于Vue中的 template。JSX是JS的语法糖，编译时JSX会通过Babel编译成JS，babel会将jsx转译成一个名为React.createElement()函数调用

作用：用来简化创建虚拟dom

语法规则 ：

+ 定义虚拟dom时，不要写引号
+ 标签混入js表达式时要用{}
+ 内联样式，要用style={{key:value}}的形式去写
+ 样式的类名指定不要用class，要用className
+ 只有一个跟标签
+ 标签必须闭合
+ 标签首字母 
    - 若小写字母开头，则将该标签转为html中同名元素，若html中无该标签对应的同名元素，则报错
    - 若大写字母开头，react就去渲染对应的组件，若组件没有定义，则报错

**React底层调和后，终将变成什么？**

我们写的jsx会先转换成React.element，再转化成React.fiber的过程，React element对象的每一个子节点都会形成一个与之对应的fiber对象，然后通过sibling，return，child将每一个fiber对象联系起来

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698497093636-31d692b5-a18b-4253-b79e-17dda3888a78.png)

fiber对应关系：

+ child：一个由父级fiber指向子级fiber的指针
+ return：一个子级fiber指向父级fiber的指针
+ sibling：一个fiber指向下一个兄弟fiber的指针

**React.createElement参数：**

+ 如果是组件类型，会传入组件对应的类或函数，如果是dom元素类型，传入div或者span之类的字符串，每种类型都会有对应的tag
+ 第二个参数：一个对象，在dom类型中为标签属性，在组建中尉props
+ 其他参数依次为children，根据顺序排列

**jsx可以防止注入攻击：ReactDom在渲染所有输入内容之前，默认会进行转义，它可以确保在你的应用中，永远不会注入那些并非自己明确编写的内容，所有的内容在渲染之前都被转换成了字符串，这样可以有效的防止xss攻击**

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698497032429-3a15a08c-f349-4e62-8e70-378c15b3189b.png)

# 异步渲染
1. 时间分片（Time Slicing）：
+ Time Slicing 是 Fiber 的完全体形态, React 在 渲染 的时候, 会将任务拆分成多个小任务, 这些细分的任务则会在主线程空闲的时候进行执行, 在执行任务的期间可以随时进行暂停
+ 使用时间切片的缺点是, 任务运行的总时间变长了, 这是因为它每处理完一个小任务后, 主线程会空闲出来, 并且在下一个小任务开始处理之前有一小段延迟, 但是为了避免卡死浏览器, 这种取舍是很有必要的
+ 这里使用到了一个原生的 API, window.requestIdleCallback() 该方法参数是一个回调函数, 这个函数将在浏览器空闲时期被调用, 这使开发者能够在主事件循环上执行后台和低优先级工作, 而不会影响延迟关键事件, 如动画和输入响应
2. 悬停或者暂停 (Suspense)：悬停或者暂停 (Suspense)
+ 在 render 函数中, 我们可以写入一个异步请求, 请求数据
+ react 会从我们缓存中读取这个缓存
+ 如果有缓存了, 直接进行正常的 render
+ 如果没有缓存, 那么会抛出一个 异常, 这个异常是一个 promise(很有意思, 通过抛出异常来实现)
+ 当这个 promise 完成后(请求数据完成), react 会继续回到原来的 render 中 (实际上是重新执行一遍 render), 把数据render 出来
+ 完全同步写法, 没有任何异步 callback 之类的东西

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699708487983-0f88a1c0-b9b3-4ead-9e59-a42305e5a1fc.png)

> Suspense 的核心概念与错误边界非常相似, 错误边界能够在应用的任何地方捕捉未捕获的异常, 来处理从该组件下面抛出的所有异常。无独有偶, Suspense 组件捕获任何由子组件抛出的异常(Promise), 不同的是我们并不需要一个特定的组件来充当边界, 因为 Suspense 组件自己就是, 它可以让我们定义 fallback 来决定后备的渲染组件
>

# 使用React.lazy和suspense实现动态加载
```javascript
const LazyComponent = React.lazy(() => import('./test.js'))

export default function Index(){
   return <Suspense fallback={<div>loading...</div>} >
       <LazyComponent />
   </Suspense>
}
```

整个流程是这样的，React.lazy 包裹的组件会标记 REACT_LAZY_TYPE 类型的 element，在调和阶段会变成 LazyComponent 类型的 fiber ，React 对 LazyComponent 会有单独的处理逻辑：

+ 第一次渲染首先会执行 init 方法，里面会执行 lazy 的第一个函数，得到一个Promise，绑定 Promise.then 成功回调，回调里得到将要渲染组件 defaultExport ，这里要注意的是，如上面的函数当第二个 if 判断的时候，因为此时状态不是 Resolved ，所以会走 else ，抛出异常 Promise，抛出异常会让当前渲染终止。
+ 这个异常 Promise 会被 Suspense 捕获到，Suspense 会处理 Promise ，Promise 执行成功回调得到 defaultExport（将想要渲染组件），然后 Susponse 发起第二次渲染，第二次 init 方法已经是 Resolved 成功状态，那么直接返回 result 也就是真正渲染的组件。这时候就可以正常渲染组件了。

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699708782866-802fa0a3-08cd-404e-8baf-843dbb9ea57e.png)

## React.lazy原理
lazy 内部模拟一个 promiseA 规范场景。完全可以理解 React.lazy 用 Promise 模拟了一个请求数据的过程，但是请求的结果不是数据，而是一个动态的组件。下一次渲染就直接渲染这个组件，所以是 React.lazy 利用 Suspense **接收 Promise ，执行 Promise ，然后再渲染**这个特性做到动态加载的。

```javascript
function lazy(ctor){
  return {
     $$typeof: REACT_LAZY_TYPE,
     _payload:{
        _status: -1,  //初始化状态
        _result: ctor,
     },
     _init:function(payload){
       if(payload._status===-1){ /* 第一次执行会走这里  */
          const ctor = payload._result;
          const thenable = ctor();
          payload._status = Pending;
          payload._result = thenable;
          thenable.then((moduleObject)=>{
            const defaultExport = moduleObject.default;
            resolved._status = Resolved; // 1 成功状态
            resolved._result = defaultExport;/* defaultExport 为我们动态加载的组件本身  */ 
          })
       	}
      	if(payload._status === Resolved){ // 成功状态
          return payload._result;
        }
        else {  //第一次会抛出Promise异常给Suspense
          throw payload._result; 
        }
     }
  }
}

```



# 虚拟dom
一层对真实dom的抽象，以js对象作为基础的树，用对象的属性来描述节点，最终通过一系列操作将这棵树映射到真实环境上，最少包含tag（标签名），属性（attrs）和子元素对象（children）

vue是通过createElement生成的vnode

createElement接收五个参数：

1. context：Vnode的上下文环境，是Component类型
2. tag：表示标签，它可以是一个字符串，也可以是一个Component
3. data表示Vnode的数据，它是一个VnodeData类型
4. children表示当前Vnode的子节点，它可以是任意类型的
5. normalizationType表示子节点规范的类型，类型不同规范的方法也就不一样，主要是参考render函数是编译生成的还是用户手写的
6. alwaysNormalize：根据 alwaysNormalize 设置 normalizationType

**使用虚拟dom算法的损耗计算：**

总损耗 = 虚拟dom增删改 + (与diff算法效率有关)真是dom增删改 + （较少的节点）排版和重绘

直接操作真实dom的损耗：

总损耗 = 真实dom完全增删改 + (可能较多的节点的)排版和重绘

**作用：**

dom元素非常庞大，而且页面很多的性能问题都是dom操作引起的，虚拟dom除了diff算法可以优化页面性能以外，其实抽离了原本的渲染过程，实现了跨平台的能力

**缺点：**

+ 极致性能：在一些性能要求极高的应用中, 虚拟 DOM 无法进行针对性的极致优化: 因为从虚拟 DOM 到更新真实 DOM 之间还需要进行一些额外的计算(比如 diff 算法), 而这中间就多了一些消耗, 肯定没有直接操作 DOM 来得快
+ 首次渲染大量 DOM 时, 需要将虚拟树转换为实际的 DOM 元素, 并插入到页面中, 这个过程需要额外的计算和操作, 可能会比直接操作实际 DOM 更慢
+ 适用度: 虚拟 DOM 需要在内存中创建和维护一个额外的虚拟树结构, 用于表示页面的状态。这可能会导致一定的内存消耗增加, 特别是在处理大型或复杂的应用程序时, 所以虚拟 DOM 更适用于动态或频繁变化的内容, 而对于静态内容 (几乎不会变化的部分), 虚拟 DOM 的优势可能不明显, 因为它仍然需要进行比较和更新的计算

## 虚拟dom一定比直接操作真实dom快
+ 同样的功能, 在虚拟 DOM 中必须需要进行更多的计算、损耗, 所以从理论上来讲虚拟 DOM 只会更慢, 但这里其实有个前提, 前提就是操作真实 DOM 的方式要做到最优, 但是单单这一点对于大部分开发人员来说其实是很难的、而且就算做到了也要耗费很多精力, 同时也会增加维护成本。
+ 首次渲染或者所有节点都需要进行更新的时候, 这个时候采用虚拟 DOM 会比直接操作原生 DOM 多一重构建虚拟 DOM 树的操作, 这会更大的占用内存和延长渲染时间
+ 对于频繁更新、删除操作: 直接操作真实 DOM(没有经过优化, 直接操作整个 DOM 树)的情况下, 虚拟 DOM 也行会更快, 因为相对来说操作 DOM 的消耗会比操作 JS 高
+ 得失: 在构建一个实际应用的时候, 出于可维护性的考虑, 我们很难为每一个地方都去做手动优化吗, 但是呢? 虚拟 DOM 在不需要手动优化的情况下, 却能够给我们带来一系列的优化、同时带来更好的开发体验, 当然为此我们也只需要付出一点点性能
+ 总结: **操作真实 DOM 如果能做到最优, 那么必然会比虚拟 DOM 更快, 否则结果就不好说咯**

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698500637884-0c819d59-d751-46e4-9f22-b5503515b55f.png)

## 为什么操作dom会比js代价更大
1. 对比：
+ 访问和修改 DOM 元素需要通过浏览器的底层接口提供的 API 来实现的, 与直接在内存中操作 JavaScript 对象相比, 通过浏览器接口进行 DOM 操作涉及到更多的层级和复杂性, 从而导致性能开销增加
+ DOM 操作引起页面重新渲染和重排, 当对 DOM 元素进行修改时, 浏览器需要重新计算元素的布局和样式, 并重新渲染整个页面或部分页面。这个过程称为重排 (reflow) 和重绘 (repaint), 它对于页面的性能和响应时间有一定的影响, 增加了页面的负担和性能开销
2. 为了减少对 DOM 操作的代价, 可以采取以下优化措施：
+ 批量操作: 将多个 DOM 操作合并成一个批量操作, 减少页面的重排和重绘次数
+ 使用文档片段 (DocumentFragment): 将多个 DOM 元素的操作放在文档片段中, 然后一次性插入到页面中, 减少页面渲染的次数
+ 缓存 DOM 查询结果: 避免多次查询同一个 DOM 元素, 将查询结果缓存在变量中以提高性能。
+ 使用事件委托: 将事件处理程序绑定在父元素上, 通过事件冒泡机制处理子元素的事件, 减少事件绑定的数量

# diff算法
diff算法主要就是查找新旧虚拟dom之间的差异，react中的diff算法时间复杂度为O(n)

1. **diff策略**
+ tree层级：同层级比较

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698502773098-48bc105a-e626-407d-9efc-a7ba9f9900cb.png)

+ component层级：如果是同一个类型的组件, 则会继续往下 diff 运算, 如果不是一个类型组件, 那么将直接删除这个组件下的所有子节点, 然后创建新的 DOM。如下图所示, 当 D 类型组件换成了 G 后, 即使两者的结构非常类似, 也会将 D 类型的组件删除再重新创建 G

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698502811333-dc414ef8-8162-4b49-aa2a-b489a7c781d3.png)

+ element层级：是同一层级的节点的比较规则, 根据每个节点在对应层级的唯一 key 作为标识, 并且对于同一层级的节点操作只有 3 种, 分别为 INSERT_MARKUP(插入)、MOVE_EXISTING(移动)、REMOVE_NODE(删除)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698502848261-4f5c6a8e-66df-4881-8939-682fcb93d0fd.png)

> 1. key 的值必须保证 唯一 且 稳定, 有了 key 属性后, 就可以与组件建立了一种对应关系, react 根据 key 来决定是销毁还是重新创建组件, 是更新还是移动组件
> 2. index 的使用存在的问题: 大部分情况下可能没有啥问题, 但是如果涉及到数据变更(更新、新增、删除), 这时 index 作为 key 会导致展示错误的数据, 其实归根结底, 使用 index 的问题在于两次渲染的 index 是相同的, 所以组件并不会重新销毁创建, 而是直接进行更新
>

> **react不能通过双端对比进行diff算法优化是因为目前fiber上没有设置反向链表**
>

1. **从左向右老节点进行比对查找能复用的旧节点，如果有老节点比对不成功的，则停止这一轮的比对，并记录停止的位置**
2. **如果第一轮比对，能把所有的新节点都比对完毕，则删除旧节点还没进行比对的节点**
3. **如果第一轮的比对，没能将所有的新节点都比对完毕，则继续从第一轮比对停值的位置继续开始循环新节点，拿每一个新节点去老节点里面进行查找，有匹配的则复用，没匹配成功的则在协调位置的时候打上placement的标记**
4. **在所有新节点比对完毕之后，检查还有没有没进行复用的旧节点，如果有，就全部删除**

# React 17 不再需要引入在组件中显式地引入 React 这又是为什么呢?
+ React 更新引入了 react/jsx-runtime, 改变了 JSX 编译模式, 不再是 React.createElement
+ 同时编译工具(react 的预设 @babel/preset-react), 针对 jsx 不但会帮我们进行编译, 还会帮我们手动引入所需要的包
+ 那早期版本是不是更新了 @babel/preset-react 也可以不需要手动引入? 不可以, 因为这里是使用新的编译方式, 旧的版本并不支持

# 事件
事件系统可分为三个部分：

1. 事件合成系统，初始化会注册不同的事件插件
2. 在一次渲染过程中，对事件标签中事件的收集，向 container 注册事件
3. 一次用户交互，事件触发，到事件执行一系列过程

## 事件合成
1. 概念：React 应用中，元素绑定的事件并不是原生事件，而是React 合成的事件，比如 onClick 是由 click 合成，onChange 是由 blur ，change ，focus 等多个事件合成。

> 绑定事件并不是一次性绑定所有事件，比如发现了 onClick 事件，就会绑定 click 事件，比如发现 onChange 事件，会绑定 [blur，change ，focus ，keydown，keyup] 多个事件。
>

### 事件插件机制
React 有一种事件插件机制，比如上述 onClick 和 onChange ，会有不同的事件插件 SimpleEventPlugin ，ChangeEventPlugin 处理.

**为什么要用不同的事件插件处理不同的react事件？首先对于不同的事件，有不同的处理逻辑；对应的事件源对象也有所不同，React 的事件和事件源是自己合成的，所以对于不同事件需要不同的事件插件处理。**

```javascript
const registrationNameModules = {
    onBlur: SimpleEventPlugin,
    onClick: SimpleEventPlugin,
    onClickCapture: SimpleEventPlugin,
    onChange: ChangeEventPlugin,
    onChangeCapture: ChangeEventPlugin,
    onMouseEnter: EnterLeaveEventPlugin,
    onMouseLeave: EnterLeaveEventPlugin,
    ...
}
```

## 事件注册机制
1. 通过事件委托的方式，将所有事件都绑定在了document来进行统一处理。从v17.0.0开始, React 不会再将事件处理添加到 document 上, 而是将事件处理添加到渲染 React 树的根 DOM 容器中。不过无论是在document上还是根dom容器上监听事件，都可以归为事件委托

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699717574194-42bb1299-3047-4d38-a3fd-14e0c0f0cd08.png)

2. 每次绑定都会将事件处理函数，存储起来

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698505369592-58bf4d4d-9913-44d4-8742-e41fb02d6ebe.png)

**组件挂载阶段时，ReactDOM.render()，调用react.createElement，得到虚拟dom树，然后处理组件props是否是事件，得到事件类型和回调函数，ducument上注册对应的事件类型，存储事件回调到listenerBank中**

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699718737028-8cbb10c3-d778-4505-be21-50f51341e6f0.png)

### 对于同一个DOM分别绑定原生事件，合成事件中阻止事件冒泡为什么会阻止合成事件的执行？
合成事件是事件委托的一种实现, 主要是利用事件冒泡机制将所有事件在 document 进行统一处理, 根据 事件流, 事件执行顺序为 捕获阶段、目标阶段、冒泡阶段, 当我们在原生事件上阻止事件冒泡, 那么事件就无法冒泡到 document, 那么合成事件自然无法执行！

### React为什么要将所有事件绑定到document上？
**优点：**

+ 减少事件注册, 减少内存消耗, 提升性能, 不需要注册那么多的事件了, 一种事件类型只在 document 上注册一次即可; 
+ 统一处理, 并提供合成事件对象, 抹平浏览器的兼容性差异

**缺点：**如果层级过多, 冒泡过程中可能会被某层给阻止掉

### 从 v17.0.0 开始, React 不再将事件处理添加到 document 上, 而是将事件处理添加到渲染 React 树的根容器中这又是为什么呢？
+ 如果页面上有多个 React 版本, 事件都会被附加在 document 上, 这时嵌套的 React 树调用 e.stopPropagation() 停止了事件冒泡, 外部的树仍会接收到该事件(因为只是阻止了 React 事件的冒泡), 这就使嵌套不同版本的 React 难以实现
+ 如果你系统只用了一个 react 版本, 那没啥区别; 但有些复杂的系统, 由于历史原因, 或者用了微前端, 它就同时用很多个版本的 react, 这就不一样了, 如果很多个版本的 react, 都往 document 上去绑定, 就容易出现混乱

## 阻止默认行为
原生事件：e.preventDefault() 和 return false 可以用来阻止事件默认行为，由于在 React 中给元素的事件并不是真正的事件处理函数。所以导致 return false 方法在 React 应用中完全失去了作用。

React事件 在React应用中，可以用 e.preventDefault() 阻止事件默认行为，这个方法并非是原生事件的 preventDefault ，由于 React 事件源 e 也是独立组建的，所以 preventDefault 也是单独处理的。

## 事件触发流程
1. 批量更新：执行dispatchEvent，会传入真的事件源 button 元素本身。通过元素可以找到 button 对应的 fiber。

fiber和原生dom建立连接的方式：

React 在初始化真实 DOM 的时候，用一个随机的 key internalInstanceKey 指针指向了当前 DOM 对应的 fiber 对象，fiber 对象用 stateNode 指向了当前的 DOM 元素。

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699719310256-c5a0ea62-dd6a-473c-aaac-a1ae4e8be625.png)

2. 合成事件源：通过 onClick 找到对应的处理插件 SimpleEventPlugin ，合成新的事件源 e ，里面包含了 preventDefault 和 stopPropagation 等方法
3. 形成事件执行队列：

在第一步通过原生 DOM 获取到对应的 fiber ，接着会从这个 fiber 向上遍历，遇到元素类型 fiber ，就会收集事件，用一个数组收集事件：

    - 如果遇到捕获阶段事件 onClickCapture ，就会 unshift 放在数组前面。以此模拟事件捕获阶段。
    - 如果遇到冒泡阶段事件 onClick ，就会 push 到数组后面，模拟事件冒泡阶段。
    - 一直收集到最顶端 app ，形成执行队列，在接下来阶段，依次执行队列里面的函数。

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699720504314-563f08ed-8495-4f1e-8aef-2c4f759c248d.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699720517762-ad732fa2-9f67-4df3-b3c2-4fb3440a2a20.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699720559889-1c6ca420-89d0-40f8-8ed8-f24f76377c24.png)

## React 18事件
### 事件初始化
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699721476586-9e30f461-31fa-4499-9ef7-d70161df487e.png)

在 React 新版的事件系统中，在 createRoot 会一口气向外层容器上注册完全部事件。

listenToAllSupportedEvents函数主要目的就是通过 listenToNativeEvent 绑定浏览器事件，这里引出了两个常量，allNativeEvents 和 nonDelegatedEvents ，它们分别代表的意思如下：

+ allNativeEvents：allNativeEvents 是一个 set 集合，保存了 81 个浏览器常用事件。 
+ nonDelegatedEvents ：这个也是一个集合，保存了浏览器中不会冒泡的事件，一般指的是媒体事件，比如 pause，play，playing 等，还有一些特殊事件，比如 cancel ，close，invalid，load，scroll 。

此时如果发生一次点击事件，就会触发两次 dispatchEvent ：

+ 第一次捕获阶段的点击事件；
+ 第二次冒泡阶段的点击事件；

```javascript
function listenToAllSupportedEvents(rootContainerElement) {
    /* allNativeEvents 是一个 set 集合，保存了大多数的浏览器事件 */
    allNativeEvents.forEach(function (domEventName) {
      if (domEventName !== 'selectionchange') {
         /* nonDelegatedEvents 保存了 js 中，不冒泡的事件 */ 
        if (!nonDelegatedEvents.has(domEventName)) {
          /* 在冒泡阶段绑定事件 */ 
          listenToNativeEvent(domEventName, false, rootContainerElement);
        }
        /* 在捕获阶段绑定事件 */
        listenToNativeEvent(domEventName, true, rootContainerElement);
      }
    });
}
// 如果事件是不冒泡的，那么会执行一次，listenToNativeEvent，第二个参数为 true 。 
// 如果是常规的事件，那么会执行两次 listenToNativeEvent，分别在冒泡和捕获阶段绑定事件。
function listenToNativeEvent(domEventName, isCapturePhaseListener, rootContainerElement){
  var listener = dispatchEvent.bind(null,domEventName,...)
  if(isCapturePhaseListener){
      target.addEventListener(eventType, dispatchEvent, true);
  }else{
      target.addEventListener(eventType, dispatchEvent, false);
  }
}
```

### 事件触发
1. 执行 dispatchEvent 事件：dispatchEvent 如果是正常的事件，就会通过 batchedUpdates（批量更新） 来处理 dispatchEventsForPlugins ，dispatchEventsForPlugins该函数会首先通过 getEventTarget 找到发生事件的元素，也就是事件源。然后创建一个待更新的事件队列，接下来通过 extractEvents 找到待更新的事件，然后通过 processDispatchQueue 执行事件。

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699722443059-22d85e0c-b784-41ff-8313-61536eefa6d3.png)

# react应用的启动模式
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698891730765-e727e083-e5c1-453b-b6c5-9801e1030372.png)

1. legacy模式：ReactDOM.render(<App />, rootNode). 这是当前 React app 使用的方式. 这个模式可能不支持[这些新功能(concurrent 支持的所有功能)](https://zh-hans.reactjs.org/docs/concurrent-mode-patterns.html#the-three-steps)
2. Blocking模式：ReactDOM.createBlockingRoot(rootNode).render(<App />). 目前正在实验中, 它仅提供了 concurrent 模式的小部分功能, 作为迁移到 concurrent 模式的第一个步骤.
3. concurrent模式：ReactDOM.createRoot(rootNode).render(<App />). 目前在实验中, 未来稳定之后，打算作为 React 的默认开发模式. 这个模式开启了所有的新功能.

3 种模式在调用更新时都会执行updateContainer. updateContainer函数串联了react-dom与react-reconciler, 之后的逻辑进入了react-reconciler包

无论是哪种模式，react在初始化的时候都会创建三个全局对象

+ ReactDOM(Blocking)Root

属于react-dom包, 该对象[暴露有render,unmount方法](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/client/ReactDOMRoot.js#L62-L104), 通过调用该实例的render方法, 可以引导 react 应用的启动.

+ fiberRoot

属于react-reconciler包, 作为react-reconciler在运行过程中的全局上下文, 保存 fiber 构建过程中所依赖的全局状态.

其大部分实例变量用来存储fiber 构造循环(详见[两大工作循环](https://react-illustration-series.osrc.com/main/workloop))过程的各种状态.react 应用内部, 可以根据这些实例变量的值, 控制执行逻辑.

> 无论哪种模式下，在ReactDom(Blocking)Root的创建过程中, 都会调用一个相同的函数createRootImpl, 查看后续的函数调用, 最后会创建fiberRoot 对象(在这个过程中, 特别注意RootTag的传递过程):
>
> 三种模式下的tag是各不相同（分别是ConcurrentRoot, BlockingRoot, LegacyRoot）
>

+ HostRootFiber

属于react-reconciler包, 这是 react 应用中的第一个 Fiber 对象, 是 Fiber 树的根节点, 节点的类型是HostRoot.

在创建HostRootFiber时, 其中fiber.mode属性, 会与 3 种RootTag(ConcurrentRoot,BlockingRoot,LegacyRoot)关联起来.

注意:fiber树中所有节点的mode都会和HostRootFiber.mode一致(新建的 fiber 节点, 其 mode 来源于父节点),所以**HostRootFiber.mode**非常重要, 它决定了以后整个 fiber 树构建过程.

# 调度
GUI 渲染线程和 JS 引擎线程是相互排斥的，比如开发者用 js 写了一个遍历大量数据的循环，在执行 js 时候，会阻塞浏览器的渲染绘制，给用户直观的感受就是卡顿。vue是通过template 模版收集依赖的过程，轻松构建响应式，使得在一次更新中，vue 能够迅速响应，找到需要更新的范围，然后以组件粒度更新组件，渲染视图。但是在 React 中，一次更新 React 无法知道此次更新的波及范围，所以 React 选择从根节点开始 diff ，查找不同，更新这些不同。所以react使用：**如果浏览器有绘制任务那么执行绘制任务，在空闲时间执行更新任务**

## 时间分片
浏览器每次执行一次事件循环（一帧）都会做如下事情：处理事件，执行 js ，调用 requestAnimation ，布局 Layout ，绘制 Paint ，在一帧执行后，如果没有其他事件，那么浏览器会进入休息时间，那么有的一些不是特别紧急 React 更新，就可以执行了。

使用[requestIdleCallback](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestIdleCallback)在浏览器有空余的时间，浏览器就会调用 requestIdleCallback 的回调

```javascript
requestIdleCallback(callback,{ timeout })
```

+ callback 回调，浏览器空余时间执行回调函数。
+ timeout 超时时间。如果浏览器长时间没有空闲，那么回调就不会执行，为了解决这个问题，可以通过 requestIdleCallback 的第二个参数指定一个超时时间。

React 为了防止 requestIdleCallback 中的任务由于浏览器没有空闲时间而卡死，所以设置了 5 个优先级。

+ Immediate -1 需要立刻执行。
+ UserBlocking 250ms 超时时间250ms，一般指的是用户交互。
+ Normal 5000ms 超时时间5s，不需要直观立即变化的任务，比如网络请求。
+ Low 10000ms 超时时间10s，肯定要执行的任务，但是可以放在最后处理。
+ Idle 一些没有必要的任务，可能不会执行。

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699770044145-907b785f-c9a3-4620-9d12-556970d2bdd9.png)

React中使用messagechannel来实现时间片, **MessageChannel在浏览器事件循环中属于宏任务, 所以调度中心永远是异步执行回调函数.**

> 为了让视图流畅地运行，可以按照人类能感知到最低限度每秒 60 帧的频率划分时间片，这样每个时间片就是 16ms 。也就是这 16 毫秒要完成如上 js 执行，浏览器绘制等操作
>

```javascript
  let scheduledHostCallback = null 
  /* 建立一个消息通道 */
  var channel = new MessageChannel();
  /* 建立一个port发送消息 */
  var port = channel.port2;

  channel.port1.onmessage = function(){
      /* 执行任务 */
      scheduledHostCallback() 
      /* 执行完毕，清空任务 */
      scheduledHostCallback = null
  };
  /* 向浏览器请求执行更新任务 */
  requestHostCallback = function (callback) {
    scheduledHostCallback = callback;
    if (!isMessageLoopRunning) {
      isMessageLoopRunning = true;
      port.postMessage(null);
    }
  };
```

+ 在一次更新中，React 会调用 requestHostCallback ，把更新任务赋值给 scheduledHostCallback ，然后 port2 向 port1 发起 postMessage 消息通知。
+ port1 会通过 onmessage ，接受来自 port2 消息，然后执行更新任务 scheduledHostCallback ，然后置空 scheduledHostCallback ，借此达到异步执行目的。

## 异步调度原理
react发生一次更新会统一走 ensureRootIsScheduled（注册调度任务）

1. 前半部分: 判断是否需要注册新的调度(如果无需新的调度, 会退出函数)
2. 后半部分：注册调度任务
+ performSyncWorkOnRoot或performConcurrentWorkOnRoot被封装到了任务回调(scheduleCallback)中
+ 等待调度中心执行任务, 任务运行其实就是执行performSyncWorkOnRoot或performConcurrentWorkOnRoot
    - 对于正常更新会走 performSyncWorkOnRoot 逻辑，最后会走 workLoopSync
    - 对于低优先级的异步更新会走 performConcurrentWorkOnRoot 逻辑，最后会走 workLoopConcurrent

performSyncWorkOnRoot：

    1. fiber 树构造
    2. 异常处理: 有可能 fiber 构造过程中出现异常
    3. 调用输出

> performConcurrentWorkOnRoot的逻辑与performSyncWorkOnRoot的不同之处在于, 对于可中断渲染的支持:
>
> 1. 调用performConcurrentWorkOnRoot函数时, 首先检查是否处于render过程中, 是否需要恢复上一次渲染.
> 2. 如果本次渲染被中断, 最后返回一个新的 performConcurrentWorkOnRoot 函数, 等待下一次调用.
>

## 任务队列管理
在Scheduler.js中, 维护了一个taskQueue, 任务队列管理就是围绕这个taskQueue展开.

```javascript
// Tasks are stored on a min heap
var taskQueue = []; // 小顶堆数组，堆排
var timerQueue = [];  // 预留给延时任务使用的
```

taskQuene：里面存的都是过期的任务，依据任务的过期时间( expirationTime ) 排序，需要在调度的 workLoop 中循环执行完这些任务。

timerQueue：里面存的都是没有过期的任务，依据任务的开始时间( startTime )排序，在调度 workLoop 中 会用advanceTimers检查任务是否过期，如果过期了，放入 taskQueue 队列。

### 创建任务：scheduleCallback
无论是正常更新任务 workLoopSync 还是低优先级的任务 workLoopConcurrent，都会交由调度器scheduleCallback统一调度的

对于正常更新任务，最后会变成类似如下结构：

```javascript
scheduleCallback(Immediate,workLoopSync)
```

对于异步任务：

```javascript
currentTime = getCurrentTime();
/* 计算超时等级，就是如上那五个等级 */
var priorityLevel = inferPriorityFromExpirationTime(currentTime, expirationTime);
scheduleCallback(priorityLevel,workLoopConcurrent)
```

scheduleCallback函数：

1. 获取当前时间，getCurrentTime()
2. 根据传入的优先级, 设置任务的过期时间 expirationTime
3. 创建新任务
4. 通过任务的开始时间( startTime ) 和 当前时间( currentTime ) 比较:当 startTime > currentTime, 说明未过期, 存到 timerQueue，当 startTime <= currentTime, 说明已过期, 存到 taskQueue。
5. 如果任务过期，并且没有调度中的任务，那么调度 requestHostCallback。本质上调度的是 flushWork
6. 如果任务没有过期，用 requestHostTimeout 延时执行 handleTimeout

```javascript
function scheduleCallback(){
   /* 计算过期时间：超时时间  = 开始时间（现在时间） + 任务超时的时间（上述设置那五个等级）     */
   const expirationTime = startTime + timeout;
   /* 创建一个新任务 */
   const newTask = { ... }
  if (startTime > currentTime) {
      /* 通过开始时间排序 */
      newTask.sortIndex = startTime;
      /* 把任务放在timerQueue中 */
      push(timerQueue, newTask);
      /*  执行setTimeout ， */
      requestHostTimeout(handleTimeout, startTime - currentTime);
  }else{
    /* 通过 expirationTime 排序  */
    newTask.sortIndex = expirationTime;  
    /* 把任务放入taskQueue */
    push(taskQueue, newTask);
    /*没有处于调度中的任务， 然后向浏览器请求一帧，浏览器空闲执行 flushWork */
     if (!isHostCallbackScheduled && !isPerformingWork) {
        isHostCallbackScheduled = true;
         requestHostCallback(flushWork)
     }
  }
} 
```

#### requestHostTimeout
当一个任务，没有超时，那么 React 把它放入 timerQueue中了，但是它什么时候执行呢 ？这个时候 Schedule 用 requestHostTimeout 让一个未过期的任务能够到达恰好过期的状态， 那么需要延迟 startTime - currentTime 毫秒就可以了。requestHostTimeout 就是通过 setTimeout 来进行延时指定时间的。

requestHostTimeout 延时执行 handleTimeout，cancelHostTimeout 用于清除当前的延时器。

```javascript
requestHostTimeout = function (cb, ms) {
  _timeoutID = setTimeout(cb, ms);
};
cancelHostTimeout = function () {
  clearTimeout(_timeoutID);
};
```

#### handleTimeout
延时指定时间后，调用的 handleTimeout 函数， handleTimeout 会把任务重新放在 requestHostCallback 调度。

+ 通过 advanceTimers 将 timeQueue 中过期的任务转移到 taskQueue 中。
+ 然后调用 requestHostCallback 调度过期的任务。

```javascript
function handleTimeout(){
  isHostTimeoutScheduled = false;
  /* 将 timeQueue 中过期的任务，放在 taskQueue 中 。 */
  advanceTimers(currentTime);
  /* 如果没有处于调度中 */
  if(!isHostCallbackScheduled){
      /* 判断有没有过期的任务， */
      if (peek(taskQueue) !== null) {   
      isHostCallbackScheduled = true;
      /* 开启调度任务 */
      requestHostCallback(flushWork);
    }
  }
}
```

#### advanceTimers
_如果任务已经过期，那么将 timerQueue 中的过期任务，放入taskQueue_

```javascript
function advanceTimers(){
   var timer = peek(timerQueue);
   while (timer !== null) {
      if(timer.callback === null){
        pop(timerQueue);
      }else if(timer.startTime <= currentTime){ /* 如果任务已经过期，那么将 timerQueue 中的过期任务，放入taskQueue */
         pop(timerQueue);
         timer.sortIndex = timer.expirationTime;
         push(taskQueue, timer);
      }
   }
}
```

### 消费任务
React 的更新任务最后都是放在 taskQueue 中的。

创建任务之后, 最后请求调度requestHostCallback(flushWork)，flushWork函数作为参数被传入调度中心内核（MessageChannel）等待回调, 在调度中心中, 只需下一个事件循环就会执行回调, 最终执行flushWork。

<font style="color:rgb(37, 41, 51);">flushWork 如果有延时任务执行的话，那么会先暂停延时任务，然后调用 workLoop(任务调度循环) ，去真正执行超时的更新任务</font>

```javascript
function flushWork(){
  if (isHostTimeoutScheduled) { /* 如果有延时任务，那么先暂定延时任务*/
    isHostTimeoutScheduled = false;
    cancelHostTimeout();
  }
  try{
     /* 执行 workLoop 里面会真正调度我们的事件  */
     workLoop(hasTimeRemaining, initialTime)
  }
}
```

workloop：<font style="color:rgb(37, 41, 51);">是调度中的 workLoop，会依次更新过期任务队列中的任务</font>

保存当前时间，用于判断任务是否过期，获取队列中的第一个任务，当该任务存在的时候：

+ 当该任务没有过期但是执行时间超过了限制，则(毕竟只有5ms, shouldYieldToHost()返回true). 停止继续执行, 让出主线程
+ 获取到当前任务的回调
    - 如果该回调为函数则执行，回调完成后，判断是否还有连续（派生）回调
        * 如果产生了连续回调（如fiber树太大, 出现了中断渲染），保留currentTask
        * 没有的话则将currentTask移出队列
    - 如果任务被取消，也就是说currentTask.callback = null，将其移出队列
+ 更新currentTask（队列中的第一个）
+ 当currentTask队列没有清空的时候, 返回true. 等待调度中心下一次回调；task队列清空的话则返回false

```javascript
// 省略部分无关代码
function workLoop(hasTimeRemaining, initialTime) {
  let currentTime = initialTime; // 保存当前时间, 用于判断任务是否过期
  currentTask = peek(taskQueue); // 获取队列中的第一个任务
  while (currentTask !== null) {
    if (
      currentTask.expirationTime > currentTime &&
      (!hasTimeRemaining || shouldYieldToHost())
    ) {
      // 虽然currentTask没有过期, 但是执行时间超过了限制(毕竟只有5ms, shouldYieldToHost()返回true). 停止继续执行, 让出主线程
      break;
    }
    const callback = currentTask.callback;
    if (typeof callback === 'function') {
      currentTask.callback = null;
      currentPriorityLevel = currentTask.priorityLevel;
      const didUserCallbackTimeout = currentTask.expirationTime <= currentTime;
      // 执行回调
      const continuationCallback = callback(didUserCallbackTimeout);
      currentTime = getCurrentTime();
      // 回调完成, 判断是否还有连续(派生)回调
      if (typeof continuationCallback === 'function') {
        // 产生了连续回调(如fiber树太大, 出现了中断渲染), 保留currentTask
        currentTask.callback = continuationCallback;
      } else {
        // 把currentTask移出队列
        if (currentTask === peek(taskQueue)) {
          pop(taskQueue);
        }
      }
    } else {
      // 如果任务被取消(这时currentTask.callback = null), 将其移出队列
      pop(taskQueue);
    }
    // 更新currentTask
    currentTask = peek(taskQueue);
  }
  if (currentTask !== null) {
    return true; // 如果task队列没有清空, 返回true. 等待调度中心下一次回调
  } else {
    return false; // task队列已经清空, 返回false.
  }
}
```

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699776222404-122505dd-3840-46b8-8eeb-c288c3b82cc0.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699776246098-68831373-5810-4ced-ac53-3add529e57f9.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699776196148-a9ba0b23-7608-4886-a6e8-9dcfae762e8e.png)

# 调和与fiber
## ReactElement，Fiber，DOM三者的关系
1. ReactElement对象：<font style="color:rgb(37, 41, 51);">是 React 视图层在代码层级上的表象，也就是开发者写的 jsx 语法。</font>所有采用jsx语法树写的节点，都会被编译器转换，最终会以React.createElement(...)的方式, 创建出来一个与之对应的ReactElement对象
2. fiber对象：fiber对象是通过ReactElement对象进行创建的，多个fiber对象构成了一棵fiber树，fiber树是构造DOM树的数据模型，fiber树的任何改动，最后都体现到DOM树
3. DOM对象：<font style="color:rgb(37, 41, 51);">元素在浏览器上给用户直观的表象，</font>DOM将文档解析为一个由节点和对象（包含属性和方法的对象）组成的结构集合, 也就是常说的DOM树.

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699776553494-959aa0d7-380d-4f8a-af56-299c309e05a1.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698936711372-4881f777-6aee-4495-b717-420b2528fcf4.png)

## React组件的渲染主要经历的两个阶段
+ **调度阶段(Reconciler)**：这个阶段 React 用新数据生成新的虚拟 DOM, 遍历虚拟 DOM, 然后通过 Diff 算法, 快速找出需要更新的元素, 放到更新队列中去
+ **渲染阶段(Renderer)**：这个阶段 React 根据所在的渲染环境, 遍历更新队列, 将对应元素更新(在浏览器中, 就是更新对应的 DOM 元素)

调度阶段新老架构中有不同的处理方式：

+ 早期 16 之前 React 在 diff 阶段是通过一个自顶向下递归算法，来查找需要对当前 DOM 进行更新或替换的操作列表，一旦开始，会持续占用主线程，很难被中断，当虚拟 DOM 特别庞大的时候，主线程就被长期占用，页面的交互、布局、渲染会被停止，造成页面的卡顿，

> **这里举个例子：**假设更新一个组件需要 1ms，如果有 200 个组件要更新，那就需要 200ms，在这 200ms 的更新过程中，浏览器唯一的主线程都在专心运行更新操作，无暇去做任何其他的事情。想象一下，在这 200ms 内，用户往一个 input 元素中输入点什么，敲击键盘也不会获得响应，因为渲染输入按键结果也是浏览器主线程的工作，但是浏览器主线程被 React 占用，抽不出空，最后的结果就是用户敲了按键看不到反应，等 React 更新过程结束之后，那些按键会一下出现在 input 元素里，这就是所谓的界面卡顿。
>

+ Fiber是React 16中采用的新的调度处理方法，主要目标是支持虚拟dom的一个渐进式渲染

## Fiber的设计思路
> 因为浏览器的页面是一帧一帧绘制出来的, 当每秒绘制的帧数(FPS)达到 60 时, 页面是流畅的, 小于这个值时, 用户会感觉到卡顿; 转换成时间就是 16ms(10000 / 60) 内如果当前帧内执行的任务没有完成, 就会造成卡顿；
>

1. Fiber：是实现了一个基于优先级和 requestIdleCallback(执行的前提条件是当前浏览器处于空闲状态) 的一个循环 任务调度 算法, 他在渲染虚拟 DOM、 diff 阶段将任务拆分为多个小任务、这样的话就可以随时进行中止和恢复、同时又根据每个任务的优先级来执行任务
2. Fiber是把render/update分片，拆解成多个小任务来执行，每次只检查树上部分节点，做完此部分后，若当前一帧（16ms）内还有足够的时间就继续做下一个小任务, 时间不够就停止操作, 等主线程空闲时再恢复
3. Fiber 是根据一个 fiber 节点 (VDOM 节点) 进行来拆分, 以 fiber node 为一个任务单元, 一个组件实例都是一个任务单元，任务循环中, 每处理完一个 fiber node, 可以中断/挂起/恢复
4. 不同的任务分配不同的优先级, Fiber 根据任务的优先级来动态调整任务调度, 先做高优先级的任务
+ Immediate：最高优先级, 会马上执行的不能中断
+ UserBlocking：这一般是用户交互的结果, 需要及时反馈
+ Normal：普通等级的, 比如网络请求等不需要用户立即感受到变化的
+ Low：低优先级的, 这种任务可以延后, 但最后始终是要执行的
+ Idle：最低等级的任务, 可以被无限延迟的, 比如 console.log()

## fiber保存的信息
```javascript
function FiberNode(){
  this.tag = tag;                  // fiber 标签 证明是什么类型fiber。
  this.key = key;                  // key调和子节点时候用到。 
  this.type = null;                // dom元素是对应的元素类型，比如div，组件指向组件对应的类或者函数。  
  this.stateNode = null;           // 指向对应的真实dom元素，类组件指向组件实例，可以被ref获取。
  this.return = null;              // 指向父级fiber
  this.child = null;               // 指向子级fiber
  this.sibling = null;             // 指向兄弟fiber 
  this.index = 0;                  // 索引
  this.ref = null;                 // ref指向，ref函数，或者ref对象。
  this.pendingProps = pendingProps;// 在一次更新中，代表element创建
  this.memoizedProps = null;       // 记录上一次更新完毕后的props
  this.updateQueue = null;         // 类组件存放setState更新队列，函数组件存放
  this.memoizedState = null;       // 类组件保存state信息，函数组件保存hooks信息，dom元素为null
  this.dependencies = null;        // context或是时间的依赖项
  this.mode = mode;                //描述fiber树的模式，比如 ConcurrentMode 模式
  this.effectTag = NoEffect;       // effect标签，用于收集effectList
  this.nextEffect = null;          // 指向下一个effect
  this.firstEffect = null;         // 第一个effect
  this.lastEffect = null;          // 最后一个effect
  this.expirationTime = NoWork;    // 通过不同过期时间，判断任务是否过期， 在v17版本用lane表示。
  this.alternate = null;           // 双缓存树，指向缓存的fiber。更新阶段，两颗树互相交替。
}
```

## 双缓冲技术（double buffering）
> 双缓存：<font style="color:rgb(37, 41, 51);">canvas 绘制动画的时候，如果上一帧计算量比较大，导致清除上一帧画面到绘制当前帧画面之间有较长间隙，就会出现白屏。为了解决这个问题，canvas 在内存中绘制当前动画，绘制完毕后直接用当前帧替换上一帧画面，由于省去了两帧替换间的计算时间，不会出现从白屏到出现画面的闪烁情况。这种在内存中构建并直接替换的技术叫做</font>**<font style="color:rgb(37, 41, 51);">双缓存</font>**<font style="color:rgb(37, 41, 51);">。</font>
>

workInProgress的应用实际上就是React的双缓冲技术(double buffering).

fiber的过程就是把ReactElement转换成fiber树的过程，在这个过程中，内存里会同时存在2棵fiber树：

1. <font style="color:rgb(37, 41, 51);">current：</font>代表当前界面的fiber树（已经被展示出来，挂载到fiberRoot.current上），如果是初次构造（初始化渲染），页面还没有渲染，此时界面对应的fiber树为空（fiberRoot.current = null）
2. <font style="color:rgb(37, 41, 51);">workInProgress：</font>正在构造的fiber树（即将展示出来，挂载到HostRootFiber.alternate上, 正在构造的节点称为workInProgress）。当构造完成之后, 重新渲染页面, 最后切换fiberRoot.current = workInProgress, 使得fiberRoot.current重新指向代表当前界面的fiber树。

两个全局变量：fiberRoot和HostRootFiber

1. 构造过程中，fiberRoot.current指向当前界面对应的fiber树.

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698975590790-1796af8f-f336-4c83-8d25-961272a390c1.png)

2. 构造完成并渲染, 切换fiberRoot.current指针, 使其继续指向当前界面对应的fiber树(原来代表界面的 fiber 树, 变成了内存中).

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698975685940-f4d7d438-a6a9-4fbb-9845-dbecc4527373.png)

## fiber更新机制
### 初始化
1. **<font style="color:rgb(37, 41, 51);">启动阶段：创建fiberRoot和hostRootFiber</font>**
+ fiberRoot：首次构建应用， 创建一个 fiberRoot ，作为整个 React 应用的根基。
+ rootFiber： 如下通过 ReactDOM.render 渲染出来的，如上 Index 可以作为一个 rootFiber。一个 React 应用可以有多 ReactDOM.render 创建的 rootFiber ，但是只能有一个 fiberRoot（应用根节点）。

```javascript
ReactDOM.render(<Index/>, document.getElementById('app'));
```

<font style="color:rgb(37, 41, 51);">第一次挂载的过程中，会将 fiberRoot 和 rootFiber 建立起关联</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699777347566-c02decc5-9519-4e6a-b8e0-7b4c760c27af.png)

2. **<font style="color:rgb(37, 41, 51);">构造阶段：workInProgress和current</font>**
+ <font style="color:rgb(37, 41, 51);">workInProgress是：正在内存中构建的 Fiber 树称为 workInProgress Fiber 树。在一次更新中，所有的更新都是发生在 workInProgress 树上。在一次更新之后，workInProgress 树上的状态是最新的状态，那么它将变成 current 树用于渲染视图。</font>
+ <font style="color:rgb(37, 41, 51);">current：正在视图层渲染的树叫做 current 树。</font>

<font style="color:rgb(37, 41, 51);">会到 rootFiber 的渲染流程，首先会复用当前 current 树（ rootFiber ）的 </font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">alternate</font><font style="color:rgb(37, 41, 51);"> 作为 workInProgress ，如果没有 alternate （初始化的 rootFiber 是没有 alternate ），那么会创建一个 fiber 作为 workInProgress 。会用 alternate 将新创建的 workInProgress 与 current 树建立起关联。这个关联过程只有初始化第一次创建 alternate 时候进行。</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699777461958-3b5d6998-9cfa-4a23-86c1-f81156d072af.png)

3. **<font style="color:rgb(37, 41, 51);">深度调和子节点，渲染视图</font>**

<font style="color:rgb(37, 41, 51);">在新创建的 alternates 上，完成整个 fiber 树的遍历，包括 fiber 的创建。</font>

在深度优先遍历中, 每个fiber节点都会经历 2 个阶段：

    1. **探寻阶段 beginWork，向下调和。**<font style="color:rgb(37, 41, 51);">由 fiberRoot 按照 child 指针逐层向下调和，期间会执行函数组件，实例类组件，diff 调和子节点，打不同effectTag。</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699781932196-cb49a6c1-0ad9-4250-91b0-b4b0f4d0e627.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699779373760-f1bdb2db-9dfd-4772-9899-65d04ed1a8f6.png)

beginWork(current, unitOfWork, subtreeRenderLanes)针对所有的 Fiber 类型, 其中的每一个 case 处理一种 Fiber 类型. updateXXX函数(如: updateHostRoot, updateClassComponent 等)的主要逻辑：

    1. 根据 ReactElement对象创建所有的fiber节点, 最终构造出fiber树形结构(设置return和sibling指针)
    2. 设置fiber.flags(二进制形式变量, 用来标记 fiber节点 的增,删,改状态, 等待completeWork阶段处理)
    3. 设置fiber.stateNode局部状态(如Class类型节点: fiber.stateNode=new Class())

updatexxx函数不同的updateXXX函数处理的fiber节点类型不同,  总的目的是为了向下生成子节点.  在这个过程中把一些需要持久化的数据挂载到fiber节点上;  把fiber节点的特殊操作设置到fiber.flags，主要逻辑可以概括为3个步骤：

    1. 根据fiber.pendingProps, fiber.updateQueue等输入数据状态, 计算fiber.memoizedState作为输出状态
    2. 获取下级ReactElement对象
        1. class 类型的 fiber 节点
        * 构建React.Component实例
        * 把新实例挂载到fiber.stateNode上
        * 执行render之前的生命周期函数
        * 执行render方法, 获取下级reactElement
        * 根据实际情况, 设置fiber.flags
        2. function 类型的 fiber 节点
        * 执行 function, 获取下级reactElement
        * 根据实际情况, 设置fiber.flags
        3. HostComponent 类型(如: div, span, button 等)的 fiber 节点
        * pendingProps.children作为下级reactElement
        * 如果下级节点是文本节点,则设置下级节点为 null. 准备进入completeUnitOfWork阶段
        * 根据实际情况, 设置fiber.flags
        4. 其他类型...
    3. 根据ReactElement对象, 调用reconcileChildren生成Fiber子节点(只生成次级子节点)
    - 根据实际情况, 设置fiber.flags

**fiber树的根节点是HostRootFiber节点, 所以第一次进入beginWork会调用updateHostRoot(current, workInProgress, renderLanes)**

```javascript
// 省略与本节无关代码
function updateHostRoot(current, workInProgress, renderLanes) {
  // 1. 状态计算, 更新整合到 workInProgress.memoizedState中来
  const updateQueue = workInProgress.updateQueue;
  const nextProps = workInProgress.pendingProps;
  const prevState = workInProgress.memoizedState;
  const prevChildren = prevState !== null ? prevState.element : null;
  cloneUpdateQueue(current, workInProgress);
  // 遍历updateQueue.shared.pending, 提取有足够优先级的update对象, 计算出最终的状态 workInProgress.memoizedState
  processUpdateQueue(workInProgress, nextProps, null, renderLanes);
  const nextState = workInProgress.memoizedState;
  // 2. 获取下级`ReactElement`对象
  const nextChildren = nextState.element;
  const root: FiberRoot = workInProgress.stateNode;
  if (root.hydrate && enterHydrationState(workInProgress)) {
    // ...服务端渲染相关, 此处省略
  } else {
    // 3. 根据`ReactElement`对象, 调用`reconcileChildren`生成`Fiber`子节点(只生成`次级子节点`)
    reconcileChildren(current, workInProgress, nextChildren, renderLanes);
  }
  return workInProgress.child;
}
```

    2. **回溯阶段 completeWork。**<font style="color:rgb(37, 41, 51);">向上归并的过程，如果有兄弟节点，会返回 sibling兄弟，没有返回 return 父级，一直返回到 fiebrRoot ，期间可以形成effectList，对于初始化流程会创建 DOM ，对于 DOM 元素进行事件收集，处理style，className等。</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699779401296-2707be0d-1dad-4b29-9367-2d2f276588cf.png)

处理beginWork探寻阶段已经创建出来的fiber节点，核心逻辑：

    1. 调用completeWork
    - 给fiber节点(tag=HostComponent, HostText)创建 DOM 实例, 设置fiber.stateNode局部状态(如tag=HostComponent, HostText节点: fiber.stateNode 指向这个 DOM 实例).
    - 为dom节点设置属性，绑定事件
    - 设置fiber.flags标记
    2. 把当前fiber对象的副作用队列(firstEffect和lastEffect)添加到父节点的副作用队列之后, 更新父节点的firstEffect和lastEffect指针.
    3. 识别beginWork阶段设置的fiber.flags, 判断当前 fiber 是否有副作用(增,删,改), 如果有, 需要将当前 fiber 加入到父节点的effects队列, 等待commit阶段处理.

到此整个fiber树构造循环已经执行完毕, 拥有一棵完整的fiber树, 并且在fiber树的根节点上挂载了副作用队列, 副作用队列的顺序是层级越深子节点越靠前.

```javascript
export default class Index extends React.Component{
   state={ number:666 } 
   handleClick=()=>{
     this.setState({
         number:this.state.number + 1
     })
   }
   render(){
     return <div>
       hello，world
       <p > 《React进阶实践指南》 { this.state.number } 👍  </p>
       <button onClick={ this.handleClick } >点赞</button>
     </div>
   }
}
```

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699777571249-1e698bb6-225f-472d-b9ed-263e40ee88b6.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699778472885-86fa1cb3-d6fa-42bb-b80d-d47766fb4d34.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699778119814-058fb0ce-328a-42d7-803f-be3a5769be0a.png)

### 更新
<font style="color:rgb(37, 41, 51);">首先会走如上的逻辑，重新创建一颗 workInProgresss 树，复用当前 current 树上的 alternate ，作为新的 workInProgress ，由于初始化 rootfiber 有 alternate ，所以对于剩余的子节点，React 还需要创建一份，和 current 树上的 fiber 建立起 alternate 关联。渲染完毕后，workInProgresss 再次变成 current 树。</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699778757118-0f7ceaf2-b936-40c6-9ad6-d2f4612ccb1a.png)

### fiber的渲染
fiber树的基本特点：

+ 无论是首次构造或者是对比更新, 最终都会在内存中生成一棵用于渲染页面的fiber树(即fiberRoot.finishedWork).
+ 这棵将要被渲染的fiber树有 2 个特点:
    1. 副作用队列挂载在根节点上(具体来讲是finishedWork.firstEffect)
    2. 代表最新页面的DOM对象挂载在fiber树中首个HostComponent类型的节点上(具体来讲DOM对象是挂载在fiber.stateNode属性上)

#### 渲染前
准备工作：

1. 设置全局状态(如: 更新fiberRoot上的属性)
2. 重置全局变量(如: workInProgressRoot, workInProgress等)
3. 再次更新副作用队列: 只针对根节点fiberRoot.finishedWork
+ 默认情况下根节点的副作用队列是不包括自身的, 如果根节点有副作用, 则将根节点添加到副作用队列的末尾
+ 注意只是延长了副作用队列, 但是fiberRoot.lastEffect指针并没有改变. 比如首次构造时, 根节点拥有Snapshot标记:

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699352121507-1b88c621-bb3c-49e3-aa65-d35b55b8a1d3.png)

#### 渲染
整个渲染逻辑都在commitroot中

1. beforeMutation（commitBeforeMutationEffects）

dom 变更之前, 处理副作用队列中带有Snapshot,Passive标记的fiber节点

    1. 处理snapshot标记
    - 对于ClassComponent类型节点, 调用了instance.getSnapshotBeforeUpdate生命周期函数
    - 对于HostRoot类型节点, 调用clearContainer清空了容器节点(即div#root这个 dom 节点).
    2. 处理passive标记
    - Passive标记只会在使用了hook对象的function类型的节点上存在。在commitRoot的第一个阶段, 为了处理hook对象(如useEffect), 通过scheduleCallback单独注册了一个调度任务task, 等待调度中心scheduler处理.
2. mutation

dom 变更, 界面得到更新. 处理副作用队列中带有Placement, Update, Deletion, Hydrating标记的fiber节点.

    - <font style="color:rgb(37, 41, 51);">置空 ref ，在 ref 章节讲到对于 ref 的处理</font>
    - 处理 DOM 突变:

> 最终会调用appendChild, commitUpdate, removeChild这些react-dom包中的函数.  在渲染器react-dom包中进行实现. 这些函数就是直接操作 DOM, 所以执行之后, 界面也会得到更新.
>
> commitMutationEffects执行之后, 在commitRootImpl函数中切换当前fiber树(root.current = finishedWork),保证fiberRoot.current指向代表当前界面的fiber树
>

        1. 新增: 函数调用栈 commitPlacement -> insertOrAppendPlacementNode -> appendChild
        2. 更新: 函数调用栈 commitWork -> commitUpdate
        3. 删除: 函数调用栈 commitDeletion -> removeChild
3. layout

dom 变更后, 处理副作用队列中带有Update | Callback标记的fiber节点.

    - 对于ClassComponent节点, 调用生命周期函数componentDidMount或componentDidUpdate, 调用update.callback回调函数.<font style="color:rgb(37, 41, 51);">setState 的callback</font>
    - <font style="color:rgb(37, 41, 51);">对于函数组件会执行 useLayoutEffect 钩子</font>
    - 对于HostComponent节点, 如有Update标记, 需要设置一些原生状态(如: focus等)
    - <font style="color:rgb(37, 41, 51);">如果有 ref ，会重新赋值 ref</font>

#### 渲染后
在渲染完成后, 需要做一些重置和清理工作:

1. 清除副作用队列
+ 由于副作用队列是一个链表, 由于单个fiber对象的引用关系, 无法被gc回收.
+ 将链表全部拆开, 当fiber对象不再使用的时候, 可以被gc回收.
2. 检测更新
+ 在整个渲染过程中, 有可能产生新的update(比如在componentDidMount函数中, 再次调用setState()).
+ 如果是常规(异步)任务, 不用特殊处理, 调用ensureRootIsScheduled确保任务已经注册到调度中心即可.
+ 如果是同步任务, 则主动调用flushSyncCallbackQueue(无需再次等待 scheduler 调度), 再次进入 fiber 树构造循环

## Fiber的影响
由于 Fiber 采用了全新的调度方式, 任务的更新过程可能会被打断, 这意味着在组件更新过程中, render 及其下面几个生命周期函数可能会被调用多次, 所以这几个生命周期函数中不应出现副作用：

+ shouldComponentUpdate
+ componentWillMount(UNSAFE_componentWillMount)
+ componentWillReceiveProps(UNSAFE_componentWillReceiveProps)
+ componentWillUpdate(UNSAFE_componentWillUpdate)

## React调度流程图
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698507584273-fd47e411-8486-4111-92c5-bea3b7af1778.png)

# state
setState 或者 useState 中修改状态的方法

+ 第一个参数还可以是一个函数, 函数的参数是当前的状态, 同时函数的返回值将最为新的状态值
+ 第二个参数, 当状态更新后, 并且组件已经重新渲染的时候会被调用, 一般用于获取修改后的状态

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698508142196-cbf52b72-14cc-42dc-83fd-115262b9c0cf.png)

## React更新机制
## 触发setState底层会发生什么
**如果调用setState 方法，实际上是 React 底层调用 Updater 对象上的 enqueueSetState 方法。**

> enqueneSetState方法：类组件初始化过程中绑定了负责更新的Updater对象。enqueneSetState创建了一个update，然后放入当fiber对象的待更新队列中，最后开启调度更新，进入更新流程
>
> ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699597997159-379ec292-a8ba-41cc-b2e3-15ae47ecf186.png)
>

1. setState会产生当前更新的优先级（老版本用expirationtime,新版本用lane）
2. react会从fiber root根部向下调和子节点，调和阶段将对比发生更新的地方，更新对比expirationTime，找到发生更新的组件，合并state，然后触发render函数得到新的UI视图层，完成render阶段
3. 在commit阶段中，替换真实DOM，完成此次更新流程
4. 仍在commit阶段，会执行setState中callback函数，到此为止完成了一次setState全程

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699597665870-095e9eef-81fd-4b7f-8994-14260bf20007.png)

render阶段的作用：根据一次更新中产生的新状态值，通过 React.createElement ，替换成新的状态，得到新的 React element 对象，新的 element 对象上，保存了最新状态值。 createElement 会产生一个全新的props。

## 是异步还是同步的？
+ **在组件生命周期或 React 事件中, setState 是异步**
+ **在 setTimeout/setInterval 或者原生 dom 事件中, setState 是同步**

本质上来讲 setState 是同步的, 之所以出现异步的假象是因为要进行 状态合并 或者说是 批处理, 需要等生命周期、事件处理器执行完毕, 再批量修改状态! 当然在实际开发中, 在合成事件和生命周期函数里, 完全可以将其视为异步的

> 那么在 setTimeout/setInterval 中又为什么看起来像同步的呢? 这里主要和微任务和宏任务有关, 如下是个演示代码, setTimeout 里面回调会等到, 主体代码执行完才会执行, 这时 isBatchingUpdates 已经是 false, 这时执行 setState 后会直接修改 this.state, 所以整个过程看起来就像是同步
>

## setState机制
+ 在react的setState函数实现中，会根据一个变量isBatchingUpdates判断是直接更新 this.state 还是放到队列中回头再说

> isBatchingUpdates为true时，不进行state的更新操作
>

+ isBatchingUpdates 默认是 false, 当 React 在执行生命周期或调用事件处理函数之前会将其设置为 true, 当执行完生命周期或者事件处理函数再改为 false 然后才会一起更新状态、更新组件, 所以整个过程看起来像异步的
+ 在原生事件中, 由于不会调用 React 批处理机制, 所以 isBatchingUpdates 一直是 false, 所以如果调用 setState 会直接更新 this.state, 整个过程看起来就像是同步

> React 18 中, 实现自动批处理, 所以不管任何情况所有状态的修改都会进行批处理了, 简单理解就是在 React 18 所以状态的修改都将是 "异步" 的
>

## 为什么要设计成异步(批处理)
1. 保证 state 和 props 的一致性

props 必然异步, 因为只有因为当父组件重渲染了我们才知道 props 是啥。实际开发中我们经常会将状态提升到父组件, 和兄弟组件进行共享, 这时如果 state 和 props 表现不一致那么这个操作很大概率就会引起一些 bug，所以 React 更愿意保证内部的一致性和状态提升的安全性, 而不总是追求代码的简洁性

2. 提高性能：在渲染前会有意地进行 等待, 直到所有在组件的事件处理函数内调用的 setState() 完成之后, 统一更新 state, 这样可以通过避免不必要的重新渲染来提升性能
3. 更多的可能性：当切换当新页面, 通过 setState 异步, 让 React 幕后渲染页面

## 批量更新
在 React 事件执行之前会调用batchUpdate方法设置 isBatchingEventUpdates=true 打开开关，开启事件批量更新，当该事件结束，再通过 isBatchingEventUpdates = false; 关闭开关，然后在 scheduleUpdateOnFiber 中根据这个开关来确定是否进行批量更新

**异步操作会打破批量更新的规则**

在异步环境下，可以通过React-DOM中的批量更新方法unstable_batchedUpdates手动批量更新

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699719519851-f6526c6f-c8c4-4cf6-b42e-181498c90329.png)

我们可以将更新任务放在flushSync回调函数内部，以此提高更新的优先级

flushSync中的setState>正常执行上下文中的setState>setTimeout，Promise中的setState

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699719530900-cb47aae0-57be-4b05-a2c4-36257fb8d3d9.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699719530951-6d46f241-a5c7-436e-a991-761d44c431f1.png)

# 高阶组件
1. 本质上就是一个函数, 是一个参数为组件, 返回值为新组件的函数
2. 高阶组件内部实现方式：
+ 属性代理: 创建新组件并渲染传入的组件, 通过 props 属性来为组件添加值或方法

```javascript
function HOC(WrapComponent){
  return class Advance extends React.Component{
    state = {
      name:'alien'
    }
    render() {
      return <WrapComponent  { ...this.props } { ...this.state }  />
    }
  }
}
```

优点：

    - 属性代理可以和业务组件低耦合，零耦合，对于条件渲染和 props 属性增强，只负责控制子组件渲染和传递额外的 props 就可以了，所以无须知道，业务组件做了些什么。所以正向属性代理，更适合做一些开源项目的 HOC ，目前开源的 HOC 基本都是通过这个模式实现的。
    - 同样适用于类组件和函数组件
    - 可以完全隔离业务组件的渲染，因为属性代理说白了是一个新的组件，相比反向继承，可以完全控制业务组件是否渲染
    - 可以嵌套使用，多个 HOC 是可以嵌套使用的，而且一般不会限制包装 HOC 的先后顺序

缺点：

    - 一般无法直接获取原始组件的状态，如果想要获取，需要 ref 获取组件实例
    - 无法直接继承静态属性。如果需要继承需要手动处理，或者引入第三方库
    - 因为本质上是产生了一个新组件，所以需要配合 forwardRef 来转发 ref
+ 反向继承: 通过继承方式实现, 继承传入的组件, 然后新增一些方法、属性

```javascript
class Index extends React.Component{
  render(){
    return <div> hello,world  </div>
  }
}
function HOC(Component){
  /* 直接继承需要包装的组件 */
  return class wrapComponent extends Component{ 
      
  }
}
export default HOC(Index) 
```

优点：

    - 方便获取组件内部状态，比如 state ，props ，生命周期，绑定的事件函数等
    - es6继承可以良好继承静态属性。所以无须对静态属性和方法进行额外的处理

缺点：

    - 函数组件无法使用
    - 和被包装的组件耦合度高，需要知道被包装的原始组件的内部状态
    - 如果多个反向继承 HOC 嵌套在一起，当前状态会覆盖上一个状态。这样带来的隐患是非常大的，比如说有多个 componentDidMount ，当前 componentDidMount 会覆盖上一个 componentDidMount 。这样副作用串联起来，影响很大

## 作用
+ 强化 props: 类似 withRouter 为组件添加 props 属性, 强化组件功能
+ 劫持控制渲染逻辑: 通过反向继承方式, 拦截原组件的生命周期、渲染、内部组件状态...
+ 动态加载组件, 根据 props 属性, 动态渲染组件, 比如添加 logding、错误处理等待...
+ 为组件添加事件: 为传入的组件包裹一层, 并绑定事件

## 缺点：
1. 高阶组件内部, 尽量不要试图通过继承的方式, 修改传入的组件, 那样可能会拦截原组件的生命周期、渲染、内部组件状态, 从而引起不必要的麻烦
2. 透传与自身无关的 props, 同时需要避免属性的覆盖问题
3. 不要在 render 方法中使用高阶组件: 在 render 中使用, 每次渲染都会重新生成新的组件, 造成不必要的卸载、挂载, 会造成性能问题, 而且重新挂载会导致组件以及子组件状态的丢失
4. 务必复制静态属性(因为返回的是新的类, 原组件的静态属性会丢失): 手动绑定、或者使用 React 官方提供的工具
5. Refs 不会被传递: 需要使用 React.forwardRef 进行处理

## hooks能取代hoc高阶组件吗？
完全替代是不能的(因为高阶组件被滥用了)：

1. 官方给出的答案是可以替代的, 因为高阶组件的出现主要目的就是为了复用状态相关逻辑(强化 props), 在这块 hooks 是可以完全替代的, 而还有其独到的优势
2. 但是后来高阶组件除了用于逻辑的复用还被滥用:
    - 在内部实现动态渲染, 根据 props 动态渲染: 这个完全可以通过组件的方式来实现, 组件在渲染上拥有更高的自由度, 可以根据父组件提供的数据进行动态渲染
    - 通过继承拦截生命周期、或者篡改 props: 本身就不应该这么做, 容易出现各种问题

# Hook
## 为什么有hooks？
1. 让函数组件也能做类组件的事，有自己的状态，可以处理一些副作用，能获取ref，也能做数据缓存
2. 解决逻辑复用难的问题：之前，组件之间的逻辑复用必须通过组合或抽取共享部分到高阶组件中来实现。Hooks 使得组件之间的逻辑复用更加灵活，可以通过自定义 Hook 来共享状态逻辑，使得组件之间的关注点分离更加清晰。
3. 放弃面向对象编程，拥抱函数式编程。

Hook使用了js的闭包机制，而不用在JavaScript 已经提供了解决方案的情况下，还引入特定的 React API。

目前没有针对于getSnapshotBeforeUpdate，getDerivedStateFromError 和 componentDidCatch 生命周期的 Hook 等价写法

## hooks与fiber（workInProgress）
hooks<font style="color:rgb(37, 41, 51);">可以作为函数组件本身和函数组件对应的 fiber 之间的沟通桥梁。</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699978415693-a34f65d1-ea04-4572-be1d-1d82c43827c8.png)

<font style="color:rgb(37, 41, 51);">hooks 对象本质上是主要以三种处理策略存在 React 中：</font>

+ ContextOnlyDispatcher： 第一种形态是防止开发者在函数组件外部调用 hooks ，所以第一种就是报错形态，只要开发者调用了这个形态下的 hooks ，就会抛出异常。
+ HooksDispatcherOnMount： 第二种形态是函数组件初始化 mount ，因为之前讲过 hooks 是函数组件和对应 fiber 桥梁，这个时候的 hooks 作用就是建立这个桥梁，初次建立其 hooks 与 fiber 之间的关系。
+ HooksDispatcherOnUpdate：第三种形态是函数组件的更新，既然与 fiber 之间的桥已经建好了，那么组件再更新，就需要 hooks 去获取或者更新维护状态。

```javascript
type Update<S, A> = {|
  lane: Lane,
  action: A,
  eagerReducer: ((S, A) => S) | null,
  eagerState: S | null,
  next: Update<S, A>,
  priority?: ReactPriorityLevel,
|};

type UpdateQueue<S, A> = {|
  pending: Update<S, A> | null,
  dispatch: (A => mixed) | null,
  lastRenderedReducer: ((S, A) => S) | null,
  lastRenderedState: S | null,
|};

export type Hook = {|
  // memorized：保持在内存中的局部状态
  memoizedState: any, 
  // baseState：baseQuene中所有update对象合并之后的状态
  baseState: any,
  // baseQuene：存储update对象的环形链表, 只包括高于本次渲染优先级的update对象.
  baseQueue: Update<any, any> | null, 
  // quene：存储update对象的环形链表，包括所有优先级的update对象
  queue: UpdateQueue<any, any> | null,
  // next：一个指针，指向链表中的下一个hook。
  next: Hook | null, // next指针
|};
```

```javascript
const HooksDispatcherOnMount = { /* 函数组件初始化用的 hooks */
    useState: mountState,
    useEffect: mountEffect,
    ...
}
const  HooksDispatcherOnUpdate ={/* 函数组件更新用的 hooks */
   useState:updateState,
   useEffect: updateEffect,
   ...
}
const ContextOnlyDispatcher = {  /* 当hooks不是函数内部调用的时候，调用这个hooks对象下的hooks，所以报错。 */
   useEffect: throwInvalidHookError,
   useState: throwInvalidHookError,
   ...
}
```

## 函数组件触发
<font style="color:rgb(37, 41, 51);">所有函数组件的触发是在 renderWithHooks 方法中，在 fiber 调和过程中，遇到 FunctionComponent 类型的 fiber（函数组件），就会用 updateFunctionComponent 更新 fiber ，在 updateFunctionComponent 内部就会调用 renderWithHooks 。</font>

```javascript
let currentlyRenderingFiber
function renderWithHooks(current,workInProgress,Component,props){
    currentlyRenderingFiber = workInProgress;
    workInProgress.memoizedState = null; /* 每一次执行函数组件之前，先清空状态 （用于存放hooks列表）*/
    workInProgress.updateQueue = null;    /* 清空状态（用于存放effect list） */
    ReactCurrentDispatcher.current =  current === null || current.memoizedState === null ? HooksDispatcherOnMount : HooksDispatcherOnUpdate /* 判断是初始化组件还是更新组件 */
    let children = Component(props, secondArg); /* 执行我们真正函数组件，所有的hooks将依次执行。 */
    ReactCurrentDispatcher.current = ContextOnlyDispatcher; /* 将hooks变成第一种，防止hooks在函数组件外部调用，调用直接报错。 */
}
```

workInProgress 正在调和更新函数组件对应的 fiber 树。

+ 对于类组件 fiber ，用 memoizedState 保存 state 信息，对于函数组件 fiber ，用 memoizedState 保存 hooks 信息。
+ 对于函数组件 fiber ，updateQueue 存放每个 useEffect/useLayoutEffect 产生的副作用组成的链表。在 commit 阶段更新这些副作用。
+ 然后判断组件是初始化流程还是更新流程，如果初始化用 HooksDispatcherOnMount 对象，如果更新用 HooksDispatcherOnUpdate 对象。函数组件执行完毕，将 hooks 赋值给 ContextOnlyDispatcher 对象。引用的 React hooks都是从 ReactCurrentDispatcher.current 中的， React 就是通过赋予 current 不同的 hooks 对象达到监控 hooks 是否在函数组件内部调用。
+ Component ( props ， secondArg ) 这个时候函数组件被真正的执行，里面每一个 hooks 也将依次执行。
+ 每个 hooks 内部为什么能够读取当前 fiber 信息，因为 currentlyRenderingFiber ，函数组件初始化已经把当前 fiber 赋值给 currentlyRenderingFiber ，每个 hooks 内部读取的就是 currentlyRenderingFiber 的内容。

## Hooks与fiber如何建立联系
<font style="color:rgb(37, 41, 51);">hooks 初始化流程使用的是 mountState，mountEffect 等初始化节点的hooks，将 hooks 和 fiber 建立起联系，那么是如何建立起关系呢，每一个hooks 初始化都会执行 mountWorkInProgressHook ，接下来看一下这个函数。</font>

<font style="color:rgb(37, 41, 51);">首先函数组件对应 fiber 用 memoizedState 保存 hooks 信息，每一个 hooks 执行都会产生一个 hooks 对象，hooks 对象中，保存着当前 hooks 的信息，不同 hooks 保存的形式不同。每一个 hooks 通过 next 链表建立起关系。</font>

```javascript
function mountWorkInProgressHook() {
  const hook = {  memoizedState: null, baseState: null, baseQueue: null,queue: null, next: null,};
  if (workInProgressHook === null) {  // 只有一个 hooks
    currentlyRenderingFiber.memoizedState = workInProgressHook = hook;
  } else {  // 有多个 hooks
    workInProgressHook = workInProgressHook.next = hook;
  }
  return workInProgressHook;
}
```

## hooks 为什么要通常放在顶部，hooks 不能写在 if 条件语句中
在更新过程中，如果通过 if 条件语句，增加或者删除 hooks，在复用 hooks 过程中，会产生复用 hooks 状态和当前 hooks 不一致的问题

<font style="color:rgb(37, 41, 51);">更新 hooks 逻辑和之前 fiber 章节中讲的双缓冲树更新差不多，会首先取出 workInProgres.alternate 里面对应的 hook ，然后根据之前的 hooks 复制一份，形成新的 hooks 链表关系。</font>hooks是基于数组（链表）实现的，在调用时hooks按顺序加入数组中，如果使用循环，条件或嵌套函数很有可能导致数组取值错位，执行错误的 Hook，进入到第一种形态ContextOnlyDispatcher并报错

## hook的调用机制
hook的调用机制的一般流程：

1. 注册钩子：首先，我们需要在程序中注册钩子。这可以通过调用特定的函数或方法来完成，该函数或方法会告诉程序在哪些事件上触发钩子。
2. 事件发生：当注册的事件发生时，程序会触发相应的钩子。
3. 执行钩子函数：一旦钩子触发，程序将执行与钩子相关联的函数或代码块。这些函数或代码块是我们自己定义的，它们用于自定义或扩展程序的行为。
4. 返回控制权：当钩子函数执行完毕后，程序将继续执行原本的逻辑，并返回控制权给原来的代码。

## 状态派发
useState 解决了函数组件没有 state 的问题

每一次改变state，底层的处理

+ 首先用户每一次调用 dispatchAction（比如如上触发 setNumber ）都会先创建一个 update ，然后把它放入待更新 pending 队列中。
+ 然后判断如果当前的 fiber 正在更新，那么也就不需要再更新了
+ 反之，说明当前 fiber 没有更新任务，那么会拿出上一次 state 和 这一次 state 进行对比，如果相同，那么直接退出更新。如果不相同，那么发起更新调度任务。这就解释了，为什么函数组件 useState 改变相同的值，组件不更新了。
+ useState中的dispatchAction会默认比较两次state是否相同，然后决定是否更新组件

## 副作用是什么？
可以理解为除了js线程的其他线程处理的事情：

+ 对外部可变数据或变量的修改: 全局变量 / 闭包变量 / dom对象 / bom对象的读写操作
+ 外部接口的调用尤其是IO： 
    - dom对象 / bom对象的方法调用;
    - xhr / fetch这样的网络IO；
    - console / LocalStorage这样的磁盘IO
+ 异常的抛出：函数中的某些代码可能会抛出异常或者执行出错

## 处理副作用
+ mountWorkInProgressHook 产生一个 hooks ，并和 fiber 建立起关系。
+ 通过 pushEffect 创建一个 effect，并保存到当前 hooks 的 memoizedState 属性下
+ pushEffect 除了创建一个 effect ， 还有一个重要作用，就是如果存在多个 effect 或者 layoutEffect 会形成一个副作用链表，绑定在函数组件 fiber 的 updateQueue 上。

## 更新流程
判断 deps 项有没有发生变化，如果没有发生变化，更新副作用链表就可以了；如果发生变化，更新链表同时，打执行副作用的标签：fiber => fiberEffectTag，hook => HookHasEffect。在 commit 阶段就会根据这些标签，重新执行副作用。

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698311632737-d40e6668-12d6-45ba-9aeb-8271a307cb85.png)

## 生命周期对应的hook
+ constructor：函数组件不需要构造函数。你可以通过调用 [useState](https://zh-hans.reactjs.org/docs/hooks-reference.html#usestate) 来初始化 state。如果计算的代价比较昂贵，你可以传一个函数给 useState
+ getDerivedStateFromProps：改为在渲染时安排一次更新
+ shouldComponentUpdate：详见下方React.memo
+ render：这是函数组件体本身
+ componentDidMount，componentDidUpdate，componentWillUnmount：useEffect Hook可以表达所有这些的组合
+ getSnapshotBeforeUpdate，getDerivedStateFromError 和 componentDidCatch：目前没有对应的的 Hook 等价写法

## 为什么要使用自定义hooks
自定义 hooks 是在 React Hooks 基础上的一个拓展，可以根据业务需求制定满足业务需要的组合 hooks ，更注重的是逻辑单元。通过业务场景不同，到底需要React Hooks 做什么，怎么样把一段逻辑封装起来，做到复用，这是自定义 hooks 产生的初衷。

自定义 hooks 也可以说是 React Hooks 聚合产物，其内部有一个或者多个 React Hooks 组成，用于解决一些复杂逻辑。

## Hook API
> [「React 进阶」 React 全部 Hooks 使用大全 （包含 React v18 版本 ） - 掘金](https://juejin.cn/post/7118937685653192735)
>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698581319659-e415a60c-839d-4b1b-b52f-12e42496528a.png)

### 数据更新驱动
### useState
1. **useState 可以使函数组件像类组件一样拥有 state，函数组件通过 useState 可以让组件重新渲染，更新视图。**
2. 基本使用：

const [ ①state , ②dispatch ] = useState(③initData)

+ state，目的提供给 UI ，作为渲染视图的数据源
+ dispatchAction 改变 state 的函数，可以理解为推动函数组件渲染的渲染函数。
+  initData 有两种情况，第一种情况是非函数，将作为 state 初始化的值。 第二种情况是函数，函数的返回值作为 useState 初始化的值
3. React使用Object.is比较算法来比较state
4. 批处理状态更新：React可以将多个状态更新分组到单个重新渲染中以提高性能，通常这会提高性能，不会影响应用程序的行为。在react18之前只对react事件处理程序中的更新进行批处理。从react18之后开始，默认情况下为所有更新启用批处理。在罕见的情况下，您需要强制同步应用DOM更新，您可以将其包装为flushSync。但是，这可能会影响性能，因此只能在需要时这样做。
+ **使用useState时候，使用push，pop，splice等直接更改不能获取到新值，应该采用析构方式**

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698075593786-173d0f4b-1880-43a4-915b-587eb79b0c87.png)

+ **useState设置状态的时候，只有第一次生效，后期需要更新状态，必须通过useEffect**
+ 如果 useState 返回数组，那么你可以顺便对数组中的变量命名，代码看起来也比较干净。而如果是对象的话返回的值必须和 useState 内部实现返回的对象同名，这样你只能在 function component 中使用一次，想要多次使用 useState 必须得重命名返回值。

### useReducer
1. useReducer 是 react-hooks 提供的能够在无状态组件中运行的类似redux的功能 api
2. 基本使用：

```javascript
const [ ①state , ②dispatch ] = useReducer(③reducer)
```

+ state：更新之后的 state 值
+ dispatch：派发更新的 dispatchAction 函数, 本质上和 useState 的 dispatchAction 是一样的
+ reducer：一个函数 reducer ，我们可以认为它就是一个 redux 中的 reducer , reducer的参数就是常规reducer里面的state和action, 返回改变后的state, 这里有一个需要注意的点就是：**如果返回的 state 和之前的 state ，内存指向相同，那么组件将不会更新**

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698581734684-f91bdc34-80f2-44bd-a96c-8fd5be8aedee.png)

### useSyncExternalStore
[useSyncExternalStore – React 中文文档](https://react.docschina.org/reference/react/useSyncExternalStore)

1. 订阅外部store的React Hook

> useSyncExternalStore 能够让 React 组件在 concurrent 模式下安全地有效地读取外接数据源，在组件渲染过程中能够检测到变化，并且在数据源发生变化的时候，能够调度更新。当读取到外部状态发生了变化，会触发一个强制更新，来保证结果的一致性。
>

```javascript
const snapshot = useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot?)
```

+ subscribe：一个应当订阅该 store 并返回一个取消订阅的函数
+ getSnapshot：返回组件需要的 store 中的数据的快照
+ getServerSnapshot：可选，一个函数，返回 store 中数据的初始快照。它只会在服务端渲染时，以及在客户端进行服务端渲染内容的 hydration 时被用到。快照在服务端与客户端之间必须相同，它通常是从服务端序列化并传到客户端的。如果你忽略此参数，在服务端渲染这个组件会抛出一个错误。
2. 作用：
+ 订阅外部store
+ 订阅浏览器API
+ 把逻辑抽取到自定义Hook
+ 添加服务端渲染支持

### useTransition
是一个帮助你在不阻塞 UI 的情况下更新状态的 React Hook。

```javascript
const [isPending, startTransition] = useTransition()
```

返回一个数组，数组有两个状态值：

+ isPending：当处于过渡状态的标志
+ startTransition：把里面的更新任务变成过渡任务

### useDeferredValue
延迟更新 UI 的某些部分。当迫切的任务执行后，再得到新的状态，而这个新的状态就称之为 DeferredValue

```javascript
const deferredValue = useDeferredValue(value)
```

useDeferredValue 接受一个值，并返回该值的新副本，该副本将推迟到更紧急地更新之后。如果当前渲染是一个紧急更新的结果，比如用户输入，React 将返回之前的值，然后在紧急渲染完成后渲染新的值。使用 useDeferredValue 的好处是，React 将在其他工作完成（而不是等待任意时间）后立即进行更新，并且像 startTransition 一样，延迟值可以暂停，而不会触发现有内容的意外降级。

useDeferredValue仅延迟你传递给他的值，如果想要在紧急更新期间防止子组件重新渲染，则还必须使用 React.memo 或 React.useMemo 记忆该子组件

### useDeferredValue和useTransition的异同？
相同：

+ useDeferredValue 本质上和内部实现与 useTransition 一样都是标记成了过渡更新任务

不同：

+ useTransition 是把 startTransition 内部的更新任务变成了过渡任务transtion,而 useDeferredValue 是把原值通过过渡任务得到新的值，这个值作为延时状态。 一个是处理一段逻辑，另一个是生产一个新的状态。

### 执行副作用
### useEffect
1. 给函数组件增加了操作副作用的能力。它跟 class 组件中的 componentDidMount、componentDidUpdate 和 componentWillUnmount 具有相同的用途，只不过被合并成了一个 API。

```javascript
useEffect(setup,dependencies?)
// 例如：
useEffect(()=>{
  return destory
},dep)
```

> 副作用：数据获取，设置订阅以及手动更改React组件中的DOM都属于副作用
>

2. 使用 useEffect 调度的 effect 不会阻塞浏览器更新屏幕，这让你的应用看起来响应更快
3. 为什么在effect中返回一个函数？这是effect可选的删除机制，每个effect都可以返回一个清除函数，如此才可以将添加和移除订阅的逻辑放在一起。react会在组件卸载的时候执行清除操作，react会在执行当前effect之前对上一个effect进行清除
4. 如果想要通过跳过 Effect 进行性能优化，第二个参数需要包含所有外部作用域中会随时间变化并且在effect中使用的变量，否则你的代码会引用到先前渲染中的旧变量。如果想执行只运行一次的 effect（仅在组件挂载和卸载时执行），可以传递一个空数组（[]）作为第二个参数。这就告诉 React 你的 effect 不依赖于 props 或 state 中的任何值，所以它永远都不需要重复执行。这并不属于特殊情况 —— 它依然遵循依赖数组的工作方式。
5. 使用场景：
+ 数据获取和订阅
+ DOM操作
+ 副作用操作
+ 生命周器钩子

### useLayoutEffect
> **可能会影响性能，尽可能使用useEffect**
>

1. useLayoutEffect 是 [useEffect](https://react.docschina.org/reference/react/useEffect) 的一个版本，在浏览器重新绘制屏幕之前触发。

```javascript
useLayoutEffect(setup, dependencies?)
```

### useEffect和useLayoutEffect的异同？
+ useLayoutEffect采用了同步执行
+ useLayoutEffect是在 DOM 更新之后，浏览器绘制之前，这样可以方便修改 DOM，获取 DOM 信息，这样浏览器只会绘制一次，如果修改 DOM 布局放在 useEffect ，那 useEffect 执行是在浏览器绘制视图之后，接下来又改 DOM ，就可能会导致浏览器再次回流和重绘。而且由于两次绘制，视图上可能会造成闪现突兀的效果。
+ useLayoutEffect callback 中代码执行会阻塞浏览器绘制，等价于<font style="color:rgb(18, 18, 18);">componentDidMount生命周期</font>
+ 如果你使用服务端渲染，请记住，_无论_useLayoutEffect_还是_useEffect 都无法在 Javascript 代码加载完成之前执行，都不会在服务端渲染中执行

### useInsertionEffect
1. 用法和 useEffect 和 useLayoutEffect 一样
2. useInsertionEffect 主要是解决 CSS-in-JS 在渲染中注入样式的性能问题。这个 hooks 主要是应用于这个场景，在其他场景下 React 不期望用这个 hooks

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698583375738-aa89b6a8-3a24-40e0-8591-577785e586ba.png)

### 状态获取与传递
### useContext
+ 接收一个 context 对象（React.createContext 的返回值）并返回该 context 的当前值。当前的 context 值由上层组件中距离当前组件最近的 <MyContext.Provider> 的 value prop 决定。当组件上层最近的 <MyContext.Provider> 更新时，该 Hook 会触发重渲染，并使用最新传递给 MyContext provider 的 context value 值。即使祖先使用 [React.memo](https://zh-hans.reactjs.org/docs/react-api.html#reactmemo) 或 [shouldComponentUpdate](https://zh-hans.reactjs.org/docs/react-component.html#shouldcomponentupdate)，也会在组件本身使用 useContext 时重新渲染。
+ useContext的参数必须是context对象本身
+ 调用了 useContext 的组件总会在 context 值变化时重新渲染。如果重渲染组件的开销较大，你可以 [通过使用 memoization 来优化](https://github.com/facebook/react/issues/15156#issuecomment-474590693)。

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698583526224-6204c783-e394-498d-90ed-b163269dc068.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698583578725-6d1d793f-5585-44de-969f-b7de57adf222.png)

### useRef
useRef 可以用来获取元素，缓存状态，useRef 返回一个可变的 ref 对象，其 .current 属性被初始化为传入的参数（initialValue）。返回的 ref 对象在组件的整个生命周期内持续存在。

```javascript
const cur = React.useRef(initState)
console.log(cur.current)
```

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698583728778-35e28d05-c2a2-47b1-bb6a-714405982150.png)

+ useRef()和自建一个{current:...}对象的唯一区别是，useRef会在每次渲染时返回同一个ref对象
+ 变更 .current 属性不会引发组件重新渲染。如果想要在 React 绑定或解绑 DOM 节点的 ref 时运行某些代码，则需要使用[回调 ref](https://zh-hans.reactjs.org/docs/hooks-faq.html#how-can-i-measure-a-dom-node) 来实现。
+ 除了 [初始化](https://react.docschina.org/reference/react/useRef#avoiding-recreating-the-ref-contents) 外不要在渲染期间写入或者读取 ref.current，否则会使组件行为变得不可预测
+ 返回的ref对象在组件的整个生命周期内保持不变

**类组件有一个实例 instance 能够维护像 ref 这种信息，但是由于函数组件每次更新都是一次新的开始，所有变量重新声明，所以 useRef 不能像 createRef 把 ref 对象直接暴露出去，如果这样每一次函数组件执行就会重新声明 Ref，此时 ref 就会随着函数组件执行被重置，hooks 和函数组件对应的 fiber 对象建立起关联，将 useRef 产生的 ref 对象挂到函数组件对应的 fiber 上，函数组件每次执行，只要组件不被销毁，函数组件对应的 fiber 对象一直存在，所以 ref 等信息就会被保存下来。**

使用场景：

+ 访问dom元素或组件实例
+ 缓存数值或对象的引用：如果我们有一些需要在组件多次渲染之间保持稳定的值（不触发重新渲染），可以使用useRef来存储这些值。由于useRef返回的引用对象在组件重新渲染时保持不变，因此可以将其用于存储不需要触发重新渲染的数据
+ 存储上一次的值：在某些情况下，我们需要比较当前值和上一次的值，并根据值的变化执行相应的操作。这时，可以使用useRef来存储上一次的值，然后在下一次渲染时进行比较

### useImperativeHandle
useImperativeHandle 可以让你在使用 ref 时自定义暴露给父组件的实例值。在大多数情况下，应当避免使用 ref 这样的命令式代码。useImperativeHandle应当与forwardRef一起使用

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698583846917-a8859137-0d72-40e7-b3f2-4ea3904b773c.png)

```javascript
// 用useImperativeHandle，使得父组件能让子组件中的input自动赋值并聚焦。
function Son (props,ref) {
  console.log(props)
  const inputRef = useRef(null)
  const [ inputValue , setInputValue ] = useState('')
  useImperativeHandle(ref,()=>{
   const handleRefs = {
     /* 声明方法用于聚焦input框 */
     onFocus(){
      inputRef.current.focus()
     },
     /* 声明方法用于改变input的值 */
     onChangeValue(value){
       setInputValue(value)
     }
   }
   return handleRefs
  },[])
  return <div>
    <input
      placeholder="请输入内容"
      ref={inputRef}
      value={inputValue}
    />
  </div>
}

const ForwarSon = forwardRef(Son)

class Index extends React.Component{
  inputRef = null
  handerClick(){
   const { onFocus , onChangeValue } =this.cur
   onFocus()
   onChangeValue('let us learn React!')
  }
  render(){
    return <div style={{ marginTop:'50px' }} >
      <ForwarSon ref={node => (this.inputRef = node)} />
      <button onClick={this.handerClick.bind(this)} >操控子组件</button>
    </div>
  }
}
```

### 状态派生与保存
### useMemo
useMemo 可以在函数组件 render 上下文中同步执行一个函数逻辑，这个函数的返回值可以作为一个新的状态缓存起来

> useMemo会记录上一次执行 create 的返回值，并把它绑定在函数组件对应的 fiber 对象上，只要组件不销毁，缓存值就一直存在，但是 deps 中如果有一项改变，就会重新执行 create ，返回值作为新的值记录到 fiber 对象上。
>
> **组件初次渲染时，执行一次computeFn，把函数返回值缓存起来。组件重新渲染时，通过浅比较检查依赖数组depenencies有没有变化，如果没有****<font style="color:rgb(37, 41, 51);">，不重复执行</font>**** ****<font style="background-color:rgb(255, 245, 245);">computeFn</font>****，而是直****<font style="color:rgb(37, 41, 51);">接返回之前缓存的结果。</font>**
>
> <font style="color:rgb(102, 102, 102);">useMemo 不只是缓存了函数的返回值，同时保证了返回值的引用地址不变。这个非常重要。</font>
>

```javascript
const cacheSomething = useMemo(create,deps)
```

+ create：第一个参数为一个函数，函数的返回值作为缓存值
+ deps：存放当前 useMemo 的依赖项，在函数组件下一次执行的时候，会对比 deps 依赖项里面的状态，是否有改变，如果有改变重新执行 create ，得到新的缓存值
+ cacheSomething：返回值，执行 create 的返回值。如果 deps 中有依赖项改变，返回的重新执行 create 产生的值，否则取上一次缓存值。

使用前判断：

1. <font style="color:rgb(37, 41, 51);">检查是否是 </font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">computeFn</font><font style="color:rgb(37, 41, 51);"> 导致了页面的卡顿</font>
2. <font style="color:rgb(37, 41, 51);">检查 </font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">computeFn</font><font style="color:rgb(37, 41, 51);"> 是否需要跟随组件渲染执行</font>
3. <font style="color:rgb(37, 41, 51);">检查 </font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">computeFn</font><font style="color:rgb(37, 41, 51);"> 本身是否有优化空间</font>
4. <font style="color:rgb(37, 41, 51);">如果以上两点都无法做到，最后考虑使用 useMemo</font>

用法：

+ 缓存计算结果：

```javascript
function Scope(){
    const style = useMemo(()=>{
      let computedStyle = {}
      // 经过大量的计算
      return computedStyle
    },[])
    return <div style={style} ></div>
}
```

+ 缓存组件，减少子组件rerender次数

```javascript
function Scope ({ children }){
   const renderChild = useMemo(()=>{ children()  },[ children ])
   return <div>{ renderChild } </div>
}
```

+ 派生新状态

```javascript
function Scope() {
  const keeper = useKeep()
  const { cacheDispatch, cacheList, hasAliveStatus } = keeper
 
  /* 通过 useMemo 得到派生出来的新状态 contextValue  */
  const contextValue = useMemo(() => {
    return {
      cacheDispatch: cacheDispatch.bind(keeper),
      hasAliveStatus: hasAliveStatus.bind(keeper),
      cacheDestory: (payload) => cacheDispatch.call(keeper, { type: ACTION_DESTORY, payload })
    }
  
  }, [keeper])
  return <KeepaliveContext.Provider value={contextValue}>
  </KeepaliveContext.Provider>
}
```

### useCallback
允许你在多次渲染中缓存函数的 React Hook。

```javascript
const cachedFn = useCallback(fn, dependencies)
```

> useMemo 和 useCallback 接收的参数都是一样，都是在其依赖项发生变化后才执行，都是返回缓存的值，区别在于 useMemo 返回的是函数运行的结果，useCallback 返回的是函数
>

> 这个回调函数是经过处理后的也就是说父组件传递一个函数给子组件的时候，由于是无状态组件每一次都会重新生成新的 props 函数，这样就使得每一次传递给子组件的函数都发生了变化，这时候就会触发子组件的更新，这些更新是没有必要的，此时我们就可以通过 usecallback 来处理此函数，然后作为 props 传递给子组件。
>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698586458048-2cdb14c5-04a3-4f73-91b8-3359916a1f8b.png)

### 浅比较
React的浅比较是基于Object.is进行实现的

浅比较流程：

+ 首先会直接比较新老 props 或者新老 state 是否相等。如果相等那么不更新组件。
+ 判断新老 state 或者 props ，有不是对象或者为 null 的，那么直接返回 false ，更新组件。
+ 通过 Object.keys 将新老 props 或者新老 state 的属性名 key 变成数组，判断数组的长度是否相等，如果不相等，证明有属性增加或者减少，那么更新组件。
+ 遍历老 props 或者老 state ，判断对应的新 props 或新 state ，有没有与之对应并且相等的（这个相等是浅比较），如果有一个不对应或者不相等，那么直接返回 false ，更新组件。 到此为止，浅比较流程结束， PureComponent 就是这么做渲染节流优化的。

```javascript
import is from './objectIs';
import hasOwnProperty from './hasOwnProperty';

/**
 * Performs equality by iterating through keys on an object and returning false
 * when any key has values which are not strictly equal between the arguments.
 * Returns true when the values of all keys are strictly equal.
 */
function shallowEqual(objA: mixed, objB: mixed): boolean {
  if (is(objA, objB)) {
    return true;
  }

  if (
    typeof objA !== 'object' ||
    objA === null ||
    typeof objB !== 'object' ||
    objB === null
  ) {
    return false;
  }

  const keysA = Object.keys(objA);
  const keysB = Object.keys(objB);

  if (keysA.length !== keysB.length) {
    return false;
  }
  
  // Test for A's keys different from B.
  for (let i = 0; i < keysA.length; i++) {
    const currentKey = keysA[i];
    if (
      !hasOwnProperty.call(objB, currentKey) ||
      !is(objA[currentKey], objB[currentKey])
    ) {
      return false;
    }
  }

  return true;
}

```

[Object.is](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/is)：Object.is() 与 [==](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Equality) 运算符并不等价。== 运算符在测试相等性之前，会对两个操作数进行类型转换（如果它们不是相同的类型），这可能会导致一些非预期的行为，例如 "" == false 的结果是 true，但是 Object.is() 不会对其操作数进行类型转换。

Object.is() 也不等价于 [===](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Strict_equality) 运算符。Object.is() 和 === 之间的唯一区别在于它们处理带符号的 0 和 NaN 值的时候。=== 运算符（和 == 运算符）将数值 -0 和 +0 视为相等，但是会将 [NaN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN) 视为彼此不相等。

+ 都是undefiend，都是null，都是true，都是false
+ 都是长度相同、字符相同、顺序相同的字符串
+ 都是相同的对象（意味着两个值都引用了内存中的同一对象）
+ 都是 [BigInt](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt) 且具有相同的数值
+ 都是 [symbol](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol) 且引用相同的 symbol 值
+ 都是数字且
    - 都是 +0
    - 都是 -0
    - 都是 [NaN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)
    - 都有相同的值，非零且都不是 [NaN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)

### 工具hooks
### useDebugValue
可用于在react开发者工具中显示自定义hook的标签

我们可以通过useDebugValue来延迟格式debug值

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698586653575-ae0929b5-7823-4a2b-8dc3-323cd87b549a.png)

> 我们不推荐你向每个自定义 Hook 添加 debug 值。当它作为共享库的一部分时才最有价值。在某些情况下，格式化值的显示可能是一项开销很大的操作。除非需要检查 Hook，否则没有必要这么做。因此，useDebugValue 接受一个格式化函数作为可选的第二个参数。该函数只有在 Hook 被检查时才会被调用。它接受 debug 值作为参数，并且会返回一个格式化的显示值。
>

### useId
它可以在 client 和 server 生成唯一的 id , 解决了在服务器渲染中，服务端和客户端产生 id 不一致的问题，更重要的是保障了 React v18 中 streaming renderer （流式渲染） 中 id 的稳定性，避免 hydration 不匹配的 hook

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698587167262-0ef48371-abd4-4894-bb8a-ee8a3a8080aa.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698587174404-6585de57-a4ba-4f8c-b2c0-db9c9124c81b.png)

## 使用useRef和useCallback实现防抖
```javascript
import React, { useState, useRef, useCallback } from 'react';
import ReactDom from 'react-dom';

function DebounceExample() {
  const debounceTimeoutRef = useRef(null);

  const handleClick = useCallback(() => {
    // 执行防抖逻辑
    if (debounceTimeoutRef.current) {
      clearTimeout(debounceTimeoutRef.current);
    }

    debounceTimeoutRef.current = setTimeout(() => {
      console.log('防抖生效，执行点击操作');
      // 这里可以放置你的点击操作逻辑
    }, 500); // 设置防抖延迟时间
  }, []);

  return (
    <div>
      <button onClick={handleClick}>点击</button>
    </div>
  );
}

ReactDom.render(<DebounceExample />, document.getElementById('app'));
```

```javascript
import React, { useEffect, useRef, useCallback, useState } from "react";
import ReactDom from 'react-dom';

const useDebounce = (callback, delay) => {
  const timerRef = useRef();

  const debouncedCallback = useCallback((...args) => {
    if (timerRef.current) {
      clearTimeout(timerRef.current);
    }

    timerRef.current = setTimeout(() => {
      callback(...args);
    }, delay);
  }, [callback, delay]);

  useEffect(() => {
    return () => {
      if (timerRef.current) {
        clearTimeout(timerRef.current);
      }
    };
  }, []);

  return debouncedCallback;
};

const ValidationComponent = () => {
  const [inputValue, setInputValue] = useState("");
  
  const handleValidation = useDebounce((value) => {
    // 在这里执行验证逻辑
    console.log('输入内容', inputValue)
  }, 500);

  const handleChange = (event) => {
    const { value } = event.target;
    setInputValue(value);

    // 这里调用防抖后的验证函数
    handleValidation(value);
  };

  return (
    <div>
      <input type="text" value={inputValue} onChange={handleChange} />
    </div>
  );
};

ReactDom.render(<ValidationComponent />, document.getElementById('app'));
      
```

## React.memo，useMemo，useCallback的区别？
> [解读 useMemo, useCallback 和 React.memo，不再盲目做优化 - 掘金](https://juejin.cn/post/7090820276547485709?searchId=202311090914585973E233EFF781362AF7)
>

React.memo：缓存的是整个组件，当props没有改变时，是浅比较实现的，组件就不需要重新渲染

> + React.memo: 第二个参数 返回 true 组件不渲染 ， 返回 false 组件重新渲染。和 shouldComponentUpdate 相反，shouldComponentUpdate : 返回 true 组件渲染 ， 返回 false 组件不渲染。
> + memo 当二个参数 compare 不存在时，会用**浅比较原则**处理 props ，相当于仅比较 props 版本的 pureComponent 。
> + memo 同样适合类组件和函数组件。
>

useMemo：缓存的是第一个参数（函数）的返回值

useCallback：缓存的是一个函数本身以及它的引用地址

> React.memo（）包裹的组件如果传入的props发生变化，组件一定会更新吗？
>
> 不是，如果 props 中存在回调函数或者多层嵌套的复杂对象，组件是不会进行更新的
>

# 类组件如何限制state视图
## PureComponent
PureComponent 类似于 [Component](https://react.docschina.org/reference/react/Component)，但是当 props 和 state 与之前保持一致时会跳过重新渲染。React 仍然支持类式组件，但我们不建议在新代码中使用。

PureComponent会对props和state进行浅比较，跳过不必要的更新，提高组件性能。无法判断复杂数据类型（引用类型）的变化。

**注：**

+ 避免使用箭头函数。不要给是 PureComponent 子组件绑定箭头函数，因为父组件每一次 render ，如果是箭头函数绑定的话，都会重新生成一个新的箭头函数， PureComponent 对比新老 props 时候，因为是新的函数，所以会判断不想等，而让组件直接渲染，PureComponent 作用终会失效。
+ PureComponent 的父组件是函数组件的情况，绑定函数要用 useCallback 或者 useMemo 处理。这种情况还是很容易发生的，就是在用 class + function 组件开发项目的时候，如果父组件是函数，子组件是 PureComponent ，那么绑定函数要小心，因为函数组件每一次执行，如果不处理，还会声明一个新的函数，所以 PureComponent 对比同样会失效，如下情况：

```javascript
class Index extends React.PureComponent{}
export default function (){
    const callback = function handerCallback(){} /* 每一次函数组件执行重新声明一个新的callback，PureComponent浅比较会认为不想等，促使组件更新  */
    return <Index callback={callback}  />
}
```

## shouldComponentUpdate
可以通过判断前后state变化来决定组件需不需要更新，需要更新返回true，否则返回false

## 类组件中的setState和函数组件中的useState有什么异同
  相同点：

+ setState和useState()更新视图，底层都调用了scheduleUpdateOnFiber方法，而且事件驱动情况下都有批量更新规则

  不同点；

+ 在不是 pureComponent 组件模式下， setState 不会浅比较两次 state 的值，只要调用 setState，在没有其他优化手段的前提下，就会执行更新。但是 **useState 中的 dispatchAction 会默认比较两次 state 是否相同，然后决定是否更新组件。**
+ setState 有专门监听 state 变化的回调函数 callback，可以获取最新state；但是在函数组件中，只能通过 useEffect 来执行 state 变化引起的副作用。

# 打破渲染限制：
+ forceUpdate。类组件更新如果调用的是 forceUpdate 而不是 setState ，会跳过 PureComponent 的浅比较和 shouldComponentUpdate 自定义比较。其原理是组件中调用 forceUpdate 时候，全局会开启一个 hasForceUpdate 的开关。当组件更新的时候，检查这个开关是否打开，如果打开，就直接跳过 shouldUpdate 。
+ context穿透，上述的几种方式，都不能本质上阻断 context 改变，而带来的渲染穿透，所以开发者在使用 Context 要格外小心，既然选择了消费 context ，就要承担 context 改变，带来的更新作用。

## 渲染控制流程图
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699698535886-275ab77d-3c1a-4fe6-9cd5-ed7819d3c1c8.png)

# react-router
1. 单页面应用  用 React 或者 Vue 构建的应用都是单页面应用，单页面应用是使用一个 html 前提下，一次性加载 js ， css 等资源，所有页面都在一个容器页面下，页面切换实质是组件的切换。

## History，react-router，react-router-dom三者关系
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700637381946-914c319b-cfb5-4fa5-9f92-038321cd570d.png)

+ history：history 是整个 React-router 的核心，里面包括两种路由模式下改变路由的方法，和监听路由变化方法等。
+ react-router：既然有了 history 路由监听/改变的核心，那么需要调度组件负责派发这些路由的更新，也需要容器组件通过路由更新，来渲染视图。所以说 React-router 在 history 核心基础上，增加了 Router ，Switch ，Route 等组件来处理视图渲染。
+ react-router-dom：在 react-router 基础上，增加了一些 UI 层面的拓展比如 Link ，NavLink 。以及两种模式的根部路由 BrowserRouter ，HashRouter 

## 两种路由模式
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700639080202-3a70bbf6-81ba-4226-a335-952b6cf85df6.png)

1. history模式

同一个文档的history对象出现变化时，就会触发popstate事件，history.pushState 可以使浏览器地址改变，但是无需刷新页面

用 history.pushState() 或者 history.replaceState() 不会触发 popstate 事件。 popstate 事件只会在浏览器某些行为下触发, 比如点击后退、前进按钮或者调用 history.back()、history.forward()、history.go()方法。

> 为什么pushState()或者replaceState()不触发popState事件的情况下，ReactRouter还能挂在对应路由的组件？
>
> ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700639306028-af0efab6-797a-4f4b-92e6-7eb1b341ef3b.png)
>

2. hash history模式
+ 通过 <font style="color:rgb(216,57,49);">window.location.hash</font> 属性获取和设置 hash 值。开发者在哈希路由模式下的应用中，切换路由，本质上是改变 <font style="color:rgb(216,57,49);">window.location.hash</font>
+ 监听路由：onHashchange，hash路由模式下，监听路由变化用的是hashchange

## React-router基本构成
### location，history，match
+ history对象：<font style="color:rgb(37, 41, 51);">history对象保存改变路由方法 push ，replace，和监听路由方法 listen 等。</font>
+ <font style="color:rgb(37, 41, 51);">location对象：可以理解为当前状态下的路由信息，包括 pathname ，state 等</font>
+ <font style="color:rgb(37, 41, 51);">match对象：这个用来证明当前路由的匹配信息的对象。存放当前路由path 等信息</font>

## 路由组件
### Router组件：整个应用路由的传递者和派发更新者
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700639448202-318ef801-e458-48e9-b60e-e453b59db7e5.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700639467990-96e5e894-6328-499b-8d93-ac68fb79d234.png)

+ react-router是通<font style="background-color:rgb(251,191,188);">过context上下文方式传递的路由信息，context 改变，会使消费 context 组件更新，这就能合理解释了，当开发者触发路由改变，为什么能够重新渲染匹配组件</font>
+ props.history是通过BrowserRouter或HashRouter创建的history对象，并传递过来的，当路由改变，会触发listen方法，传递新生成的location，然后通过setState来改变context中的value，所以改变路由，本质上是location改变带来的更新作用

### Route组件：用于匹配路由，路由匹配，渲染组件
四种Route编写格式：

+ component形式：将组件直接传递给 Route 的 component 属性，Route 可以将路由信息隐式注入到页面组件的 props 中，但是无法传递父组件中的信息，比如如上 mes
+ render形式：Route 组件的 render 属性，可以接受一个渲染函数，函数参数就是路由信息，可以传递给页面组件，还可以混入父组件信息
+ children形式：直接作为 children 属性来渲染子组件，但是这样无法直接向子组件传递路由信息，但是可以混入父组件信息
+ renderProps形式：可以将 childen 作为渲染函数执行，可以传递路由信息，也可以传递父组件信息。

```javascript
function Index(){ 
  const mes = { name:'alien',say:'let us learn React!' }
  return <div>      
    <Meuns/>
    <Switch>
      <Route path='/router/component' component={RouteComponent}   /> { /* Route Component形式 */ }
      <Route path='/router/render' render={(props)=> <RouterRender { ...props }  /> }  {...mes}  /> { /* Render形式 */ }
      <Route path='/router/children'  > { /* chilren形式 */ }
        <RouterChildren  {...mes} />
      </Route>
      <Route path="/router/renderProps"  >
        { (props)=> <RouterRenderProps {...props} {...mes}  /> }  {/* renderProps形式 */}
      </Route>
    </Switch>
  </div>
}
export default Index

```

我们可以使用<font style="background-color:rgb(251,191,188);">react-router-config</font>库中提供的renderRoutes更优雅的渲染Route：

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700639754812-aa27f5d9-7dba-4ac4-b9fd-890b8d930b11.png)

### switch
```plain
<div>
   <Route path='/home'  component={Home}  />
   <Route path='/list'  component={List}  />
   <Route path='/my'  component={My}  />
</div>
```

<font style="color:rgb(37, 41, 51);">这样会影响页面的正常展示和路由的正常切换吗？答案是否定的，这样对于路由切换页面展示没有影响，但是值得注意的是，如果在页面中这么写，</font>**<font style="color:rgb(37, 41, 51);">三个路由都会被挂载</font>**<font style="color:rgb(37, 41, 51);">，但是每个页面路由展示与否，是通过 Route 内部 location 信息匹配的。</font>

<font style="color:rgb(37, 41, 51);">那么 Switch 作用是先通过匹配选出一个正确路由 Route 进行渲染。</font>

```plain
<Switch>
   <Route path='/home'  component={Home}  />
   <Route path='/list'  component={List}  />
   <Route path='/my'  component={My}  />
</Switch>
```

<font style="color:rgb(37, 41, 51);">如果通过 Switch 包裹后，那么页面上只会展示一个正确匹配的路由。比如路由变成 </font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">/home</font><font style="color:rgb(37, 41, 51);"> ，那么只会挂载 </font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">path='/home'</font><font style="color:rgb(37, 41, 51);"> 的路由和对应的组件 Home 。综上所述 Switch 作用就是匹配唯一正确的路由并渲染。</font>

### redirect：重定向
+ <font style="color:rgb(37, 41, 51);">当如果修改地址栏或者调用 api 跳转路由的时候，当找不到匹配的路由的时候，并且还不想让页面空白，那么需要重定向一个页面。</font>
+ <font style="color:rgb(37, 41, 51);">当页面跳转到一个无权限的页面，期望不能展示空白页面，需要重定向跳转到一个无权限页面。</font>

## <font style="color:rgb(37, 41, 51);">路由改变到页面跳转流程图</font>
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700640271363-5a5d2102-6244-4a75-b186-084dc5036e9c.png)

## 路由状态获取
+ 路由组件props：<font style="color:rgb(37, 41, 51);">被 Route 包裹的路由组件 props 中会默认混入 history 等信息，那么如果路由组件的子组件也想共享路由状态信息和改变路由的方法，那么 props 可以是一个很好的选择</font>

```javascript
class Home extends React.Component{
  render(){
    return <div>
      <Children {...this.props}  />
    </div>
  }
}
```

+ <font style="color:rgb(37, 41, 51);">withRouter：</font><font style="color:rgb(37, 41, 51);">对于距离路由组件比较远的深层次组件，通常可以用 react-router 提供的 </font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">withRouter</font><font style="color:rgb(37, 41, 51);"> 高阶组件方式获取 histroy ，loaction 等信息。</font>

```javascript
import { withRouter } from 'react-router-dom'
@withRouter
class Home extends React.Component{
    componentDidMount(){
        console.log(this.props.history)
    }
    render(){
        return <div>
            { /* ....*/ }
        </div>
    }
}
```

+ useHistory和useLocation：<font style="color:rgb(37, 41, 51);">对于函数组件，可以用 </font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">React-router</font><font style="color:rgb(37, 41, 51);"> 提供的自定义 hooks 中的 useHistory 获取 history 对象，用 useLocation 获取 location 对象</font>

```javascript
import { useHistory ,useLocation  } from 'react-router-dom'
function Home(){
  const history = useHistory() /* 获取history信息 */
  const useLocation = useLocation() /* 获取location信息 */
}
```

## 路由带参数跳转
+ 路由跳转：有声明式路由和函数式路由两种
    1. 声明式：<NavLink to='/home' /> ，利用 react-router-dom 里面的 Link 或者 NavLink 
    2. 函数式：histor.push('/home')
+ 参数传递：<font style="color:rgb(37, 41, 51);">页面间需要传递信息</font>
    - <font style="color:rgb(37, 41, 51);">url拼接：不安全，不建议使用</font>
    - **<font style="color:rgb(37, 41, 51);">state路由状态：</font>**

```javascript
const name = 'alien'
const mes = 'let us learn React!'
history.push({
    pathname:'/home',
    state:{
        name,
        mes
    }
})
const {state = {}} = this.prop.location
const { name , mes } = state
```

    - <font style="color:rgb(37, 41, 51);">动态路径参数路由</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700640615535-1cfa50e0-0b04-4a24-9ebd-c986666542b6.png)



# react-redux
## react-redux，redux，react三者关系
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700643070502-09b5b03c-9349-4b9b-85d7-a52fd5fca739.png)

+ Redux： 首先 Redux 是一个应用状态管理js库，它本身和 React 是没有关系的，换句话说，Redux 可以应用于其他框架构建的前端应用，甚至也可以应用于 Vue 中。
+ React-Redux：React-Redux 是连接 React 应用和 Redux 状态管理的桥梁。React-redux 主要专注两件事，一是如何向 React 应用中注入 redux 中的 Store ，二是如何根据 Store 的改变，把消息派发给应用中需要状态的每一个组件。
+ React：这个就不必多说了。

## redux——三大原则
1. 单向数据流：<font style="color:rgb(37, 41, 51);">整个 redux ，数据流向都是单向的</font>
2. state只读：<font style="color:rgb(37, 41, 51);">在 Redux 中不能通过直接改变 state ，来让状态发生变化，如果想要改变 state ，那就必须触发一次 action ，通过 action 执行每个 reducer</font>
3. <font style="color:rgb(37, 41, 51);">纯函数执行：每一个 reducer 都是一个纯函数，里面不要执行任何副作用，返回的值作为新的 state ，state 改变会触发 store 中的 subscribe </font>

## <font style="color:rgb(37, 41, 51);">redux——发布订阅思想</font>
<font style="color:rgb(37, 41, 51);">redux 可以作为发布订阅模式的一个具体实现。redux 都会创建一个 store ，里面保存了状态信息，改变 store 的方法 dispatch ，以及订阅 store 变化的方法 subscribe 。</font>

## <font style="color:rgb(37, 41, 51);">redux——中间件</font>
<font style="color:rgb(37, 41, 51);">Redux 提供了中间件机制，使用者可以根据需要来强化 dispatch 函数，传统的 dispatch 是不支持异步的，但是可以针对 Redux 做强化，于是有了 </font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">redux-thunk</font><font style="color:rgb(37, 41, 51);">，</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">redux-actions</font><font style="color:rgb(37, 41, 51);"> 等中间件</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700643432960-e23bb836-668d-4376-a5a8-625a9d4ee390.png)

## redux——核心api
+ createStore：

```javascript
const Store = createStore(rootReducer,initialState,middleware)
```

    - rootReducer：<font style="color:rgb(37, 41, 51);">redux 的 reducer ，如果有多个那么可以调用 combineReducers 合并。</font>
    - <font style="color:rgb(37, 41, 51);">initialState：初始化的 state</font>
    - <font style="color:rgb(37, 41, 51);">middleware：如果有中间件，那么存放 redux 中间件</font>
+ combineReducers：<font style="color:rgb(37, 41, 51);">正常状态可以会有多个 reducer ，combineReducers 可以合并多个reducer</font>

```javascript
/* 将 number 和 PersonalInfo 两个reducer合并   */
const rootReducer = combineReducers({ number:numberReducer,info:InfoReducer })
```

+ <font style="color:rgb(37, 41, 51);">applyMiddleware：用于注册中间件，支持多个参数，每一个参数都是一个中间件。每次触发 action ，中间件依次执行</font>

```javascript
const middleware = applyMiddleware(logMiddleware)
```



## react-redux
<font style="color:rgb(37, 41, 51);">接受 Redux 的 Store，并把它合理分配到所需要的组件中；</font>

<font style="color:rgb(37, 41, 51);">订阅 Store 中 state 的改变，促使消费对应的 state 的组件更新；</font>

1. provider：<font style="color:rgb(37, 41, 51);">保存 redux 中的 store ，分配给所有需要 state 的子孙组件</font>

```javascript
export default function Root(){
  return <Provider store={Store} >
      <Index />
  </Provider>
}
```

2. connect
    1. <font style="color:rgb(37, 41, 51);">能够从 props 中获取改变 state 的方法 Store.dispatch</font>
    2. <font style="color:rgb(37, 41, 51);">如果 connect 有第一个参数，那么会将 redux state 中的数据，映射到当前组件的 props 中，子组件可以使用消费</font>
    3. <font style="color:rgb(37, 41, 51);">当需要的 state ，有变化的时候，会通知当前组件更新，重新渲染视图</font>

```javascript
function connect(mapStateToProps?, mapDispatchToProps?, mergeProps?, options?)
- mapStateToProps: 组件依赖 redux 的 state，映射到业务组件的 props 中，state 改变触发，业务组
  件 props 改变，触发业务组件更新视图。当这个参数没有的时候，当前组件不会订阅 store 的改变
  const mapStateToProps = state => ({ number: state.number })
- mapDispatchToProps: 将 redux 中的 dispatch 方法，映射到业务组件的 props 中
	const mapDispatchToProps = dispatch => {
    return {
      numberAdd: () => dispatch({ type: 'ADD' }),
      setInfo: () => dispatch({ type: 'SET' }),
    }
  }
- mergeProps: 
	/*
  * stateProps , state 映射到 props 中的内容
  * dispatchProps， dispatch 映射到 props 中的内容。
  * ownProps 组件本身的 props
  */
  (stateProps, dispatchProps, ownProps) => Object
```

### 原理
> [「源码解析」一文吃透react-redux源码（useMemo经典源码级案例） - 掘金](https://juejin.cn/post/6937491452838559781)
>

1. <font style="color:rgb(37, 41, 51);">Provider注入Store</font>

react-redux是通过context上下文来保存传递Store的，但是上下文<font style="color:rgb(37, 41, 51);">value 保存的除了 Store 还有 subscription 。</font>

<font style="color:rgb(37, 41, 51);">subscription 可以理解为订阅器，在 React-redux 中一方面用来订阅来自 state 变化，另一方面通知对应的组件更新。在 Provider 中的订阅器 subscription 为根订阅器</font>

<font style="color:rgb(37, 41, 51);">在 Provider 的 useEffect 中，进行真正的绑定订阅功能，其原理内部调用了 store.subscribe ，只有根订阅器才会触发store.subscribe</font>

```javascript
const ReactReduxContext =  React.createContext(null)
function Provider({ store, context, children }) {
   /* 利用useMemo，跟据store变化创建出一个contextValue 包含一个根元素订阅器和当前store  */ 
  const contextValue = useMemo(() => {
      /* 创建了一个根级 Subscription 订阅器 */
    const subscription = new Subscription(store)
    return {
      store,
      subscription
    } /* store 改变创建新的contextValue */
  }, [store])
  useEffect(() => {
    const { subscription } = contextValue
    /* 触发trySubscribe方法执行，创建listens */
    subscription.trySubscribe() // 发起订阅
    return () => {
      subscription.tryUnsubscribe()  // 卸载订阅
    } 
  }, [contextValue])  /*  contextValue state 改变出发新的 effect */
  const Context = ReactReduxContext
  return <Context.Provider value={contextValue}>{children}</Context.Provider>
}
```

2. <font style="color:rgb(37, 41, 51);">Subscription订阅器</font>

<font style="color:rgb(37, 41, 51);">层层订阅，上订下发</font>

    - <font style="color:rgb(37, 41, 51);">层层订阅：</font><font style="color:rgb(37, 41, 51);">React-Redux 采用了层层订阅的思想，上述内容讲到 Provider 里面有一个 Subscription ，提前透露一下，每一个用 connect 包装的组件，内部也有一个 Subscription ，而且这些订阅器一层层建立起关联，Provider中的订阅器是最根部的订阅器，可以通过 trySubscribe 和 addNestedSub 方法可以看到。还有一个注意的点就是，如果父组件是一个 connect ，子孙组件也有 connect ，那么父子 connect 的 Subscription 也会建立起父子关系。</font>
    - <font style="color:rgb(37, 41, 51);">上订下发：在调用 trySubscribe 的时候，能够看到订阅器会和上一级的订阅器通过 addNestedSub 建立起关联，当 store 中 state 发生改变，会触发 store.subscribe ，但是只会通知给 Provider 中的根Subscription，根 Subscription 也不会直接派发更新，而是会下发给子代订阅器（ connect 中的 Subscription ），再由子代订阅器，决定是否更新组件，层层下发。</font>

```javascript
/* 发布订阅者模式 */
export default class Subscription {
  constructor(store, parentSub) {
  //....
  }
  /* 负责检测是否该组件订阅，然后添加订阅者也就是listener */
  addNestedSub(listener) {
    this.trySubscribe()
    return this.listeners.subscribe(listener)
  }
  /* 向listeners发布通知 */
  notifyNestedSubs() {
    this.listeners.notify()
  }
  /* 开启订阅模式 首先判断当前订阅器有没有父级订阅器 ， 如果有父级订阅器(就是父级Subscription)，把自己的handleChangeWrapper放入到监听者链表中 */
  trySubscribe() {
    /*
    parentSub  即是provide value 里面的 Subscription 这里可以理解为 父级元素的 Subscription
    */
    if (!this.unsubscribe) {
      this.unsubscribe = this.parentSub
        ? this.parentSub.addNestedSub(this.handleChangeWrapper)
        /* provider的Subscription是不存在parentSub，所以此时trySubscribe 就会调用 store.subscribe   */
        : this.store.subscribe(this.handleChangeWrapper)
      this.listeners = createListenerCollection()
    }
  }
  /* 取消订阅 */
  tryUnsubscribe() {
     //....
  }
}
```

3. connect控制更新
+ <font style="color:rgb(37, 41, 51);">connect 中有一个 selector 的概念，selector 有什么用？就是通过 mapStateToProps ，mapDispatchToProps ，把 redux 中 state 状态合并到 props 中，得到最新的 props</font>
+ <font style="color:rgb(37, 41, 51);">每一个 connect 都会产生一个新的 Subscription ，和父级订阅器建立起关联，这样父级会触发子代的 Subscription 来实现逐层的状态派发。</font>
+ <font style="color:rgb(37, 41, 51);">Subscription 通知的是 checkForUpdates 函数，checkForUpdates 会形成新的 props ，与之前缓存的 props 进行浅比较，如果不相等，那么说明 state 已经变化了，直接触发一个useReducer 来更新组件，上述代码片段中，我用 useState 代替 useReducer 了，如果相等，那么当前组件不需要更新，直接通知子代 Subscription ，检查子代 Subscription 是否更新，完成整个流程。</font>

```javascript
function connect(mapStateToProps,mapDispatchToProps){
  const Context = ReactReduxContext
  /* WrappedComponent 为connect 包裹的组件本身  */   
  return function wrapWithConnect(WrappedComponent){
    function createChildSelector(store) {
      /* 选择器  合并函数 mergeprops */
      return selectorFactory(store.dispatch, { mapStateToProps,mapDispatchToProps })
    }
    /* 负责更新组件的容器 */
    function ConnectFunction(props){
      /* 获取 context内容 里面含有 redux中store 和父级subscription */
      const contextValue = useContext(ContextToUse)
      /* 创建子选择器,用于提取state中的状态和dispatch映射，合并到props中 */
      const childPropsSelector = createChildSelector(contextValue.store)
      const [subscription, notifyNestedSubs] = useMemo(() => {
        /* 创建一个子代Subscription，并和父级subscription建立起关系 */
        const subscription = new Subscription(
          store,
          didStoreComeFromProps ? null : contextValue.subscription // 父级subscription，通过这个和父级订阅器建立起关联。
        )
         return [subscription, subscription.notifyNestedSubs]
        }, [store, didStoreComeFromProps, contextValue])
        
        /* 合成的真正的props */
        const actualChildProps = childPropsSelector(store.getState(), wrapperProps)
        const lastChildProps = useRef()
        /* 更新函数 */
        const [ forceUpdate, ] = useState(0)
        useEffect(()=>{
          const checkForUpdates =()=>{
            newChildProps = childPropsSelector()
            if (newChildProps === lastChildProps.current) { 
              /* 订阅的state没有发生变化，那么该组件不需要更新，通知子代订阅器 */
              notifyNestedSubs() 
            }else{
             /* 这个才是真正的触发组件更新的函数 */
             forceUpdate(state=>state+1)
             lastChildProps.current = newChildProps /* 保存上一次的props */
            }
          }
          subscription.onStateChange = checkForUpdates
          //开启订阅者 ，当前是被connect 包转的情况 会把 当前的 checkForceUpdate 放在存入 父元素的addNestedSub中 ，一点点向上级传递 最后传到 provide 
          subscription.trySubscribe()
          /* 先检查一遍，反正初始化state就变了 */
          checkForUpdates()
        },[store, subscription, childPropsSelector])

         /* 利用 Provider 特性逐层传递新的 subscription */
        return  <ContextToUse.Provider value={{  ...contextValue, subscription}}>
           <WrappedComponent  {...actualChildProps}  />
        </ContextToUse.Provider>  
      }
      /* memo 优化处理 */
    const Connect = React.memo(ConnectFunction) 
    return hoistStatics(Connect, WrappedComponent)  /* 继承静态属性 */
  }
}

```

