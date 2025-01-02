官网：[https://docs.nestjs.cn/10/firststeps](https://docs.nestjs.cn/10/firststeps)

遵循[SOLID](https://zh.wikipedia.org/wiki/SOLID_(%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E8%AE%BE%E8%AE%A1))原则

# 基本使用
```javascript
src
 ├── app.controller.spec.ts // 对于基本控制器的单元测试样例
 ├── app.controller.ts // 带有单个路由的基本控制器示例。
 ├── app.module.ts // 应用程序的根模块。
 ├── app.service.ts // 带有单个方法的基本服务
 └── main.ts // 应用程序入口文件。它使用 NestFactory 用来创建 Nest 应用实例。
```

# 5种HTTP数据传输方式
+ url param
+ query
+ form-urlencoded：直接用 form 表单提交数据就是这种，它和 query 字符串的方式的区别只是放在了 body 里，然后指定下 content-type 是 `application/x-www-form-urlencoded`。因为内容也是 query 字符串，所以也要用 encodeURIComponent 的 api 或者 query-string 库处理下。这种格式也很容易理解，get 是把数据拼成 query 字符串放在 url 后面，于是表单的 post 提交方式的时候就直接用相同的方式把数据放在了 body 里。通过 & 分隔的 form-urlencoded 的方式需要对内容做 url encode，如果传递大量的数据，比如上传文件的时候就不是很合适了，因为文件 encode 一遍的话太慢了，这时候就可以用 form-data。![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1733227207861-642735e6-4f0d-4eb7-a73a-613466adf42a.png)
+ form-data：form data 不再是通过 & 分隔数据，而是用 --------- + 一串数字做为 boundary 分隔符。因为不是 url 的方式了，自然也不用再做 url encode。form-data 需要指定 content type 为 `multipart/form-data`，然后指定 boundary 也就是分割线。body 里面就是用 boundary 分隔符分割的内容。很明显，这种方式适合传输文件，而且可以传输多个文件。但是毕竟多了一些只是用来分隔的 boundary，所以请求体会增大。![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1733227297227-54735771-2d03-45e3-82d3-cac9f622e7a8.png)
+ json

# Ioc解决了什么痛点问题
Ioc：Inverse of Control。控制反转

后端系统中存在很多对象：

+ Controller 对象：接收 http 请求，调用 Service，返回响应
+ Service 对象：实现业务逻辑
+ Repository 对象：实现对数据库的增删改查

除此之外还有数据库链接对象DataSource、配置对象Config等

**Controller 依赖了 Service 实现业务逻辑，Service 依赖了 Repository 来做增删改查，Repository 依赖 DataSource 来建立连接，DataSource 又需要从 Config 对象拿到用户名密码等信息。**

从主动创建依赖到被动等待依赖注入就叫Ioc

+ **没有使用Ioc：**要经过一系列的初始化之后才可以使用 Controller 对象。在应用初始化的时候，需要理清依赖的先后关系，创建一大堆对象组合起来，还要保证不要多次 new

```javascript
const config = new Config({ username: 'xxx', password: 'xxx'});

const dataSource = new DataSource(config);
const repository = new Repository(dataSource);
const service = new Service(repository);
const controller = new Controller(service);
```

+ 使用了Ioc：它有一个放对象的容器，程序初始化的时候会扫描 class 上声明的依赖关系，然后把这些 class 都给 new 一个实例放到容器里。创建对象的时候，还会把它们依赖的对象注入进去。
+ ![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1734596870355-b78da089-4407-4701-90f9-8ca3bad16d5a.png)

# 控制器
<font style="color:rgb(77, 77, 77);">控制器的目的是接收应用的特定请求。</font>**路由**<font style="color:rgb(77, 77, 77);">机制控制哪个控制器接收哪些请求。通常，每个控制器有多个路由，不同的路由可以执行不同的操作。</font>

<font style="color:rgb(77, 77, 77);">为了创建一个基本的控制器，我们使用类和</font>`装饰器`<font style="color:rgb(77, 77, 77);">。装饰器将类与所需的元数据相关联，并使 Nest 能够创建路由映射（将请求绑定到相应的控制器）。</font>

<font style="color:rgb(77, 77, 77);">Nest 提供的装饰器及其代表的底层平台特定对象的对照列表：</font>

| `@Request()，@Req()` | `req` |
| --- | --- |
| `@Response()，@Res()` | `res` |
| `@Next()` | `next` |
| `@Session()` | `req.session` |
| `@Param(key?: string)` | `req.params`<font style="color:rgb(77, 77, 77);">/</font>`req.params[key]` |
| `@Body(key?: string)` | `req.body`<font style="color:rgb(77, 77, 77);">/</font>`req.body[key]` |
| `@Query(key?: string)` | `req.query`<font style="color:rgb(77, 77, 77);">/</font>`req.query[key]` |
| `@Headers(name?: string)` | `req.headers`<font style="color:rgb(77, 77, 77);">/</font>`req.headers[name]` |
| `@Ip()` | `req.ip` |
| `@HostParam()` | `req.hosts` |


+ 路由通配符

<font style="color:rgb(77, 77, 77);">路由同样支持模式匹配。例如，星号被用作通配符，将匹配任何字符组合。</font>

```javascript
@Get('ab*cd')
findAll() {
  return 'This route uses a wildcard';
}
```

<font style="color:rgb(77, 77, 77);">路由路径 </font>`'ab*cd'`<font style="color:rgb(77, 77, 77);"> 将匹配 </font>`abcd`<font style="color:rgb(77, 77, 77);"> 、</font>`ab_cd`<font style="color:rgb(77, 77, 77);"> 、</font>`abecd`<font style="color:rgb(77, 77, 77);"> 等。字符 </font>`?`<font style="color:rgb(77, 77, 77);"> 、</font>`+`<font style="color:rgb(77, 77, 77);"> 、 </font>`*`<font style="color:rgb(77, 77, 77);"> 以及 </font>`()`<font style="color:rgb(77, 77, 77);"> 是它们的正则表达式对应项的子集。连字符（</font>`-`<font style="color:rgb(77, 77, 77);">） 和点（</font>`.`<font style="color:rgb(77, 77, 77);">）按字符串路径逐字解析。</font>

+ <font style="color:rgb(77, 77, 77);">状态码</font>

@HttpCode需要从@nestjs/common包导入

```javascript
@Post()
@HttpCode(204)
create() {
  return 'This action adds a new cat';
}
```

> <font style="color:rgb(77, 77, 77);">通常，状态码不是固定的，而是取决于各种因素。在这种情况下，您可以使用类库特有（library-specific）的 </font>`**response**`<font style="color:rgb(77, 77, 77);"> （通过</font>` @Res()`<font style="color:rgb(77, 77, 77);">注入 ）对象（或者在出现错误时，抛出异常）。</font>
>

+ <font style="color:rgb(77, 77, 77);">Headers</font>

@Header需要从@nestjs/common包

使用@header()装饰器或<font style="color:rgb(77, 77, 77);">类库特有的响应对象，（并直接调用 </font>`res.header()`<font style="color:rgb(77, 77, 77);">）。</font>

```javascript
@Post()
@Header('Cache-Control', 'none')
create() {
  return 'This action adds a new cat';
}
```

+ 重定向：@Redirect()装饰器有两个可选参数，url和statusCode。如果省略，则statusCode默认为302

```javascript
@Get()
@Redirect('https://nestjs.com', 301)
```

<font style="color:rgb(77, 77, 77);">有时您可能想动态地决定 HTTP 状态代码或重定向 URL。通过从路由处理方法返回一个如下格式的对象：</font>

```javascript
{
  "url": string,
  "statusCode": number
}
```

返回的值将覆盖传递给`@Redirect()`<font style="color:rgb(77, 77, 77);">装饰器的所有参数。 例如：</font>

```javascript
@Get('docs')
@Redirect('https://docs.nestjs.com', 302)
getDocs(@Query('version') version) {
  if (version && version === '5') {
    return { url: 'https://docs.nestjs.com/v5/' };
  }
}

```

+ 路由参数

```javascript
@Get(':id')
findOne(@Param() params): string {
  console.log(params.id);
  return `This action returns a #${params.id} cat`;
}

```

+ <font style="background-color:#F1A2AB;">子域路由</font>

`<font style="background-color:#F1A2AB;">@Controller</font>`<font style="color:rgb(77, 77, 77);background-color:#F1A2AB;"> 装饰器可以接受一个 </font>`<font style="background-color:#F1A2AB;">host</font>`<font style="color:rgb(77, 77, 77);background-color:#F1A2AB;"> 选项，以要求传入请求的 </font>`<font style="background-color:#F1A2AB;">HTTP</font>`<font style="color:rgb(77, 77, 77);background-color:#F1A2AB;"> 主机匹配某个特定值。</font>

```javascript
@Controller({ host: 'admin.example.com' })
export class AdminController {
  @Get()
  index(): string {
    return 'Admin page';
  }
}

```

与一个路由路径path类似，该hosts选项可以使用参数表述token来捕获主机名中该位置的动态之可以使用@HostParam()装饰器访问以这种方式声明的主机参数

```javascript
@Controller({ host: ':account.example.com' })
export class AccountController {
  @Get()
  getInfo(@HostParam('account') account: string) {
    return account;
  }

```

+ 异步性：Nest完美支持异步函数

```javascript
@Get()
async findAll(): Promise<any[]> {
  return [];
}

```

+ 请求负载：通过添加@Body参数来解决![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1735464521722-72bffeb5-f422-41c6-9a01-dba39bd69995.png)

# Providers
Providers都可以通过constructor注入依赖关系。Provider只是一个用@Injectable()装饰器注释的类。

## 服务


