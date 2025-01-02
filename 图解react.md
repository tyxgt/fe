[图解React原理系列](https://7km.top/)

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

 



# fiber构造循环&任务调度循环
**区别：**

+ 任务调度循环是以二叉堆为数据结构(详见[react 算法之堆排序](https://react-illustration-series.osrc.com/algorithm/heapsort)), 循环执行堆的顶点, 直到堆被清空，

fiber构造循环是以树为数据结构, 从上至下执行深度优先遍历(详见[react 算法之深度优先遍历](https://react-illustration-series.osrc.com/algorithm/dfs)).

+ 任务调度循环的逻辑偏向宏观, 它调度的是每一个任务(task), 而不关心这个任务具体是干什么的(甚至可以将Scheduler包脱离react使用), 具体任务其实就是执行回调函数performSyncWorkOnRoot或performConcurrentWorkOnRoot.

fiber构造循环的逻辑偏向具体实现, 它只是任务(task)的一部分(如performSyncWorkOnRoot包括: fiber树的构造, DOM渲染, 调度检测), 只负责fiber树的构造.

**联系：**

+ fiber构造循环是任务调度循环中的任务(task)的一部分. 它们是从属关系, 每个任务都会重新构造一个fiber树.

# react运行主干逻辑
1. **输入**：将每一次更新视为一次更新需求（如: 新增, 删除, 修改节点之后）
2. **注册调度任务**：react-reconciler收到更新需求之后, 并不会立即构造fiber树, 而是去调度中心scheduler注册一个新任务task, 即把更新需求转换成一个task.
3. **执行调度任务**：调度中心scheduler通过任务调度循环来执行task(task的执行过程又回到了react-reconciler包中).
+ fiber构造循环是task的实现环节之一, 循环完成之后会构造出最新的 fiber 树.
+ commitRoot是task的实现环节之二, 把最新的 fiber 树最终渲染到页面上, task完成.

# reconciler运作流程
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698830408712-c4786856-c425-4004-87db-df78d5eeeb43.png)

## react-reconciler包的主要作用
1. 输入: 暴露api函数(如: scheduleUpdateOnFiber), 供给其他包(如react包)调用.
2. 注册调度任务: 与调度中心(scheduler包)交互, 注册调度任务task, 等待任务回调.
3. 执行任务回调: 在内存中构造出fiber树, 同时与与渲染器(react-dom)交互, 在内存中创建出与fiber对应的DOM节点.
4. 输出: 与渲染器(react-dom)交互, 渲染DOM节点.

## reconciler各函数作用：
1. scheduleUpdateOnFiber：（输入）
    1. 不经过调度, 直接进行fiber构造.
    2. 注册调度任务, 经过Scheduler包的调度, 间接进行fiber构造
2. ensureRootIsScheduled：（注册调度任务）
    1. 前半部分: 判断是否需要注册新的调度(如果无需新的调度, 会退出函数)
    2. 后半部分：注册调度任务
    - performSyncWorkOnRoot或performConcurrentWorkOnRoot被封装到了任务回调(scheduleCallback)中
    - 等待调度中心执行任务, 任务运行其实就是执行performSyncWorkOnRoot或performConcurrentWorkOnRoot

```javascript
// ... 省略部分无关代码
function ensureRootIsScheduled(root: FiberRoot, currentTime: number) {
  // 前半部分: 判断是否需要注册新的调度
  const existingCallbackNode = root.callbackNode;
  const nextLanes = getNextLanes(
    root,
    root === workInProgressRoot ? workInProgressRootRenderLanes : NoLanes,
  );
  const newCallbackPriority = returnNextLanesPriority();
  if (nextLanes === NoLanes) {
    return;
  }
  // 节流防抖
  if (existingCallbackNode !== null) {
    const existingCallbackPriority = root.callbackPriority;
    if (existingCallbackPriority === newCallbackPriority) {
      return;
    }
    cancelCallback(existingCallbackNode);
  }
  // 后半部分: 注册调度任务 省略代码...

  // 更新标记
  root.callbackPriority = newCallbackPriority;
  root.callbackNode = newCallbackNode;
}
```

ensureRootIsScheduled函数会与scheduler包通信, 最后注册一个task并等待回调.

    1. 在task注册完成之后, 会设置fiberRoot对象上的属性(fiberRoot是 react 运行时中的重要全局对象, 可参考[React 应用的启动过程](https://react-illustration-series.osrc.com/main/bootstrap#%E5%88%9B%E5%BB%BA%E5%85%A8%E5%B1%80%E5%AF%B9%E8%B1%A1)), 代表现在已经处于调度进行中
    2. 再次进入ensureRootIsScheduled时(比如连续 2 次setState, 第 2 次setState同样会触发reconciler运作流程中的调度阶段), 如果发现处于调度中, 则需要一些节流和防抖措施, 进而保证调度性能.
    - 节流：判断条件existingCallbackPriority === newCallbackPriority，新旧更新的优先级相同，如连续多次执行setState，则无需注册新的task，直接沿用上一个优先级相同的task，直接退出调用
    - 防抖：判断条件existingCallbackPriority !== newCallbackPriority，新旧优先级不同，则取消旧task，重新注册task
3. performSyncWorkOnRoot：（执行任务回调）
    1. fiber 树构造
    2. 异常处理: 有可能 fiber 构造过程中出现异常
    3. 调用输出
4. performConcurrentWorkOnRoot的逻辑与performSyncWorkOnRoot的不同之处在于, 对于可中断渲染的支持:
    1. 调用performConcurrentWorkOnRoot函数时, 首先检查是否处于render过程中, 是否需要恢复上一次渲染.
    2. 如果本次渲染被中断, 最后返回一个新的 performConcurrentWorkOnRoot 函数, 等待下一次调用.
5. commitRoot：（输出）
    1. commitBeforeMutationEffects
    - dom 变更之前, 主要处理副作用队列中带有Snapshot,Passive标记的fiber节点
    2. commitMutationEffects
    - dom 变更, 界面得到更新. 主要处理副作用队列中带有Placement, Update, Deletion, Hydrating标记的fiber节点
    3. commitLayoutEffects
    - dom 变更后, 主要处理副作用队列中带有Update | Callback标记的fiber节点.

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

# 可中断渲染
react 中最广为人知的可中断渲染(render 可以中断, 部分生命周期函数有可能执行多次, UNSAFE_componentWillMount,UNSAFE_componentWillReceiveProps)只有在HostRootFiber.mode === ConcurrentRoot | BlockingRoot才会开启. 如果使用的是legacy, 即通过ReactDOM.render(<App/>, dom)这种方式启动时HostRootFiber.mode = NoMode, 这种情况下无论是首次 render 还是后续 update 都只会进入同步工作循环, reconciliation没有机会中断, 所以生命周期函数只会调用一次.

在时间切片的基础之上, 如果单个task.callback执行时间就很长(假设 200ms). 就需要task.callback自己能够检测是否超时, 所以在 fiber 树构造过程中, 每构造完成一个单元, 都会检测一次超时([源码链接](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberWorkLoop.old.js#L1637-L1639)), 如遇超时就退出fiber树构造循环, 并返回一个新的回调函数(就是此处的continuationCallback)并等待下一次回调继续未完成的fiber树构造.

# 优先级管理
可中断渲染,时间切片(time slicing),异步渲染(suspense)等特性, 在源码中得以实现都依赖于优先级管理.

react内部对于优先级的管理，根据功能的不同分为三种类型：

+ LanePriority：fiber调度优先级，位于react-reconciler包，也就是lane（车道模型）
+ Scheduler Priority：调度优先级，位于scheduler包
+ ReactPriorityLevel：优先级等级，位于react-reconciler包中的SchedulerWithReactIntegration.js负责上述2套优先级体系的转换

## lane（车道模型）
1. Lane类型被定义为二进制变量, 利用了位掩码的特性, 在频繁运算的时候占用内存少, 计算速度快,

Lane和Lanes就是单数和复数的关系, 代表单个任务的定义为Lane, 代表多个任务的定义为Lanes

2. Lane是对于expirationTime的重构, 以前使用expirationTime表示的字段, 都改为了lane
3. 使用lanes相比expirationTime模型的优势：
+ Lanes把任务优先级从批量任务中分离出来, 可以更方便的判断单个任务与批量任务的优先级是否重叠.

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698904947257-abee42f9-69de-4f82-8f09-ff1af79d8c47.png)

+ Lanes使用单个 32 位二进制变量即可代表多个不同的任务, 也就是说一个变量即可代表一个组(group), 如果要在一个 group 中分离出单个 task, 非常容易.

> 在expirationTime模型设计之初, react 体系中还没有[Suspense 异步渲染](https://zh-hans.reactjs.org/docs/concurrent-mode-suspense.html)的概念. 现在有如下场景: 有 3 个任务, 其优先级 A > B > C, 正常来讲只需要按照优先级顺序执行就可以了. 但是现在情况变了: A 和 C 任务是CPU密集型, 而 B 是IO密集型(Suspense 会调用远程 api, 算是 IO 任务), 即 A(cpu) > B(IO) > C(cpu). 此时的需求需要将任务B从 group 中分离出来, 先处理 cpu 任务A和C.
>
> ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698904966799-0f41b4c9-cab3-4d15-a2e2-05ae5f5c5d2b.png)
>

**注：**

1. 可以使用的比特位一共有 31 位
2. 共定义了[18 种车道(Lane/Lanes)变量](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberLane.js#L74-L103), 每一个变量占有 1 个或多个比特位, 分别定义为Lane和Lanes类型.

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698905144545-48528788-af3f-4005-95d9-3c6389f97558.png)

3. 每一种车道(Lane/Lanes)都有对应的优先级, 所以源码中定义了 18 种优先级
4. 占有低位比特位的Lane变量对应的优先级越高

> + 最高优先级为SyncLanePriority对应的车道为SyncLane = 0b0000000000000000000000000000001.
> + 最低优先级为OffscreenLanePriority对应的车道为OffscreenLane = 0b1000000000000000000000000000000.
>

# React调度原理（scheduler）
## 内核
调度中心最核心的代码在SchedulerHostConfig.default.js中，该文件导出了以下8个函数：

```javascript
export let requestHostCallback; // 请求及时回调: port.postMessage
export let cancelHostCallback; // 取消及时回调: scheduledHostCallback = null
export let requestHostTimeout; // 请求延时回调: setTimeout
export let cancelHostTimeout; // 取消延时回调: cancelTimeout
export let shouldYieldToHost; // 是否让出主线程(currentTime >= deadline && needsPaint): 让浏览器能够执行更高优先级的任务(如ui绘制, 用户输入等)
export let requestPaint; // 请求绘制: 设置 needsPaint = true
export let getCurrentTime; // 获取当前时间
export let forceFrameRate; // 强制设置 yieldInterval (让出主线程的周期). 这个函数虽然存在, 但是从源码来看, 几乎没有用到
```

**调度相关：请求或取消调度**

+ requestHostCallback

请求回调之后scheduledHostCallback = callback, 然后通过MessageChannel发消息的方式触发performWorkUntilDeadline函数, 最后执行回调scheduledHostCallback.

**MessageChannel在浏览器事件循环中属于宏任务, 所以调度中心永远是异步执行回调函数.**

```javascript
// 接收 MessageChannel 消息
const performWorkUntilDeadline = () => {
  if (scheduledHostCallback !== null) {
    const currentTime = getCurrentTime(); // 1. 获取当前时间
    deadline = currentTime + yieldInterval; // 2. 设置deadline
    const hasTimeRemaining = true;
    try {
      // 3. 执行回调, 返回是否有还有剩余任务
      const hasMoreWork = scheduledHostCallback(hasTimeRemaining, currentTime);
      if (!hasMoreWork) {
        // 没有剩余任务, 退出
        isMessageLoopRunning = false;
        scheduledHostCallback = null;
      } else {
        port.postMessage(null); // 有剩余任务, 发起新的调度
      }
    } catch (error) {
      port.postMessage(null); // 如有异常, 重新发起调度
      throw error;
    }
  } else {
    isMessageLoopRunning = false;
  }
  needsPaint = false; // 重置开关
};

const channel = new MessageChannel();
const port = channel.port2;
channel.port1.onmessage = performWorkUntilDeadline;

// 请求回调
requestHostCallback = function(callback) {
  // 1. 保存callback
  scheduledHostCallback = callback;
  if (!isMessageLoopRunning) {
    isMessageLoopRunning = true;
    // 2. 通过 MessageChannel 发送消息
    port.postMessage(null);
  }
};
// 取消回调
cancelHostCallback = function() {
  scheduledHostCallback = null;
};
```

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698911829369-d77d2474-bd64-4e9a-85e1-f7997d37742a.png)

+ cancelHostCallback
+ requestHostTimeout
+ cancelHostTimeout

**时间切片(time slicing)相关:  执行时间分割, 让出主线程(把控制权归还浏览器, 浏览器可以处理用户输入, UI 绘制等紧急任务).**

+ getCurrentTime: 获取当前时间
+ shouldYieldToHost: 是否让出主线程

判定条件：

    - currentTime >= deadline: 只有时间超过deadline之后才会让出主线程(其中deadline = currentTime + yieldInterval).
        * yieldInterval默认是5ms, 只能通过forceFrameRate函数来修改(事实上在 v17.0.2 源码中, 并没有使用到该函数).
        * 如果一个task运行时间超过5ms, 下一个task执行之前, 会把控制权归还浏览器.
    - navigator.scheduling.isInputPending():  用于判断是否有输入事件(包括: input 框输入事件, 点击事件等).
+ requestPaint: 请求绘制
+ forceFrameRate: 强制设置 yieldInterval(从源码中的引用来看, 算一个保留函数, 其他地方没有用到)

## 任务队列管理
在Scheduler.js中, 维护了一个taskQueue, 任务队列管理就是围绕这个taskQueue展开.

```javascript
// Tasks are stored on a min heap
var taskQueue = []; // 小顶堆数组，堆排
var timerQueue = [];  // 预留给延时任务使用的
```

1. 创建任务
    1. 获取当前时间，getCurrentTime()
    2. 根据传入的优先级, 设置任务的过期时间 expirationTime
    3. 创建新任务

```javascript
var newTask = {
  id: taskIdCounter++, // id: 一个自增编号
  callback, // callback: 传入的回调函数
  priorityLevel, // priorityLevel: 优先级等级
  startTime, // startTime: 创建task时的当前时间
  expirationTime, // expirationTime: task的过期时间, 优先级越高 expirationTime = startTime + timeout 越小
  sortIndex: -1,
};
newTask.sortIndex = expirationTime; // sortIndex: 排序索引, 全等于过期时间. 保证过期时间越小, 越紧急的任务排在最前面
```

    4. 如果未超时则加入任务队列
    5. 请求调度 
2. 消费任务

创建任务之后, 最后请求调度requestHostCallback(flushWork)，flushWork函数作为参数被传入调度中心内核等待回调, 在调度中心中, 只需下一个事件循环就会执行回调, 最终执行flushWork。flushWork中调用了workLoop（任务调度循环）

workloop：

保存当前时间，用于判断人物是否过期，获取队列中的第一个任务，当该任务存在的时候：

+ 当该任务没有过期但是执行时间超过了限制，则(毕竟只有5ms, shouldYieldToHost()返回true). 停止继续执行, 让出主线程
+ 没有过期的时候，获取到当前任务的回调
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

3. 时间切片原理：消费任务队列的过程中, 可以消费1~n个 task, 甚至清空整个 queue. 但是在每一次具体执行task.callback之前都要进行超时检测, 如果超时可以立即退出循环并等待下一次调用.

# fiber树构造
根据React运行的内存状态，分为两种情况：

1. 初次创建：在React应用层首次启动时，页面还没有渲染，此时并不会进入对比过程，相当于直接构造一棵全新的树
2. 对比更新：React应用启动后，界面已经渲染，如果再次发生更新，创建新fiber之前需要和旧fiber进行对比，最后的fiber树有可能是全新的，也可能是部分更新的

## ReactElement，Fiber，DOM三者的关系
1. ReactElement对象：所有采用jsx语法树写的节点，都会被编译器转换，最终会以React.createElement(...)的方式, 创建出来一个与之对应的ReactElement对象
2. fiber对象：fiber对象是通过ReactElement对象进行创建的，多个fiber对象构成了一棵fiber树，fiber树是构造DOM树的数据模型，fiber树的任何改动，最后都体现到DOM树
3. DOM对象：DOM将文档解析为一个由节点和对象（包含属性和方法的对象）组成的结构集合, 也就是常说的DOM树.

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698936711372-4881f777-6aee-4495-b717-420b2528fcf4.png)

## 全局变量
![画板](https://cdn.nlark.com/yuque/0/2023/jpeg/2366100/1698974431042-b2263a82-7afb-4658-a9f2-22eac81b2304.jpeg)

## 执行上下文
在全局变量中有executionContext, 代表渲染期间的执行栈(或叫做执行上下文), 它也是一个二进制表示的变量, 在源码中一共定义了 8 种执行栈：

```javascript
type ExecutionContext = number;
export const NoContext = /*             */ 0b0000000;
const BatchedContext = /*               */ 0b0000001;
const EventContext = /*                 */ 0b0000010;
const DiscreteEventContext = /*         */ 0b0000100;
const LegacyUnbatchedContext = /*       */ 0b0001000;
const RenderContext = /*                */ 0b0010000;
const CommitContext = /*                */ 0b0100000;
```

事实上，正是executionContext在操控reconciler运作流程

```javascript
export function scheduleUpdateOnFiber(
  fiber: Fiber,
  lane: Lane,
  eventTime: number,
) {
  if (lane === SyncLane) {
    // legacy或blocking模式
    if (
      (executionContext & LegacyUnbatchedContext) !== NoContext &&
      (executionContext & (RenderContext | CommitContext)) === NoContext
    ) {
      performSyncWorkOnRoot(root);
    } else {
      // 后续的更新
      // 进入第2阶段, 注册调度任务
      ensureRootIsScheduled(root, eventTime);
      if (executionContext === NoContext) {
        // 如果执行上下文为空, 会取消调度任务, 手动执行回调
        // 进入第3阶段, 进行fiber树构造
        flushSyncCallbackQueue();
      }
    }
  } else {
    // concurrent模式
    // 无论是否初次更新, 都正常进入第2阶段, 注册调度任务
    ensureRootIsScheduled(root, eventTime);
  }
}
```

在 render 过程中, 每一个阶段都会改变executionContext(render 之前, 会设置executionContext |= RenderContext; commit 之前, 会设置executionContext |= CommitContext), 假设在render过程中再次发起更新(如在UNSAFE_componentWillReceiveProps生命周期中调用setState)则可通过executionContext来判断当前的render状态.

## 双缓冲技术（double buffering）
workInProgress的应用实际上就是React的双缓冲技术(double buffering).

fiber的过程就是把ReactElement转换成fiber树的过程，在这个过程中，内存里会同时存在2棵fiber树：

1. 代表当前界面的fiber树（已经被展示出来，挂载到fiberRoot.current上），如果是初次构造（初始化渲染），页面还没有渲染，此时界面对应的fiber树为空（fiberRoot.current = null）
2. 正在构造的fiber树（即将展示出来，挂载到HostRootFiber.alternate上, 正在构造的节点称为workInProgress）。当构造完成之后, 重新渲染页面, 最后切换fiberRoot.current = workInProgress, 使得fiberRoot.current重新指向代表当前界面的fiber树。

> ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698975542137-1e387bc8-64be-496c-8784-099990149f90.png)
>

1. 构造过程中，fiberRoot.current指向当前界面对应的fiber树.

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698975590790-1796af8f-f336-4c83-8d25-961272a390c1.png)

2. 构造完成并渲染, 切换fiberRoot.current指针, 使其继续指向当前界面对应的fiber树(原来代表界面的 fiber 树, 变成了内存中).

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698975685940-f4d7d438-a6a9-4fbb-9845-dbecc4527373.png)

## 车道模型Lane的具体应用
### update优先级（update.lane）
update对象是一个环形链表，对于单个的update对象来讲，update.lane代表它的优先级，称之为update优先级

创建update对象的createUpdate构造函数，其优先级是由外界传入的

```javascript
export function createUpdate(eventTime: number, lane: Lane): Update<*> {
  const update: Update<*> = {
    eventTime,
    lane,
    tag: UpdateState,
    payload: null,
    callback: null,
    next: null,
  };
  return update;
}
```

创建update对象的两种情况：

无论是应用初始化或者发起组件更新, 创建update.lane的逻辑都是一样的, 都是根据当前时间, 创建一个 update 优先级.

1. 应用初始化，在react-reconciler包中的updateContainer函数中

```javascript
export function updateContainer(
  element: ReactNodeList,
  container: OpaqueRoot,
  parentComponent: ?React$Component<any, any>,
  callback: ?Function,
): Lane {
  const current = container.current;
  const eventTime = requestEventTime();
  // 根据当前时间, 创建一个update优先级
  const lane = requestUpdateLane(current); 
  // lane被用于创建update对象
  const update = createUpdate(eventTime, lane); 
  update.payload = { element };
  enqueueUpdate(current, update);
  scheduleUpdateOnFiber(current, lane, eventTime);
  return lane;
}
```

2. 发起组件更新，假设在class组件中调用setState

```javascript
const classComponentUpdater = {
  isMounted,
  enqueueSetState(inst, payload, callback) {
    const fiber = getInstance(inst);
     // 根据当前时间, 创建一个update优先级
    const eventTime = requestEventTime();
    // lane被用于创建update对象
    const lane = requestUpdateLane(fiber); 
    const update = createUpdate(eventTime, lane);
    update.payload = payload;
    enqueueUpdate(fiber, update);
    scheduleUpdateOnFiber(fiber, lane, eventTime);
  },
};
```

requestUpdateLane函数：

返回一个合适的update优先级

+ legacy模式：返回SyncLane
+ blocking模式：返回SyncLane
+ concurrent模式：
    - 正常情况下， 根据当前的调度优先级来生成一个lane
    - 特殊情况下(处于 suspense 过程中), 会优先选择TransitionLanes通道中的空闲通道(如果所有TransitionLanes通道都被占用, 就取最高优先级）

```javascript
export function requestUpdateLane(fiber: Fiber): Lane {
  // Special cases
  const mode = fiber.mode;
  if ((mode & BlockingMode) === NoMode) {
    // legacy 模式
    return (SyncLane: Lane);
  } else if ((mode & ConcurrentMode) === NoMode) {
    // blocking模式
    return getCurrentPriorityLevel() === ImmediateSchedulerPriority
      ? (SyncLane: Lane)
      : (SyncBatchedLane: Lane);
  }
  // concurrent模式
  if (currentEventWipLanes === NoLanes) {
    currentEventWipLanes = workInProgressRootIncludedLanes;
  }
  const isTransition = requestCurrentTransition() !== NoTransition;
  if (isTransition) {
    // 特殊情况, 处于suspense过程中
    if (currentEventPendingLanes !== NoLanes) {
      currentEventPendingLanes =
        mostRecentlyUpdatedRoot !== null
          ? mostRecentlyUpdatedRoot.pendingLanes
          : NoLanes;
    }
    return findTransitionLane(currentEventWipLanes, currentEventPendingLanes);
  }
  // 正常情况, 获取调度优先级
  const schedulerPriority = getCurrentPriorityLevel();
  let lane;
  if (
    (executionContext & DiscreteEventContext) !== NoContext &&
    schedulerPriority === UserBlockingSchedulerPriority
  ) {
    // executionContext 存在输入事件. 且调度优先级是用户阻塞性质
    lane = findUpdateLane(InputDiscreteLanePriority, currentEventWipLanes);
  } else {
    // 调度优先级转换为车道模型
    const schedulerLanePriority = schedulerPriorityToLanePriority(
      schedulerPriority,
    );
    lane = findUpdateLane(schedulerLanePriority, currentEventWipLanes);
  }
  return lane;
}
```

scheduleUpdateOnFiber是输入阶段的必经函数，当lane === SyncLane也就是 legacy 或 blocking 模式中, 注册完回调任务之后(ensureRootIsScheduled(root, eventTime)), 如果执行上下文为空, 会取消 schedule 调度, 主动刷新回调队列flushSyncCallbackQueue()。

#### setState是同步还是异步？
+ 如果逻辑进入flushSyncCallbackQueue(executionContext === NoContext), 则会主动取消调度, 并刷新回调, 立即进入fiber树构造过程. 当执行setState下一行代码时, fiber树已经重新渲染了, 故setState体现为同步.
+ 正常情况下, 不会取消schedule调度. 由于schedule调度是通过MessageChannel触发(宏任务), 故体现为异步.

### 渲染优先级
这是一个全局概念, 每一次render之前, 首先要确定本次render的优先级。无论是legacy还是concurrent模式，正式render之前，都会调用getNextLanes获取一个优先级。该函数会根据fiberRoot对象上的属性（expiredLanes, suspendedLanes, pingedLanes等), 确定出当前最紧急的lanes.。此处返回的lanes会作为全局渲染的优先级, 用于fiber树构造过程中. 针对fiber对象或update对象, 只要它们的优先级(如: fiber.lanes和update.lane)比渲染优先级低, 都将会被忽略。

### fiber优先级
fiber对象的数据结构中，其中有两个属性与优先级相关：

1. fiber.lanes: 代表本节点的优先级
2. fiber.childLanes: 代表子节点的优先级

fiber.lanes和fiber.childLanes的初始值都为NoLanes, 在fiber树构造过程中, 使用全局的渲染优先级(renderLanes)和fiber.lanes判断fiber节点是否更新：

+ 如果全局的渲染优先级renderLanes不包括fiber.lanes, 证明该fiber节点没有更新, 可以复用.
+ 如果不能复用, 进入创建阶段.

## 栈帧管理
在React源码中，每一次执行fiber树构造(也就是调用performSyncWorkOnRoot或者performConcurrentWorkOnRoot函数)的过程，都需要一些全局变量来保存状态。如果从单个变量来看,  它们就是一个个的全局变量。如果将这些全局变量组合起来,  它们代表了当前fiber树构造的活动记录。通过这一组全局变量,  可以还原fiber树构造过程(比如时间切片的实现过程，fiber树构造过程被打断之后需要还原进度,  全靠这一组全局变量)。 所以每次fiber树构造是一个独立的过程,  需要独立的一组全局变量,  在React内部把这一个独立的过程封装为一个栈帧stack(简单来说就是每次构造都需要独立的空间）所以在进行fiber树构造之前,  如果不需要恢复上一次构造进度, 都会刷新栈帧

## 初次创建
### 启动阶段
在进入react-conciler之前，内存状态图为

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698978806258-a3e6ad7b-db05-4f16-948e-494cd007c881.png)然后进入react-reconciler包调用，根据update对象的创建，此时的内存结构为

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698978869269-2e58834d-35ae-42c8-9076-2dc686b94bde.png)

### 构造阶段
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698978977749-8048dd77-bfbd-4064-839e-9da3bb324e56.png)

在Legacy模式下且首次渲染时, 有 2 个函数markUpdateLaneFromFiberToRoot和performSyncWorkOnRoot。其中markUpdateLaneFromFiberToRoot(fiber, lane)函数在fiber树构造(对比更新)中才会发挥作用, 因为在初次创建时并没有与当前页面所对应的fiber树, 所以核心代码并没有执行, 最后直接返回了FiberRoot对象。

```javascript
// ...省略部分代码
export function scheduleUpdateOnFiber(
  fiber: Fiber,
  lane: Lane,
  eventTime: number,
) {
  // 标记优先级
  const root = markUpdateLaneFromFiberToRoot(fiber, lane);
  if (lane === SyncLane) {
    if (
      (executionContext & LegacyUnbatchedContext) !== NoContext &&
      (executionContext & (RenderContext | CommitContext)) === NoContext
    ) {
      // 首次渲染, 直接进行`fiber构造`
      performSyncWorkOnRoot(root);
    }
    // ...
  }
}
function performSyncWorkOnRoot(root) {
  let lanes;
  let exitStatus;
  if (
    root === workInProgressRoot &&
    includesSomeLane(root.expiredLanes, workInProgressRootRenderLanes)
  ) {
    // 初次构造时(因为root=fiberRoot, workInProgressRoot=null), 所以不会进入
  } else {
    // 1. 获取本次render的优先级, 初次构造返回 NoLanes
    lanes = getNextLanes(root, NoLanes);
    // 2. 从root节点开始, 至上而下更新
    exitStatus = renderRootSync(root, lanes);
  }

  // 将最新的fiber树挂载到root.finishedWork节点上
  const finishedWork: Fiber = (root.current.alternate: any);
  root.finishedWork = finishedWork;
  root.finishedLanes = lanes;
  // 进入commit阶段
  commitRoot(root);

  // ...后面的内容本节不讨论
}
function renderRootSync(root: FiberRoot, lanes: Lanes) {
  const prevExecutionContext = executionContext;
  executionContext |= RenderContext;
  // 如果fiberRoot变动, 或者update.lane变动, 都会刷新栈帧, 丢弃上一次渲染进度
  if (workInProgressRoot !== root || workInProgressRootRenderLanes !== lanes) {
    // 刷新栈帧, legacy模式下都会进入
    prepareFreshStack(root, lanes);
  }
  do {
    try {
      workLoopSync();
      break;
    } catch (thrownValue) {
      handleError(root, thrownValue);
    }
  } while (true);
  executionContext = prevExecutionContext;
  // 重置全局变量, 表明render结束
  workInProgressRoot = null;
  workInProgressRootRenderLanes = NoLanes;
  return workInProgressRootExitStatus;
}
```

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698980057264-916331e9-8324-4033-940a-12d1d2ba8705.png)

#### 循环构造
> workLoopSync, 虽然本节在Legacy模式下进行讨论, 此处还是对比一下workLoopConcurrent
>

```javascript
function workLoopSync() {
  while (workInProgress !== null) {
    performUnitOfWork(workInProgress);
  }
}

function workLoopConcurrent() {
  // Perform work until Scheduler asks us to yield
  while (workInProgress !== null && !shouldYield()) {
    performUnitOfWork(workInProgress);
  }
}
// ... 省略部分无关代码
function performUnitOfWork(unitOfWork: Fiber): void {
  // unitOfWork就是被传入的workInProgress
  const current = unitOfWork.alternate;
  let next;
  next = beginWork(current, unitOfWork, subtreeRenderLanes);
  unitOfWork.memoizedProps = unitOfWork.pendingProps;
  if (next === null) {
    // 如果没有派生出新的节点, 则进入completeWork阶段, 传入的是当前unitOfWork
    completeUnitOfWork(unitOfWork);
  } else {
    workInProgress = next;
  }
}
```

**workLoopConcurrent相比于Sync, 会多一个停顿机制, 这个机制实现了时间切片和可中断渲染**

整个fiber树构造是一个深度优先遍历,  其中有 2 个重要的变量workInProgress和current

workInProgress和current都视为指针

workInProgress指向当前正在构造的fiber节点

current = workInProgress.alternate(即fiber.alternate), 指向当前页面正在使用的fiber节点. 初次构造时, 页面还未渲染, 此时current = null.

在深度优先遍历中, 每个fiber节点都会经历 2 个阶段:

1. **探寻阶段 beginWork**
2. **回溯阶段 completeWork**

#### 探寻阶段beginWork
beginWork(current, unitOfWork, subtreeRenderLanes)针对所有的 Fiber 类型, 其中的每一个 case 处理一种 Fiber 类型. updateXXX函数(如: updateHostRoot, updateClassComponent 等)的主要逻辑：

1. 根据 ReactElement对象创建所有的fiber节点, 最终构造出fiber树形结构(设置return和sibling指针)
2. 设置fiber.flags(二进制形式变量, 用来标记 fiber节点 的增,删,改状态, 等待completeWork阶段处理)
3. 设置fiber.stateNode局部状态(如Class类型节点: fiber.stateNode=new Class())

updatexxx函数不同的updateXXX函数处理的fiber节点类型不同,  总的目的是为了向下生成子节点.  在这个过程中把一些需要持久化的数据挂载到fiber节点上;  把fiber节点的特殊操作设置到fiber.flags，主要逻辑可以概括为3个步骤：

1. 根据fiber.pendingProps, fiber.updateQueue等输入数据状态, 计算fiber.memoizedState作为输出状态
2. 获取下级ReactElement对象
    1. class 类型的 fiber 节点
    - 构建React.Component实例
    - 把新实例挂载到fiber.stateNode上
    - 执行render之前的生命周期函数
    - 执行render方法, 获取下级reactElement
    - 根据实际情况, 设置fiber.flags
    2. function 类型的 fiber 节点
    - 执行 function, 获取下级reactElement
    - 根据实际情况, 设置fiber.flags
    3. HostComponent 类型(如: div, span, button 等)的 fiber 节点
    - pendingProps.children作为下级reactElement
    - 如果下级节点是文本节点,则设置下级节点为 null. 准备进入completeUnitOfWork阶段
    - 根据实际情况, 设置fiber.flags
    4. 其他类型...
3. 根据ReactElement对象, 调用reconcileChildren生成Fiber子节点(只生成次级子节点)
+ 根据实际情况, 设置fiber.flags

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

#### 回溯阶段completeWork
处理beginWork探寻阶段已经创建出来的fiber节点，核心逻辑：

1. 调用completeWork
+ 给fiber节点(tag=HostComponent, HostText)创建 DOM 实例, 设置fiber.stateNode局部状态(如tag=HostComponent, HostText节点: fiber.stateNode 指向这个 DOM 实例).
+ 为dom节点设置属性，绑定事件
+ 设置fiber.flags标记
2. 把当前fiber对象的副作用队列(firstEffect和lastEffect)添加到父节点的副作用队列之后, 更新父节点的firstEffect和lastEffect指针.
3. 识别beginWork阶段设置的fiber.flags, 判断当前 fiber 是否有副作用(增,删,改), 如果有, 需要将当前 fiber 加入到父节点的effects队列, 等待commit阶段处理.

### 初次创建过程总结
示例

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699346267361-7f80d3be-0896-4bda-a5b6-2c1610d5463b.png)

构造前

进入循环构造前会调用prepareFreshStack刷新栈帧, 在进入fiber树构造循环之前, 保持这这个初始化状态：

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699345103258-234f4205-52d8-401b-ac82-90fb5df6ee79.png)

performUnitOfWork第 1 次调用(只执行beginWork)：

+ 执行前: workInProgress指针指向HostRootFiber.alternate对象, 此时current = workInProgress.alternate指向fiberRoot.current是非空的(初次构造, 只在根节点时, current非空).
+ 执行过程: 调用updateHostRoot
    - 在reconcileChildren阶段, 向下构造次级子节点fiber(<App/>), 同时设置子节点(fiber(<App/>))[fiber.flags |= Placement](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactChildFiber.old.js#L376-L378)
+ 执行后: 返回下级节点fiber(<App/>), 移动workInProgress指针指向子节点fiber(<App/>)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699345198267-2bbe1b46-91a9-48c6-a598-3c4aa3257c95.png)

performUnitOfWork第 2 次调用(只执行beginWork):

+ 执行前: workInProgress指针指向fiber(<App/>)节点, 此时current = null
+ 执行过程: 调用updateClassComponent
    - 本示例中, class 实例存在生命周期函数componentDidMount, 所以会设置fiber(<App/>)节点[workInProgress.flags |= Update](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberClassComponent.old.js#L892-L894)
    - 另外也会为了React DevTools能够识别状态组件的执行进度, 会设置[workInProgress.flags |= PerformedWork](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberBeginWork.old.js#L379)
    - 需要注意classInstance.render()在本步骤执行后, 虽然返回了render方法中所有的ReactElement对象, 但是随后reconcileChildren只构造次级子节点
    - 在reconcileChildren阶段, 向下构造次级子节点div
+ 执行后: 返回下级节点fiber(div), 移动workInProgress指针指向子节点fiber(div)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699345470086-1e9451fa-c920-42f7-a79a-df9e06a00b73.png)

performUnitOfWork第 3 次调用(只执行beginWork):

+ 执行前: workInProgress指针指向fiber(div)节点, 此时current = null
+ 执行过程: 调用updateHostComponent
    - 在reconcileChildren阶段, 向下构造次级子节点(本示例中, div有 2 个次级子节点)
+ 执行后: 返回下级节点fiber(header), 移动workInProgress指针指向子节点fiber(header)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699345539277-8b9106f6-a4a3-4e8e-be7d-06b1f0c17ee2.png)

performUnitOfWork第 4 次调用(执行beginWork和completeUnitOfWork):

+ beginWork执行前: workInProgress指针指向fiber(header)节点, 此时current = null
+ beginWork执行过程: 调用updateHostComponent
    - 本示例中header的子节点是一个[直接文本节点](https://github.com/facebook/react/blob/8e5adfbd7e605bda9c5e96c10e015b3dc0df688e/packages/react-dom/src/client/ReactDOMHostConfig.js#L350-L361),设置[nextChildren = null](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberBeginWork.old.js#L1147)(直接文本节点并不会被当成具体的fiber节点进行处理, 而是在宿主环境(父组件)中通过属性进行设置. 所以无需创建HostText类型的fiber节点, 同时节省了向下遍历开销.).
    - 由于nextChildren = null, 经过reconcileChildren阶段处理后, 返回值也是null
+ beginWork执行后: 由于下级节点为null, 所以进入completeUnitOfWork(unitOfWork)函数, 传入的参数unitOfWork实际上就是workInProgress(此时指向fiber(header)节点)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699345616595-ba111530-fb56-423c-a900-8ec874e87a35.png)

+ completeUnitOfWork执行前: workInProgress指针指向fiber(header)节点
+ completeUnitOfWork执行过程: 以fiber(header)为起点, 向上回溯

第一次循环

1. 执行completeWork函数
    - 创建fiber(header)节点对应的DOM实例, 并append子节点的DOM实例
    - 设置DOM属性, 绑定事件等(本示例中, 节点fiber(header)没有事件绑定)
2. 上移副作用队列: 由于本节点fiber(header)没有副作用(fiber.flags = 0), 所以执行之后副作用队列没有实质变化(目前为空).
3. 向上回溯: 由于还有兄弟节点, 把workInProgress指针指向下一个兄弟节点fiber(<Content/>), 退出completeUnitOfWork.

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699345889654-e1ab3785-c923-440a-85dd-cb9fb37d83e2.png)

performUnitOfWork第 5 次调用(执行beginWork):

+ 执行前:workInProgress指针指向fiber(<Content/>)节点.
+ 执行过程: 这是一个class类型的节点, 与第 2 次调用逻辑一致.
+ 执行后: 返回下级节点fiber(p), 移动workInProgress指针指向子节点fiber(p)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699345932429-2a11017d-2105-4e85-91af-4607014f4131.png)

performUnitOfWork第 6 次调用(执行beginWork和completeUnitOfWork)：

与第 4 次调用中创建fiber(header)节点的逻辑一致. 先后会执行beginWork和completeUnitOfWork, 最后构造 DOM 实例, 并将把workInProgress指针指向下一个兄弟节点fiber(p).

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699345999772-27705852-edf7-4bb1-86b8-38bd8c0110ce.png)

performUnitOfWork第 7 次调用(执行beginWork和completeUnitOfWork):

+ beginWork执行过程: 与上次调用中创建fiber(p)节点的逻辑一致
+ completeUnitOfWork执行过程: 以fiber(p)为起点, 向上回溯

第 1 次循环:

1. 执行completeWork函数: 创建fiber(p)节点对应的DOM实例, 并append子树节点的DOM实例
2. 上移副作用队列: 由于本节点fiber(p)没有副作用, 所以执行之后副作用队列没有实质变化(目前为空).
3. 向上回溯: 由于没有兄弟节点, 把workInProgress指针指向父节点fiber(<Content/>)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699346040953-1ced334a-c1e7-4fbb-947c-66dfacefb534.png)

第 2 次循环:

1. 执行completeWork函数: class 类型的节点不做处理
2. 上移副作用队列:
    - 本节点fiber(<Content/>)的flags标志位有改动(completedWork.flags > PerformedWork), 将本节点添加到父节点(fiber(div))的副作用队列之后(firstEffect和lastEffect属性分别指向副作用队列的首部和尾部).
3. 向上回溯: 把workInProgress指针指向父节点fiber(div)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699346102591-ebc1d764-0c7a-45a0-816a-a12f2b1cb2ce.png)

第 3 次循环:

1. 执行completeWork函数: 创建fiber(div)节点对应的DOM实例, 并append子树节点的DOM实例
2. 上移副作用队列:
    - 本节点fiber(div)的副作用队列不为空, 将其拼接到父节点fiber<App/>的副作用队列后面.
3. 向上回溯: 把workInProgress指针指向父节点fiber(<App/>)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699346121042-70dd453b-40d2-47d7-9f93-c5a40d677f26.png)

第 4 次循环:

1. 执行completeWork函数: class 类型的节点不做处理
2. 上移副作用队列:
    - 本节点fiber(<App/>)的副作用队列不为空, 将其拼接到父节点fiber(HostRootFiber)的副作用队列上.
    - 本节点fiber(<App/>)的flags标志位有改动(completedWork.flags > PerformedWork), 将本节点添加到父节点fiber(HostRootFiber)的副作用队列之后.
    - 最后队列的顺序是子节点在前, 本节点在后
3. 向上回溯: 把workInProgress指针指向父节点fiber(HostRootFiber)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699346143988-aef8d08d-cec1-4a5e-a629-84fa190cb759.png)

第 5 次循环:

1. 执行completeWork函数: 对于HostRoot类型的节点, 初次构造时设置[workInProgress.flags |= Snapshot](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberCompleteWork.old.js#L693)
2. 向上回溯: 由于父节点为空, 无需进入处理副作用队列的逻辑. 最后设置workInProgress=null, 并退出completeUnitOfWork

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699346169766-ad9e2591-ae47-4fef-b5c1-99359d3badbb.png)

到此整个fiber树构造循环已经执行完毕, 拥有一棵完整的fiber树, 并且在fiber树的根节点上挂载了副作用队列, 副作用队列的顺序是层级越深子节点越靠前.

renderRootSync函数退出之前, 会重置workInProgressRoot = null, 表明没有正在进行中的render. 且把最新的fiber树挂载到fiberRoot.finishedWork上. 这时整个 fiber 树的内存结构如下(注意fiberRoot.finishedWork和fiberRoot.current指针,在commitRoot阶段会进行处理):

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699346229055-9141e179-4018-4ce5-b622-25d4e5e22266.png)

## 对比更新
基于如下示例

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699346538206-cb187b3d-4d4c-4803-abdb-dd876ec83c63.png)

### 三种更新方式
1. Class组件中调用setState.
2. Function组件中调用hook对象暴露出的dispatchAction.
3. 在container节点上重复调用render

无论从哪个入口进行更新, 最终都会进入scheduleUpdateOnFiber, 再次证明scheduleUpdateOnFiber是输入阶段的必经函数

### 对比更新与初次渲染的不同点
1. [markUpdateLaneFromFiberToRoot](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberWorkLoop.old.js#L625-L667)函数, 只在对比更新阶段才发挥出它的作用, 它找出了fiber树中受到本次update影响的所有节点, 并设置这些节点的fiber.lanes或fiber.childLanes(在legacy模式下为SyncLane)以备fiber树构造阶段使用.
2. 对比更新没有直接调用performSyncWorkOnRoot, 而是通过调度中心来处理, 由于本示例是在Legacy模式下进行, 最后会同步执行performSyncWorkOnRoot.(详细原理可以参考[React 调度原理(scheduler)](https://react-illustration-series.osrc.com/main/scheduler)). 所以其调用链路performSyncWorkOnRoot--->renderRootSync--->workLoopSync与初次构造中的一致.

进入循环构造(workLoopSync)前, 会刷新栈帧(调用prepareFreshStack)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699347816518-ff61fe3f-b88e-4335-804c-bbc3a0d36676.png)

注意:

+ fiberRoot.current指向与当前页面对应的fiber树, workInProgress指向正在构造的fiber树.
+ 刷新栈帧会调用createWorkInProgress(), 使得workInProgress.flags和workInProgress.effects都已经被重置. 且workInProgress.child = current.child. 所以在进入循环构造之前, HostRootFiber与HostRootFiber.alternate共用一个child(这里是fiber(<App/>)).

### bailout解救
bailout用于判断子树节点是否完全复用, 如果可以复用, 则会略过 fiber 树构造.

与初次创建不同, 在对比更新过程中, 如果是老节点, 那么current !== null, 需要进行对比, 然后决定是否复用老节点及其子树(即bailout逻辑).

1. !includesSomeLane(renderLanes, updateLanes)这个判断分支, 包含了渲染优先级和update优先级的比较, 如果当前节点无需更新, 则会进入bailout逻辑.
2. 最后会调用bailoutOnAlreadyFinishedWork:
+ 如果同时满足!includesSomeLane(renderLanes, workInProgress.childLanes), 表明该 fiber 节点及其子树都无需更新, 可直接进入回溯阶段(completeUnitOfWork)
+ 如果不满足!includesSomeLane(renderLanes, workInProgress.childLanes), 意味着子节点需要更新, clone并返回子节点

注意: cloneChildFibers内部调用createWorkInProgress, 在构造fiber节点时会优先复用workInProgress.alternate(不开辟新的内存空间), 否则才会创建新的fiber对象.

### updatexxx函数
updateXXX函数的主干逻辑与初次构造过程完全一致, 总的目的是为了向下生成子节点, 并在这个过程中调用reconcileChildren调和函数, 只要fiber节点有副作用, 就会把特殊操作设置到fiber.flags

对比更新过程的不同之处:

1. bailoutOnAlreadyFinishedWork
+ 对比更新时如果遇到当前节点无需更新(如: class类型的节点且shouldComponentUpdate返回false), 会再次进入bailout逻辑.
2. reconcileChildren调和函数
+ 调和函数是updateXXX函数中的一项重要逻辑, 它的作用是向下生成子节点, 并设置fiber.flags.
+ 初次创建时fiber节点没有比较对象, 所以在向下生成子节点的时候没有任何多余的逻辑, 只管创建就行.
+ 对比更新时需要把ReactElement对象与旧fiber对象进行比较, 来判断是否需要复用旧fiber对象.

调和函数目的:

1. 给新增,移动,和删除节点设置fiber.flags(新增,移动: Placement, 删除: Deletion)
2. 如果是需要删除的fiber, [除了自身打上Deletion之外, 还要将其添加到父节点的effects链表中](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactChildFiber.old.js#L275-L294)

## fiber树渲染
fiber树的基本特点：

+ 无论是首次构造或者是对比更新, 最终都会在内存中生成一棵用于渲染页面的fiber树(即fiberRoot.finishedWork).
+ 这棵将要被渲染的fiber树有 2 个特点:
    1. 副作用队列挂载在根节点上(具体来讲是finishedWork.firstEffect)
    2. 代表最新页面的DOM对象挂载在fiber树中首个HostComponent类型的节点上(具体来讲DOM对象是挂载在fiber.stateNode属性上)

初次构造：

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699348469488-ac787507-ef0c-458f-8d94-06f0d4e3ebaa.png)

对比更新：

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699348504024-5a124f9b-dbcd-40c0-9359-171df1046214.png)

### commitRoot
整个渲染逻辑都在commitroot中

#### 渲染前
准备工作：

1. 设置全局状态(如: 更新fiberRoot上的属性)
2. 重置全局变量(如: workInProgressRoot, workInProgress等)
3. 再次更新副作用队列: 只针对根节点fiberRoot.finishedWork
+ 默认情况下根节点的副作用队列是不包括自身的, 如果根节点有副作用, 则将根节点添加到副作用队列的末尾
+ 注意只是延长了副作用队列, 但是fiberRoot.lastEffect指针并没有改变. 比如首次构造时, 根节点拥有Snapshot标记:

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699352121507-1b88c621-bb3c-49e3-aa65-d35b55b8a1d3.png)

#### 渲染
commitRootImpl函数中, 渲染阶段的主要逻辑是处理副作用队列, 将最新的 DOM 节点(已经在内存中, 只是还没渲染)渲染到界面上.

整个渲染过程被分为 3 个函数分布实现：

1. commitBeforeMutationEffects
+ dom 变更之前, 处理副作用队列中带有Snapshot,Passive标记的fiber节点.
    1. 处理snapshot标记
    - 对于ClassComponent类型节点, 调用了instance.getSnapshotBeforeUpdate生命周期函数
    - 对于HostRoot类型节点, 调用clearContainer清空了容器节点(即div#root这个 dom 节点).
    2. 处理passive标记
    - Passive标记只会在使用了hook对象的function类型的节点上存在。在commitRoot的第一个阶段, 为了处理hook对象(如useEffect), 通过scheduleCallback单独注册了一个调度任务task, 等待调度中心scheduler处理.
2. commitMutationEffects
+ dom 变更, 界面得到更新. 处理副作用队列中带有Placement, Update, Deletion, Hydrating标记的fiber节点.

处理 DOM 突变:

> 最终会调用appendChild, commitUpdate, removeChild这些react-dom包中的函数.  在渲染器react-dom包中进行实现. 这些函数就是直接操作 DOM, 所以执行之后, 界面也会得到更新.
>
> commitMutationEffects执行之后, 在commitRootImpl函数中切换当前fiber树(root.current = finishedWork),保证fiberRoot.current指向代表当前界面的fiber树
>

    1. 新增: 函数调用栈 commitPlacement -> insertOrAppendPlacementNode -> appendChild
    2. 更新: 函数调用栈 commitWork -> commitUpdate
    3. 删除: 函数调用栈 commitDeletion -> removeChild
1. commitLayoutEffects
+ dom 变更后, 处理副作用队列中带有Update | Callback标记的fiber节点.
    - 对于ClassComponent节点, 调用生命周期函数componentDidMount或componentDidUpdate, 调用update.callback回调函数.
    - 对于HostComponent节点, 如有Update标记, 需要设置一些原生状态(如: focus等)

这 3 个函数处理的对象是副作用队列和DOM对象：

+ 副作用队列所在节点: 根节点, 即HostRootFiber节点.
+ DOM对象所在节点: 从上至下首个HostComponent类型的fiber节点, 此节点 fiber.stateNode实际上指向最新的 DOM 树.

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699352251260-0808e940-56ce-4e6a-9f87-f3bf61b1b92f.png)

#### 渲染后
在渲染完成后, 需要做一些重置和清理工作:

1. 清除副作用队列
+ 由于副作用队列是一个链表, 由于单个fiber对象的引用关系, 无法被gc回收.
+ 将链表全部拆开, 当fiber对象不再使用的时候, 可以被gc回收.
2. 检测更新
+ 在整个渲染过程中, 有可能产生新的update(比如在componentDidMount函数中, 再次调用setState()).
+ 如果是常规(异步)任务, 不用特殊处理, 调用ensureRootIsScheduled确保任务已经注册到调度中心即可.
+ 如果是同步任务, 则主动调用flushSyncCallbackQueue(无需再次等待 scheduler 调度), 再次进入 fiber 树构造循环

# 状态与副作用
fiber众多属性中，有两类属性十分关键：

1. fiber节点的自身状态: 在renderRootSync[Concurrent]阶段, 为子节点提供确定的输入数据, 直接影响子节点的生成.
2. fiber节点的副作用: 在commitRoot阶段, 如果fiber被标记有副作用, 则副作用相关函数会被(同步/异步)调用.

```javascript
export type Fiber = {|
  /** 1. fiber节点自身状态相关 */
  // 输入属性, 从ReactElement对象传入的 props. 
  // 它和fiber.memoizedProps比较可以得出属性是否变动.
  pendingProps: any,
  // 上一次生成子节点时用到的属性, 生成子节点之后保持在内存中. 
  // 向下生成子节点之前叫做pendingProps, 生成子节点之后会把
  // pendingProps赋值给memoizedProps用于下一次比较.
  memoizedProps: any,
  // 存储update更新对象的队列, 每一次发起更新, 都需要在该队列上创建一个update对象.
  updateQueue: mixed,
  // 上一次生成子节点之后保持在内存中的局部状态.
  memoizedState: any,

  /** 2. fiber节点副作用(Effect)相关 */
  // 标志位, 表明该fiber节点有副作用
  flags: Flags,
  subtreeFlags: Flags, // v17.0.2未启用
  deletions: Array<Fiber> | null, // v17.0.2未启用
  // 单向链表, 指向下一个副作用 fiber节点
  nextEffect: Fiber | null,
  // 单向链表, 指向第一个副作用 fiber 节点
  firstEffect: Fiber | null,
  // 单向链表, 指向最后一个副作用 fiber 节点.
  lastEffect: Fiber | null,
|};
```

单个fiber节点的副作用队列最后都会上移到根节点上,  所以在commitRoot阶段中, react提供了 3 种处理副作用的方式,  另外, 副作用的设计可以理解为对状态功能不足的补充.

+ 状态是一个静态的功能, 它只能为子节点提供数据源
+ 而副作用是一个动态功能, 由于它的调用时机是在fiber树渲染阶段, 故它拥有更多的能力, 能轻松获取突变前快照, 突变后的DOM节点等. 甚至通过调用api发起新的一轮fiber树构造, 进而改变更多的状态, 引发更多的副作用

# Hook原理
Hook最终也是为了控制fiber节点的状态和副作用

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
  memoizedState: any, // 当前状态
  baseState: any, // 基状态
  baseQueue: Update<any, any> | null, // 基队列
  queue: UpdateQueue<any, any> | null, // 更新队列
  next: Hook | null, // next指针
|};
```

+ memorized：保持在内存中的局部状态
+ baseState：baseQuene中所有update对象合并之后的状态
+ baseQuene：存储update对象的环形链表, 只包括高于本次渲染优先级的update对象.
+ quene：存储update对象的环形链表，包括所有优先级的update对象
+ next：一个指针，指向链表中的下一个hook。

Hook是一个链表，单个hook拥有自己的状态memoizedState和自己的更新队列hook.queue

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699366324955-ab2546e8-af41-4cc0-af17-ae48dd1e9b5d.png)

## Hook分类
## 状态Hook
狭义上讲, useState, useReducer可以在function组件添加内部的state, 且useState实际上是useReducer的简易封装, 是一个最特殊(简单)的useReducer. 所以将useState, useReducer称为状态Hook.

广义上讲, 只要能实现数据持久化且没有副作用的Hook, 均可以视为状态Hook, 所以还包括useContext, useRef, useCallback, useMemo等. 这类Hook内部没有使用useState/useReducer, 但是它们也能实现多次render时, 保持其初始值不变(即数据持久化)且没有任何副作用.

得益于[双缓冲技术(double buffering)](https://react-illustration-series.osrc.com/main/fibertree-prepare#%E5%8F%8C%E7%BC%93%E5%86%B2%E6%8A%80%E6%9C%AF), 在多次render时, 以fiber为载体, 保证复用同一个Hook对象, 进而实现数据持久化

## 副作用Hook
副作用Hook则会修改fiber.flags。所有存在副作用的fiber节点, 都会被添加到父节点的副作用队列后, 最后在commitRoot阶段处理这些副作用节点。另外, 副作用Hook还提供了副作用回调

## 调用function前
1. 处理函数

从fiber树构造的视角来看, 不同的fiber类型, 只需要调用不同的处理函数返回fiber子节点. 所以在[performUnitOfWork->beginWork](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberBeginWork.old.js#L3340-L3354)函数中, 调用了多种处理函数。

2. 全局变量

```javascript
// 渲染优先级
let renderLanes: Lanes = NoLanes;

// 当前正在构造的fiber, 等同于 workInProgress, 为了和当前hook区分, 所以将其改名
let currentlyRenderingFiber: Fiber = (null: any);

// Hooks被存储在fiber.memoizedState 链表上
let currentHook: Hook | null = null; // currentHook = fiber(current).memoizedState

let workInProgressHook: Hook | null = null; // workInProgressHook = fiber(workInProgress).memoizedState

// 在function的执行过程中, 是否再次发起了更新. 只有function被完全执行之后才会重置.
// 当render异常时, 通过该变量可以决定是否清除render过程中的更新.
let didScheduleRenderPhaseUpdate: boolean = false;

// 在本次function的执行过程中, 是否再次发起了更新. 每一次调用function都会被重置
let didScheduleRenderPhaseUpdateDuringThisPass: boolean = false;

// 在本次function的执行过程中, 重新发起更新的最大次数
const RE_RENDER_LIMIT = 25;
```

3. renderWithHooks。调用function后逻辑被分为3个部分：
    1. 调用function前：设置全局变量，标记渲染优先级和当前fiber, 清除当前fiber的遗留状态.
    2. 调用function：<font style="color:rgb(69, 77, 100);">构造出</font><font style="color:rgb(213, 97, 97);background-color:rgb(246, 247, 249);">Hooks</font><font style="color:rgb(69, 77, 100);">链表，</font>生成子级ReactElement对象
    3. 调用function后：重置全局变量,并返回children
        * 为了保证不同的function节点在调用时renderWithHooks互不影响, 所以退出时重置全局变量

## 调用function
+ 在function中, 如果使用了Hook api(如: useEffect, useState), 就会创建一个与之对应的Hook对象；
+ 将hook对象挂载到fiber.memorizedState上，多个hook以链表结构保存
+ fiber树更新阶段, 把current.memoizedState链表上的所有Hook按照顺序克隆到workInProgress.memoizedState上, 实现数据的持久化.

# React合成事件
从v17.0.0开始, React 不会再将事件处理添加到 document 上, 而是将事件处理添加到渲染 React 树的根 DOM 容器中。不过无论是在document上还是根dom容器上监听事件，都可以归为事件委托

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699368814922-89d0d6ff-54c3-4a47-aa6a-0f571a7bd3d7.png)

## 事件绑定
React在启动时会创建全局对象，其中在创建[fiberRoot](https://react-illustration-series.osrc.com/main/bootstrap#create-root-impl)对象时, 调用[createRootImpl](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/client/ReactDOMRoot.js#L120-L169)，其中[listenToAllSupportedEvents](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/events/DOMPluginEventSystem.js#L316-L349)函数会进行事件代理，该函数核心逻辑为：

1. 节流优化, 保证全局注册只被调用一次.
2. 遍历allNativeEvents, 调用listenToNativeEvent监听冒泡和捕获阶段的事件.
    - allNativeEvents包括了大量的原生事件名称, 它是在DOMPluginEventSystem.js中[被初始化](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/events/DOMPluginEventSystem.js#L89-L93)

最后是通过addEventListener监听了原生事件.

### 原生listener
listener是通过createEventListenerWrapperWithPriority函数产生:

+ 根据优先级设置listenerWrappergetEventPriorityForPluginSystem后返回不同的优先级, 最终会有 3 种情况：
    1. DiscreteEvent: 优先级最高, 包括click, keyDown, input等事件,对应的listener是[dispatchDiscreteEvent](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/events/ReactDOMEventListener.js#L121-L142)
    2. UserBlockingEvent: 优先级适中, 包括drag, scroll等事件, 对应的listener是[dispatchUserBlockingUpdate](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/events/ReactDOMEventListener.js#L144-L180)
    3. ContinuousEvent: 优先级最低,包括animation, load等事件, 对应的listener是dispatchEvent

> [dispatchDiscreteEvent](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/events/ReactDOMEventListener.js#L121-L142)，[dispatchUserBlockingUpdate](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/events/ReactDOMEventListener.js#L144-L180)，dispatchEvent其实都是对dispatchEvent的封装
>

对于不同的domEventName调用

+ 返回 listenerWrapper

```javascript
export function createEventListenerWrapperWithPriority(
  targetContainer: EventTarget,
  domEventName: DOMEventName,
  eventSystemFlags: EventSystemFlags,
): Function {
  // 1. 根据优先级设置 listenerWrapper
  const eventPriority = getEventPriorityForPluginSystem(domEventName);
  let listenerWrapper;
  switch (eventPriority) {
    case DiscreteEvent:
      listenerWrapper = dispatchDiscreteEvent;
      break;
    case UserBlockingEvent:
      listenerWrapper = dispatchUserBlockingUpdate;
      break;
    case ContinuousEvent:
    default:
      listenerWrapper = dispatchEvent;
      break;
  }
  // 2. 返回 listenerWrapper
  return listenerWrapper.bind(
    null,
    domEventName,
    eventSystemFlags,
    targetContainer,
  );
}
```

## 事件触发
当原生事件触发之后, 首先会进入到dispatchEvent这个回调函数. 而dispatchEvent函数是react事件体系中最关键的函数, 其调用链路较长, 核心步骤如图所示：

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699369553280-e4568155-914b-4dca-b2ca-1aa05daed5dd.png)

+ [attemptToDispatchEvent](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/events/ReactDOMEventListener.js#L274-L331)：
    - 定位原生 DOM 节点
    - 获取与 DOM 节点对应的 fiber 节点
    - 通过插件系统, 派发事件
+ extractEvents
    - 收集所有listener回调
    - 构造合成事件(SyntheticEvent), 添加到派发队列(dispatchQueue)
+ processDispatchQueue
    - 遍历dispatchListeners数组, 执行[executeDispatch](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/events/DOMPluginEventSystem.js#L222-L231)派发事件, 在fiber节点上绑定的listener函数被执行.
    - 根据捕获(capture)或冒泡(bubble)的不同, 采取了不同的遍历方式:
        1. capture事件: 从上至下调用fiber树中绑定的回调函数, 所以倒序遍历dispatchListeners.
        2. bubble事件: 从下至上调用fiber树中绑定的回调函数, 所以顺序遍历dispatchListeners.

**总结：**

1. 监听原生事件: 对齐DOM元素和fiber元素
2. 收集listeners: 遍历fiber树, 收集所有监听本事件的listener函数.
3. 派发合成事件: 构造合成事件, 遍历listeners进行派发.

