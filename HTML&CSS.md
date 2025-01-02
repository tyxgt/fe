# html文档根节点
 Document。当浏览器载入html文档，就会成为document对象，document对象是html文档的根节点。我们可以根据document对象对html页面中所有元素进行访问，document对象是window对象的一部分。  

# Doctype声明
> [HTML <!DOCTYPE> 声明](https://www.w3school.com.cn/tags/tag_doctype.asp)
>

doctype声明有助于在浏览器中正确显示网页，位于文档中最前面的位置，不是标签，用来告知web浏览器使用哪种版本的html，应该以什么样的文档类型定义来解析文档，不同的渲染模式会影响浏览器对css代码甚至js脚本的解析。  

浏览器渲染页面的两种模式：可以通过document.compatMode获取

+ css1Compat：标准模式，默认模式，浏览器使用W3C的标准解析渲染页面，在标准模式中。浏览器以其支持的最高标准呈现页面  
+ BackCompat：怪异模式（混杂模式），浏览器使用自己的怪异模式解析渲染页面，在怪异模式总，页面以一种比较宽松的向后兼容的方式显示  
    -  盒模型：混杂模式下是怪异盒模型  
    -  标准模式下，div中只有图片的时候，图片下会有3px的空白，混杂模式不会留有空白  ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695195006950-e473e3ac-f542-4e3b-9875-b81f946f87ea.png)
    -  混杂模式下，字体不会继承父元素的字体  
    -  混杂模式下给内联元素设置宽高都是有效的  
    -  标准模式基于内容的百分比；混杂模式下，如果父级元素设置行内，浮动，绝对定位，以父级的宽度为标准，如果父级没有宽度，往上一层找  
    -  标准模式下，如果元素超出框外，会自动裁剪；混杂模式下，如果元素超出框外，元素会自动适应内容  

## HTML5
```javascript
<!DOCTYPE html>
```

 HTML5不基于SGML，不需要引用DTD，但需要doctype约束浏览器行为HTML4.01中的doctype需要对DTD（文档类型定义）进行引用，HTML4.01是基于SGML（标准通用标记语言）的  

HTML5没有DTD，因此也就没有严格模式与混杂模式的区别，HTML5有相对宽松的方法，实现时，已经尽可能大的实现了向后兼容（HTML5没有严格和混杂之分）

## HTMl 4.01 Strict
 该 DTD 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集 （Framesets）。  

```javascript
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
"http://www.w3.org/TR/html4/strict.dtd">
```

##  HTML 4.01 Transitional  
 该 DTD 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集 （Framesets）。

```javascript
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
```

## HTML 4.01 Frameset
 该 DTD 等同于 HTML 4.01 Transitional，但允许框架集内容。  

```javascript
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN"
"http://www.w3.org/TR/html4/frameset.dtd">
```

## XHTML 1.0 Strict
 该 DTD 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集 （Framesets）。必须以格式正确的 XML 来编写标记。  

```javascript
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

## XHTML 1.0 Transitional  
 该 DTD 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集 （Framesets）。必须以格式正确的 XML 来编写标记。  

```javascript
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

## XHTML 1.0 Frameset
 该 DTD 等同于 XHTML 1.0 Transitional，但允许框架集内容。  

```javascript
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```

## XHTML 1.1
该 DTD 等同于 XHTML 1.0 Strict，但允许添加模型（例如提供对东亚语系的 ruby 支持）。  

```javascript
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN"
"http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
```

# input支持的原生事件
+ onfocus：当input 获取到焦点时触发
+ onchange：该事件在表单元素的内容改变且失去焦点时触发
+ oninput：元素获取用户输入时触发
+ onsearch：用户向搜索域输入文本时触发
+ onselect：用户选取文本时触发
+ onblur：当input失去焦点时触发
+ onkeydown： 按下按键时的事件触发
+ onkeyup：当按键抬起的时候触发的事件，在该事件触发之前一定触发了onkeydown事件
+ onclick：主要是用于 input type=button，input作为一个按钮使用时的鼠标点击事件

# 标签
+ title： 定义文档标题，lang属性，规定元素中内容的元素编码  
+ meta： html元信息，字符集声明，视口声明等  
    -  content属性：定义与 http-equiv 或 name 属性相关的元信息。  
    -  charset属性：定义文档的字符编码  
    -  http-equiv属性：将content属性关联到http头部  
        *  content-type：规定文档的字符编码  
        *  default-style：规定要使用的预定义的样式表  
        *  refresh：定义文档自动刷新的时间间隔  
    - name属性： 将content属性关联到一个属性  ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695197219177-3697231f-3a40-498e-aa69-c606cd993bd1.png)

> 利用meta标签实现定时刷新
>

```html
// 每隔30秒刷新一次文档
<head>
  <meta http-equiv="refresh" content="30">
</head>
```

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695198043926-3d8a4375-c89e-4508-981c-2a3471c71272.png)

常用的meta标签

1. charset：用来描述HTML文档的编码类型

```javascript
<meta charset="UTF-8">
```

2. keywords，页面关键词

```javascript
<meta name="keywords" content="关键词">
```

3. description，页面描述

```javascript
<meta name="description" content="页面描述内容">
```

4. refresh，页面重定向和刷新

```javascript
<meta http-equiv="refresh" content="0;url=">
```

5. viewport，适配移动端，可以控制视口的大小和比例

```html
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
```

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695198364055-090b0ef7-a974-498b-850e-d92b10b22bae.png)

6. 搜索引擎索引方式

```javascript
<meta name="robots" content="index, follow">
```

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695198437466-69838966-b907-472b-b0b5-1c37faf27fa0.png)

7. link：引入外部样式表
8. base： 定义文档基础url地址，在文档中所有的相对地址形式都以此为相对地址  
9. head： 定义文档头部信息，引用脚本，只是浏览器找样式表，提供元信息  

# h5新特性
[前端HTML5面试官和应试者一问一答 | 七日打卡 - 掘金](https://juejin.cn/post/6917044041863397383)

[前端面试题之HTML篇](https://www.yuque.com/cuggz/interview/gme0bw)中第六个问题

1. 语义化标签

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695198548152-52dc2fed-6a96-4cc3-ae58-0a998be697e9.png)

2. 媒体标签
+ audio

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695198759549-199fce0b-f598-4722-94ad-402f52b9d754.png)

3. 表单

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695198783135-5bc23b92-c22a-47d1-9a05-1675a0d8bc24.png)

4. 进度条，度量器

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695198806651-549b0d9f-9d28-4e8f-b345-73094c4847d3.png)

5. dom查询操作
    1. document.querySelector()
    2. document.querySelectorAll()
6. web存储：localstorage和sessionstorage
7. 拖放： 拖放是一种常见的特性，即抓取对象以后拖到另一个位置，设置元素可以拖放  

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695198850408-acad88e2-13c5-4047-ae8b-cea7b3f0a130.png)

8. 画布： canvas元素使用js网页上绘制图像，画布是一个矩形区域，可以控制每一像素，canvas拥有多种 绘制路径，举行，原型，字符以及添加图像的方法  
9. websocket
10. input标签新增属性：placeholder。autocomplete。autofocus，required  
11. history api： go forward back pushstate
12.  地理定位：deolocation用于定位用户的位置  

# 字符编码
[程序员必备：彻底弄懂常见的7种中文字符编码 - 掘金](https://juejin.cn/post/6844903714151464974)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697449665889-913671dc-7f1c-4057-a119-20b3138e4c55.png)

+ ascii：每个字母或符号占1byte，并且最高位为0，因此ascii能编码的字母和符号只有128个，标准的只有 128个，现在大多使用的是扩展的 
+ GB2312：最早一版的中文编码，每个字占据2bytes。由于要和ASCII兼容，那这2bytes最高位不可以为0了 （否则和ASCII会有冲突）。在GB2312中收录了6763个汉字以及682个特殊符号，已经囊括了生活中最常用 的所有汉字。  
+ GBK：GBK中在保证不和GB2312、ASCII冲突（即兼容GB2312和ASCII）的前提下，也用每个字占据 2bytes的方式又编码了许多汉字。经过GBK编码后，可以表示的汉字达到了20902个，另有984个汉语标点 符号、部首等。值得注意的是这20902个汉字还包含了繁体字，但是该繁体字与台湾Big5编码不兼容，因为 同一个繁体字很可能在GBK和Big5中数字编码是不一样的。 
+ GB18030：GB18030多出来的汉字使用4bytes编码 
+ UTF-8：UTF8解决字符间分隔的方式是数二进制中最高位连续1的个数来决定这个字是几字节编码。0开头的 属于单字节，和ASCII码重合，做到了兼容。  

##  浏览器乱码的原因是什么？如何解决  
原因： 

+ 网页源代码是gbk的编码，而内容中的中文字是utf-8编码的，这样浏览器打开即会出现html乱码，反之也会出现乱码 
+ html网页编码是gbk，而是程序从数据库中调出呈现是utf-8编码的内容也会造成编码乱码 
+ 浏览器不能自动检测网页编码，造成网页乱码 

解决办法：

+ 使用软件编辑html网页内容 
+ 如果网页设置编码是gbk，而数据库存储数据编码格式是utf-8，此时需要程序查询数据库数据显示数据前进行程序转码 
+ 如果浏览器浏览时候出现网页乱码，在浏览器中找到转换编码的菜单进行转换  

# SEO优化
> 服务端渲染有利于seo优化
>

搜索引擎优化，采用易于搜索引擎索引的合理手段，使网站各项基本要素适合搜索引擎检索原则，对用户更友好，更适合爬虫获取信息使得用户在访问网站时能排在前面。  

+ 控制首页链接数量  
+ 扁平化的目录层次，即目录尽量不超过三级  
+ 导航优化，图片代码必须添加alt和title
+ 网站的结构布局  
+ 利用布局，将重要内容的html代码放在前面  
+ 控制页面大小，减少http请求，提高网站的加载速度  

# 语义化
+  网页加载慢时，css未加载时页面清晰，可读好看  
+  提升用户体验，title，alt  
+  有利于seo  
+  方便其他设备解析页面：例如屏幕阅读器，盲人阅读器，移动设备等  
+  使代码具有可读性，便于团队开发以及维护  

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697449951852-901cfd3c-96f0-4e89-a63e-7fc14ce4a99f.png)

# src与href  
+ src（source）用于替换当前元素，href用于在当前文档于应用资源之间确立联系

当浏览器解析到该元素时，会暂停其他资源的下载和处理，直至将该资源加载，编译，执行完毕，图片和框架等元素也如此，类似于将所指资源嵌入当前标签内；  

+ href是Hypertext Reference的缩写，指向网络资源所在位置，如果我们在文档中添加那么浏览器会识别该文档为css并行下载资源并且不会停止对当前文档的处理  

>  link会异步加载css，而@import会在页面加载完成后开始加载，会出现页面初始无样式，也就是白屏 问题  
>

# HTML5的离线存储怎么使用，它的工作原理是什么  
离线存储指的是在用户没有与因特网连接时，可以正常访问站点或者应用，在用户与因特网连接时，更新用户机器上的缓存文件  

原理：HTML5的离线存储是基于一个新建的.appcahe文件的缓存机制，通过这个文件上的解析清单离线存储资源，这些资源就会像cookie一样被存储下来，之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示  

使用方法：  

1. 创建一个和html同名的manifest文件，然后在页面头部加入manifest属性  ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697450368919-fdc8fcfc-723c-4134-98f9-5bdb0d9025da.png)
2. 在cache.manifest文件中编写需要离线存储的资源  

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697450401837-6f3769ab-1af0-48bb-969a-b718af63dab2.png)

+  CACHE：表示需要离线存储的资源列表，由于包含manifest文件的页面会被自动离线存储，所以 不需要把页面自身也列出来  
+  NETWORK：表示在它下面列出来的资源只有在在线的情况下才能访问，他们不会被离线存储， 所以在离线情况下无法使用这些资源，不过，如果在CACHE和NETWORK中有一个相同的资源， 那么这个资源还是会被离线存储，也就是说CACHE优先级更高  
+  FALLBACK：表示如果访问第一个资源失败，那么就使用第二个资源来替换他  
3. 在离线状态时，操作window.applicationCache进行离线缓存的操作  

** 如何更新缓存：  **

1. 更新manifest文件 
2. 通过js操作 
3. 清除浏览器缓存  

**注：**

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697450508120-04a474ea-5e5f-4266-bba3-1d23bcae6f6e.png)

# iframe
[<iframe> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe)

[为iframe正名，你可能并不需要微前端 - 掘金](https://juejin.cn/post/7185070739064619068)

内联框架元素，能够将另一个html页面嵌入到当前页面中

适用场景：

+ 页面本身简单，没有弹窗，没有浮层，高度固定的纯信息展示页，用iframe
+ <font style="color:rgb(37, 41, 51);">如果页面是包含弹窗、信息提示、或者高度不是固定的话，需要看iframe是否占据了全部的内容区域，如果是像下图这种经典的导航+菜单+内容结构、并且整个内容区域都是iframe，那么可以放心大胆地尝试iframe，否则，需要慎重考虑方案选型。</font>![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1691983202978-36c39287-3d71-48ba-baf9-717a789b4643.png)

优点：

+ 减少数据传输，减少网页加载时间
+ 使用方便
+ 可以使用脚本并行加载
+ 可以实现跨子域通信
+ 方便开发，减少代码重复率

缺点：

+ 部分使用会产生跨域
+ 会产生很多的页面，不易于管理
+ 浏览器的后退按钮无效
+ iframe会阻塞主页面的onload时间
+ 无法被一些搜索引擎识别
+ 会导致浏览器内存占用爆炸，崩溃

> [X-Frame-Options - HTTP | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/X-Frame-Options)
>
> <font style="color:rgb(81, 87, 103);background-color:rgb(247, 248, 250);">X-Frame-Options: deny 表示该页面不允许在 frame 中展示，即便是在相同域名的页面中嵌套也不允许。</font>  
<font style="color:rgb(81, 87, 103);background-color:rgb(247, 248, 250);">X-Frame-Options: SAMEORIGIN，那么页面就可以在同域名页面的 frame 中嵌套。</font>
>

> <font style="color:rgb(81, 87, 103);background-color:rgb(247, 248, 250);">跨子域通信方式：</font>
>
> + postMessage 方法：postMessage 是 HTML5 提供的一种跨窗口通信的方法，可以在不同的窗口（包括 iframe）之间发送消息。通过在发送消息的窗口中调用 postMessage 方法，可以向指定的目标窗口发送消息，并通过事件监听器在目标窗口中接收消息。在 iframe 中，可以通过以下方式发送消息：
>
> ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1701077992630-415c7914-7a2d-491b-9856-21a2f183e4eb.png)
>
> + 使用窗口名称（window.name）：iframe 中的窗口可以通过设置 window.name 属性来共享数据。通过在发送消息的窗口中设置 window.name 属性，然后在接收消息的窗口中读取 window.name 属性，可以实现跨子域通信。这种方法在一些特定的场景下非常有用，但需要注意安全性风险，因为窗口名称可以被其他域的脚本访问。
> + 使用代理页面：创建一个位于父窗口域名下的代理页面，该代理页面可以与子窗口进行通信，并将消息转发给父窗口。通过在子窗口中加载代理页面，并在代理页面中使用 postMessage 方法将消息传递给父窗口，可以实现跨子域通信。这种方法需要在代理页面中进行安全性验证，以确保只接收来自特定域的消息。
>

# 伪类与伪元素
**伪类用于当已有元素处于的某个状态时，为其添加的样式；  **

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697450610412-7b693ad0-d181-434e-be43-7958ba732eef.png)

 **伪元素用于创建一些不存在文档树的元素并为其添加样式；  **

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697450643668-01a0ad61-20de-4e8b-b854-b4fafa603c62.png)

# css中可以继承和不可以继承的属性有哪些  
有继承性的属性： 

1. 字体系列属性：font-family/weight/size/style
2. 文本系列属性：text-indent，text-align，line-height，word-spacing——单词之间的间距，letterspacing——中文或者字母之间的间距，text-transform——控制文本大小写，color
3. 元素可见性：visibility
4. 列表布局属性：list-style
5. 光标属性：cursor  

 没有继承的属性：  

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697450753791-89453206-cadf-4171-9c56-0d45c1be4686.png)

# 引入方式  
+ link
+ style标签 
+ 行间样式 
+ @import引入 
+ js  

 行内样式>链接样式>导入样式  

@import与link的区别： 

+ link是html标签，除了能导入css资源还能导入别的资源，图片，字体等，@import是css的语法，只能引入css的资源 
+ link导入的样式会在页面加载时同时加载，@import导入的样式需要等待页面加载完成后加载 
+ link没有兼容性问题，@import不支持IE5以下 
+ link可以通过js操作dom动态引入样式表改变样式，而@import不可以  

# 浏览器中识别样式表的顺序  
+ 文件开头的Byte order mark字符值，不过一般编辑器闭关不能看到文件头里的bom值 
+ http响应头里的content-type字段包含的charset所指定的值 
+ css文件头里定义的@charset规则里值定的字符编码 
+ 标签里的charset属性，——html5中已经废除 
+ 默认是utf-8  

# 自定义属性实现变量  
需要用--x的格式声明

:root { 

--theme-color: red; 

} 

h1 { 

color: var(--theme-color); 

}  

# 行级元素，块级元素，空元素
区别：

+ 行级元素允许多个排列，块级元素独占一行
+ 块级元素可以设置width,height, 行级元素设置无效
+ 行级元素水平设置margin padding有效，竖直无效

行级元素：span，strong，em，a，br

块级元素：div，p，ul，ol，li，form，address，h

行级块元素：img, input

空元素指没有内容的html元素，空元素实在开始标签中关闭的，空元素没有闭合标签

常见的有：imput，br——换行，hr——分割线，img，link，meta

> 行块级元素：将对象设置为inline对象，但对象的内容作为block对象呈现，之后的内敛对象会被排列在同一行内
>

# bfc
块级格式化上下文，为页面中一个独立的小容器，是独立布局的，有自己的渲染规则，盒子里面的子元素的样式不会影响到外面的元素

触发方式：

+ float属性不为none——left，right，inherit
+ 根元素或包含根元素的元素
+ overflow：hidden/auto 。不为visible
+ position为**absolute或fixed**
+ display：inline-block/table-cell/table-caption/table/inline-table/flow-root/flex/inline-flex/grid/inline-grid
+ contain值为layout,content或strict。属性允许开发者声明当前元素和它的内容尽可能的独立于 DOM 树的其他部分。

用途：

+ 清除父元素内部子元素的浮动——子元素浮动脱离文档流，不能撑开父元素高度，此时设置父元素bfc，包裹子元素即可
+ 解决外边距浮动问题。**margin collapsing——外边距叠加：**当相邻块元素的外边距结合为为单一的外边距时，就会取两者之间较大的值，出现于垂直方向。
+ 两栏布局。前一个元素浮动，后一个元素触发bfc便可实现两栏布局

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697451061352-ea4c80c9-d912-4707-bef4-e7d8caccedbf.png)

# <font style="color:#000000;">css书写规范</font>
[CSS书写规范和顺序 - 掘金](https://juejin.cn/post/6844903914110713869)

样式属性规范：

+ 定位：position，z-index，left，right，top，bottom，clip等
+ 自身属性：width，height，min-height，max-height，min-width，max-width等
+ 文字样式：color，font-size，letter-spacing，text-align等
+ 背景：background-image、border等
+ 文本属性：text-align、vertical-align、text-wrap、text-transform、text-indent、text-decoration 、letter-spacing、word-spacing、white-space、text-overflow等
+ css3中属性：content、box-shadow、animation、border-radius、transform等

# margin重叠问题
两个块级元素的上外边距和下外边距可能会合并为一个外边距，其大小会取外边距值大的哪个，这种行为就是外边距折叠，注：浮动的元素和绝对定位这种脱离文档流的元素的外边距不会折叠，重叠只会出现在垂直方向

计算原则：

+ 如果两者都是整数，那么就取最大值
+ 如果一正一负，就会正值减去负值的绝对值
+ 两个都是负值，用0减去两个中绝对值大的那个

解决办法：

1. 兄弟元素折叠 
    - 底部元素变为行内盒子：display：inline-block
    - 底部元素设置浮动：float
    - 底部元素的position的值为absolute/fixed
2. 父子之间重叠 
    - 父元素加入overflow：hidden
    - 父元素添加透明边框：border：1px solid transparent
    - 子元素变为行内盒子display：inline-block
    - 子元素加入浮动属性或者定位

# z-index
z-index用来描述定义一个元素在屏幕z轴上的堆叠顺序。

z-index仅仅在定位元素上有效果

判断在z轴上的堆叠顺序，不仅仅是直接比较两个元素的z-index的大小，这个堆叠顺序实际是由元素的 层叠上下文，层叠等级共同决定的

z-index元素的position属性需要是relative，absolute或者fixed

z-index属性会在下列情况下失效：

+ 父元素position为relative时，子元素的z-index失效，解决：父元素position改为absolute或者static
+ 元素没有设置position属性为非static属性，解决：设置该元素的position属性为relative，absolute或者fixed的一种
+ 元素在设置z-index的同时还设置了float浮动，解决：float浮动去除，改为display：inline-block；

# 层叠上下文
[彻底搞懂CSS层叠上下文、层叠等级、层叠顺序、z-index_两个div,分别为a和b,a的zindex为500,b的zindex为1000,然后a里有一个名为c-CSDN博客](https://blog.csdn.net/llll789789/article/details/97562099)

如何产生层叠上下文：

1. html中的根元素——根层叠上下文
2. 普通元素设置position属性为非static值并设置z-index为具体数值，产生层叠上下文
3. css新属性也可以产生层叠上下文 
    1. 父元素的display属性值为 `flex|inline-flex`，子元素 `z-index`属性值不为 `auto`的时候，子元素为层叠上下文元素；
    2. opacity不为1
    3. 元素的transform不为none
    4. 元素的 `filter`属性值不是 `none`；

层叠顺序：

+ 形成层叠上下文的背景和边框
+ 负z-index值
+ 块级盒
+ 浮动盒
+ 行内盒
+ z-index:0;
+ 正z-index值

# 盒子模型
<font style="color:rgb(51, 51, 51);">可以通过设置box-sizing来标识其时哪一种盒模型。content-box为标准盒模型，border-box为怪异盒模型（ie盒模型为怪异盒模型）</font>![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697463252173-6c8aead9-b26f-4988-bd3b-f7e9ad1329a8.png)

标准盒模型：<font style="color:rgb(51, 51, 51);">contentbox。width = content</font>![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697463329150-db6d62b0-e6cf-486e-a15c-49414cfd4b2e.png)

<font style="color:rgb(51, 51, 51);">怪异盒模型——borderbox。width = content+padding+border</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697463363395-062e9190-9099-467e-981c-7e8a2dc07ca1.png)

> <font style="color:rgb(119, 119, 119);">需要在border外侧添加空白，且空白处不需要背景时，使用margin</font>
>
> <font style="color:rgb(119, 119, 119);">需要在border内侧添加空白，且空白处需要背景时，使用padding</font>
>

# css选择器及权重
| ！important | infinity |
| --- | --- |
| 行间样式 | 1000 |
| id | 100 |
| class|属性|伪类 | 10 |
| 标签|伪元素 | 1 |
| * | 0 |


<font style="color:rgb(51, 51, 51);">权重部分数字为256进制</font>

<font style="color:rgb(51, 51, 51);">属性选择器：</font>

+ [_attribute_] 用于选取带有指定属性的元素。
+ <font style="color:rgb(51, 51, 51);">[</font>_attribute_<font style="color:rgb(51, 51, 51);">=</font>_value_<font style="color:rgb(51, 51, 51);">]</font><font style="color:rgb(51, 51, 51);"> 用于选取带有指定属性和值的元素。</font>
+ <font style="color:rgb(51, 51, 51);">[</font>_attribute_<font style="color:rgb(51, 51, 51);">~=</font>_value_<font style="color:rgb(51, 51, 51);">] 用于选取属性值中</font>**<font style="color:rgb(51, 51, 51);">包含</font>**<font style="color:rgb(51, 51, 51);">指定词汇的元素。</font>
+ <font style="color:rgb(51, 51, 51);">[</font>_attribute_<font style="color:rgb(51, 51, 51);">|=</font>_value_<font style="color:rgb(51, 51, 51);">] 用于选取带有以</font>**<font style="color:rgb(51, 51, 51);">指定值开头</font>**<font style="color:rgb(51, 51, 51);">的属性值的元素，该值必须是整个单词。</font>
+ <font style="color:rgb(51, 51, 51);">[</font>_attribute_<font style="color:rgb(51, 51, 51);">^=</font>_value_<font style="color:rgb(51, 51, 51);">]</font><font style="color:rgb(51, 51, 51);"> 匹配属性值以指定值开头的每个元素。</font>
+ <font style="color:rgb(51, 51, 51);">[</font>_attribute_<font style="color:rgb(51, 51, 51);">$=</font>_value_<font style="color:rgb(51, 51, 51);">]</font><font style="color:rgb(51, 51, 51);"> 匹配属性值以指定值结尾的每个元素。</font>
+ <font style="color:rgb(51, 51, 51);">[*attribute</font>**=**<font style="color:rgb(51, 51, 51);">value*] 匹配属性值中包含指定值的每个元素。</font>

<font style="color:rgb(51, 51, 51);">>直接子元素选择器 </font>

<font style="color:rgb(51, 51, 51);">+直接相邻选择器——相邻的同级元素 </font>

<font style="color:rgb(51, 51, 51);">~普通相邻选择器——同级元素</font>

# position定位
1. absoluted： 绝对定位，相对于非static定位的第一个父元素进行定位。会脱离文档流  
2. relative：相对定位，相对于其出生位置进行定位，不会脱离文档流元素还具有原先的物理空间
3. static：默认值，没有定位
4. fixed：<font style="color:rgb(51, 51, 51);">相对于浏览器视口进行定位</font>，脱离文档流
5. sticky：relative和fixed的结合体  

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1692087800420-bc3d9915-24f4-4806-810b-8732b575eb25.png)

# 水平垂直居中
## 水平居中
+ **<font style="color:rgb(51, 51, 51);">行内</font>**<font style="color:rgb(51, 51, 51);">：text-align:center</font>
+ <font style="color:rgb(51, 51, 51);">margin:0 auto;</font>
+ <font style="color:rgb(51, 51, 51);">绝对定位和margin-left：自身宽度的-1/2或者transform:translateX(-50%)</font>
+ <font style="color:rgb(51, 51, 51);">position：absolute；上下左右设置为0，margin设置为auto</font>
+ <font style="color:rgb(51, 51, 51);">display：flex；justify-content：center；</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697464132569-726e0acc-531a-4a60-9811-4d8be1ccef54.png)

## <font style="color:rgb(51, 51, 51);">垂直居中</font>
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697464202063-10391078-7f36-44c1-ab69-2a78a06e8204.png)

**<font style="color:rgb(51, 51, 51);">多行文本垂直居中</font>**

<font style="color:rgb(51, 51, 51);">flex+align-items</font>

# <font style="color:rgb(51, 51, 51);">单行，多行文本溢出隐藏</font>
+ <font style="color:rgb(51, 51, 51);">单行文本溢出：</font>

```css
overflow: hidden;
text-overflow: ellipsis
white-space: nowrap
```

+ 多行文本溢出

```css
overflow: hidden;
text-overflow: ellipsis
display: -webkit-box;
// 设置伸缩盒子的子元素排列方式，从上到下垂直排列
-webkit-box-orient: vertical;
-webkit-line-clamp: 3;
```

# <font style="color:rgb(51, 51, 51);">如何用一个div实现textarea</font>
<font style="color:rgb(51, 51, 51);">textarea 标签定义多行文本的文本输入控件</font>

```css
<div class="left" contenteditable="true"></div>
```

# float
1. float即为浮动，在css中的作用是使元素脱离正常的文档流并使其移动到其父元素的最左边或者最右边
2. 文档流：在html中文档流即为元素从上到下排列的顺序
3. 脱离文档流：元素从正常的排列顺序被抽离
4. 最左/右边：元素往左或往右移动直至碰到另一个浮动元素或父元素内容去的边界(不包括padding)

## float的影响
+ 对**父元素**来说，元素浮动之后，脱离当前正常的文档流，所以无法撑开父元素，造成父元素塌陷
+ 对**非浮动**的**兄弟元素**来说。如果兄弟元素为**块级元素**，在现代浏览器以及IE8+下，该元素会忽略浮动元素并占据它的位置，并且元素处于浮动元素下层(z-index无法改变他们的层叠位置)，但它的内部文字和其他行内元素都会环绕浮动元素。**在IE 6、7下则分别都有不同的表现，IE 6、7中，该兄弟元素会紧跟在浮动元素的右侧，并且在IE6中两者之间留有3px的空隙。这就是著名的“IE 3px bug”。**如果兄弟元素为**内联元素，**则元素会环绕浮动元素排列![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697464412025-28b40d44-8877-4994-92d7-481ad0d9742d.png)
+ <font style="color:rgb(51, 51, 51);">对</font>**<font style="color:rgb(51, 51, 51);">浮动</font>**<font style="color:rgb(51, 51, 51);">的</font>**<font style="color:rgb(51, 51, 51);">兄弟元素</font>**<font style="color:rgb(51, 51, 51);">来说，如果是</font>**<font style="color:rgb(51, 51, 51);">同一方向</font>**<font style="color:rgb(51, 51, 51);">的浮动元素，会紧跟在后面；如果是</font>**<font style="color:rgb(51, 51, 51);">反方向</font>**<font style="color:rgb(51, 51, 51);">的浮动元素，在同一条直线上。</font>![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697464425183-504263c2-e0cc-4ca3-8bc4-bc5b47140b91.png)
+ <font style="color:rgb(51, 51, 51);">对</font>**<font style="color:rgb(51, 51, 51);">自身元素</font>**<font style="color:rgb(51, 51, 51);">的影响：float对象被视作块对象，即display属性为block；</font>
+ <font style="color:rgb(51, 51, 51);">对</font>**<font style="color:rgb(51, 51, 51);">子元素</font>**<font style="color:rgb(51, 51, 51);">的影响：当一个元素浮动时，在没有清除浮动的情况下，无法撑开父元素，但是可以让自己的浮动子元素撑开自身，在没有定义具体宽度的情况下，是自身的宽度从100%变为自适应(浮动元素display:block).其高度宽度均为浮动元素高度合非浮动元素高度之间的最大值</font>

<font style="color:rgb(51, 51, 51);">优点：</font>

1. <font style="color:rgb(51, 51, 51);">文本环绕图片——float最初的意义</font>
2. <font style="color:rgb(51, 51, 51);">应用于网页布局</font>

## <font style="color:rgb(51, 51, 51);">清除浮动</font>
**<font style="color:rgb(51, 51, 51);">当一个元素是浮动的，如果没有关闭浮动时，其父元素不会包含这个浮动元素，因为此时浮动元素从文档流脱离</font>**

1. <font style="color:rgb(51, 51, 51);">clear：both：使用这种方法需要增加一个额外元素，并且定义其样式为clear:both </font>**<font style="color:rgb(51, 51, 51);">不建议使用</font>**
2. <font style="color:rgb(51, 51, 51);">overflow：在浮动元素上加入overflow:hidden/auto，auto对seo比较好;</font>**<font style="color:rgb(51, 51, 51);">overflow:visible</font>**<font style="color:rgb(51, 51, 51);">无法达到清除浮动的效果。（触发bfc）</font>
3. <font style="color:rgb(51, 51, 51);">clearfix：利用:after和:before在元素内部插入两个元素 —— 伪类+clear:both</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697466533460-28a57cfb-2cd8-43d4-9a79-991720264f3b.png)

4. <font style="color:rgb(51, 51, 51);">给父元素设置display:inline-block属性</font>
5. <font style="color:rgb(51, 51, 51);">给父级div设置height属性</font>

# <font style="color:rgb(51, 51, 51);">flex布局</font>
<font style="color:rgb(51, 51, 51);">flex布局——弹性布局，用来为盒装模型提供最大的灵活性设置flex之后，子元素</font>**<font style="color:rgb(51, 51, 51);">的float,clear,vertical-align</font>**<font style="color:rgb(51, 51, 51);">属性将失效。容器默认存在两根轴，水平</font>**<font style="color:rgb(51, 51, 51);">主轴</font>**<font style="color:rgb(51, 51, 51);">和垂直</font>**<font style="color:rgb(51, 51, 51);">交叉轴</font>**<font style="color:rgb(51, 51, 51);">，容易实现垂直居中</font>

**设置在父容器上的值：**

+ <font style="color:rgb(51, 51, 51);"> flex-direction：row|column|row-reverse|column -reverse决定主轴方向 </font>
+ <font style="color:rgb(51, 51, 51);"> flex-wrap：nowrap|wrap|wrap-reverse决定如果一条轴线排不下如何换行 </font>
+ <font style="color:rgb(51, 51, 51);"> flex-flow:1和2的简写形式，默认值为row nowrap </font>
+ <font style="color:rgb(51, 51, 51);"> justify-content：定义了项目再主轴上的对齐方式  
</font><font style="color:rgb(51, 51, 51);">flex-start/end|center|space-between/around </font>
+ <font style="color:rgb(51, 51, 51);"> align-items：定义在交叉轴上如何对齐  
</font><font style="color:rgb(51, 51, 51);">flex-start/end|center||baseline|stretch——如果项目未设置高度或设为auto，将占满整个容器的高度 </font>
+ <font style="color:rgb(51, 51, 51);"> align-content：属性定义了多根轴线的对齐方式  
</font><font style="color:rgb(51, 51, 51);">flex-start/end|center|space-between/around|stretch </font>

> <font style="color:rgb(51, 51, 51);">align-content对于单行的盒子不起作用，可以通过flex-wrap属性使其生效</font>
>
> <font style="color:rgb(51, 51, 51);">align-items不区分单行或多行</font>
>

<font style="color:rgb(51, 51, 51);"> </font>**<font style="color:rgb(51, 51, 51);">设置在项目上的值：</font>**

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697467268430-06a1c366-131c-4150-97d8-a872728186f4.png)

## Flex: 1
1. flex: none : 0 0 auto
2. flex: auto : 1 1 auto
3. flex: 1 : 1 1 0
4. flex默认值为0 1 auto

# grid布局
**容器属性：**

<font style="color:rgb(51, 51, 51);">grid-template-columns:定义每一列的列宽</font>

<font style="color:rgb(51, 51, 51);">grid-template-rows：属性定义每一行的行高</font>

<font style="color:rgb(51, 51, 51);">grid-row-gap：设置行与行的间距</font>

<font style="color:rgb(51, 51, 51);">grid-column-gap：设置列与列的间距</font>

<font style="color:rgb(51, 51, 51);">grid-gap：为grid-row-gap与grid-column-gap的合并简写方式grid-template-areas：定义区域</font>

<font style="color:rgb(51, 51, 51);">grid-auto-flow：row|column划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格，默认的放置顺序是先行后列</font>

<font style="color:rgb(51, 51, 51);">justify-items：设置单元格内容的水平位置</font>

<font style="color:rgb(51, 51, 51);">align-items：设置单元格内容的垂直位置</font>

<font style="color:rgb(51, 51, 51);">place-items属性是align-items属性和justify-items属性的合并简写形式。</font>

<font style="color:rgb(51, 51, 51);">justify-content属性是整个内容区域在容器里面的水平位置</font>

<font style="color:rgb(51, 51, 51);">align-content属性是整个内容区域的垂直位置</font>

<font style="color:rgb(51, 51, 51);">place-content属性是align-content属性和justify-content属性的合并简写形式。</font>

<font style="color:rgb(51, 51, 51);">grid-auto-columns属性和grid-auto-rows属性用来设置，浏览器自动创建的多余网格的列宽和行高。</font>

<font style="color:rgb(51, 51, 51);">grid-template属性是grid-template-columns、grid-template-rows和grid-template-areas这三个属性的合并简写形式。</font>

<font style="color:rgb(51, 51, 51);">grid属性是grid-template-rows、grid-template-columns、grid-template-areas、 grid-auto-rows、grid-auto-columns、grid-auto-flow这六个属性的合并简写形式。</font>

**<font style="color:rgb(51, 51, 51);">项目属性：</font>**

<font style="color:rgb(51, 51, 51);">grid-column-start：左边框所在的垂直网格线</font>

<font style="color:rgb(51, 51, 51);">grid-column-end：右边框所在的垂直网格线</font>

<font style="color:rgb(51, 51, 51);">grid-row-start：上边框所在的水平网格线grid-row-end ：下边框所在的水平网格线</font>

<font style="color:rgb(51, 51, 51);">grid-column属性是grid-column-start和grid-column-end的合并简写形式，</font>

<font style="color:rgb(51, 51, 51);">grid-row属性是grid-row-start属性和grid-row-end的合并简写形式。</font>

<font style="color:rgb(51, 51, 51);">grid-area属性指定项目放在哪一个区域。grid-area属性还可用作grid-row-start、grid-column-start、grid-row-end、grid-column-end的合并简写形式，直接指定项目的位置。</font>

<font style="color:rgb(51, 51, 51);">justify-self属性设置单元格内容的水平位置</font>

<font style="color:rgb(51, 51, 51);">align-self属性设置单元格内容的垂直位置</font>

# <font style="color:rgb(51, 51, 51);">移动端适配</font>
[移动端适配原理与方案详解 - 掘金](https://juejin.cn/post/6959047144065990663)

<font style="color:rgb(51, 51, 51);">移动端适配是指在不通过尺寸的手机设备上，页面能相对达到合理的展示或者保持统一效果的等比例缩放</font>

<font style="color:rgb(51, 51, 51);">主要有两个维度：</font>

+ <font style="color:rgb(51, 51, 51);">适配不同像素密度：针对不同的像素密度，使用css媒体查询，选择不同精度的图片，以保证图片不会失真</font>
+ <font style="color:rgb(51, 51, 51);">适配不同屏幕大小：由于不同的屏幕有着不同的逻辑像素大小，所以如果直接使用px作为开发单位，会使得开发的页面在某一款手机上可以准确显示，但是在另外一款手机上就会失真，为了适配不同屏幕的大小，应按照比例还原设计稿的内容</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697467561624-ca178206-9fd0-45a8-8a40-fc81f80c825a.png)

## img标签中srcset属性的作用
<font style="color:rgb(51, 51, 51);">响应式页面中经常用到根据屏幕密度设置不同的图片，这时就用到了img标签的srcset属性，srcset属性用于设置不同屏幕密度下，img会自动加载不同的图片</font>

```javascript
<img src='image-128.png' srccset='image-256.png 2x'/>
在屏幕密度为1x的情况下加载image-128.png，屏幕密度为2x时加载image-256.png
```

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697467892425-47346187-ba3b-4a9e-b414-70b91e207c80.png)

<font style="color:rgb(51, 51, 51);">默认显示128px，如果失去宽度大于360px，则显示340px</font>

# 响应式布局
<font style="color:rgb(51, 51, 51);">优点：</font>

+ <font style="color:rgb(51, 51, 51);">面对不同分辨率设备灵活性强；</font>
+ <font style="color:rgb(51, 51, 51);">能够快捷解决多设备显示适应问题</font>

<font style="color:rgb(51, 51, 51);">缺点：</font>

+ <font style="color:rgb(51, 51, 51);">兼容各种设备工作量大，效率低下</font>
+ <font style="color:rgb(51, 51, 51);">代码累赘，会出现隐藏无用的元素，加载时间长</font>

<font style="color:rgb(51, 51, 51);">响应式界面的基本规则：媒体查询，边界断点的规则设定；内容的可伸缩效果；网格布局；主要内容呈现及图片的高质量</font>

+ <font style="color:rgb(51, 51, 51);">可伸缩的内容区块：内容区块的再一定程度上能够自动调整，以确保填满整个界面</font>
+ <font style="color:rgb(51, 51, 51);">可自由排布的内容区块：当页面尺寸变动较大时，能够减少或增加排布的列数</font>
+ <font style="color:rgb(51, 51, 51);">适应页面尺寸的边距：到页面尺寸发生更大变化时，区块的边距也应该变化</font>
+ <font style="color:rgb(51, 51, 51);">能够适应比例变化的图片：对于常见的宽度调整，图片在隐去两侧部分时，依旧保持美观可用</font>
+ <font style="color:rgb(51, 51, 51);">能够自动隐藏/部分显示的内容：如在电脑上显示的的大段描述文本，在手机上就只能少量显示或全部隐藏</font>
+ <font style="color:rgb(51, 51, 51);">能自动折叠的导航和菜单：展开还是收起，应该根据页面尺寸来判断</font>
+ <font style="color:rgb(51, 51, 51);">放弃使用像素作为尺寸单位：用dp(对于前端来说，这里可能是rem)尺寸等方法来确保页面在分辨率相差很大的设备上，看起来也能保持一致。同时也要求提供的图片应该比预想的更大，才能适应高分辨率的屏幕</font>

## <font style="color:rgb(51, 51, 51);">媒体查询</font>
<font style="color:rgb(51, 51, 51);">针对不同的媒体类型定义不同的样式，当重置浏览器窗口大小的过程中，页面也会格局浏览器的宽度和高度重新渲染页面移动端优先首先使用的是min-width，PC端优先使用的max-width。</font>

<font style="color:rgb(51, 51, 51);">语法：</font>

```css
@media not|only mediatype and (expressions){
    css代码...
}
```

<font style="color:rgb(51, 51, 51);">缺点：在浏览器大小改变前，需要改变的样式太多，代码繁琐</font>

[媒体查询入门指南 - 学习 Web 开发 | MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Media_queries)

[CSS Media媒体查询使用大全，完整媒体查询总结 - 奔跑的太阳花 - 博客园](https://www.cnblogs.com/lguow/p/9316598.html)

<font style="color:rgb(51, 51, 51);">media属性用于为不同的媒介类型规定不同的样式</font>

+ <font style="color:rgb(51, 51, 51);">screen：计算机屏幕</font>
+ <font style="color:rgb(51, 51, 51);">tty：电传打字机以及使用等宽字符网络的类似媒介</font>
+ <font style="color:rgb(51, 51, 51);">tv：电视类型设备（低分辨率，有限的屏幕翻滚能力）</font>
+ <font style="color:rgb(51, 51, 51);">projection：放映机</font>
+ <font style="color:rgb(51, 51, 51);">handheld：手持设备（小屏幕，有限的带宽）</font>
+ <font style="color:rgb(51, 51, 51);">print：打印预览模式/打印页</font>
+ <font style="color:rgb(51, 51, 51);">braille：盲人用点字法反馈设备</font>
+ <font style="color:rgb(51, 51, 51);">aural：语音合成器</font>
+ <font style="color:rgb(51, 51, 51);">all：适合所有设备</font>

<font style="color:rgb(51, 51, 51);">可以通过meta viewport设置布局窗口</font>

<font style="color:rgb(51, 51, 51);"></font>

| **<font style="color:rgb(51, 51, 51);">width</font>** | **<font style="color:rgb(51, 51, 51);">设置布局视口的宽度，为一个正整数，或字符串"width-device"</font>** |
| --- | --- |
| <font style="color:rgb(51, 51, 51);">initial-scale</font> | <font style="color:rgb(51, 51, 51);">设置页面的初始缩放值，为一个数字，可以带小数</font> |
| <font style="color:rgb(51, 51, 51);background-color:rgb(248, 248, 248);">minimum-scale</font> | <font style="color:rgb(51, 51, 51);background-color:rgb(248, 248, 248);">允许用户的最小缩放值，为一个数字，可以带小数</font> |
| <font style="color:rgb(51, 51, 51);">maximum-scale</font> | <font style="color:rgb(51, 51, 51);">允许用户的最大缩放值，为一个数字，可以带小数</font> |
| height | <font style="color:rgb(51, 51, 51);background-color:rgb(248, 248, 248);">设置布局视口的高度，这个属性对我们并不重要，很少使用</font> |
| <font style="color:rgb(51, 51, 51);">user-scalable</font> | <font style="color:rgb(51, 51, 51);">是否允许用户进行缩放，值为"no"或"yes"</font> |


## <font style="color:rgb(51, 51, 51);">百分比布局</font>
<font style="color:rgb(51, 51, 51);">使得浏览器中组件的宽和高随着浏览器的高度的变化而变化，从而实现响应式的效果</font>

1. <font style="color:rgb(51, 51, 51);">子元素的height或width中使用百分比，是相对于子元素的直接父元素</font>
2. <font style="color:rgb(51, 51, 51);">子元素的top和bottom如果设置百分比，则相对于直接非static定位(默认定位)的父元素的高度；子元素的top和bottom如果设置百分比，则相对于直接非static定位(默认定位)的父元素的高度</font>
3. <font style="color:rgb(51, 51, 51);">子元素padding和margin如果设置成百分比，不论垂直还是水平方向都相对于直接父元素的width</font>
4. <font style="color:rgb(51, 51, 51);">border-radius设置为百分比则是相对于自身的宽度</font><font style="color:rgb(51, 51, 51);">缺点：</font>
    1. <font style="color:rgb(51, 51, 51);">计算困难</font>
    2. <font style="color:rgb(51, 51, 51);">由于margin，padding以及widthheight的相对父元素对象不同则使得布局问题变得复杂</font>

## <font style="color:rgb(51, 51, 51);">rem布局</font>
<font style="color:rgb(51, 51, 51);">rem是根据</font>**<font style="color:rgb(51, 51, 51);">根元素font-size</font>**<font style="color:rgb(51, 51, 51);">来决定元素大小的</font><font style="color:rgb(51, 51, 51);">布局思想：</font>

1. <font style="color:rgb(51, 51, 51);">一般不要给元素设置具体的高度，但是对于一些小图标可以设定具体宽度值</font>
2. <font style="color:rgb(51, 51, 51);">高度值可以设置固定值，根据设计稿进行</font>
3. <font style="color:rgb(51, 51, 51);">所有设置的固定值都用rem做单位（首先在HTML总设置一个基准值：px和rem的对应比例，然后在效果图上获取px值，布局的时候转化为rem值）</font>
4. <font style="color:rgb(51, 51, 51);">js获取真实屏幕的宽度，让其除以设计稿的宽度，算出比例，将之前的基准值按照比例进行重新的设定，这样项目就可以在移动端自适应了</font><font style="color:rgb(51, 51, 51);">缺点：</font>**<font style="color:rgb(51, 51, 51);">必须通过js来动态控制根元素font-size的大小，也就是说css样式和js代码有一定的耦合性，必须将font-size的代码放在css样式之前</font>**

<font style="color:rgb(51, 51, 51);">动态rem方案需要做的工作：</font>

+ <font style="color:rgb(51, 51, 51);">meta标签设置viewport宽度为屏幕宽度</font>
+ <font style="color:rgb(51, 51, 51);">根据不同屏幕修改根元素font-size大小，一般者只为屏幕宽度的十分之一（lib-flexible库）</font>
+ <font style="color:rgb(51, 51, 51);">开发环境配置 </font>[postcss-px2rem](https://link.juejin.cn/?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fpostcss-px2rem)<font style="color:rgb(51, 51, 51);"> 或者类似插件；</font>
+ <font style="color:rgb(51, 51, 51);">根据设计稿写样式，元素宽高直接取设计稿宽高即可，单位为 px，插件会将其转换为 rem；</font>
+ <font style="color:rgb(51, 51, 51);">段落文本也按照设计稿写，单位为px，不需要转换为 rem；</font>

## <font style="color:rgb(51, 51, 51);">视口单位</font>
<font style="color:rgb(51, 51, 51);">vw,vh与视图窗口有关，vw是相对于试图窗口的宽度</font><font style="color:rgb(51, 51, 51);">vw ：|相对于视窗的宽度，1vw 等于视口宽度的1%，即视窗宽度是100vw</font><font style="color:rgb(51, 51, 51);">vh ：|相对于视窗的高度，1vh 等于视口高度的1%，即视窗高度是100vh</font><font style="color:rgb(51, 51, 51);">vmin ：|vw和vh中的较小值</font><font style="color:rgb(51, 51, 51);">vmax ： vw和vh中的较大值</font><font style="color:rgb(51, 51, 51);">使用视口单位来实现响应式的做法：</font>

1. <font style="color:rgb(51, 51, 51);">对于设计稿的尺寸转换单位，使用sass函数进行编译</font>
2. <font style="color:rgb(51, 51, 51);">无论文本还是布局宽度，间距等都是用rem作为单位</font>
3. <font style="color:rgb(51, 51, 51);">1物理像素线（也就是普通屏幕下1px,高清屏幕下0.5px的情况）采用transform属性scale实现</font>
4. <font style="color:rgb(51, 51, 51);">对于需要保持宽高比的图应该用padding-top实现</font>
5. <font style="color:rgb(51, 51, 51);">搭配vw和rem：因为失去了最大最小宽度的限制</font>
    1. <font style="color:rgb(51, 51, 51);">给根元素大小设置随着视口变化而变化的vw单位，这样就可以实现动态改变其大小</font>
    2. <font style="color:rgb(51, 51, 51);">限制根元素字体大小的最大最小值，配合body最大宽度和最小宽度</font>

## <font style="color:rgb(51, 51, 51);">图片响应式</font>
<font style="color:rgb(51, 51, 51);">图片响应式两个方面：</font>

1. <font style="color:rgb(51, 51, 51);">大小自适应，保证图片在不同的分辨率下出现压缩，拉伸的情况</font>
2. <font style="color:rgb(51, 51, 51);">根据不同的屏幕分辨率和设备像素比来选择高分辨率的图片，从而减少网络带宽</font>

**<font style="color:rgb(51, 51, 51);">实现方式：</font>**

1. <font style="color:rgb(51, 51, 51);">使用max-width：能够随着容器大小进行缩放</font>
2. <font style="color:rgb(51, 51, 51);">使用srcset </font>

```plain
<img srcset='1.jpg 580w, 2.png 618w'>
// 580px显示1.jpg
// 618px显示2.png
```

3. <font style="color:rgb(51, 51, 51);">使用background-image</font>
4. <font style="color:rgb(51, 51, 51);">使用picture标签</font>

## <font style="color:rgb(51, 51, 51);">css像素和物理像素</font>
<font style="color:rgb(51, 51, 51);">css像素（逻辑像素）：在js或者css代码中使用的px单位是css像素，是一个抽象单位</font>

<font style="color:rgb(51, 51, 51);">物理像素：只与设备的硬件密度有关，任何设备的物理像素都是固定的</font>

<font style="color:rgb(51, 51, 51);">设备像素比dpr = 物理像素 / css像素</font>

## <font style="color:rgb(51, 51, 51);">视口</font>
1. <font style="color:rgb(51, 51, 51);">布局视口</font>
2. <font style="color:rgb(51, 51, 51);">视觉视口：浏览器内看到的网站的显示区域，用户可以通过缩放来查看网页的显示内容，从而改变视觉视口。视觉视口的定义，就像拿着一个放大镜分别从不同距离观察同一个物体，视觉视口仅仅类似于放大镜中显示的内容，因此视觉视口不会影响布局视口的宽度和高度。</font>
3. <font style="color:rgb(51, 51, 51);">理想视口：理想的布局视口</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697468934034-1d0c6100-3fd1-4eff-a723-463ccb861067.png)

## <font style="color:rgb(51, 51, 51);">字体适配</font>
<font style="color:rgb(51, 51, 51);">pc上最小font-size = 12px</font>

<font style="color:rgb(51, 51, 51);">手机上最小font-size = 8px</font>

# <font style="color:rgb(51, 51, 51);">移动端1px问题</font>
> <font style="color:rgb(119, 119, 119);">retina：</font>[视网膜显示_百度百科](https://baike.baidu.com/item/Retina%20Display/1917712)
>

<font style="color:rgb(51, 51, 51);">1px问题指的是在以下retina屏幕的机型上，移动端页面的1px会变得很粗，呈现出不止1px的效果</font>

<font style="color:rgb(51, 51, 51);">window.devicePixelRatio = 设备的物理像素 / css像素</font>

<font style="color:rgb(51, 51, 51);">dpr：window.devicePixelRatio 设备像素比</font>

<font style="color:rgb(51, 51, 51);">解决方案：</font>

> [7 种方法解决移动端 Retina 屏幕 1px 边框问题 - 掘金](https://juejin.cn/post/6844903456717668359?searchId=20231017233947FA4859E7FF5EB4404F59)
>

1. <font style="color:rgb(51, 51, 51);">直接写0.5px</font>
2. <font style="color:rgb(51, 51, 51);">伪元素先放大后缩小：在目标元素的后面追加一个 ::after 伪元素，让这个元素布局为 absolute 之后、整个伸展开铺在目标元素上，然后把它的宽和高都设置为目标元素的2倍，border值设为1px。接着借助 CSS 动画特效中的放缩能力，把整个伪元素缩小为原来的 50%。此时，伪元素的宽高刚好可以和原有的目标元素对齐，而 border 也缩小为了 1px 的二分之一，间接的实现了0.5px的效果</font>
3. <font style="color:rgb(51, 51, 51);">媒体查询根据不同dpr缩放</font>

# <font style="color:rgb(51, 51, 51);">设置小于12px的字体</font>
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697468992017-66f90284-44f0-457d-bb21-f1ee5ab8d2a5.png)<font style="color:rgb(51, 51, 51);"></font>

# <font style="color:rgb(51, 51, 51);">css布局单位</font>
1. 像素 px
2. 百分比%：一般认为子元素的百分比相对于父元素
3. em和rem：em相对于父元素。rem相对于根元素
4. vw/vh： 
+ vw：相对于视窗的宽度，视窗宽度时100vw
+ vh：相对于视窗的高度，视窗高度时100vh
+ vmin：vw和vh中的较小值
+ vmax：vw和vh中的较大值

# 两栏布局
+ 左边浮动，右侧overflow：hidden
+ 左边浮动，右边margin
+ calc函数
+ flex：容器设置为flex布局，左边子元素设置为固定宽度200px，右边子元素flex：1
+ 绝对定位，将父级元素设置为相对定位，左边元素设置为absolute定位，并且宽度设置为200px，将右边的margin-left的值设置为200px；
+ 利用绝对定位：将父级元素设置为相对定位，左边元素宽度设置为200 PX，右边元素设置为绝对定位，左边定位为200 PX，其余方向定位为0。

# 三栏布局
一般指的是左右两栏宽度固定，中间自适应的布局

1.  利用绝对定位，左右两栏设置为绝对定位，中间设置对应方向大小的margin的值  

```css
.outer{
  position: relative;
  height: 100px;
}
.left{
	position: absolute;
  width: 100px;
  height: 100px;
  background: tomato;
}
.right{
	position: absolute;
  top: 0;
  left: 0;
  width: 200px;
  height: 100px;
  background: gold;
}
.center{
  margin-left: 100px;
  margin-right: 200px;
  height: 100px;
  background: lightgreen;
}
```

2. 利用flex布局，左右两栏设为固定大小，中间一栏设置为flex：1；  

```css
.outer{
  display: flex;
  height: 100px;
}
.left{
  width: 100px;
  background: tomato;
}
.right{
  width: 100px;
  background: gold;
}
.center{
  flex: 1;
  background: lightgreen;
}
```

3. 利用浮动, 左右两栏设置固定大小，并设置对应方向的浮动，中间一栏设置左右两个方向的margin 值，注意这种方式，中间一栏必须放到最后  

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697505667707-1edc9f1e-3fd9-4eca-abf7-1e62933aa47a.png)

4.  圣杯布局：利用浮动和负边距来实现，父级元素设置左右的padding，三列均设置向左浮动，中间一列放在最前面，宽度设置为父级元素的宽度，因此后面两列都被挤到了下一行，通过设置margin负值将 其移动到上一行，在利用相对定位，定位到两边  

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697505964588-66eeca72-4add-43f3-9323-ca488e840d7b.png)

5. 双飞翼布局：相对于圣杯布局来说左右位置的保留是通过中间列的margin值来实现的，而不是通过父 元素的padding来实现的，本质上来说，也是通过浮动和外边距负值来实现的  

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697506562520-842d53b5-b259-4122-a208-7fe337e19915.png)

# 隐藏元素的方法
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697531142067-61c78389-c2ad-4d91-af76-8673ece7cbdd.png)

## display:none;visibility:hidden和opacity:0;的异同
+ 是否占据空间 
    - **display:none;隐藏之后不占位置，**
    - visibility:hidden;opacity:0;隐藏后仍然占据位置
+ 子元素是否继承 
    - **display：none不会被子元素继承，父元素都不在了，子元素也不会显示出来**
    - visibility: hidden会被子元素继承，但是可以通过设置子元素visibility:visible来显示子元素
    - opaciity会被子元素继承，但是不能设置子元素opacity：0来重新显示
+ 事件绑定 
    - diaplay:none；的元素已经不在页面存在了，因此无法触发她绑定的事件
    - visibility:hidden不会触发他上面绑定的事件
    - **opacity：0元素上面绑定的事件是可以触发的**
+ 过渡动画：transition对visibility:hidden和display：none是无效的，**对opacity有效**

# css如何针对不同浏览器做适应
+ 浏览器css样式初始化normalize.css
+ 浏览器兼容前缀 
    - -o-://Opera
    - -ms-：//IE
    - -moz-：//firefox
    - -webkit：//chrome

# 重绘、重排、复合（composite）：重绘不一定引起重排，但重排必定重绘
[重排、重绘、合成以及如何优化（基于Chrome）_引起页面重排,重绘,合成的操作-CSDN博客](https://blog.csdn.net/wangfeijiu/article/details/106821464)

重绘：当我们对dom的修改导致了样式的变化，却并未影响其几何属性，浏览器不需要重新计算元素的集合属性，直接为该元素绘制新的样式

重排/回流：当我们对DOM的修改引发了几何尺寸的变化时，浏览器需要重新计算元素的几何属性，其他元素的几何属性和位置也会因此受到影响，然后再将计算的结果绘制出来。

> 如果每次触发回流重绘都进行页面的回流重绘的花，浏览器负担就太重了，所以浏览器会维护一个队列，把所有回引起回流，重绘的操作放入这个队列，登队列中的操作到了一定的数量或者到了一定的时间间隔，浏览器就会flush队列，进行一个批处理，这样就会让多次的回流，重绘变成一次回流重绘![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697531292864-b26b4da5-b374-4460-98be-81c1792c6a25.png)
>

**引起重排的属性和方法**：

+ 添加或删除可见的dom元素
+ 元素尺寸改变——边距，填充，边框，宽度和高度
+ 内容变化
+ 浏览器窗口尺寸改变——resize事件发生
+ 计算offsetWidth和offsetHeight属性——元素布局宽高也就是除了margin
+ 设置style属性的值

** 引起重绘的属性和方法：  **

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697531320023-f42fd718-460a-4232-8e9a-7eaddb1191d8.png)

减少回流和重绘：

+ 使用transform替代top
+ 使用visibility替换display:none,因为前者值会引起重绘后者引发回流
+ 不要把节点的属性值放在一个循环里当成循环里的变量。
+ 不要使用 table 布局，可能很小的一个小改动会造成整个 table 的重新布局
+ 动画实现的速度的选择，动画速度越快，回流次数越多，也可以选择使用 requestAnimationFrame——`你希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行`
+ CSS 选择符从右往左匹配查找，避免节点层级过多——选择器少套
+ 将频繁重绘或者回流的节点设置为图层，图层能够阻止该节点的渲染行为影响别的节点。比如对于 video 标签来说，浏览器会自动将该节点变为图层。
+ will-change：tranform做优化（css硬件加速）

## 为什么transform替换top可以减少回流和重绘
> [为什么使用transform偏移比使用定位top性能更好？重绘、回流是什么？_Da Duang的博客-CSDN博客](https://blog.csdn.net/weixin_43996061/article/details/122977367)
>

因为top会改变和模型，而transform不会改变盒模型，top会引起回流，transform不会

# li与li之间又看不见的空白间隔是什么原因引起的？如何解决？
浏览器会把inline内联元素间的空白字符（空格，换行，tab等）渲染成一个空格，为了美观，通常是一个li放在一行，这导致li换行后产生换行字符，他变成一个空格，占用了一个字符的宽度

解决方案：

+ 为li设置folat：left；不足：有些容器不能设置浮动
+ 将所有li写在同一行：不足：不美观
+ 将ul内的字符尺寸直接设置为0，即font-size:0。不足：ul中的其他字符尺寸也被设置为0，需要额外重新设定其他字符尺寸，且在safari浏览器仍然会出现空白间隔
+ 消除ul的字符间隔letter-spacing：-8px；不足：这也设置了li内的字符间隔，因此需要将li内的字符间隔设置为默认letter-spacing：normal

# css3新属性
[CSS3 背景 | 菜鸟教程](https://www.runoob.com/css3/css3-backgrounds.html)

+ 新增各种css选择器（：not(.input)：所有class不是’input‘的节点）
+ 圆角：border-radius
+ 多列布局 multi-column layout
+ 阴影和反射 shadow  reflect
+ 文字特效 text-shadow
+ 文字渲染 text-decoration
+ 渐变 
    - 线性渐变 background-image：linear-gradient
    - 径向渐变 background-image：lradial-gradient
+ 过渡：transition：property duration timing-function delay
+ 旋转 transform
+ 增加了旋转rotate()，缩放scale()，倾斜skew()，动画，多背景
+ 2D动画

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697531632632-dde2b1a9-26a9-48ef-b8f6-9451ef553cd5.png)

+ 3D动画：想要开启3D动画，需要在父元素上开启3d立体空间

transform-style:flat 子元素不开启3d空间默认值

transform-style:preserve-3d 子元素开启立体空间

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697531673502-d625825b-6510-4f73-8eb9-4c0341319ec7.png)

+ 帧动画@keyframes

语法：

```javascript
@keyframes animationname {keyframes-selector {css-styles;}}
// animationname 定义animation的名称
// keyframes-selector 动画持续时间的百分比
// 0-100% 或者from&to
// css-styles 一个或多个合法的css样式属性
使用举例
@keyframes mymove
{
    from {top:0px;}
    to {top:200px;}
}
@keyframes mymove
{
    0% {top:0px;}
    100% {top:200px;}
}
```

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697531733246-e4d2cb36-7f90-484e-bd55-e7a9cb32cdae.png)

+ flex布局
+ 媒体查询

# 图片格式
> [(转)了解一下，各种图片格式的区别 - mukekeheart - 博客园](https://www.cnblogs.com/mukekeheart/p/9995050.html#:~:text=%E7%B4%A2%E5%BC%95%E8%89%B2%EF%BC%9A%E7%94%A8%E4%B8%80%E4%B8%AA%E6%95%B0%E5%AD%97%E6%9D%A5%E4%BB%A3%E8%A1%A8%EF%BC%88%E7%B4%A2%E5%BC%95%EF%BC%89%E4%B8%80%E7%A7%8D%E9%A2%9C%E8%89%B2%EF%BC%8C%E5%9C%A8%E5%AD%98%E5%82%A8%E5%9B%BE%E7%89%87%E7%9A%84%E6%97%B6%E5%80%99%EF%BC%8C%E5%AD%98%E5%82%A8%E4%B8%80%E4%B8%AA%E6%95%B0%E5%AD%97%E7%9A%84%E7%BB%84%E5%90%88%EF%BC%8C%E5%90%8C%E6%97%B6%E5%AD%98%E5%82%A8%E6%95%B0%E5%AD%97%E5%88%B0%E5%9B%BE%E7%89%87%E9%A2%9C%E8%89%B2%E7%9A%84%E6%98%A0%E5%B0%84%E3%80%82%20%E8%BF%99%E7%A7%8D%E6%96%B9%E5%BC%8F%E5%8F%AA%E8%83%BD%E5%AD%98%E5%82%A8%E6%9C%89%E9%99%90%E7%A7%8D%E9%A2%9C%E8%89%B2%EF%BC%8C%E9%80%9A%E5%B8%B8%E6%98%AF256%E7%A7%8D%E9%A2%9C%E8%89%B2%EF%BC%8C%E5%AF%B9%E5%BA%94%E5%88%B0%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%B3%BB%E7%BB%9F%E4%B8%AD%EF%BC%8C%E4%BD%BF%E7%94%A8%E4%B8%80%E4%B8%AA%E5%AD%97%E8%8A%82%E7%9A%84%E6%95%B0%E5%AD%97%E6%9D%A5%E7%B4%A2%E5%BC%95%E4%B8%80%E7%A7%8D%E9%A2%9C%E8%89%B2%E3%80%82%20%E7%9B%B4%E6%8E%A5%E8%89%B2%EF%BC%9A%E4%BD%BF%E7%94%A8%E5%9B%9B%E4%B8%AA%E6%95%B0%E5%AD%97%E6%9D%A5%E4%BB%A3%E8%A1%A8%E4%B8%80%E7%A7%8D%E9%A2%9C%E8%89%B2%EF%BC%8C%E8%BF%99%E5%9B%9B%E4%B8%AA%E6%95%B0%E5%AD%97%E5%88%86%E5%88%AB%E4%BB%A3%E8%A1%A8%E8%BF%99%E4%B8%AA%E9%A2%9C%E8%89%B2%E4%B8%AD%E7%BA%A2%E8%89%B2%E3%80%81%E7%BB%BF%E8%89%B2%E3%80%81%E8%93%9D%E8%89%B2%E4%BB%A5%E5%8F%8A%E9%80%8F%E6%98%8E%E5%BA%A6%E3%80%82,%E7%8E%B0%E5%9C%A8%E6%B5%81%E8%A1%8C%E7%9A%84%E6%98%BE%E7%A4%BA%E8%AE%BE%E5%A4%87%E5%8F%AF%E4%BB%A5%E5%9C%A8%E8%BF%99%E5%9B%9B%E4%B8%AA%E7%BB%B4%E5%BA%A6%E5%88%86%E5%88%AB%E6%94%AF%E6%8C%81256%E7%A7%8D%E5%8F%98%E5%8C%96%EF%BC%8C%E6%89%80%E4%BB%A5%E7%9B%B4%E6%8E%A5%E8%89%B2%E5%8F%AF%E4%BB%A5%E8%A1%A8%E7%A4%BA2%E7%9A%8432%E6%AC%A1%E6%96%B9%E7%A7%8D%E9%A2%9C%E8%89%B2%E3%80%82%20%E5%BD%93%E7%84%B6%E5%B9%B6%E9%9D%9E%E6%89%80%E6%9C%89%E7%9A%84%E7%9B%B4%E6%8E%A5%E8%89%B2%E9%83%BD%E6%94%AF%E6%8C%81%E8%BF%99%E4%B9%88%E5%A4%9A%E7%A7%8D%EF%BC%8C%E4%B8%BA%E5%8E%8B%E7%BC%A9%E7%A9%BA%E9%97%B4%E4%BD%BF%E7%94%A8%EF%BC%8C%E6%9C%89%E5%8F%AF%E8%83%BD%E5%8F%AA%E6%9C%89%E8%A1%A8%E8%BE%BE%E7%BA%A2%E3%80%81%E7%BB%BF%E3%80%81%E8%93%9D%E7%9A%84%E4%B8%89%E4%B8%AA%E6%95%B0%E5%AD%97%EF%BC%8C%E6%AF%8F%E4%B8%AA%E6%95%B0%E5%AD%97%E4%B9%9F%E5%8F%AF%E8%83%BD%E4%B8%8D%E6%94%AF%E6%8C%81256%E7%A7%8D%E5%8F%98%E5%8C%96%E4%B9%8B%E5%A4%9A%E3%80%82%20%E7%82%B9%E9%98%B5%E5%9B%BE%EF%BC%8C%E4%B9%9F%E5%8F%AB%E5%81%9A%E4%BD%8D%E5%9B%BE%EF%BC%8C%E5%83%8F%E7%B4%A0%E5%9B%BE%E3%80%82)
>
> [每个前端工程师都应该了解的图片知识(长文建议收藏)](https://mp.weixin.qq.com/s/j6V5CLeHJzU5WxysmnQUqg)
>
> 索引色：用一个数字来代表一种颜色，在存储图片的时候，存储一个数字的组合，同时存储数字到图片颜色的映射，这种方式只能存储有限种颜色，通常是256中颜色，对应到计算机系统中，使用一个字节的数字来索引一种颜色。
>
> 直接色：使用四个数字来代表一种颜色，这四个数字分别代表这个颜色中红色、绿色、蓝色以及透明度。现在流行的显示设备可以在这四个维度分别支持256种变化，所以直接色可以表示2的32次方种颜色。当然并非所有的直接色都支持这么多种，为压缩空间使用，有可能只有表达红、绿、蓝的三个数字，每个数字也可能不支持256种变化之多。
>

+ **JPG/JPEG**：有损压缩，体积小，加载快，**不支持透明 **
    1. 当我们把图片体积压缩至原有体积的 50% 以下时，JPG 仍然可以保持住 60% 的品质。
    2. JPG格式以24位存储单个图，可以呈现1600万种颜色，可以应对大多数场景对颜色的要求
    3. 在开发过程中，JPG图片经常作为**<font style="color:#DF2A3F;">大的背景图，轮播图或Banner图出现</font>**
    4. JPEG是有损的，采用直接色的点阵图，色彩丰富，适合用来存储照片，不适合用来存储企业logo，线框类的图，因为有损压缩会导致图片模糊，而直接色的选用，又会导致图片文件较GIF更大
+ **PNG-8与PNG-24**（可移植网络图形格式）：无损压缩，质量高，体积大，支持透明 
    1. 这里的8和24指的是二进制数的位数。8 位的 PNG 最多支持 256 种颜色，而 24 位的可以呈现约 1600 万种颜色。
    2. 常用于**呈现小的Logo，颜色简单并且对比强烈的图片或者背景**
    3. PNG-8是使用索引色的点阵图，是非常好的GIF格式替代者，在相同的图片效果下，png-8具有更小的文件体积，除此之外png-8还支持透明度的调节，而GIF并不支持，除非需要动画的支持，否则没有理由是用GIF而不是png-8
    4. png-24是使用直接色的点阵图，优点在于它压缩了图片的数据，似的同样效果的图片，png-24格式的文件大小要比BMP小的多，但是还是比JPEG，GIF，PNG-8大得多
+ **SVG**（可缩放矢量图形）：文本文件，体积小，不失真，兼容性好 
    1. 是一种基于XMl语法的图像格式，SVG对图像的处理不是基于像素点是基于对图像的形状描述
    2. 可以写在HTML里或者以.svg为后缀的独立文件
    3. 无损的矢量图，适合用于**绘制logo，icon**等
+ **Base64**：文本文件，依赖编码，小图标解决方案 
    1.  Base64 是一种用于传输 8Bit 字节码的编码方式，通过对图片进行 Base64 编码，我们可以直接将编码结果写入 HTML 或者写入 CSS，从而减少 HTTP 请求的次数。 
    2.  Base64编码后，图片大小会膨胀为原文件的 4/3，如果我们把大图也编码到 HTML 或 CSS 文件中，后者的体积会明显增加，即便我们减少了 HTTP 请求，也无法弥补这庞大的体积带来的性能开销，得不偿失，所以我们不使用Base64解决大图片 
    3.  使用Base64的场景： 
        *  **图片的实际尺寸很小**（大家可以观察一下掘金页面的 Base64 图，几乎没有超过 2kb 的） 
        *  **图片无法以雪碧图的形式与其它小图结合**（合成雪碧图仍是主要的减少 HTTP 请求的途径，Base64 是雪碧图的补充） 

> 雪碧图（css sprites）—— 小图标解决方案
>
> 将小图标和背景图像合并到一张图片上，然后利用 CSS 的背景定位来显示其中的每一部分的技术。Base64是作为雪碧图的补充而存在的
>
> 优点：
>
> + 利用css sprites能很好的减少网页的请求，从而大大提高了页面的性能
> + css sprites能减少图片的字节，把3张图片合并成1张图片的字节总是小于这3张图片的字节总和
>
> 缺点：
>
> + 在图片合并时，要把多张图片有序的，合理的合并成一张图片，还要留好足够的空间，防止板块内出现不必要的北京，在宽屏即高分辨率下的自适应页面，如果背景不够宽，很容易出现背景断裂
> + 对开发同学来说有点麻烦，需要借助photoshop或者其他工具来对每个北京单元测量其准确的位置
> + 精灵图维护的时候比较麻烦，页面背景有少许改动，就要改这张合并的图片，无需改的地方尽量不要动，这样避免改动更多的css，如果在原来的地方放不下，最好往下加图片，这样图片的字节就增加了，还要改动css
>

                    *  图片的更新频率非常低（不需我们重复编码和修改文件内容，维护成本较低） 
                4.  可以使用Webpack的url-loader除了具备基本的 Base64 转码能力，还可以结合文件大小，帮我们判断图片是否有必要进行 Base64 编码。 
+ **WebP**：加快图片加载速度，支持有损压缩和无损压缩。 
    1. 与 PNG 相比，WebP 无损图像的尺寸缩小了 26％。
    2. 在等效的 [SSIM ](https://zh.wikipedia.org/wiki/%E7%B5%90%E6%A7%8B%E7%9B%B8%E4%BC%BC%E6%80%A7)（结构相似性）质量指数下，WebP 有损图像比同类 JPEG 图像小 25-34％。
    3. 无损 WebP 支持透明度（也称为 alpha 通道），仅需 22％ 的额外字节。对于有损 RGB 压缩可接受的情况，有损 WebP 也支持透明度，与 PNG 相比，通常提供 3 倍的文件大小。
    4. WebP 还会增加服务器的负担：与编码JPG文件相比，编码同样质量的Webp文件会占用更多的计算资源
    5. 使用直接色的点阵图
+ **BMP**（Bitmap）：无损的，既支持索引色也支持直接色的点阵图，这种图片几乎没有对数据进行压缩，所以**BMP格式的图片经常是较大的文件**
+ **GIF**：无损的，采用索引色的点阵图，使用LZW压缩算法进行编码，文件小，是GIF格式的优点，同时，FGIF格式还具有支持动画以及透明的优点，但是GIF格式仅支持8bit的索引色，所以**GIF格式适用于对色彩要求不高同时需要文件体积较小的场景**

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699434893396-5faa0ff6-4234-4945-8a71-2565529342c8.png)

# css优化和提高性能的方法有哪些
+ 加载性能： 
    - css压缩，将写好的css进行打包压缩，可以减少文件体积
    - css单一样式：当需要下边距和左边距的时候，很多时候会选择用margin： top 0 bottom 0，但margin-bottom：bottom margin-Left：Left执行效率会更高。
    - 减少使用@import，建议使用link，因为后者在页面加载时一起加载，前者时等待页面加载完成之后再进行加载
+ 选择器性能：

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697531858450-674cd763-7a2b-4963-bea1-0720c5e42a12.png)

+ 渲染性能
    - 慎重使用高性能属性：浮动，定位
    - 尽量减少页面重排重绘
    - 去除空规则：{}。空规则的产生原因一般来说是为了与预留样式，去除这些规则无疑能减少css文档体积
    - 属性值为0时，不加单位
    - 属性值为浮动小数0.**，可以省略小数点之前的0
    - 标准化各种浏览器前缀：带浏览器前缀的在前，标准属性在后
    - 不适用@import前缀，它会影响css的加载速度
    - 选择器优化嵌套，尽量避免层级过深
    - css雪碧图
    - 正确使用display属性，由于display的作用，某些央视组合回无效，徒增样式体积的同时也影响解析性能
    - 不滥用web字体，中文网站来说，web fonts可能很陌生，国外却很流行。web Fonts通常体积庞大，而且一些浏览器在下载web fonts时会阻塞页面，渲染损伤性能。
+ 可维护性，健壮性
    - 将具有相同属性的样式抽离出来，整合并通过class在页面中进行使用，提高css的可维护性
    - 样式与内容分离，将css代码定义到外部css中

# css工程化
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697531927417-9bea7037-9299-458c-85b6-981875baa4bc.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697531960428-353bd059-a8a7-461d-84b5-66ba549569a1.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697531985592-2ef82f10-a065-4a1e-903f-c8b842e8201c.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697531995635-5a5a3844-8c1c-4fcb-abc3-28931f9ea2a8.png)

# css模块化
重要作用：

+ 防止全局污染，样式被覆盖
+ 命名混乱
+ css代码冗余，体积庞大

解决方案：

+ Css module：依赖于webpack构建和css-loader等loader处理，将css交给js来动态加载
+ 放弃css：css in js用js对象方式写css，然后作为style方式赋给React的Dom元素，这种写法将不需要 .css .less .scss 等文件，取而代之的是每一个组件都有一个写对应样式的 js 文件。

## css module
> [【工程化】深入浅出 CSS Modules - 掘金](https://juejin.cn/post/6952665769209495566)
>
> [webpack5 如何开启css Modules - 掘金](https://juejin.cn/post/7063299278273249317)
>

> css Modules ，使得项目中可以像加载 js 模块一样加载 css ，本质上通过一定自定义的命名规则生成唯一性的 css 类名，从根本上解决 css 全局污染，样式覆盖的问题。对于 css modules 的配置，推荐使用 css-loader，因为它对 CSS Modules 的支持最好，而且很容易使用
>

Css module是使用一些构建工具，比如webpack，对css类名和选择器限定作用域的一种方式

Css modules中定义的类名必须通过类似设置变量的方式给HTML设置，如果想直接使用类名，而不是编译后的哈希字符串就需要使用:global的语法，就可以声明一个全局的规则。

给每个样式都起与众不同的名字，保证不会出现重名

默认情况下，react脚手架搭建出来的项目，只有.module.css支持模块化，如果是自己搭建的话，可以支持.css文件的后缀，css module推荐的命名是驼峰式，命名规则可以通过css-loader进行配置

**解决的问题：**

+ 全局污染：需要使用权重更高的选择器进行覆盖
+ 命名混乱：随着项目越来越大，命名越来越混乱
+ 层次结构不清晰：为了避免样式名的冲突，我们写的选择器也越来越复杂，然后命名也越来越长
+ 代码难以复用：需要从大量的代码中找到自己想复用的样式
+ 代码压缩不彻底：选择器的越来越复杂和命名越来越长导致了代码的压缩也就不彻底

**class组合**

在css modules中，一个选择器可以继承另一个选择器的规则，这称为“组合”

```css
.font-red {
  color: red;
}
.App-header {
  // 我们定义一个font-end，然后在.App-header中使用 composes: font-red;继承
  composes: font-red; 
}
```

不仅可以选择同一个文件中的，还可以继承其他文件中的css规则

```css
// another.module.css
.font-blue {
  color: blue;
}
// App.module.css
.App-header {
  composes: font-blue from './another.module.css';
}
```

**使用变量**

我们还可以使用变量

```css
// colors.module.css文件
@value blue: #0c77f8;

// App.module.css文件中
@value colors: "./colors.module.css";
@value blue from colors;
.App-header {
  color: blue;
}
```

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698308918913-4aee7501-b4ec-40db-a7a4-f0a13b78819b.png)

[css module-CSDN博客](https://blog.csdn.net/xun__xing/article/details/108253723)

[滑动验证页面](https://segmentfault.com/a/1190000015738767?utm_source=sf-similar-article)

[http://getbem.com/introduction/](http://getbem.com/introduction/)

