# 怎么理解Vue框架
是一个用于创建用户界面的开源js框架，也是一个创建**单页应用**的web应用框架

核心特性：

+ 数据驱动（MVVM）[MVC、MVP、MVVM、MVPVM区别](https://www.jianshu.com/p/7398ec36d591)

Model-view-viewmodel，与mvp模式的区别是mvvm双向绑定

m：模型层，数据保存，负责处理业务逻辑以及和服务器端进行交互

v：视图层，用户界面，负责将数据模型转化为UI展示出来

vm：视图模型层，v与vm的绑定器，用来连接Model和view，是model和view之间的通信桥梁

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1693907655326-9739b8b8-b3db-4982-a11b-271beb1d7b81.png)

> mvc：当我们用js代码更新model时，view就会自动更新。通信都是单向的
>
> ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1693908201165-8e9edf2f-2649-42fd-9d65-1516c8bd970f.png)
>
> c：业务逻辑——controler，当用户对view有操作时他负责去修改相应 model，当model的值发生变化时他负责去更新对应view
>
> mvp：各部分通信是双向的，v与m不泛生通信，通过p传递，v中不部属任何逻辑
>
> ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1693908647037-7fbad4c9-fb66-45bf-886e-e4719d8f9ac0.png)
>

+ 组件化：
    - <font style="color:#000000;">降低整个系统的耦合度</font>
    - <font style="color:#000000;">调试方便，由于整个系统是通过组件组合起来的，在出现问题的时候，可以用排除法直接移除组件，或者根据报错的组件快速定位问题，之所以能够快速定位，是因为每个组件之间低耦合，职责单一，所以逻辑会比分析整个系统要简单</font>
    - <font style="color:#000000;">提高可维护性，由于每个组件的职责单一，并且组件在系统中是被复用的，所以对代码进行优化可获得系统的整体升级</font>
+ <font style="color:#000000;">指令系统</font>
+ <font style="color:#000000;">简洁，体积小，运行效率高</font>

# 模板编译原理
template=》ast（抽象语法树）=》静态节点标记=》生成render函数代码字符串

vue的编译过程就是将template转化为render函数的过程

1. 将模板字符串转换成elements asts
2. 对ast进行静态节点标记，主要用来做虚拟dom的渲染优化
3. 使用element asts生成render函数代码字符串

> + 将模板template转为ast结构的js对象
> + 用ast得到的js对象拼装render和staticRenderFns函数
> + render和staticRenderFns函数被调用后生成虚拟vnode节点，该节点包含创建dom节点所需信息
> + vm.patch函数通过虚拟dom算法利用vnode节点创建真是dom节点
>

# 单页应用&多页应用
单页应用：只有一个主页面的应用，浏览器在一开始就要加载所有的html，css，js

优点：

+ 用户体验好，内容的改变不需要加载整个页面（**局部刷新**），对服务器压力小
+ 前后端分离
+ 完全的前端组件化，代码组织方式更加规范，便于修改

缺点：url模式为**哈希模式**

+ 首次加载需要大量的静态资源
+ **不利于seo优化**，数据在前端渲染，意味着没有seo
+ 页面导航不可用，需自行实现前进和后退

多页应用：包含多个页面，跳转时全部刷新，**整页刷新**，**history模式**

> 每个页面都是一个主页面，都市独立的，当我们在访问另一个页面的时候，都需要重新加载html，js，css文件，公共文件则根据需求按需加载
>

优点：

+ 多页应用的首屏时间快：当访问网页的时候，服务器返回一个新的html文档，只经过了一个http请求
+ 搜索引擎优化效果好：搜索引擎在做网页排名的时候，要根据网页内容才能给网页权重，来进行网页的排名。搜索引擎是可以识别html内容的，而我们每个页面所有的内容都放在html中，所以这种多页应用，**seo排名效果好**

缺点：

切换慢，每次跳转都需要发出http请求

数据传递需要通过url、cookie、localstorage等传递

## 给单页应用做seo优化
1. 服务端渲染
2. 静态化：
    1. 一种是通过程序将动态页面抓取并保存为静态页面，这样的页面实际存在于服务器的硬盘中
    2. 通过web服务器的url rewrite方式，通过web服务器内部模块按一定的规则而将外部的url请求转化为内部的文件地址
3. 使用phantonjs针对爬虫处理，原理是通过ngnix配置，判断访问来源是否为爬虫，如果是则搜索引擎的爬虫请求会转发到一个node server，再通过phantomjs来解析完整的html，返回给爬虫

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1693983095826-e86be0a3-e17b-4c13-9702-546a119afef6.png)

user-agent:  [User-Agent - HTTP | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/User-Agent)

# SPA首屏加载慢怎么解决
首屏时间（First Contentful Paint），指的是浏览器从响应用户输入网址地址，到首屏内容渲染完成的时 间，此时整个网页不一定要全部渲染完成，但需要展示当前视窗需要的内容  

首屏加载白屏的原因：

+ 网络延迟
+ 资源文件体积是否过大
+ 资源是否重复发送请求去加载
+ 加载脚本的时候，渲染内容堵塞

> FP： 加载html。首次绘制，代表浏览器第一次向屏幕传输像素的事件，也就是页面在屏幕上首次发生视觉变化 的时间  
>
> FCP： 加载静态资源css，js之后解析html，生成html浏览器在第一次像屏幕绘制内容，只有首次绘制文本，图 片，非白色的canvas或svg时才算做fcp  
>
> FMP： ajax请求数据之后，首次有效绘制。表示页面的主要内容发开始出现在屏幕上的时间点，是测量用户 加载体验的主要目标  
>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695277634329-ebdcc659-8142-4732-b422-550b786959e3.png)

```javascript
// 通过DOMContentLoad或者performance来计算出首屏时间
// 方案一：
document.addEventListener('DOMContentLoaded', (event) => {
    console.log('first contentful painting');
});
// 方案二：
performance.getEntriesByName("first-contentful-paint")[0].startTime

// performance.getEntriesByName("first-contentful-paint")[0]
// 会返回一个 PerformancePaintTiming的实例，结构如下：
{
  name: "first-contentful-paint",
  entryType: "paint",
  startTime: 507.80000002123415,
  duration: 0,
};
```

**解决方法：**

+ 减小入口文件体积
    - 路由懒加载 
    - 动态加载路由
+ 静态资源本地缓存

后端返回资源问题：

    - 采用HTTP缓存，设置Cache-Control，Last-Moodified，Etag等响应头
    - 采用Service Worker离线缓存

前端合理利用localStorage

+ UI框架按需加载
+ 图片资源的压缩

对页面上使用到的icon，可以使用在线字体图标，或者雪碧图，用以减轻http请求压力

+ 组件重复打包

在webpack的config文件中，修改CommonsChunkPlugin的配置

> [CommonsChunkPlugin](https://segmentfault.com/a/1190000012828879)	
>
> [CommonsChunkPlugin | webpack 中文文档](https://www.webpackjs.com/plugins/commons-chunk-plugin/)
>
>  [SplitChunksPlugin | webpack 中文文档](https://www.webpackjs.com/plugins/split-chunks-plugin/)
>

```javascript
// minChunks为3表示会把使用3次及以上的包抽离出来，
// 放进公共依赖文件，避免了重复加载组件
minChunks: 3
```

+ 开启Gzip压缩

拆完包之后，我们再用gzip做一下压缩 安装compression-webpack-plugin

```javascript
cnmp i compression-webpack-plugin -D
```

在vue.config.js中引入并修改webpack配置

```javascript
const CompressionPlugin = require('compression-webpack-plugin')

configureWebpack: (config) => {
	if (process.env.NODE_ENV === 'production') {
    // 为生产环境修改配置...
    config.mode = 'production'
    return {
      plugins: [new CompressionPlugin({
        test: /\.js$|\.html$|\.css/, //匹配文件名
        threshold: 10240, //对超过10k的数据进行压缩
        deleteOriginalAssets: false //是否删除原文件
      })]
  }
}

// 使用示例
module.exports = {
  // ...其他配置项
  plugins: [
    new CompressionWebpackPlugin({
      algorithm: 'gzip', // 压缩算法，默认为gzip
      test: /\.(js|css)$/, // 匹配要进行压缩的文件类型
      threshold: 10240, // 资源文件大于10KB时才会进行压缩
      minRatio: 0.8 // 压缩比例达到0.8时才会进行压缩
    })
  ],
  // ...其他配置项
};
```

<font style="color:#000000;">在服务器我们也要做相应的配置 如果发送请求的浏览器支持</font><font style="color:#000000;">gzip</font><font style="color:#000000;">，就发送给它</font><font style="color:#000000;">gzip</font><font style="color:#000000;">格式的文件</font>

+ 使用SSR：vue建议使用Nuxt.js实现服务端渲染
+ loading：添加loading css效果
+ [quicklink](https://github.com/GoogleChromeLabs/quicklink)：在浏览器空闲时的时候去指定需要加载的数据

> quicklink 中使用 fetch API 实现高优先级资源的加载。这是因为浏览器中会为所有的请求都设置一个优先级，高优请求会被优先执行；目前，fetch 在 Chrome 中属于高优先级，在 Safari 中属于中等优先级。
>
> [https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestIdleCallback](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestIdleCallback)
>
> [【性能优化】quicklink：实现原理与给前端的启发](https://www.jianshu.com/p/7dc94efe7e2e)
>
> [https://juejin.cn/post/6844903746476965896](https://juejin.cn/post/6844903746476965896)
>
> [这个库居然能够快速打开页面的链接 - 掘金](https://juejin.cn/post/7173571155843334174?searchId=20230921151029D2C6894B36A5B2A22615)
>

+ 骨架屏：骨架屏就是在进入项目的FP阶段，给它来一个类似轮廓的东西，当我们的页面加载完成之后就消失

# vue项目打包优化
1. 路由懒加载
2. 分析包大小： 终端中运行npm run preview -- --report，这个命令会从我们的入口 main.js 进行依赖分 析，分析出各个包的大小。最终会在生成的 dist 文件夹下生成一个 report.html 的文件，打开后就可 以看到我们在项目使用文件占据的空间大小啦~  
3. webpack配置排除打包：将一些不常用的包，排除在生成的打包文件以外，vue.config.js中添加externals项
4. 引用网络资源：
    1. cdn： 内容分发网络，用它来提高访问速度，将一些静态资源，css，js，图片或者视频放在第三方 的cdn服务器上，可以加快访问速度  

在开发环境时，文件资源还是可以从本地node_modules中取出，而只有项目上线了，才需要去使用外部资源，此时可以使用环境变量来进行区分

5. 打包去除console.log
6. 关闭资源地图productionSourceMap

首先，由于最新版的脚手架不自带配置文件了，先在根目录新建vue.config.js文件，关闭productionSourceMap，在vue.config.js中写入以下内容

```javascript
module.exports = {
	productionSourceMap: false
}
```

7. 开启Gzip压缩：安装插件 compression-webpack-plugin ，打开代码压缩，npm install --save-dev compression-webpack-plugin，现在的vue.config.js代码如下，需要注意服务器上的ngnix也必须gzip才能生效
8. 代码压缩，先安装插件 npm i -D uglifyjs-webpack-plugin ，然后在最上方引入依赖  
9. 图片压缩npm install image-webpack-loader --save-dev
10. 首屏骨架屏优化
11. 首先预先加载模块，vue-cli默认开启prefetch（预先加载模块），提前获取用户未来可能会访问的内容
12. ngnix配置缓存

# 生命周期
## vue2生命周期
+ **beforecreate**：创建前，在实例初始化以后，数据观测data observer和event/watcher事件配置之前被调用。当前阶段data,methods,computed以及watch上的方法和数据都不能被访问
    - **全局配置和插件的初始化：**在 beforeCreate 钩子函数中，你可以进行全局配置的初始化，例如注册全局组件、注册全局指令、配置全局事件总线等
    - **数据初始化：**在 beforeCreate 钩子函数中，你可以进行一些数据的初始化操作，例如设置默认值、对接收的属性进行处理等
    - **非响应式的数据和方法：**在 beforeCreate 钩子函数中，你可以定义一些不需要响应式绑定的数据和方法。这些数据和方法可以在组件的其他生命周期中使用，但不会触发视图更新
    - **访问注入的依赖：**在 beforeCreate 钩子函数中，你可以访问通过依赖注入（Dependency Injection）提供的服务或数据。例如，访问全局的配置信息、访问路由器实例等。
+ **created**：创建完成， 实例已经创建完成后被调用。

在这一步，实例已完成以下配置：

    - 数据观测，
    - 属性和方法的运算，
    - watch/event事件回调。

> 这里没有$el，如果非想要与Dom进行交互，可以通过 vm.$nextTick来访问dom  
>

+ **beforemount**：挂载前，在挂载开始之前被调用：相关的render函数首次被调用，在此阶段可以获取到vm.el
+ 
+ **mounted**：挂载完成，在挂载完成后发生，在当前阶段，真实的dom挂在完毕，数据完成双向绑定，可以访问到dom节点
+ **beforeupdate**：更新前， 数据更新时调用，发生在虚拟dom重新渲染和patch之前，可以在这个钩子 中进一步地更改状态，这不会触发附加的重渲染过程  
+ **update**： 更新完成。当前阶段dom已完成更新，避免自此期间更改数据，因为会导致无限循环的更新，该钩子在服务器端渲染期间不被调用  
+ **beforedestory**： 销毁前。在这一步实例仍然完全可用。我们可以在这是进行善后首位工作，比如清除定时器 
+ **destroyed**：  销毁完成。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务器端渲染期间不被调用  
+ **activated**： 被keep-alive缓存的组件激活时调用  
+ **deactivated**： 被keep-alive缓存的组件停用时调用

## vue3生命周期
完整生命周期钩子函数

+ **onBeforeMount**：挂载之前可以调用
+ **onMount**：挂载后调用
+ **onBeforeUpdate**：当响应数据改变，且重新渲染前调用
+ **onUpdated**：重新渲染后调用
+ **onBeforeUnmount**：Vue实例销毁前调用
+ **onUnmounted**：实例销毁后调用
+ **onActivated**：当keep-alive组件被激活后调用
+ **onDeactivated**：当keep-alive组件取消激活时调用
+ **onErrorCaptured**：从子组件中捕获错误时调用

## 数据请求在created和mounted的区别
created是在组件实例一旦创建完成时的后台调用，这时候页面的dom节点并未生成，mounted是在页面dom节点渲染完毕之后就立刻执行的触发时机上，cteated是比mounted发生时间更早的

相同点：都能拿到实例对象的属性和方法

放在mounted请求有可能导致页面闪动（页面dom结构已经生成），但如果在页面加载前完成就不会出现此情况，所以建议放在create生命周期当中

异步请求在created，beforeMount，mounted中进行异步请求，这三个钩子函数中data已经创建，可以将服务端返回的数据进行赋值

 如果异步请求不需要依赖dom，推荐在created钩子函数中调用异步请求，因为在create钩子函数中调用异 步请求有以下优点：

+ 能更快的获取到服务端数据，减少页面loading事件
+ ssr服务端渲染不支持beforeMount 、mounted钩子函数，所以放在created中有助于一致性；  

# computed&watch&methods
computed： 当页面中某些数据依赖其他数据进行变动时可以使用该属性  

+  支持缓存，计算完成的值在相关值不发生改变的时候可重复复用。比如刷新的时候会重新计算，使用 method会重新进行计算  
+  惰性求值：即使相关值已经改变，它也只会在调用时求值  
+  不支持异步，但是可以使用 [vue-async-computed](https://github.com/foxbenjaminfox/vue-async-computed)插件将promise的值绑定到组件属性来创建和使用 组件中的异步计算属性  

 标准计算属性与异步属性的区别：  

    -  异步属性不能有setter  
    -  直到promise的resolve为止，除非default被设置，否则该值位null  
+ 适用于一个数据受多个数据影响的场景
+ 默认只有getter，可以自定义设定setter

watch： 用于观察和监听页面上的vue实例，适用于数据变化的同时需要进行异步操作的场景  

+  不支持缓存，数据变化时立即触发回调函数中的操作  
+ 不支持异步
+  使用于一个数据影响多个数据的场景  
+  监听数据必须是data中声明过的或prop中的数据，其处理函数有两个参数immediate（组件加载立即 触发回调函数执行）与deep（深度监听） 

# v-for&v-if
v-for优先级比v-if高，最好不要一起使用

> 同时使用时，每次渲染都会先循环再进行条件判断
>

可以使用计算属性解决或在外层嵌套template，在这一层进行v-if判断，然后在内部进行v-for判断

# v-if&v-show
> [面试官：v-show和v-if有什么区别？使用场景分别是什么？](https://zhuanlan.zhihu.com/p/513061819)
>

v-if是条件渲染，确保在切换过程中条件块内的事件监听器和子组件适当的被销毁和重建

v-if是惰性的，如果在初始渲染条件为假，则什么也不做，直到条件第一次变为真时，才会开始渲染条件块

v-show不管条件是什么，元素总是被渲染，并且是简单的基于css进行切换

v-if有更高的切换开销，而v-show有更高的初始渲染开销，因此，如果需要**非常频繁的切换，则使用v-show比较好**，如果在运行时条件很少改变，则使用v-if较好

v-show由false转为true时不会触发组件的生命周期

v-if由false转为true的时候，触发组件的beforeCreate，create，beforeMount，mounted钩子，由true变为false的时候触发组件的beforeDestroy，detroyed方法

# Vue.observable（value,）
> [API — Vue.js](https://v2.cn.vuejs.org/v2/api/#Vue-observable)
>

用法：让一个对象可响应，vue内部会用它来处理data函数返回的对象，返回的对象可以直接用于渲染函数和计算属性内，并且会在发生变更时触发相应的更新。也可以作为最小化的跨组件状态存储器

<font style="background-color:rgb(248, 248, 248);">在 Vue 2.x 中，被传入的对象会直接被 </font><font style="background-color:rgb(239, 239, 239);">Vue.observable</font><font style="background-color:rgb(248, 248, 248);"> 变更，所以如</font>这里展示的<font style="background-color:rgb(248, 248, 248);">，它和被返回的对象是同一个对象。在 Vue 3.x 中，则会返回一个可响应的代理，而对源对象直接进行变更仍然是不可响应的。因此，为了向前兼容，我们推荐始终操作使用 </font><font style="background-color:rgb(239, 239, 239);">Vue.observable</font><font style="background-color:rgb(248, 248, 248);"> 返回的对象，而不是传入源对象。</font>

**Vue.observable会调用observe方法，该方法会对传入的value进行判断，****如果是非对象和虚拟dom 则不做响应式的处理；如果value已经做过了响应式处理，则直接返回observer实例，没有做过响应式的处理话就创建一个新的Observer实例，初始化传入我们需要做响应式的对象**

# mixin
> [面试官：说说你对vue的mixin的理解，有什么应用场景？ | web前端面试 - 面试官系列](https://vue3js.cn/interview/vue/mixin.html#%E4%BA%8C%E3%80%81%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF)
>

混入，来分发Vue组件中的可复用功能

本质就是一个js对象，它可以包含我们组件中任意功能选项如data、components、methods、created、computed<font style="color:rgb(44, 62, 80);">等等</font>

我们只要将共用的功能以对象的方式传入 mixins选项中，当组件使用 mixins对象时所有mixins对象的选项都将被混入该组件本身的选项中来

在Vue中我们可以**局部混入**跟**全局混入**

## 局部混入
```javascript
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}
Vue.component('componentA',{
  mixins: [myMixin]
})
```

## 全局混入
会影响到每一个组件实例

```javascript
Vue.mixin({
  created: function () {
      console.log("全局混入")
    }
})
```

## vue的几种类型的合并策略
+ 替换型：props，methods，inject，computed，会被后来者代替
+ <font style="color:#000000;">合并型：data，遍历了要合并的data的所有属性，然后根据不同情况进行合并</font>
    - <font style="color:#000000;">当目标 data 对象不包含当前属性时，调用 </font><font style="color:#000000;">set</font><font style="color:#000000;"> 方法进行合并（set方法其实就是一些合并重新赋值的方法）</font>
    - <font style="color:#000000;">当目标 data 对象包含当前属性并且当前值为纯对象时，递归合并当前对象值，这样做是为了防止对象存在新增属性</font>
+ <font style="color:#000000;">队列型：全部生命周期和</font><font style="color:#000000;">watch，</font><font style="color:#000000;">生命周期钩子和</font><font style="color:#000000;">watch</font><font style="color:#000000;">被合并为一个数组，然后正序遍历一次执行</font>
+ 叠加型：component、directives、filters，<font style="color:rgb(44, 62, 80);">通过原型链进行层层的叠加</font>

# keep-alive
keep-alive是vue中的内置组件，能在组件切换过程中将状态保留在内存中，防止重复渲染

keep-alive包裹动态组件时，会缓存不活动的组件实例，而不是销毁他们

可以设置的props属性：

+ include：字符串或正则表达式。只有名称匹配的组件会被缓存
+ exclude：字符串或正则表达式。任何名称匹配的组件都不会被缓存
+ max：最多可以缓存多少组件实例

<font style="color:rgb(44, 62, 80);">设置了 keep-alive 缓存的组件，会多出两个生命周期钩子activated和deactivated</font>

+ <font style="color:rgb(44, 62, 80);">首次进入组件时，</font>beforeRouteEnter > <font style="color:#DF2A3F;">beforeCreate</font><font style="color:#DF2A3F;"> > </font><font style="color:#DF2A3F;">created</font><font style="color:#DF2A3F;">> </font><font style="color:#DF2A3F;">mounted</font> > activated > ... ... > beforeRouteLeave > deactivated
+ 再次进入组件时，beforeRouteEnter >activated > ... ... > beforeRouteLeave > deactivated

## 缓存后如何获取数据
+ beforeRouteEnter
+ activated

## 原理
keep-alive组件中没有template，而是用了render，在该组件渲染的时候会自动执行render函数，

+ 在created生命周期函数中会创建一个this.cache是一个对象，用来存储需要缓存的组件，this.keys用来存储，
+ mounted挂载时观测 include 和 exclude 的变化，如果include 或exclude 发生了变化，即表示定义需要缓存的组件的规则或者不需要缓存的组件的规则发生了变化，那么就执行pruneCache函数，该函数内对this.cache对象进行遍历，取出每一项的name值，用其与新的缓存规则进行匹配，如果匹配不上，则表示在新的缓存规则下该组件已经不需要被缓存，则调用pruneCacheEntry函数将其从this.cache对象剔除即可，
+ 然后我们进入render阶段，首先我们获取组件的key值，拿到key值后去this.cache对象中去寻找是否有该值，如果有则表示该组件有缓存，即命中缓存，直接从缓存中拿 vnode 的组件实例，调整该组件key的顺序，将其从原来的地方删掉并重新放在最后一个，如果没有命中缓存，则将其设置进缓存，如果配置了max并且缓存的长度超过了this.max，则从缓存中删除第一个
+ 在组建销毁的时候执行pruneCacheEntry函数

# vue实例挂载的过程
+ new Vue的时候会调用_init方法
    - 定义$set、$get、$delete、$watch等方法
    - 定义$on、$off、$emit、$off等事件
    - 定义_update、$forceUpdate、$destroy生命周期
+ 调用$mount进行页面的挂载
+ 挂载的时候主要是通过mountComponent方法

**mountComponent方法：**

+ **如果没有获取到解析的render函数，则会抛出警告，render函数存在的话执行beforeMount钩子**
+ **定义updateComponent更新函数**
    - **执行render生成虚拟dom**
    - **_update将虚拟dom生成真实DOM结构，并且渲染到界面中**

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1694073735529-4b718841-e09f-432d-a254-225f68fd8969.png)

# 为什么在html中监听事件
使用v-on的几个好处：

+ 能在html模板轻松定位在就是代码里对应的方法
+ 无需在js中手动绑定事件，viewmodel代码是非常纯粹的逻辑，和dom完全解耦，更易于测试
+ 当一个viewmodel被销毁时，所有的事件处理器都会被自动删除，无需担心如何清理

# 为什么data是一个函数而不是一个对象
组件中的data写成一个函数，数据以函数返回值形式定义，这样每复用一次组件，就返回一份新的data，类似于给每个组件实例创建了一个私有的数据空间，让每个组件实例维护各自的数据，单纯写成对象形式，使得所有组件实例共用了一份data，会造成一个变了全都变了的结果

> 根实例对象data可以是对象也可以是函数（根实例是单例），不会产生数据污染情况
>
> ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695282221001-14f7994e-b41f-45ed-ae1d-57d85b4560ba.png)
>

# 响应式原理
1. 通过数据的改变去驱动dom视图的变化，使用了数据劫持+观察者模式
2. 数组和对象值变化时如何劫持到？
    1. 对象内部通过defineReactive方法，使用object.defineProperty将属性进行劫持（只会劫持已经存在的属性）。
        * 在get方法里进行依赖收集dep.depend()，每个属性都拥有自己的dep属性，存放其依赖的watcher（依赖收集）。 将自身watcher观察者实例设置给Dep.target,用以依赖收集也就是收集watcher
        * 在set方法里面进行派发更新dep.notify()，通知所有与属性相关的watch进行刷新视图
        *  多层对象通过递归进行劫持，所以当对象层级过深时，性能变差  

>  譬如说现在的的data中可能有a、b、c三个数据，getter渲染需要依赖a跟c，那么在执行getter的时候就会触发 a跟c两个数据的getter函数，在getter函数中即可判断Dep.target是否存在然后完成依赖收集，将该观察者对 象放入闭包中的Dep的subs中去。  
>

    2. 数组则是通过重写数组方法（pop,push,unshift,shift,splice,sort,reverse）进行实现，当页面使用对应属性时，每个属性都拥有自己的dep属性，存放其依赖的watcher（依赖收集），当属性变化后会通知自己对应的watcher去更新

> [深入浅出Vue响应式原理（完整版） - 掘金](https://juejin.cn/post/6844903882208837640#heading-3)
>

>  重写数组编译方法采用切片思想，先将push，unshift，splice新增的对象进行响应式处理，利用已经被响应 式处理的数据_ob_属性(observer实例对象)然后再调用那个数组原型的方法完成数组的改变  
>

```javascript
/*
 * not type checking this file because flow doesn't play well with
 * dynamically accessing methods on Array prototype
 */
import { TriggerOpTypes } from '../../v3'
import { def } from '../util/index'
const arrayProto = Array.prototype
export const arrayMethods = Object.create(arrayProto)
const methodsToPatch = ['push','pop','shift','unshift','splice','sort','reverse']
/**
 * Intercept mutating methods and emit events
 */
// NOTE: 对改变原数组的方法进行遍历
methodsToPatch.forEach(function (method) {
  // NOTE: 首先获取到数组原型上的这些方法
  // cache original method
  const original = arrayProto[method]
  // NOTE：将当前的对象重新定义它的属性
  def(arrayMethods, method, function mutator(...args) {
    // NOTE：执行原型上的该方法
    const result = original.apply(this, args)
    // NOTE：获取到当前的observer实例对象
    const ob = this.__ob__
    let inserted
    switch (method) {
      case 'push':
      case 'unshift':
        // NOTE: 获取我们要插入的元素
        inserted = args
        break
      case 'splice':
        // NOTE：获取新增元素
        inserted = args.slice(2)
        break
    }
    // NOTE：如果当前的方法为push unshift或splice则对新加入的元素执行响应式处理
    if (inserted) ob.observeArray(inserted)
    // notify change
    // NOTE：让内部的dep去通知更新
    if (__DEV__) {
      ob.dep.notify({
        type: TriggerOpTypes.ARRAY_MUTATION,
        target: this,
        key: method
      })
    } else {
      ob.dep.notify()
    }
    return result
  })
})

```

3. vue不会触发视图更新的情况：
    - 在实例创建之后新的属性到实例上
    - 直接更改数组下标来修改数组的值
4. 当给对象新增不存在的属性时，首先会把新的属性进行响应式跟踪，然后会触发对象__ob__的dep收集到的watcher去更新，当修改数组索引时我们调用数组本身的splice方法去更新数组
5. 为什么不使用Object.defineProperty对数组实现监听？
+ Object.defineProperty无法劫持数组长度length属性得到变化，而数组length属性会影响数组的变动。Object.defineProperty只能劫持已有属性,要监听数组变化,必须预设数组长度,遍历劫持,但数组长度在实际引用中是不可预料的。数组删除新增会导致索引key发生变动,每次变动都需要重新遍历,添加劫持,数据量大时非常影响性能。
+ 因为性能问题，性能代价和获得的用户体验收益不成正比。对于对象而言，每一次的数据变更都会对对象的数据进行一次枚举，一般对象本身的属性数量有限，所以对于遍历枚举等方式产生的性能损耗可以忽略不计，但是对数组而言，数组包含的元素量是可能达到成千上万，假设对于每一次数组元素的更新都触发了枚举、遍历。其带来的性能损耗将与获得的用户体验不成正比，所以vue无法检测数组的变动 
6. this.$set(要更改的数据源，要更改的具体数据，重新赋的值)——向响应式对象中添加一个属性，并确保这个新属性同样是响应式的，且触发视图更新，它必须用于向响应式对象上添加新属性。

```javascript
// 发布者具有一个订阅者列表，当该发布者被引用时则将引用的对象
// 放置订阅者列表中，每当该发布者改变时就会通知订阅者更新视图
class Dep {
  constructor(){
    // 用户存放订阅者对象的数组
    this.subs = [];
  }
  // 在订阅者数组中添加一个订阅对象
  addSub(sub){
    this.subs.push(sub);
  }
  // 通知所有订阅对象更新视图
  notify(){
    this.subs.forEach(sub => {  
      sub.update();
    })
  }
}
// 订阅者(观察者)，在引用发布对象时会被收集进入发布对象的订阅者列表中
class Watcher{
  constructor(){
    // 在new一个订阅者对象时将该对象赋值给Dep.target,在get中会用到
    Dep.target = this;
  }
  // 更新视图的方法
  update(){
    console.log('视图更新了')；
  }
}
Dep.target = null;
// 依赖收集
function defineReactive(obj,key,val){
  //new一个发布者对象
  const dep = new Dep();
  Object.defineProperty(obj, key, {
    enumerable:true,  // 可枚举属性
    // 当且仅当该属性的 configurable 键值为 true 时，
    // 该属性的描述符才能够被改变，同时该属性也能从对应的对象上被删除。
    configurable: true,  
    // 数据被读取时触发get内逻辑
    get: function reactiveGetter(){
      // 将Dep.target(当前的订阅对象存入发布者的订阅者列表中)
      dep.addSub(Dep.target);
      return val;
    };
    // 数据被操作时触发set内的逻辑
    set: function reactiveSetter(newVal){
    	if (newVal === val) return;
    /* 在set时触发发布者的通知逻辑来通知所有的订阅者对象更新视图 */
    	dep.notify();
    }
  })
}
class Vue {
  constructor(options){
    this._data = options.data;
    // 此处完成数据的响应式处理
    observer(this._data);
    // 新建一个订阅者对象，这时候Dep.target会指向这个订阅者对象
    new Watcher();
    // 在这里模拟数据渲染的过程，其会触发 test 属性的 get 逻辑
  	console.log('render~', this._data.test);
  }
}
```

# 双向绑定
除了数据驱动之外，dom的变化反过来影响数据，是一个双向的关系 

双向绑定流程：

+ new Vue()初始化，对data执行响应化处理，在observer函数中劫持监听所有属性
+ 同时对模板进行编译，找到其中动态绑定的数据，从data中获取并初始化视图，这个过程发生在compile中
+ 同时定义一个更新函数和watcher，将来对应数据变化时watcher会调用更新函数
+ 由于data的某个key在一个视图中可能出现多次，所以每个key都需要一个管家Dep来管理多个watcher
+ 将来data中数据一旦发生变化，会首先找到对应的dep，通知所有watcher执行更新函数

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1694155076563-7f3ed7da-503e-4318-ab02-823bc90e2cb9.png)

利用proxy或者object.defineProperty生成的observer劫持监听所有的属性，在属性变化后通知dep订阅者，同时compile解析指令，收集指令所以来的数据和方法，等待数据变化然后进行渲染，将来data中数据一旦发生变化，会首先找到对应的dep，触发setter通知之前所有watcher执行更新函数，告知视图更新，重新渲染页面

# vue中给对象添加新属性页面不刷新
vue不允许在已经创建的实例上动态添加新的响应式属性

+ vue.set（target, propertyName/index, value）通过vue.set向响应式对象中添加一个property，并确保这个新property同样是响应式的，且触发视图更新   **场景：为对象添加少量的新属性**
+ Obejct.assign（）直接使用object.assign()添加到对象的新属性不会触发更新，应该创建一个新的对象，合并原对象和混入对象的属性

**场景：为新对象添加大量的新属性**

```javascript
this.someObject = 
  Object.assign(
    {},
    this.someObject,
    {
      newProperty1: 1,
      newProperty2: 2,
      ...
    }
  )

```

+ $forceUpdate（）如果你发现自己需要在Vue中做一次强制更新，99.9%的情况，是你在某个地方做错了事

**场景：实在不知道怎么操作时，采取该方法强制进行刷新**

> forceUpdate：强制该组件重新渲染
>

**vue3中是用proxy实现数据响应的，直接动态添加新属性仍可以实现数据响应式**

## vue.set()和this.$set()的区别
vue.set(): <font style="color:rgb(48, 68, 85);">对于已经创建的实例，Vue 不允许动态添加根级别的响应式 property。但是，可以使用 </font><font style="color:rgb(214, 50, 0);background-color:rgb(248, 248, 248);">Vue.set(object, propertyName, value)</font><font style="color:rgb(48, 68, 85);"> 方法向嵌套对象添加响应式 property</font>

<font style="color:rgb(48, 68, 85);">this.$set：</font>

Vue.set()和this.set()这两个API的实现原理基本一模一样，都是使用了set函数，set函数是从../observer/index文件中导出的，区别在于Vue.set（）是将set函数绑定在Vue的构造函数上，this.set()是放在vue原型上的

> this.$set(要更改的数据源，要更改的具体数据，重新赋的值)——向响应式对象中添加一个属性，并确保这个新属性同样是响应式的，且触发师徒更新，他必须用于响应式对象上添加新属性
>

vue.set的源码

```javascript
import { set } from '../observer/index'

...
Vue.set = set
...
```

this.$set的源码

```javascript
import { set } from '../observer/index'

...
Vue.prototype.$set = set
...
```

# vue nextTick()—本质是一种优化策略
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1694164344497-217b63e4-c8b6-4be1-b43b-d38ea9543634.png)

Vue在修改数据后，视图不会立刻更新，而是等同一事件循环中的所有数据变化完成之后，再同一进行视图更新。$nextTick会返回一个Promise对象，可以是用async/await完成相同作用的事情

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695287388320-30818eff-4342-4861-b906-6123cfca1a0a.png)

在修改数据之后立即使用这个方法，获取更新后的DOM

```javascript
// Vue.nextTick(回调函数， 执行函数上下文)
// 示例
// 修改数据
vm.message = '修改后的值'
// DOM 还没有更新
console.log(vm.$el.textContent) // 原始的值
Vue.nextTick(function () {
  // DOM 更新了
  console.log(vm.$el.textContent) // 修改后的值
})
```

实现原理：

vue是异步执行dom更新的，一旦观察到数据变化，vue就会开启一个队列，将同一事件循环中观察到数据变化的watcher推送进这个队列，如果这个watcher被触发多次，只会被推送到队列一次，这种缓冲行为可以有效地去掉重复数据造成的不必要的计算和Dom操作，而在下一个事件循环时，vue会清空队列，并进行必要的dom更新

+ 将回调函数放入callbacks等待执行
+ 将执行函数放到微任务或者宏任务中
+ 事件循环到了微任务或者宏任务，执行函数依次执行callbacks中的回调

使用场景：

+ 在created钩子函数进行的dom操作一定要放在vue.nextTick()的回调函数中

原因是在created()钩子函数执行的时候DOM其实并未进行任何渲染，而此时进行DOM操作无异于徒劳，所以此处一定要将DOM操作的js代码放进Vue.nextTick()的回调函数中。与之对应的就是mounted钩子函数，因为该钩子函数执行时所有的DOM挂载已完成

+ 点击按钮显示原本的v-show=false隐藏起来的输入框，并获取焦点。

# 导航故障
NavigationFailureType可以帮助开发者来区分不同类型的导航故障

+ redirected：在导航守卫中调用了next（newLocation）重定向到其他地方
+ aborted：在导航守卫中调用了next（false）终端本次导航
+ cancelled：在当前导航还没有完成之前就有了一个新的导航
+ duplicated：<font style="color:rgb(60, 60, 67);">导航被阻止，因为我们已经在目标位置了</font>

<font style="color:rgb(60, 60, 67);">导航故障的属性：所有的导航属性都会有to和from属性，分别表示这次失败的导航的目标位置和当前位置</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697894013521-c3921d70-e58a-4c9b-a4c0-47cd24c56d00.png)

# 导航模式——hash history
1. 当选择mode类型后，程序会根据选择的mode类型创建不同的history对象

hash——hashhistory，history——HTML5history

2. **hash模式（带#）**

#后面hash的值的变化不会向服务器发起请求且请求时值会#发送之前的url，每次hash值发生变化时会触发hashchange事件，可以监听hashchange（window.hashchange）实现页面部分内容操作

**核心通过监听url中hash来进行路由跳转**

3. **history模式**：核心使用html5 history api——提供了对浏览器的会话历史的访问

修改历史状态

    - pushState(一个状态对象，一个标题，一个URL(可选))
    - replaceState()
    - popState()：改变url地址且不会发送请求，可以读取历史记录栈，还可以对浏览器历史记录栈进行修改

切换历史状态

    - back()——后退
    - forward()——前进
    - go()——跳转  

# vue项目本地开发完成后部署到服务器后报404是什么原因
404意味着资源不存在，只有在history模式下会出现该问题

vue属于单页应用，而单页应用是一种网络程序或网站的模型，所有用户交互是通过动态重写当前页面，不管应用有多少页面，构建物都只会产出一个index.html

**为什么hash模式没有该问题？**

hash模式是用符号#表示的，hash值变化时不会重新加载页面，所以不会有该问题

**解决方案：**

出现该问题是由于前端访问的路径后端没有进行配置，我们可以通过nginx进行配置，我们只需要将任意页面都重定向到index.html，把路由交由前端处理即可

对nginx配置文件.conf修改，添加try_fils $uri $uri/ /index.html

```javascript
location / {
	try files $uri $uri/ /index.html
}
```

>  以 try_files $uri $uri/ /index.php; 为例，
>
> 当用户请求http://servers.blog.ustc.edu.cn/example时，这里的$uri就是/example。try_files会到硬盘里尝试找这个文件。如果存在名为/$root/example（其中$root是WordPress的安装目录）的文件，就直接把这个文件的内容发送给用户。显然，目录中没有叫example的文件。 然后就看$uri/，增加了一个/，也就是看有没有名为 /$root/example/的目录。又找不到， 就会fall back到try_files的最后一个选项/index.php，发起一个内部“子请求”，也就是相当于nginx发起一个HTTP请求到http://servers.blog.ustc.edu.cn/index.php。location / { try files $uri $uri/ /index.html } 这个请求会被 location ~ \.php$ { ... } catch住，也就是进入FastCGI的处理程序。而具体的URI及参数是在REQUEST_URI中传递给FastCGI和WordPress程序的，因此不受URI变化的影响。 配置修改之后，/index.php就不在fall back的位置了，也就是说try_files在文件系统里找到index.php之后，就会直接把文件内容（也就是PHP源码）返回给用户。这里的坑就是try_files的最后一个位置（fall back）是特殊的，它会发出一个内部 “子请求” 而非直接在文件系统里查找这个文件。  
>

# $router与$route的区别
1. $router为VueRouter实例，想要导航到不同url，则使用$router.push方法，返回上一个history也是使用$router.go()方法
2. $route从当前router跳转对象里面可以获取name，path，params等（传的参数由this.$route.query或者 this.$route.params接收）

# Vue组件通信
## props和$emit
父组件向子组件传递数据是通过prop传递的

```javascript
// 父组件
<Children name="jack" age=18 />  
// 子组件
export default {
  props: {
    name: String;
    age: {
      type: Number,
      default: 18,
      require: true,  
    }
  }
}
```

子组件数据传递给父组件是通过$emit触发事件来做到的

> 子组件虽然不能直接对父组件prop进行重新赋值，但父组件是引用类型的时候，子组件可以修改父组件的props下面的属性
>

```javascript
// 父组件
<Children @add="cartAdd($event)" />  
// 子组件
this.$emit('add', good)
```

## $parent和$children
获取当前的父组件与当前的子组件，更加简单的获取Vue实例，兄弟组件深层次嵌套组件通信困难

## $attrs和$listeners
attrs包含父作用域里除 class 和 style 除外的非 props 属性集合，可以通过this.$attrs获取，传递给子组件内部的其他组件可以通过v-bind="$attrs"

listeners包含父作用域里.native除外的监听事件集合，如果还要继续给子组件内部的其他组件，就可以通过v-on="$listeners"

```javascript
// 父组件
<template>
    <child :name="name" title="1111" ></child>
</template>
export default{
    data(){
        return {
            name:"沐华"
        }
    }
}
// 子组件
<template>
    // 继续传给孙子组件
    <sun-child v-bind="$attrs"></sun-child>
</template>
export default{
    props:["name"], // 这里可以接收，也可以不接收
    mounted(){
        // 如果props接收了name 就是 { title:1111 }，否则就是{ name:"沐华", title:1111 }
        console.log(this.$attrs)
    }
}
```

## 依赖注入 provide，inject
父组件中通过provide来提供变量，然后在子组件中通过inject来注入变量（官方不推荐在实际业务中使用，但是写组件库时常用）

```javascript
// 父组件
export default{
    // 方法一 不能获取 this.xxx，只能传写死的
    provide:{
        name:"沐华",
    },
    // 方法二 可以获取 this.xxx
    provide(){
        return {
            name:"沐华",
            msg: this.msg // data 中的属性
            someMethod:this.someMethod // methods 中的方法
        }
    },
    methods:{
        someMethod(){
            console.log("这是注入的方法")
        }
    }
}

// 后代组件
export default{
    inject:["name","msg","someMethod"],
    mounted(){
        // 这里拿到的属性不是响应式的，如果想拿到最新的，可以在下面的方法中返回
        console.log(this.msg)
        this.someMethod()
    }
}
```

## $ref
获取组件实例

## Vuex
vuex状态管理，vuex通信方式比其他方式，比较复杂，而且如果不同的模块，需要建立独立的modules

## $eventBus
兄弟组件传递数据，事件总线

#  事件总线
使用步骤：

1. Vue.prototype.bus = new Vue()
2. this.bus.$on(注册事件名，回调函数)
3. this.bus.$emit(触发事件，传入参数)

```javascript
class MyVue{
  constructor(){
    // 用于存储注册的事件
    this.eventCollection = {};
  }
  // 注册事件
  on(event, fn){
    // 可以多个地方都注册同一个事件，届时需要挨个通知
    // 无则创建事件集合（数组）
    if(!this.eventCollection[event]){
      this.eventCollection[event] = [];
    }
    this.eventCollection[event].push(fn);
  }
  // 如果注册过这个事件，则遍历执行回调
  emit(event, ...params){
    if(this.eventCollection[event]){
      this.eventCollection[event].forEach(fn => {
        fn(...params)
      })
    }
  }
  // 删除指定事件的回调函数
  off(event, fn){
    var callbacks = this.eventCollection[event];
    if(callbacks){
      var index = callbacks.indexOf(fn);
      if(index !== 1){
        callbacks.splice(index, 1);
      }
    }
  }
}
```

# vue内置指令
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1694583016167-05d10bb4-9534-41e6-a0aa-328ac5cb91d7.png)

# vue自定义指令
> [Vue 自定义指令 - xiaoxustudy - 博客园](https://www.cnblogs.com/xiaoxuStudy/p/13208406.html)
>

自定义指令的创建

```javascript
Vue.directive('focus',{
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
```

自定义指令对象可以提供的几个钩子函数，均为可选的：

+ bind：<font style="color:#000000;">只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。</font>
+ <font style="color:#000000;">inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档</font><font style="color:#000000;background-color:#262626;"></font><font style="color:#000000;">中)。</font>
+ <font style="color:#000000;">update：所在组件的 VNode 更新时调用，</font>**<font style="color:#000000;">但是可能发生在其子 VNode 更新之前</font>**<font style="color:#000000;">。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新。</font>
+ <font style="color:#000000;">componentUpdated：指令所在组件的 VNode </font>**<font style="color:#000000;">及其子 VNode</font>**<font style="color:#000000;"> 全部更新后调用。</font>
+ <font style="color:#000000;">unbind：只调用一次，指令与元素解绑时调用。</font>

<font style="color:#000000;">钩子函数参数：</font>

+ <font style="color:#000000;">el：</font><font style="color:rgb(48, 68, 85);">指令所绑定的元素，可以用来直接操作 DOM。</font>
+ <font style="color:#000000;">binding：一个对象，包含以下 property</font>
    - <font style="color:#000000;">name：指令名，不包括 </font><font style="color:#000000;background-color:rgb(248, 248, 248);">v-</font><font style="color:#000000;"> 前缀。</font>
    - <font style="color:#000000;">value：指令的绑定值</font>
    - <font style="color:#000000;">oldValue：指令绑定的前一个值，仅在 </font><font style="color:#000000;background-color:rgb(248, 248, 248);">update</font><font style="color:#000000;"> 和 </font><font style="color:#000000;background-color:rgb(248, 248, 248);">componentUpdated</font><font style="color:#000000;"> 钩子中可用。无论值是否改变都可用。</font>
    - <font style="color:#000000;">expression：字符串形式的指令表达式</font>
    - <font style="color:#000000;">arg：传给指令的参数，可选。</font>
    - <font style="color:#000000;">modifiers：一个包含修饰符的对象。</font>
+ <font style="color:#000000;">vnode：Vue 编译生成的虚拟节点</font>
+ <font style="color:#000000;">oldVnode：上一个虚拟节点，仅在 </font><font style="color:#000000;background-color:rgb(248, 248, 248);">update</font><font style="color:#000000;"> 和 </font><font style="color:#000000;background-color:rgb(248, 248, 248);">componentUpdated</font><font style="color:#000000;"> 钩子中可用。</font>

<font style="color:#000000;">原理：</font>

+ <font style="color:#000000;">生成ast时，遇到指令会给当前元素添加directives属性</font>
+ <font style="color:#000000;">通过genDirectives生成指令代码</font>
+ <font style="color:#000000;">在patch前将指令的钩子提取到cbs中，在patch过程中调用对应的钩子</font>
+ <font style="color:#000000;">当执行指令对应钩子函数时，调用对应指令定义的方法</font>

<font style="color:#000000;">使用场景：</font>

+ 操作dom：修改样式，绑定事件
+ 封装插件：集成第三方插件，提供更便捷的调用方式
+ 表单验证：自定义验证规则，便于表单验证代码的重用
+ 优化性能：自定义指令实现图片懒加载
+ 权限控制：控制某些显示或隐藏

# vue单向数据流
数据总是从父组件传到子组件，子组件没有权利修改父组件传过来的数据，只能请求父组件对原始数据进行修改，这样会防止从子组件改变父组件的状态，从而导致应用的数据流向难以理解，如果实在要改变父组件的prop值可以在data里面定义一个变量并用prop的值初始化它，之后用$emit通知父组件

# vue修饰符
修饰符适用于限定类型以及类型成员的声明的一种符号

## 表单修饰符
+ lazy：在我们填完信息，光标离开标签的时候，才会将值赋予给value，也就是在change事件之后再进行信息同步

```javascript
<input type="text" v-model.lazy="value">
<p>{{value}}</p>
```

+ trim：<font style="color:rgb(44, 62, 80);">自动过滤用户输入的首空格字符，而中间的空格不会过滤</font>

```javascript
<input type="text" v-model.trim="value">
```

+ number：自动将用户的输入值转为数值类型，但如果这个值无法被parseFloat解析，则会返回原来的值

```javascript
<input v-model.number="age" type="number">
```

## 事件修饰符
+ stop：阻止了事件冒泡，相当于调用了event.stopPropagation方法
+ prevent：阻止了事件的默认行为，相当于调用了event.preventDefault方法
+ self：只当在 event.target 是当前元素自身时触发处理函数

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695348795074-81534871-95e1-4493-99f1-883c3a11af6a.png)

+ once：绑定了事件以后只能触发一次，第二次就不会触发
+ capture：使事件触发从包含这个元素的顶层开始往下触发
+ passive：移动端监听元素滚动事件的时候，会一直触发onscroll事件会让页面变卡，因此使用这个修饰符的时候，相当于给onscroll事件整了一个.lazy修饰符![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695348997840-d68cc92d-afa2-488e-83f0-ed18ac739495.png)
+ native：让组件变成像html内置标签那样监听根元素的原生事件，否则组件上使用 v-on 只会监听自定义事件.

使用.native修饰符来操作普通HTML标签是会令事件生效的

```javascript
<my-component v-on:click.native="doSomething"></my-component>
```

## 鼠标按键修饰符
+ left左键点击
+ right右键点击
+ middle中键点击

## 键盘修饰符
键盘修饰符是用来修饰键盘事件的keycode存在很多，但vue为我们提供了别名，分为两种

+ 普通键（enter，tab，delete，space，esc，up）
+ 系统修饰键（ctrl，alt，meta，shift）

## v-bind修饰符
+ .async：能对props进行一个双向绑定
+ .prop
+ .camel



# vuex是怎么实现的
vuex利用了vue的mixin机制，混合beforeCreate钩子将store注入至vue组件实例上，并注册了vuex store的引用属性$store!

# vuex中的数据在页面刷新后数据消失
> [vuex页面刷新数据丢失问题的多种解决方法 - 掘金](https://juejin.cn/post/7061858778756415519?searchId=20230913135912273CE4DC9E116E1E628E)
>

因为js的数据都是保存在浏览器的堆栈内存里面的，刷新浏览器页面，以前堆栈申请的内存被释放

解决方法：

+ 用sessionStorage或者localStorage存储数据
+ 引入插件vuex-persist

# vuex中状态是对象时，注意什么？
对象是引用类型，复制后改变属性还是会影响元数据，这样会改变state里面的状态，是不允许，所以先用深 克隆复制对象，在修改  

# action和mutation有什么区别
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1694585247406-0ccf74ce-a614-4e88-bfba-07e8d6f1ab51.png)

+ action提交的是mutation，而不是直接变更状态，mutation可以直接变更状态
+ action可以包含任一异步操作，mutation只能是同步操作
+ action是通过dispatch进行提交，而mutation是通过commit进行提交
+ 接收参数不同，mutation第一个参数是state，而action第一个参数是context

# pinia
> [pinia - 掘金](https://juejin.cn/post/7112843235675865102)
>
> [Pinia 🍍](https://pinia.web3doc.top/core-concepts/#%E4%BD%BF%E7%94%A8-store)
>

特点：

+ 支持 options api 和 composition api 两种语法创建 Store
+ 不再使用mutations
+ 没有嵌套模块，而是用组合的 stores 来代替
+ 对 ts 的支持
+ 体积小，仅1kb
+ 支持 Vue devtools、SSR 和 webpack 代码拆分
    - 跟踪动作、突变的时间线
    - Store 出现在使用它们的组件中
    - time travel 和 更容易的调试
+ 热模块替换
    - 在不重新加载页面的情况下修改您的 Store
    - 在开发时保持任何现有状态
+ 支持插件：使用插件扩展 Pinia 功能

# 受控组件&非受控组件
受控组件： 收我们控制的组件，组件的状态全称响应外部数据

非受控组件：一般情况是在初始化的时候接受外部数据，然后自己在内部存储其自身状态

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

**作用：**

dom元素非常庞大，而且页面很多的性能问题都是dom操作引起的，虚拟dom除了diff算法可以优化页面性能意外，其实抽离了原本的渲染过程，实现了跨平台的能力

# diff算法
1. 定义

根据老的节点构建一个map，方便key快速查找

是一种对比新旧虚拟dom的一种方法，找出需要更改的虚拟节点并对其进行更新，其他没有更新的则不用改变

2. 损耗计算

使用虚拟dom算法的损耗计算：

总损耗 = 虚拟dom增删改 + (与diff算法效率有关)真是dom增删改 + （较少的节点）排版和重绘

直接操作真实dom的损耗：

总损耗 = 真实dom完全增删改 + (可能较多的节点的)排版和重绘

3. diff是深度优先算法，进行同层比较

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1694586708904-0a919250-81d1-4137-b872-e468da18282e.png)

## vue diff
> [15张图，20分钟吃透Diff算法核心原理，我说的！！！ - 掘金](https://juejin.cn/post/6994959998283907102)
>
> [Vue2 VS Vue3 Diff算法的比较 - 掘金](https://juejin.cn/post/6969860972869582861?searchId=20231021233208C17E7EF618926724F0F3)
>
> [Vue2、Vue3的diff对比 - 掘金](https://juejin.cn/post/7152410265261178911#heading-6)
>

## vue2 diff
当数据改变时，会触发setter，并且通过Dep.notify去通知所有订阅者Watcher，订阅者们会调用patch方法，给真实dom打补丁，更新相应的视图

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1694586799767-23e15e7b-6daa-483b-bb43-5f2dc5fa4a27.png)

patch方法：对比当前同层的虚拟节点是否为同一种类型的标签

**判断是否是同一类型的标签的标准：**

+ key值是否一样
+ 标签名是否一样
+ 是否都为注释节点
+ 是否都定义了data
+ 当标签是input时，type是否相同

**如果是：继续执行patchNode方法**

+ 找到对应的真实dom，称为el
+ 判断newNode和oldNode是否指向同一个对象，如果是，直接return
+ 如果他们都有文本节点并且不相等，那么直接将el的文本节点设置为newVnode的文本节点
+ 如果oldVnode有子节点而newVnode没有，则删除el的子节点
+ 如果oldVnode没有子节点而newVnode有，则将newVnode的子节点真实化之后添加到el
+ 如果两者都有子节点，则执行updateChildren函数比较子节点

updateChildren方法：新的子节点集合和旧的子节点的集合各有首尾两个指针

新旧节点首尾进行四次比较，如果四次都匹配不到，则将所有旧子节点的key做一个映射到就节点下标的key=>index表，然后用新vnode的key去找出就节点中可以复用的位置

**如果不是：没必要比对，直接整个节点替换成新虚拟节点**

![](https://cdn.nlark.com/yuque/0/2023/jpeg/2366100/1697904683323-f2aadbb1-23b7-48a9-80d9-dab6a0762312.jpeg)

## vue3 diff
与vue2区别于：

1. 对于剩余节点的处理方式：
    1. vue2中<font style="color:rgb(37, 41, 51);">是通过对旧节点列表建立一个 { key, oldVnode }的映射表，然后遍历新节点列表的剩余节点，根据newVnode.key在旧映射表中寻找可复用的节点，然后打补丁并且移动到正确的位置。</font>
    2. <font style="color:rgb(37, 41, 51);">建立一个存储新节点数组中的剩余节点在旧节点数组上的索引的映射关系数组，建立完成这个数组后也即找到了可复用的节点，然后通过这个数组计算得到最长递增子序列，这个序列中的节点保持不动，然后将新节点数组中的剩余节点移动到正确的位置</font>
2. <font style="color:rgb(37, 41, 51);">vue2中对于子节点的比对是采取的头尾的四次对比而vue3中采用的是头头比较然后尾部和尾部比较</font>
3. <font style="color:rgb(37, 41, 51);">静态提升： Vue 的编译器在编译过程中，发现了一些不会变的节点或者属性，就会给这些节点打上标记。然后编译器在生成代码字符串的过程中，会发现这些静态的节点，并提升它们，将他们序列化成字符串，以此减少编译及渲染成本。有时可以跳过一整棵树</font>

## react diff
> react不能通过双端对比进行diff算法优化是因为目前fiber上没有设置反向链表
>

1. 从左向右老节点进行比对查找能复用的旧节点，如果有老节点比对不成功的，则停止这一轮的比对，并记录停止的位置
2. 如果第一轮比对，能把所有的新节点都比对完毕，则删除旧节点还没进行比对的节点
3. 如果第一轮的比对，没能将所有的新节点都比对完毕，则继续从第一轮比对停值的位置继续开始循环新节点，拿每一个新节点去老节点里面进行查找，有匹配的则复用，没匹配成功的则在协调位置的时候打上placement的标记
4. 在所有新节点比对完毕之后，检查还有没有没进行复用的旧节点，如果有，就全部删除

# Vue和react中key的作用
> [面试官：你知道vue中key的原理吗？说说你对它的理解 | web前端面试 - 面试官系列](https://vue3js.cn/interview/vue/key.html#%E4%BA%8C%E3%80%81%E8%AE%BE%E7%BD%AEkey%E4%B8%8E%E4%B8%8D%E8%AE%BE%E7%BD%AEkey%E5%8C%BA%E5%88%AB)
>

key主要用在vue的虚拟dom算法中，在新旧节点比较时辨识虚拟节点，如果不使用key，vue会使用一种使用最大限度减少动态元素并且尽可能的尝试修复或再利用相同类型元素的算法

有相同元素的子元素必须有独特的key，重复的key会造成渲染错误

也可以用于强制替换元素/组件而不是重复使用它：（以下场景）

+ 完整的触发组件的生命周期钩子
+ 触发过渡

**重点：**

+ **key会用在diff算法中，用来辨析新旧节点**
+ **不带key的时候会最大限度减少元素变动，尽可能用相同元素**
+ **带key的时候，会基于相同的key来进行排列**
+ **带key还可以触发过渡效果，以及触发组件的生命周期**

## 用index作为key的问题——就地复用
vue中使用虚拟dom且根据diff算法进行新旧对比，从而更新真实dom，key是虚拟dom对象的唯一标识

如果旧虚拟dom中找到了与新虚拟dom相同的key：  

+  若虚拟dom中的内容没变，直接使用之前的真实dom  
+  若虚拟dom中内容变了，则生成新的真实dom，随后替换掉页面中之前的真实dom  

若对数据进行逆序添加，逆序删除等破坏顺序操作：会产生没有必要的真实dom更新，页面效果没问题，但是效率低 

如果结构中还包含输入类的dom：会产生错误dom更新=》界面有问题 

如果不存在对数据的逆序添加或删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的  

[为什么不建议使用 index 作为 key 值_可拖拽列表key设置为index会有什么问题-CSDN博客](https://blog.csdn.net/z591102/article/details/106682298)

# vue要做权限管理应该怎么做？如果控制到按钮级别的权限怎么做？
> [面试官：vue要做权限管理该怎么做？如果控制到按钮级别的权限怎么做？ | web前端面试 - 面试官系列](https://vue3js.cn/interview/vue/permission.html#%E4%BA%8C%E3%80%81%E5%A6%82%E4%BD%95%E5%81%9A)
>

权限是对特定资源的访问许可，所谓权限控制，也就是确保用户只能访问到被分配的资源

而前端权限归根结底是请求的发起权，请求的发起可能有下面两种形式触发

+ 页面加载触发
+ 页面上的按钮点击触发
1. 接口权限

一般采用jwt的形式来验证，没有通过的话一般返回401，跳转到登录页面重新进行登录，登录完拿到token，将token存起来，通过axios请求拦截器进行拦截，每次请求的时候头部携带token

2. 按钮权限
    1. <font style="color:rgb(44, 62, 80);">按钮权限也可以用</font><font style="color:rgb(71, 101, 130);">v-if</font><font style="color:rgb(44, 62, 80);">判断，但是如果页面过多，每个页面页面都要获取用户权限</font><font style="color:rgb(71, 101, 130);">role</font><font style="color:rgb(44, 62, 80);">和路由表里的</font><font style="color:rgb(71, 101, 130);">meta.btnPermissions</font><font style="color:rgb(44, 62, 80);">，然后再做判断</font>
    2. <font style="color:rgb(44, 62, 80);">通过自定义指令进行按钮权限的判断</font>

<font style="color:rgb(44, 62, 80);">首先配置路由</font>

```javascript
{
  path: '/permission',
    component: Layout,
    name: '权限测试',
    meta: {
    btnPermissions: ['admin', 'supper', 'normal']
  },
  //页面需要的权限
  children: [{
    path: 'supper',
    component: _import('system/supper'),
    name: '权限测试页',
    meta: {
      btnPermissions: ['admin', 'supper']
    } //页面需要的权限
  },
             {
               path: 'normal',
               component: _import('system/normal'),
               name: '权限测试页',
               meta: {
                 btnPermissions: ['admin']
               } //页面需要的权限
             }]
}
```

<font style="color:rgb(44, 62, 80);">自定义权限鉴定指令</font>

```javascript
import Vue from 'vue'
/**权限指令**/
const has = Vue.directive('has', {
  bind: function (el, binding, vnode) {
    // 获取页面按钮权限
    let btnPermissionsArr = [];
    if(binding.value){
      // 如果指令传值，获取指令参数，根据指令参数和当前登录人按钮权限做比较。
      btnPermissionsArr = Array.of(binding.value);
    }else{
      // 否则获取路由中的参数，根据路由的btnPermissionsArr和当前登录人按钮权限做比较。
      btnPermissionsArr = vnode.context.$route.meta.btnPermissions;
    }
    if (!Vue.prototype.$_has(btnPermissionsArr)) {
      el.parentNode.removeChild(el);
    }
  }
});
// 权限检查方法
Vue.prototype.$_has = function (value) {
  let isExist = false;
  // 获取用户按钮权限
  let btnPermissionsStr = sessionStorage.getItem("btnPermissions");
  if (btnPermissionsStr == undefined || btnPermissionsStr == null) {
    return false;
  }
  if (value.indexOf(btnPermissionsStr) > -1) {
    isExist = true;
  }
  return isExist;
};
export {has}
```

<font style="color:rgb(44, 62, 80);">在使用的按钮中只需要引用</font><font style="color:rgb(71, 101, 130);">v-has</font><font style="color:rgb(44, 62, 80);">指令</font>

```vue
<el-button @click='editClick' type="primary" v-has>编辑</el-button
```

3. 菜单权限
    1. 前端定义路由信息，菜单由后端返回，<font style="color:rgb(44, 62, 80);">全局路由守卫里做判断</font>

<font style="color:rgb(44, 62, 80);">每次路由跳转的时候都要判断权限，这里的判断也很简单，因为菜单的</font><font style="color:rgb(71, 101, 130);">name</font><font style="color:rgb(44, 62, 80);">与路由的</font><font style="color:rgb(71, 101, 130);">name</font><font style="color:rgb(44, 62, 80);">是一一对应的，而后端返回的菜单就已经是经过权限过滤的</font>

<font style="color:rgb(44, 62, 80);">如果根据路由</font><font style="color:rgb(71, 101, 130);">name</font><font style="color:rgb(44, 62, 80);">找不到对应的菜单，就表示用户有没权限访问</font>

<font style="color:rgb(44, 62, 80);">如果路由很多，可以在应用初始化的时候，只挂载不需要权限控制的路由。取得后端返回的菜单后，根据菜单与路由的对应关系，筛选出可访问的路由，通过</font><font style="color:rgb(71, 101, 130);">addRoutes</font><font style="color:rgb(44, 62, 80);">动态挂载</font>

<font style="color:rgb(44, 62, 80);">缺点：</font>

        * <font style="color:rgb(44, 62, 80);">菜单需要与路由做一一对应，前端添加了新功能，需要通过菜单管理功能添加新的菜单，如果菜单配置的不对会导致应用不能正常使用</font>
        * <font style="color:rgb(44, 62, 80);">全局路由守卫里，每次路由跳转都要做判断</font>
    2. <font style="color:rgb(44, 62, 80);">菜单和路由都由后端返回</font>

<font style="color:rgb(44, 62, 80);">在将后端返回路由通过</font><font style="color:rgb(71, 101, 130);">addRoutes</font><font style="color:rgb(44, 62, 80);">动态挂载之间，需要将数据处理一下，将</font><font style="color:rgb(71, 101, 130);">component</font><font style="color:rgb(44, 62, 80);">字段换为真正的组件</font>

<font style="color:rgb(44, 62, 80);">如果有嵌套路由，后端功能设计的时候，要注意添加相应的字段，前端拿到数据也要做相应的处理</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697902019031-9411f4bd-6012-47e5-ba27-431bcc01ab4e.png)

<font style="color:rgb(44, 62, 80);">缺点：</font>

        * <font style="color:rgb(44, 62, 80);">全局路由守卫里，每次路由跳转都要做判断</font>
        * <font style="color:rgb(44, 62, 80);">前后端的配合要求更高</font>
4. 路由权限
    1. 初始化即挂载全部路由，并且在路由上标记相应的权限信息，每次路由跳转前做校验

缺点：

        * <font style="color:rgb(44, 62, 80);">加载所有的路由，如果路由很多，而用户并不是所有的路由都有权限访问，对性能会有影响。</font>
        * <font style="color:rgb(44, 62, 80);">全局路由守卫里，每次路由跳转都要做权限判断</font>
        * <font style="color:rgb(44, 62, 80);">菜单信息写死在前端，要改个显示文字或权限信息，需要重新编译</font>
        * <font style="color:rgb(44, 62, 80);">菜单跟路由耦合在一起，定义路由的时候还有添加菜单显示标题，图标之类的信息，而且路由不一定作为菜单显示，还要多加字段进行标识</font>
    2. <font style="color:rgb(44, 62, 80);">初始化的时候先挂载不需要权限控制的路由，比如登录页，404等错误页。如果用户通过URL进行强制访问，则会直接进入404，相当于从源头上做了控制。登录后，获取用户的权限信息，然后筛选有权限访问的路由，在全局路由守卫里进行调用</font><font style="color:rgb(71, 101, 130);">addRoutes</font><font style="color:rgb(44, 62, 80);">添加路由</font>

<font style="color:rgb(44, 62, 80);">按需挂载，路由就需要知道用户的路由权限，也就是在用户登录进来的时候就要知道当前用户拥有哪些路由权限</font>

<font style="color:rgb(44, 62, 80);">缺点：</font>

        * <font style="color:rgb(44, 62, 80);">全局路由守卫里，每次路由跳转都要做判断</font>
        * <font style="color:rgb(44, 62, 80);">菜单信息写死在前端，要改个显示文字或权限信息，需要重新编译</font>
        * <font style="color:rgb(44, 62, 80);">菜单跟路由耦合在一起，定义路由的时候还有添加菜单显示标题，图标之类的信息，而且路由不一定作为菜单显示，还要多加字段进行标识</font>

# vue2和vue3的区别
## 生命周期
setup是围绕beforeCreate和created生命周期钩子运行的，所以不需要显式的定义

![画板](https://cdn.nlark.com/yuque/0/2023/jpeg/2366100/1695201587304-92c51287-cf94-4487-bf59-c072c89aa0a3.jpeg)

## 多根节点
vue2中只能存在一个根节点，vue3中可以支持多个根节点

```javascript
// vue2
<template>
  <div>
    <header></header>
    <main></main>
    <footer></footer>
  </div>
</template>
// vue3
<template>
  <header></header>
  <main></main>
  <footer></footer>
</template>
```

> vue3中可以有多个根节点，为什么vue2中只有一个根节点？
>
> 当前构建和diff虚拟dom的算法还未支持这样的结构，也很难在保证性能的情况下支撑。某些实际情况 下是存在多节点组件的需求的，而不是一味的拆分细化。 react里新增了 Fragment 语法以支持类似多 节点的组件，而 Vue3 也提出了类似的语法支持，实际上需要在多节点外包裹一层特殊节点。  
>

## composition API
vue2是选项式options API，一个逻辑会散乱在文件不同位置，导致代码的可读性变差，当需要修改的时候，需要上下来回跳转文件位置

vue3的组合式api可以将同一逻辑的内容写到一起，增强了代码的可读性，内举行，还提供了较为完美的逻辑复用方案

##  异步组件（Suspense）
[Suspense | Vue.js](https://cn.vuejs.org/guide/built-ins/suspense.html#async-dependencies)

<font style="color:rgb(37, 41, 51);">Vue3 提供 Suspense 组件，</font><font style="color:#000000;">用来在组件树中协调对异步依赖的处理。它让我们可以在组件树上层等待下层的多个嵌套异步依赖项解析完成，并可以在等待时渲染一个加载状态，</font><font style="color:rgb(37, 41, 51);">如 loading</font>

1. <font style="color:#000000;">如果组件关系链上有一个 </font><font style="color:#000000;"><Suspense></font><font style="color:#000000;">，那么这个异步组件就会被当作这个 </font><font style="color:#000000;"><Suspense></font><font style="color:#000000;"> 的一个异步依赖。在这种情况下，加载状态是由 </font><font style="color:#000000;"><Suspense></font><font style="color:#000000;"> 控制，而该组件自己的加载、报错、延时和超时等选项都将被忽略。</font>
2. 异步组件也可以通过在选项中指定suspensible: false表明不用Suspense控制，并让组件始终自己控制其加载状态
3. <font style="color:rgb(37, 41, 51);">使用它，需在模板中声明，并包括两个命名插槽：default 和 fallback。Suspense 确保加载完异步内容时显示默认插槽，并将 fallback 插槽用作加载状态</font>

```javascript
<template>
  <suspense>
  // 未加载完成显示loading，加载完成显示自身
    <template #default>
      <List />
    </template>
    <template #fallback>
      <div>
        Loading...
      </div>
    </template>
  </suspense>
</template>
```

4. 事件
    1. pending事件是在进入挂起状态时触发
    2. resolve是在default插槽完成获取新内容时触发
    3. fallback事件则是在fallback插槽的内容显示时触发

## Teleport
<Teleport>是一个内置组件，他可以将一个组件内部的一部分模板传送到该组件的dom结构外层的位置去

```javascript
<button @click="dialogVisible = true">显示弹窗</button>
<teleport to="body">
  <div class="dialog" v-if="dialogVisible">
    我是弹窗，我直接移动到了body标签下
  </div>
</teleport>
```

## 响应式原理
vue2响应式原理基础是Object.defineProperty；vue3响应式原理是Proxy

vue3不选择Object.defineProperty选择proxy的原因：

Object.defineProperty无法监听对象或数组新增，删除的元素

## 虚拟dom
vue2中会将没有发生更改的节点进行比较，比较消耗性能，在vue3中，创建虚拟dom树的时候，会根据dom中的内容会不会发生变化添加静态标记， vue3对于不参与更新的元素，做静态标记并提示，只会被创建一次，再渲染时直接复用  ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695200513218-ed8cf760-7b47-433f-957e-5ef401876812.png)

```javascript
export const enum PatchFlags {
  // 动态文本节点
  TEXT = 1,
  // 动态 class
  CLASS = 1 << 1, // 2
  // 动态 style
  STYLE = 1 << 2, // 4
  // 动态属性，但不包含类名和样式
  // 如果是组件，则可以包含类名和样式
  PROPS = 1 << 3, // 8
  // 具有动态 key 属性，当 key 改变时，需要进行完整的 diff 比较。
  FULL_PROPS = 1 << 4, // 16
  // 带有监听事件的节点
  HYDRATE_EVENTS = 1 << 5, // 32
  // 一个不会改变子节点顺序的 fragment
  STABLE_FRAGMENT = 1 << 6, // 64
  // 带有 key 属性的 fragment 或部分子字节有 key
  KEYED_FRAGMENT = 1 << 7, // 128
  // 子节点没有 key 的 fragment
  UNKEYED_FRAGMENT = 1 << 8, // 256
  // 一个节点只会进行非 props 比较
  NEED_PATCH = 1 << 9, // 512
  // 动态 slot
  DYNAMIC_SLOTS = 1 << 10, // 1024
  // 静态节点
  HOISTED = -1,
  // 指示在 diff 过程应该要退出优化模式
  BAIL = -2
}
```

## diff算法
vue3中存在最长递增子序列使得我们可以保证移动次数最少

## TS支持
<font style="color:rgb(37, 41, 51);">Vue 3 对 TypeScript 的支持更加完善，提供了更好的类型推断和类型检查。</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695260830674-34bf8d3b-dcb7-44e0-a753-8f032217872e.png)

## 静态提升
<font style="color:rgb(37, 41, 51);">Vue 的编译器在编译过程中，发现了一些不会变的节点或者属性，就会给这些节点打上标记。然后编译器在生成代码字符串的过程中，会发现这些静态的节点，并提升它们，将他们序列化成字符串，以此减少编译及渲染成本。有时可以跳过一整棵树</font>

# <font style="color:rgb(37, 41, 51);">ref()和reactive()的区别</font>
1. <font style="color:rgb(77, 77, 77);">ref只能用于将基本类型数据转换成响应式数据，而reactive可以将任意对象转换成响应式数据</font>
2. <font style="color:rgb(77, 77, 77);">ref返回一个包含value属性的对象，而reactive返回一个响应式的Proxy对象</font>

> ref：
>
> 当我们在模板中使用了一个ref，然后改变了这个ref的值时，vue会自动检测到这个变化，并且相应的更新dom，这是依赖追踪的响应式系统实现的，当一个组件首次渲染时，vue会追踪在渲染过程中使用的每一个ref，然后，当一个ref被修改时，它会触发追踪它的组件的一个重新渲染
>
> ref可以持有任何类型的值，包括深层嵌套的对象、数组或者js内置的数据结构。非原始值将通过reactive()转换为响应式代理，也可以通过[shallow ref](https://cn.vuejs.org/api/reactivity-advanced.html#shallowref)来放弃深层响应性，<font style="color:#000000;">对于浅层 ref，只有 </font><font style="color:#000000;">.value</font><font style="color:#000000;"> 的访问会被追踪。浅层 ref 可以用于避免对大型数据的响应性开销来优化性能、或者有外部库管理其内部状态的情况</font>
>

> reactive：
>
> 与ref不同的是，reactive()将使对象本身具有响应性。响应式对象是Proxy，其行为和普通对象一样，不同的是，vue能够拦截<font style="color:#000000;">对响应式对象所有属性的访问和修改，以便进行依赖追踪和触发更新</font>
>
> <font style="color:#000000;">缺点：</font>
>
> + <font style="color:#000000;">有限的值类型：只能用于对象类型（对象，数组，map，set这样的集合类型），不能持有原始类型</font>
> + <font style="color:#000000;">不能替换整个对象</font>
> + <font style="color:#000000;">对解构操作不友好：当我们将响应式对象的原始类型属性结构为本地变量时，或者将该属性传递给函数时，我们将丢失响应式连接</font>
>

# <font style="color:#000000;">导航解析流程</font>
1. 导航被触发
2. <font style="color:rgb(60, 60, 67);">在失活的组件里调用 </font><font style="color:#DF2A3F;">beforeRouteLeave</font><font style="color:rgb(60, 60, 67);"> 守卫</font>
3. <font style="color:rgb(60, 60, 67);">调用全局的 </font><font style="color:#DF2A3F;">beforeEach</font><font style="color:rgb(60, 60, 67);"> 守卫</font>
4. <font style="color:rgb(60, 60, 67);">在重用的组件里调用</font><font style="color:#DF2A3F;">beforeRouteUpdate</font><font style="color:rgb(60, 60, 67);">守卫</font>
5. <font style="color:rgb(60, 60, 67);">在路由配置里调用</font><font style="color:#DF2A3F;">beforeEnter</font>
6. <font style="color:rgb(60, 60, 67);">解析异步路由组件</font>
7. <font style="color:rgb(60, 60, 67);">在被激活的组件里调用</font><font style="color:#DF2A3F;">beforeRouteEnter</font>
8. <font style="color:rgb(60, 60, 67);">调用全局的</font><font style="color:#DF2A3F;">beforeResolve</font><font style="color:rgb(60, 60, 67);">守卫</font>
9. <font style="color:rgb(60, 60, 67);">导航被确认</font>
10. <font style="color:rgb(60, 60, 67);">调用全局的</font><font style="color:#DF2A3F;">afterEach</font><font style="color:rgb(60, 60, 67);">钩子</font>
11. <font style="color:rgb(60, 60, 67);">触发dom更新</font>
12. <font style="color:rgb(60, 60, 67);">调用</font><font style="color:#DF2A3F;">beforeRouteEnter守卫中传给next的回调函数</font><font style="color:rgb(60, 60, 67);">，创建好的组件实例会作为回调函数的参数传入</font>

# <font style="color:rgb(60, 60, 67);">toRef和toRefs的区别</font>
> [还不清楚Vue3中toRef和toRefs的区别？有这篇文章就够了 - 掘金](https://juejin.cn/post/7280783758618525732?searchId=202312022036145208D5F11F462F3A0677)
>

> 为什么会需要toRef和toRefs？
>
> **如果我们不使用toRef和toRefs的话，我们使用解构的时候会丢失响应式效果**
>
> 1. 在vue中，直接使用响应式对象的属性可以实现属性的双向绑定和响应式更新。但是有时候需要将某个属性提取出来作为独立的 ref 对象，这样可以在不影响源对象的情况下，对属性进行单独的访问和修改。toRef 函数正是为了解耦属性的关联，将属性转换为一个独立的 ref 对象。
> 2. Vue 的组件系统中，父组件向子组件传递属性时，需要将这些属性声明为响应式对象。但是，如果直接将整个响应式对象传递给子组件，子组件无法通过解构或者直接访问每个属性。这时，toRefs 函数就可以将整个响应式对象转换为一个普通对象，每个属性都是独立的ref对象，子组件可以轻松解构和访问这些属性。
>

1. toRef()：可以将值、refs或getters规范化为refs，也可以基于响应式对象上的一个属性，创建一个对应的ref，这样创建的ref与其源属性保持同步：改变源属性的值将更新ref的值，反之亦然。**提取响应式对象中的属性提取出来作为独立的ref对象**

```javascript
<script setup>
  import { reactive, toRef } from 'vue';
  let info = reactive({
    name: 'Echo',
    age: 26,
  })
  let age = toRef(info, 'age');
  const updateInfoObjAge = () => {
    info.age++;
  }
  const updateAge = () => {
    age.value++;
  }
</script>
<template>
  <div id="app">
    <p>info对象中的age：{{ info.age }}</p>
    <button @click="updateInfoObjAge">更新info对象中的 age</button>
    <br />
    <p>使用toRef函数转换后的age：{{ age }}</p>
    <button @click="updateAge">更新 age</button>
  </div>
</template>
```

2. toRefs()：将一个响应式对象转换为一个普通对象，这个普通对象的每个属性都是只想源对象相应属性的ref，每个单独的ref都是使用toRef()创建的

```javascript
<script setup>
  import { reactive, toRefs } from 'vue';
  let info = reactive({
    name: 'Echo',
    age: 26,
    gender: 'Male',
  })
  let { name, age, gender } = toRefs(info);
  const update = () => {
    name.value = 'Julie';
    age.value = 33;
    gender.value = 'Female';
  }
</script>
<template>
  <div id="app">
    <p>info对象中的name：{{ info.name }}</p>
    <p>info对象中的age：{{ info.age }}</p>
    <p>info对象中的gender：{{ info.gender }}</p>
    <br />
    <p>解构出来的name：{{ name }}</p>
    <p>解构出来的age：{{ age }}</p>
    <p>解构出来的gender：{{ gender }}</p>
    <button @click="update">更新数据</button>
  </div>
</template>
```

相同点：

+ <font style="color:rgb(53, 53, 53);">toRef 和 toRefs 都用于将响应式对象的属性转换为 ref 对象</font>
+ <font style="color:rgb(53, 53, 53);">转换后的属性仍然保持响应式，对属性的修改会反映到源对象上</font>
+ <font style="color:rgb(53, 53, 53);">不管是使用 toRef 还是 toRefs 将响应式对象转成普通对象，在 script 中修改和访问其值都需要通过 .value 进行</font>

<font style="color:rgb(53, 53, 53);">不同点：</font>

+ <font style="color:rgb(53, 53, 53);">toRef 修改的是对象的某个属性，生成一个单独的 ref 对象</font>
+ <font style="color:rgb(53, 53, 53);">toRefs 修改的是整个对象，生成多个独立的 ref 对象集合</font>
+ <font style="color:rgb(53, 53, 53);">toRefs 适用于在组件传递属性或解构时使用，更加方便灵活，而 toRef 更适合提取单个属性进行操作</font>

