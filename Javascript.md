# ES6一些结构
## <font style="color:rgb(27, 27, 27);">set&weakset</font>
### <font style="color:rgb(27, 27, 27);">set</font>
+ <font style="color:rgb(27, 27, 27);">类似于数组，但是元素不能重复，向set加入值时，不会发生类型转换，5和“5”算是不重复的两个值</font>
+ <font style="color:rgb(27, 27, 27);">创建set需要通过set构造函数</font>
+ <font style="color:rgb(27, 27, 27);">size属性可用于返回set中元素的个数</font>
+ `**Set**`**的遍历顺序就是插入顺序**
+ <font style="color:rgb(27, 27, 27);">常用方法 </font>
    - <font style="color:rgb(27, 27, 27);">add(value)：添加某个元素，返回set对象本身</font>
    - <font style="color:rgb(27, 27, 27);">delete(value)：从set中删除和这个值相等的与元素，返回boolean类型</font>
    - <font style="color:rgb(27, 27, 27);">has(value)：判断set中是否存在某个元素，返回boolean类型</font>
    - <font style="color:rgb(27, 27, 27);">clear()：清空set中所有元素，没有返回值</font>
    - <font style="color:rgb(27, 27, 27);">keys()：返回键名的遍历器</font>
    - <font style="color:rgb(27, 27, 27);">values()：返回键值的遍历器</font>
    - <font style="color:rgb(27, 27, 27);">entries：返回键值对的遍历器</font>
    - <font style="color:rgb(27, 27, 27);">forEach(callback,[,thisArg])：同forEach遍历set</font>
    - <font style="color:rgb(27, 27, 27);">支持for...of</font>

### <font style="color:rgb(27, 27, 27);">weakset</font>
+ <font style="color:rgb(27, 27, 27);"> </font>**只能存放对象类型**<font style="color:rgb(27, 27, 27);">和symbol值，不能存放其他类型 </font>
+ <font style="color:rgb(27, 27, 27);"> 对元素的引用是弱引用，垃圾回收的时候不可会考虑weakset的引用，gc会回收，所以WeakSet 适合临时存放一组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，它在 WeakSet 里面的引用就会自动消失。 </font>
+ <font style="color:rgb(27, 27, 27);"> weakset不能遍历：WeakSet 内部有多少个成员，取决于垃圾回收机制有没有运行，运行前后很可能成员个数是不一样的，而垃圾回收机制何时运行是不可预测的，因此 ES6 规定 WeakSet 不可遍历。 </font>
+ <font style="color:rgb(27, 27, 27);"> 当传入一个数组时，数组中的所有成员会成为weakset的成员而不是元素本身 </font>

```javascript
const a = [[1, 2], [3, 4]];
const ws = new WeakSet(a);
// WeakSet {[1, 2], [3, 4]}
// 因为b中的元素不是对象
const b = [3, 4];
const ws = new WeakSet(b);
// Uncaught TypeError: Invalid value used in weak set(…)
```

+ <font style="color:rgb(27, 27, 27);"> 常用方法 </font>
    - <font style="color:rgb(27, 27, 27);">add(value)：添加某个元素，返回weakset对象本身</font>
    - <font style="color:rgb(27, 27, 27);">delete(value)：从weakset中删除和这个值相等的与元素，返回boolean类型</font>
    - <font style="color:rgb(27, 27, 27);">has(value)：判断weakset中是否存在某个元素，返回boolean类型</font>
    - <font style="color:rgb(27, 27, 27);">不能遍历</font>
+ <font style="color:rgb(27, 27, 27);"> 应用场景：不能使用非构造方法创建出来的对象调用running方法 </font>

## <font style="color:rgb(27, 27, 27);">map&weakmap</font>
### <font style="color:rgb(27, 27, 27);">map</font>
+ <font style="color:rgb(27, 27, 27);"> 用于存储映射关系，对象存储映射关系只能用</font>**字符串**<font style="color:rgb(27, 27, 27);">作为属性名，</font>**map可以使用其他类型作为属性名**<font style="color:rgb(27, 27, 27);"> </font>
+ <font style="color:rgb(27, 27, 27);"> size属性可用于返回map中元素的个数 </font>
+ <font style="color:rgb(27, 27, 27);"> </font>`<font style="color:rgb(27, 27, 27);">Map</font>`<font style="color:rgb(27, 27, 27);">构造函数接受数组作为参数，实际上执行的是下面的算法。  
</font>**任何具有 Iterator 接口、且每个成员都是一个双元素的数组的数据结构都可以当作**`**Map**`**构造函数的参数**<font style="color:rgb(27, 27, 27);"> </font>

```javascript
const items = [
  ['name', '张三'],
  ['title', 'Author']
];

const map = new Map();

items.forEach(
  ([key, value]) => map.set(key, value)
);
```

+ <font style="color:rgb(27, 27, 27);"> 常见方法 </font>
    - <font style="color:rgb(27, 27, 27);"> size属性：返回 Map 结构的成员总数 </font>
    - <font style="color:rgb(27, 27, 27);"> Map.prototype.get(key)：读取</font>`<font style="color:rgb(27, 27, 27);">key</font>`<font style="color:rgb(27, 27, 27);">对应的键值，如果找不到</font>`<font style="color:rgb(27, 27, 27);">key</font>`<font style="color:rgb(27, 27, 27);">，返回</font>`<font style="color:rgb(27, 27, 27);">undefined</font>`<font style="color:rgb(27, 27, 27);"> </font>
    - <font style="color:rgb(27, 27, 27);"> Map.prototype.set(key, value)：</font>`<font style="color:rgb(27, 27, 27);">set</font>`<font style="color:rgb(27, 27, 27);">方法设置键名</font>`<font style="color:rgb(27, 27, 27);">key</font>`<font style="color:rgb(27, 27, 27);">对应的键值为</font>`<font style="color:rgb(27, 27, 27);">value</font>`<font style="color:rgb(27, 27, 27);">，然后返回整个 Map 结构。如果</font>`<font style="color:rgb(27, 27, 27);">key</font>`<font style="color:rgb(27, 27, 27);">已经有值，则键值会被更新，否则就新生成该键。 </font>
    - <font style="color:rgb(27, 27, 27);"> Map.prototype.delete(key)：从map中删除和这个值相等的与元素，返回boolean类型 </font>
    - <font style="color:rgb(27, 27, 27);"> Map.prototype.has(key)：判断map中是否存在某个元素，返回boolean类型 </font>
    - <font style="color:rgb(27, 27, 27);"> Map.prototype.clear()：清空map中所有元素，没有返回值 </font>
    - <font style="color:rgb(27, 27, 27);"> Map.prototype.keys()：返回键名的遍历器 </font>
    - <font style="color:rgb(27, 27, 27);"> Map.prototype.values()：返回键值的遍历器 </font>
    - <font style="color:rgb(27, 27, 27);"> Map.prototype.entries()：返回所有成员的遍历器 </font>
    - <font style="color:rgb(27, 27, 27);"> Map.prototype.forEach(callback,[,thisArg])：遍历 Map 的所有成员 </font>
    - <font style="color:rgb(27, 27, 27);"> 支持for...of </font>
+ <font style="color:rgb(27, 27, 27);"> Map与其他类型的相互转换 </font>
    - <font style="color:rgb(27, 27, 27);"> Map转为数组 </font>`<font style="color:rgb(27, 27, 27);">[...myMap]</font>`<font style="color:rgb(27, 27, 27);"> </font>
    - <font style="color:rgb(27, 27, 27);"> 数组转为Map </font>

```javascript
new Map([
  [true, 7],
  [{foo: 3}, ['abc']]
])
```

    - <font style="color:rgb(27, 27, 27);"> Map转为对象  
</font><font style="color:rgb(27, 27, 27);">如果所有 Map 的键都是字符串，它可以无损地转为对象。  
</font><font style="color:rgb(27, 27, 27);">如果有非字符串的键名，那么这个键名会被转成字符串，再作为对象的键名。 </font>

```javascript
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

    - <font style="color:rgb(27, 27, 27);"> 对象转为Map </font>
        * <font style="color:rgb(27, 27, 27);"> 使用Object.entries() </font>

```javascript
let map = new Map(Object.entries(obj));
```

        * <font style="color:rgb(27, 27, 27);"> 实现一个转换函数 </font>

```javascript
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

objToStrMap({yes: true, no: false})
// Map {"yes" => true, "no" => false}
```

    - <font style="color:rgb(27, 27, 27);"> Map转为Json </font>

```javascript
// 第一种情况：Map 的键名都是字符串，这时可以选择转为对象 JSON
function strMapToJson(strMap) {
  return JSON.stringify(strMapToObj(strMap));
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToJson(myMap)
// '{"yes":true,"no":false}'

// 第二种情况：Map 的键名有非字符串，这时可以选择转为数组 JSON
function mapToArrayJson(map) {
  return JSON.stringify([...map]);
}

let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
mapToArrayJson(myMap)
// '[[true,7],[{"foo":3},["abc"]]]'
```

    - <font style="color:rgb(27, 27, 27);"> Json转为map </font>

### <font style="color:rgb(27, 27, 27);">weakmap</font>
+ **key只能使用对象（null除外）和Symbol值，不接受其他类型作为key**
+ <font style="color:rgb(27, 27, 27);">对元素的引用是弱引用，gc会回收</font>
+ <font style="color:rgb(27, 27, 27);">常见方法： </font>
    - <font style="color:rgb(27, 27, 27);">set(key,value)：在map中添加key，value，并且返回整个map对象</font>
    - <font style="color:rgb(27, 27, 27);">get(key)：根据key获取map中的value</font>
    - <font style="color:rgb(27, 27, 27);">has(key)：判断是否包括某一个key，返回boolean类型</font>
    - <font style="color:rgb(27, 27, 27);">delete(key)：根据key删除一个键值对，返回boolean类型</font>
+ <font style="color:rgb(27, 27, 27);">应用场景： </font>
    - <font style="color:rgb(27, 27, 27);">在网页的 DOM 元素上添加数据，就可以使用</font>`<font style="color:rgb(27, 27, 27);">WeakMap</font>`<font style="color:rgb(27, 27, 27);">结构。当该 DOM 元素被清除，其所对应的</font>`<font style="color:rgb(27, 27, 27);">WeakMap</font>`<font style="color:rgb(27, 27, 27);">记录就会自动被移除</font>
    - ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695305350298-034353a3-f5a7-4c13-9e6f-0d274aaf53e8.png)

### <font style="color:rgb(27, 27, 27);">weakRef：直接创建对象的弱引用</font>
+ <font style="color:rgb(27, 27, 27);">使用方法</font>

```javascript
let target = {};
let wr = new WeakRef(target);
```

+ <font style="color:rgb(27, 27, 27);"> 常用方法： </font>
    - <font style="color:rgb(27, 27, 27);"> deref()：如果原始对象存在，该方法返回原始对象；如果原始对象已经被垃圾回收机制清除，该方法返回</font>`<font style="color:rgb(27, 27, 27);">undefined</font>`<font style="color:rgb(27, 27, 27);"> </font>

```javascript
let target = {};
let wr = new WeakRef(target);

let obj = wr.deref();
if (obj) { // target 未被垃圾回收机制清除
  // ...
}
```

## Symbol
+ 原始数据类型
+ 通过symbol函数生成，可以作为属性名
+ 不可以使用new命令
+ 接受一个字符串作为参数，表示对symbol实例的描述；如果参数是对象则通过tostring将其转为字符串；相同参数的symbol函数的返回值是不相等的
+ 不能与其他类型的值进行运算，可以显示转换为字符串，也可以转换为布尔值，但是不能转为数值
+ 可以通过descriptioin直接返回symbol的描述——**ES10 **  Symbol([description])
+ 由于每一个 Symbol 值都是不相等的，这意味着 Symbol 值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性。这对于一个对象由多个模块构成的情况非常有用，能防止某一个键被不小心改写或覆盖。
+ 不能使用点运算符
+ 还可以使用定义一组常量，保证这组常量的值是不相等的
+ symol作为属性名，**遍历对象的时候不会出现在for...in和for...of循环中**，也不会被Object.keys(),Object.getOwnPropertySymbols()，JSON.stringify()返回，但是他也不是私有属性，**Object.getOwnPropertySymbols()**可以获取对象的所有Symbol属性名，该方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。
+ Reflect.ownKeys()也可以返回所有类型的键名，包括常规键名和symbol键名
+ **Symbol.for()——接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建一个以该字符串为名称的 Symbol 值，并将其注册到全局。**

## Proxy——代理器
[ES6 入门教程](https://es6.ruanyifeng.com/#docs/proxy)

1. Proxy用于修改某些操作的默认行为，可以理解成在目标对象之前架设一层拦截，可以对外界的访问进行过滤和改写
2. 生成Proxy实例的方法

```javascript
let proxy = new Proxy(target, handler);
```

+ target：表示所要拦截的目标对象
+ handler：也是一个对象，用来定制拦截行为。如果handler没有设置任何拦截，那就等同于直接通向原对象
3. 支持的拦截操作

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695470850778-a25638e1-33fc-49b1-9d78-1e278dc21065.png)

4. Proxy.revocable()：返回一个可取消的Proxy实例对象，使用场景为目标对象不允许直接访问，必须通过代理访问，一旦访问结束，就收回代理权，不允许再次访问
5. this问题
+ 虽然 Proxy 可以代理针对目标对象的访问，但它不是目标对象的透明代理，即不做任何拦截的情况下，也无法保证与目标对象的行为一致。**主要原因就是在 Proxy 代理的情况下，目标对象内部的****<font style="background-color:rgb(249, 242, 244);">this</font>****关键字会指向 Proxy 代理**
+ Proxy拦截函数内部的this指向的是handler对象

## Reflect
**设计目的**

1. 将Object对象的一些明显属于语言内部的方法放到Reflect对向上，现阶段，某些方法同时在Object和Reflect对象上部署，未来的新方法将只部署在Reflect对象上。也就是说，**从Reflect对象上可以拿到语言内部的方法**。
2. 修改某些Object方法的返回结果，让其变得合理。比如，Object.defineProperty(obj, name, desc)在无法定义属性时，会抛出一个错误，而Reflect.defineProperty(obj, name, desc)则会返回false。
3. 让Object操作都变成函数行为。某些Object操作是命令式，比如name in obj和delete obj[name]，而Reflect.has(obj, name)和Reflect.deleteProperty(obj, name)让它们变成了函数行为。
4. Reflect对象的方法与Proxy对象的方法一一对应，只要是Proxy对象的方法，就能在Reflect对象上找到对应的方法，这就让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。也就是说，不管Proxy怎么修改默认行为，你总可以在Reflect上获取默认行为。

## Promise
> [【建议星星】要就来45道Promise面试题一次爽到底(1.1w字用心整理) - 掘金](https://juejin.cn/post/6844904077537574919)
>
> [一篇文章解决Promise...then，async/await执行顺序类型题 - 掘金](https://juejin.cn/post/6973817105728667678)
>

是异步编程的一种解决方案，是一个对象，从它可以获取异步操作的消息。有了promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层回调嵌套的回调函数

```javascript
const promise = new Primise(function(resolve,reject){
   // some code
    if(异步操作成功){
        resolve(value);
    }else{
        reject(error)
    }
})
promise.then(function(value){
    //success
},function(error){
    //failure
})
```

### Promise对象的特点：
1. 对象的状态不受外界影响。Promise对象的三种状态：pending、fulfilled、rejected
2. 状态一旦改变，就不会再变，任何时候都可以得到这个结果。从pending变为fulfilled和从pending变为rejected两种可能。

### promise的缺点：
3. <font style="color:rgb(51, 51, 51);">无法取消promise，一旦新建立即执行，无法中途取消</font>
4. <font style="color:rgb(51, 51, 51);">如果不设置回调函数，promise内部抛出的错误，不会反映到外部</font>
5. <font style="color:rgb(51, 51, 51);">当处于pending状态时，无法知道当前进展到哪一阶段。是刚刚开始还是即将完成</font>

### <font style="color:rgb(51, 51, 51);">方法</font>
#### Promise.prototype.then()
then方法返回的是一个新的Promise实例，可以采用链式调用

一般来说不要再then()方法里面定义Reject状态的回调函数，即then方法的第二个参数，总是使用catch方法

#### Promise.prototype.catch()		
是.then(null, rejection)或.then(undefined, rejection)的别名，用于指定发生错误时的回调函数。

promise抛出一个错误，就被catch()方法指定的回调函数捕获。reject()方法的作用，等同于抛出错误

promise对象的错误具有冒泡性质，会一直向后传递，直到被捕获为止，也就是说，错误总是会被写一个catch语句捕获。

**<font style="color:rgb(51, 51, 51);">跟传统的</font>****<font style="color:rgb(51, 51, 51);background-color:rgb(243, 244, 244);">try/catch</font>****<font style="color:rgb(51, 51, 51);">代码块不同的是，如果没有使用</font>****<font style="color:rgb(51, 51, 51);background-color:rgb(243, 244, 244);">catch()</font>****<font style="color:rgb(51, 51, 51);">方法指定错误处理的回调函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应</font>**

#### Promise.prototype.finally()
finally方法用于指定不管promise对象最后状态如何，都会执行的操作，**不依赖于Promise的执行结果**

#### Promise.all()
```javascript
const p = Promise.all([p1, p2, p3]);
```

用于将多个Promise实例**包装成一个新的Promise实例**，<font style="color:rgb(51, 51, 51);">Promise.all()方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。如果不是，就会先调用下面讲到的Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。</font>

+ **<font style="color:rgb(51, 51, 51);">只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1,p2,p3的返回值组成一个数组，传递给p的回调函数</font>**
+ **<font style="color:rgb(51, 51, 51);">只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数</font>**

#### Promise.race()
```javascript
const p = Promise.race([p1, p2, p3]);
```

将多个Promise实例，包装成一个新的Promise实例，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的Promise实例的返回值，就传递给p的回调函数

#### Promise.allSettled()
用来确定一组异步操作是否都结束了，不管成功或失败。该方法返回一个新的Promise对象，只有等到参数数组的所有Promise对象都发生状态变更，返回的Promise对象才会发生状态变更，状态总是fulfilled，不会变成rejected。

状态变成fulfilled后，他的回调函数会接收到一个数组作为参数，该数组的每个成员对应前面数组的每个Promise对象

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1696416282264-584107a5-df0c-480c-86a2-37d1caee497d.png)

#### Promise.any()
<font style="color:rgb(13, 20, 30);">该方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例返回。</font>

**<font style="color:rgb(13, 20, 30);">只要参数实例有一个变成fulfilled状态，包装实例就会变成fulfilled状态；如果所有的promise实例变成rejected状态才会结束。</font>**

#### <font style="color:rgb(13, 20, 30);">Promise.resolve()</font>
将现有对象转为Promise对象

+ 参数是一个Promise实例：如果参数是 Promise 实例，那么Promise.resolve将不做任何修改、原封不动地返回这个实例。
+ 参数是一个thenable对象：thenable对象指的是具有then方法的对象。Promise.resolve()方法会将这个对象转为 Promise 对象，然后就立即执行thenable对象的then()方法。
+ 参数不是具有then()方法的对象，或根本就不是对象：如果参数是一个原始值，或者是一个不具有then()方法的对象，则Promise.resolve()方法返回一个新的 Promise 对象，状态为resolved。
+ 不带有任何参数：Promise.resolve()方法允许调用时不带参数，直接返回一个resolved状态的 Promise 对象。立即resolve()的 Promise 对象，是在本轮“事件循环”（event loop）的结束时执行，而不是在下一轮“事件循环”的开始时

#### Promise.reject()
返回一个新的Promise实例，该实例的状态为rejected

## Class
1. class中的constructor对应的是构造函数，this关键字代表实例对象；class底层其实还是构造函数，class内部的方法都是不可枚举的。class类必须使用new进行调用，否则会报错
2. class中的方法都等同于定义在Class类的Prototype上，[Object.assign()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)可以很方便的一次向类添加多个方法

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1696417947859-107296d6-3951-468b-af22-456e4039e25a.png)

3. <font style="color:rgb(13, 20, 30);">类的内部所有定义的方法，都是不可枚举的</font>
4. constructor()方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。一个类必须有constructor()方法，如果没有显式定义，一个空的constructor()方法会被默认添加。constructor()方法默认返回实例对象（即this），完全可以指定返回另外一个对象。
5. ES2022中实例属性现在除了可以定义在constructor()方法里面的this上面，也可以定义在类内部的最顶层。此时实例属性前面不需要加上this

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1696418515568-5ec78082-6821-499b-ba3b-74463d9f052b.png)![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1696418523962-17d110b5-5010-4f1b-aa9d-d101fdfeb7df.png)

6. 静态方法：如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。静态方法中this指向类而不是实例。静态方法也可以从super对象上调用，父类的静态方法子类可以调用
7. 私有属性：ES2022正式为class添加了私有属性，方法是在属性名之前使用#表示，<font style="color:rgb(51, 51, 51);">只能在类内部使用（ES2022），之前的区分方式：命名时前缀为下划线 /call改变this指向/使用Symbol，子类无法继承父类的私有属性</font>
8. <font style="color:rgb(51, 51, 51);">静态块：允许在类的内部设置一个代码块，在类生成时运行且只运行一次，主要作用是对静态属性进行初始化，以后，新建类的实例时，这个块就不运行了，静态块的内部不能有return语句</font>
9. **<font style="color:rgb(51, 51, 51);">js引擎在解析子类的时候有要求，如果我们有实现继承，那么子类的构造方法中，在使用this之前需要使用super（）或者在return之前调用super()——调用父类的constructor</font>**

> 子类必须在constructor()方法中调用super()，否则就会报错。这是因为子类自己的this对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，添加子类自己的实例属性和方法。如果不调用super()方法，子类就得不到自己的this对象
>
> 为什么子类的构造函数，一定要调用super()？原因就在于 ES6 的继承机制，与 ES5 完全不同。ES5 的继承机制，是先创造一个独立的子类的实例对象，然后再将父类的方法添加到这个对象上面，即“实例在前，继承在后”。ES6 的继承机制，则是先将父类的属性和方法，加到一个空的对象上面，然后再将该对象作为子类的实例，即“继承在前，实例在后”。这就是为什么 ES6 的继承必须先调用super()方法，因为这一步会生成一个继承父类的this对象，没有这一步就无法继承父类
>

10. <font style="color:rgb(51, 51, 51);">类的注意点：</font>
    - 严格模式：类和模块的内部，默认就是严格模式，所以不需要使用use strict指定运行模式
    - 不存在变量提升，必须保证子类在父类之后定义
    - 如果某个方法之前加上*号，就表示该方法是一个Generator函数
    - 类的方法内部如果含有this，它默认指向类的实例。但是，必须非常小心，一旦单独使用该方法，很可能报错。

> 解决办法：
>
> + 在构造方法中绑定this，这样就不会找不到print方法了
> + 是用箭头函数
> + 使用proxy
>

    - new.target：该属性一般用在构造函数之中，返回new命令作用于的那个构造函数，如果构造函数不是通过new命令或Reflect.construct（）调用的，new.target会返回undefined

## iterator和for...of循环
1. 任何<font style="color:rgb(13, 20, 30);">数据结构只要部署 Iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。</font>
2. <font style="color:rgb(13, 20, 30);">作用：</font>
    1. <font style="color:#000000;">为各种数据结构，提供一个统一的、简便的访问接口</font>
    2. <font style="color:#000000;">使得数据结构的成员能够按某种次序排列</font>
    3. **<font style="color:#000000;">ES6 创造了一种新的遍历命令for...of循环，Iterator 接口主要供for...of消费</font>**
3. <font style="color:#000000;">遍历过程：</font>

> 每一次调用指针对象的next方法，都会返回数据结构的当前成员信息。即一个包含value（当前成员的值）和done（表示遍历是否结束的布尔值）两个属性的对象
>

    1. <font style="color:#000000;">创建一个指针对象，指向当前数据结构的起始位置，也就是说，就是一个指针对象</font>
    2. <font style="color:#000000;">第一次调用指针对象的next方法，可以将指针指向数据结构的第一个成员</font>
    3. <font style="color:#000000;">第二次调用指针对象的next方法，指针就指向数据结构的第二个成员</font>
    4. <font style="color:#000000;">不断调用指针对象的next方法，直到它指向数据结构的结束位置</font>
4. <font style="color:#000000;">ES6规定，默认的Iterator接口部署在数据结构的Symbol.iterator属性，或者说一个数据结构只要具有Symbol.iterator属性，就可以认为是可遍历的。Symbol.iterator属性本身就是一个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器。至于属性名Symbol.iterator，它是一个表达式，返回Symbol对象的iterator属性，这是一个预定义好的、类型为 Symbol 的特殊值，所以要放在方括号内</font>

```javascript
const obj = {
  [Symbol.iterator] : function () {
    return {
      next: function () {
        return {
          value: 1,
          done: true
        };
      }
    };
  }
};
```

5. 原生具备Iterator接口的数据结构如下：Array、Map、Set、String、TypedArray、函数的arguments对象、NodeList对象。除了这些数据结构的Iterator接口，都需要自己在Symbol。iterator属性上面部署，这样才会被for...of循环遍历

```javascript
var iterableObj = {
  data: [1, 2, 3],
  [Symbol.iterator]: function (){
    let self = this;
    let index = 0;
    return {
      next: {
        if(index < self.data.length){
          return {
            value: self.data[index++],
            done: false,
          }
        }
        return { value: undefined, done: true };
      }
    }
  }
}
for(let item of iterableObj){
  console.log(item); // 1, 2, 3
}
```

6. 调用Iterator接口的场合
    1. 解构赋值：<font style="color:#000000;">对数组和 Set 结构进行解构赋值时，会默认调用Symbol.iterator方法</font>
    2. <font style="color:#000000;">扩展运算符</font>
    3. <font style="color:#000000;">yield*</font>
    4. <font style="color:#000000;">任何接受数组作为参数的场合：</font>![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1696773911053-489f9775-fce3-49f1-a4a4-4cb1ae1c244f.png)
7. 字符串是一个类似数组的对象，也原生具有Iterator接口
8. 遍历器对象除了具有next方法，还可以具有return方法和throw方法，return和throw方法可选
    - <font style="color:#000000;">return()方法的使用场合是，如果for...of循环提前退出（通常是因为出错，或者有break语句），就会调用return()方法。如果一个对象在完成遍历前，需要清理或释放资源，就可以部署return()方法</font>
    - <font style="color:#000000;">throw()方法主要是配合 Generator 函数使用，一般的遍历器对象用不到这个方法</font>
9. 与其他遍历语法的比较
    1. for循环：最原始的写法，比较麻烦
    2. forEach：问题在于无法中途跳出forEach循环，break和return语句都不奏效
    3. for...in循环可以遍历数组的键名，主要是为遍历对象设计的，不适用于遍历数组

缺点：

        * 数组的键名是数字，但是for...in是以字符串作为键名"0", "1"等
        * <font style="color:#000000;">for...in循环不仅遍历数字键名，还会遍历手动添加的其他键，甚至包括原型链上的键</font>
        * <font style="color:#000000;">某些情况下，for...in循环会以任意顺序遍历键名</font>
    4. <font style="color:#000000;">for...of循环：</font>

优点： 

        * 语法和for...in一样简洁，没有for...in的那些缺点
        * 不同于forEach，可以与break和return结合使用
        * 提供了遍历所有数据结构的统一操作接口

## Generator
1. <font style="color:#000000;">Generator 函数是 ES6 提供的一种异步编程解决方案。可以理解为一个状态机，封装了多个内部状态，执行generator函数会返回一个遍历器对象，该对象可以依次遍历generator函数内部的每一个状态。</font>
2. <font style="color:#000000;">形式上，Generator 函数是一个普通函数，但是有两个特征。一是，function关键字与函数名之间有一个星号；二是，函数体内部使用yield表达式，定义不同的内部状态（yield在英语里的意思就是“产出”）。</font>

```javascript
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}
var hw = helloWorldGenerator();
hw.next()
// { value: 'hello', done: false }
hw.next()
// { value: 'world', done: false }
hw.next()
// { value: 'ending', done: true }
hw.next()
// { value: undefined, done: true }
```

3. yield表达式就是一个暂停标志，只有调用next方法时，内部的指针指向该语句时才会执行，相当于提供了手动的惰性求值的语法功能
    1. <font style="color:#000000;">遇到yield表达式，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。</font>
    2. <font style="color:#000000;">下一次调用</font><font style="color:#000000;">next</font><font style="color:#000000;">方法时，再继续往下执行，直到遇到下一个</font><font style="color:#000000;">yield</font><font style="color:#000000;">表达式。</font>
    3. <font style="color:#000000;">如果没有再遇到新的</font><font style="color:#000000;">yield</font><font style="color:#000000;">表达式，就一直运行到函数结束，直到</font><font style="color:#000000;">return</font><font style="color:#000000;">语句为止，并将</font><font style="color:#000000;">return</font><font style="color:#000000;">语句后面的表达式的值，作为返回的对象的</font><font style="color:#000000;">value</font><font style="color:#000000;">属性值。</font>
    4. <font style="color:#000000;">如果该函数没有return语句，则返回的对象的value属性值为undefined。</font>
4. <font style="color:#000000;">generator函数可以不用yield表达式，这是就变成了以恶个暂缓执行函数</font>![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1696841574634-757c3618-901c-4a9f-b360-1a069716e215.png)
5. yield表达式如果用在另一个表达式之中，必须放在<font style="color:#000000;">圆括号里。yield表达式用作函数参数或放在赋值表达式的右边，可以不加括号。</font>
6. <font style="color:#000000;">由于 Generator 函数就是遍历器生成函数，因此可以把 Generator 赋值给对象的Symbol.iterator属性，从而使得该对象具有 Iterator 接口。下面代码中，Generator 函数赋值给Symbol.iterator属性，从而使得myIterable对象具有了 Iterator 接口，可以被...运算符遍历了。</font>

```javascript
var myIterable = {};
myIterable[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};
[...myIterable] // [1, 2, 3]
```

7. next方法可以带一个参数，该参数会被当做上一个yield表达式的返回值，yield表达式本身没有返回值，或者说总是返回undefiend。
8. <font style="color:#000000;">for...of循环可以自动遍历 Generator 函数运行时生成的Iterator对象，且此时不再需要调用next方法。</font>![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1696843423999-83e8c754-0261-4da7-93c4-4d3d0ac3cc55.png)
9. thunk函数：编译器的传名调用实现，往往是将参数放到一个临时函数之中，再将这个临时函数传入函数体，这个临时函数就是Thunk函数![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1696860779966-77066b5e-bb95-46e9-9d6e-d29ea30a3d7f.png)
10. co模块：用于generator函数的自动执行，co函数返回一个Promise对象，因此可以用then方法添加回调函数。

> <font style="color:#000000;">Generator 就是一个异步操作的容器。它的自动执行需要一种机制，当异步操作有了结果，能够自动交回执行权</font>
>
> ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1696861187876-8bca180b-6018-4ccd-ada4-e1e1cd49817a.png)
>

## async函数
1. async函数就是generator函数的语法糖，async函数将generator函数的星号*替换成async，将yield替换成await
2. 较generator函数的改进：
    1. 内置执行器：generator函数的执行必须靠执行器，所以才有了co模块，而async函数自带执行器
    2. 更好的语义：<font style="color:#000000;">async和await，比起星号和yield，语义更清楚了。async表示函数里有异步操作，await表示紧跟在后面的表达式需要等待结果。</font>
    3. <font style="color:#000000;">更广的适用性：co模块约定，yield命令后面只能是 Thunk 函数或 Promise 对象，而async函数的await命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）。</font>
    4. <font style="color:#000000;">返回值是promise：async函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator 对象方便多了。你可以用then方法指定下一步的操作。async函数内部return语句返回的值，会成为then方法回调函数的参数</font>
3. <font style="color:#000000;">async函数返回的Promise对象，必须等到内部所有await命令后面的Promise对象执行完，才会发生状态改变，除非遇到return语句或者抛出错误。也就是说，只有async函数内部的异步操作执行完，才会执行then方法指定的回调函数。</font>
4. <font style="color:#000000;">任何一个await语句后面的 Promise 对象变为reject状态，那么整个async函数都会中断执行。</font>![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1696946037299-4db061ff-d95b-4ee9-b8da-28e3405e4772.png)

# 数据类型
## 基本数据类型和引用数据类型——共8种
1. **基本数据类型：undefined，null（空对象指针），boolean，string，number，symbol，bigint**

> symbol代表创建后独一无二且不可变的数据类型，它主要是为了解决可能出现的全局变量冲突的问题 
>
> bigint是一种数据类型的数据，它可以表示任意精度格式的数据，使用bigint可以安全的存储和操作大整数，即使这个数已经超出了number能够表示的安全整数范围；可以用在一个整数字面量后面加n的 方式定义一个bigint，例10n，或者调用函数BigInt()并传递一个整数值或字符串值。原本js能存储的最 大数为2^53 - 1  
>
> [BigInt - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt)
>
> +  bigint不能用于math对象中的方法  
> +  不能和任何number实例混合运算，两者必须转换成同一类型
>
> ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1694595130483-42edd591-d747-4928-828e-2fe76e86ccd7.png)  
>

2. **引用数据类型：object**

> object类型包括:
>
> + 普通对象{}
> + 数组对象[]
> + 函数对象Function
> + 正则对象RegExp
> + 日期Date
> + 数学对象Math
>

3. 原始数据类型直接存储在栈中，占据空间小，大小固定

引用数据类型同时存储在栈和堆中，占据空间大，大小不固定，在栈中存储指针，该指针指向堆中该实体的起始地址，堆中存储实体

基本数据类型是值传递，即在拷贝时，会创建一个完全相等的变量，在栈中重新开辟一块内存空间存储原变量的副本

引用数据类型是引用传递，即在拷贝时，会创建一个指针指向原有变量，在栈中重新开辟一块内存空间存储指针以指向原变量

4. 每个object实例都拥有的属性和方法：
    - constructor：用于创建对象的函数
    - hasOwnProperty(proeprtyName)：用于判断当前对象实例上是否存在给定的属性，要检查的属性名必须是字符串
    - isPropertypeof(object)：用于判断当前对象是否为另一个对象的原型
    - propertyEnumerable(propertyName)：用于判断给定的属性是否可以使用for-in语句枚举，要检查的属性名必须是字符串
    - toLocalString()：返回对象的字符串表示
    - toString()：返回对象的字符串表示
    - valueof：返回对象对应的字符串，数值或布尔值表示

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1694671129316-7ca9115d-86ea-42b9-b496-4a5e74b7b9e3.png)

## 判断数据类型
[JavaScript 深入系列之数据类型检测 · Issue #94 · yuanyuanbyte/Blog](https://github.com/yuanyuanbyte/Blog/issues/94)

### typeof：常用于检测基本类型和Function，除null外
typeof返回一个字符串，表示未经计算的操作数的类型，用于区分基本数据类型，但无法区分null，无法区分引用数据类型，但能区分Function

因为js的第一个版本，在这个版本中单个值在栈中占用32位的存储单元，32位存储单位又划分为（1-3）位和实际数据，类型标签存储在低位中，而null 0~31位均为0，所以typeof会判断其为object

**typeof原理：不同的对象在底层都表示为二进制，在js中二进制前三位存储其类型信息**

    - 000：对象 
    - 010：浮点数
    - 100：字符串
    - 110：布尔值
    - 111：整数

| 类型 | 结果 |
| --- | --- |
| undefined | “undefined” |
| null | “object” |
| boolean | “boolean” |
| Number | “number” |
| bigint | “bigint” |
| string | “string” |
| Symbol | “symbol” |
| 宿主对象 | 取决于具体实现 |
| Function对象 | “function” |
| 其他任何对象 | “object” |


> typeof NaN  // 'number'
>
> NaN和自身不相等
>

### instanceof：常用于检测引用类型，无法区分基本类型
用来比较一个对象是否为某一个构造函数的实例，在运行时检测constructor.prototype是否存在于参数object的原型链上

object：某个实例对象    constructor：某个构造函数

### construtor：常用于检测引用类型，无法区分基本类型
如果创建一个对象改变他的原型，constructor就不能用来判断数据类型了

### Object.prototype.toString.call：常用于检测基本类型和引用类型
将参数转化为字符串

String(val)会依次调用val.toString, val.valueOf()

> 讲透Object.prototype.toString.call(val)
>
> 每个对象都有一个toString()方法，默认情况下该方法被每个Object对象继承，如果此方法在自定义对象中未被覆盖，toString()返回“[object type]”，其中type是对象的类型
>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1694677833350-130c6511-0bd2-4677-a110-4135dea4d86b.png)

### Reflect.apply(Object.prototype.toString, val, [])：常用于检测基本类型和引用类型
> [https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/apply](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/apply)
>

静态方法Reflect.apply()通过指定的参数列表发起对目标函数的调用

```javascript
Reflect.apply(target, thisArgument, argumentsList)
```

+ target：目标函数
+ thisArgument：target函数调用时绑定的this对象
+ argumentList：target函数调用时传入的实参列表，该参数应该是一个类数组的对象

## 数据类型转换
### 字符串转数组
1. split()
2. 扩展运算符
3. Array.from()

### 数组转字符串
1. join()
2. toString(), toLocaleString(), String()

> toLocaleString()调用每个数组的toLocaleString()方法，然后使用地区特定的分隔符将生成的字符串连接起来，形成一个字符串。**如果是为了返回时间类型的数据，推荐使用toLocaleString()**
>
> toString()方法是最保险的，返回唯一值得方法，他不会因为本地环境的改变而发生变化
>

### 类数组转数组
类数组：类数组是具有length属性，但是不具备数组原型上的方法，常见的有arguments和dom操作返回的结果

+ Array.from(类数组)
+ Array.prototype.slice.call(类数组) // 由于类数组不具有数组原型上的方法，所以这样写，这样写可以将具有length的对象转为数组
+ 扩展运算符：内部是for...of循环
+ 使用concat，Array.prototype.concat.apply([], 类数组)

### 转换为数字
+ Number()：
    - true=>1, false=>0
    - 数值直接返回
    - null返回0
    - undefined返回NaN
    - 字符串
        * 字符串包含数值，包括数值字符前面带加减号的情况，则转换为一个十进制数值
        * 如果字符串包含有效的浮点值格式，则会转换为相应的浮点值
        *  字符串包含有效的十六进制格式，转换为十进制整数
        * 空字符串返回0
    - 除上述情况返回NaN
    - 对象：调用valueof()方法，并按照上述规则转换返回的值，如果转换结果为NaN，则调用toString()方法，再按照转换字符串的规则转换
+ parseInt()：字符串最前面的空格会被忽略，从第一个非空格字符串开始转换，如果第一个字符不是数字字符，加号或减号，parseInt()立即返回NaN，parseInt()的第二个参数用于指定底数，即按照第二个参数的进制转换为10进制

# 数值分隔符_（ES2021）
使用数值分隔符的几个注意点：

+ 不能放在数值的最前面或最后面
+ 不能两个或两个以上的分隔符连在一起
+ 小数点的前后不能有分隔符
+ 科学计数法里面，表示指数的e或E前后不能有分隔符
+ <font style="color:#000000;">分隔符不能紧跟着进制的前缀0b、0B、0o、0O、0x、0X</font>
+ <font style="color:#000000;">Number()，parseInt()，parseFloat()不支持数值分隔符</font>

# <font style="color:rgb(27, 27, 27);">js浮点误差</font>
计算机编程语言里的浮点数会存在精度丢失问题，根本原因是二进制和实现位数限制有些数无法有限表示。js以IEEE-754标准格式64位双精度浮点数形式存储数字类型，在二进制科学表示法中，双精度浮点数的小数部分最多只能保留52位，再加上前面的1，其实就是保留53位有效数字，剩余的需要舍去，超出部分会舍去以下是十进制小数对应的二进制表示。IEEE-754标准是目前最广泛使用的浮点数运算标准

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697533676665-fe5f265d-70cd-43bd-9fba-5415790b6928.png)

解决办法：

将小数放大为整数进行算数运算，再缩小为小数

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697533708829-86e6e762-847e-42a0-93af-c05900f4a7d9.png)

> [表达式与运算符 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Expressions_and_operators#%E4%BD%8D%E8%BF%90%E7%AE%97%E7%AC%A6)
>
> IEEE-754标准提供了如何在计算机内存中，以二进制的方式存储十进制浮点数的具体标准
>
> [IEEE754详解（最详细简单有趣味的介绍）_明月几时有666的博客-CSDN博客](https://blog.csdn.net/gao_zhennan/article/details/120717424)
>

# undefined，undeclared，null的区别
已在作用域中声明但是还没有赋值的变量是undefined

没声明过的变量是undeclared

null表示空对象

对于undeclared变量的引用，浏览器会报引用错误

# ==和===
==会发生强制类型转换

+ 如果任一操作数为布尔值，将其转换为数值再比较
+ 如果一个操作数为字符串，另一个操作符为数值，则尝试将字符串转换为数值再比较
+ 如果一个操作数为对象，另一个操作数不是，则调用对象的valueOf()方法取得其原始值，再根据之前的规则进行比较
+ null == undefined，null和undefined不能再转换成其他类型的值再作比较
+ 任一操作符为NaN，则相等操作符返回false，不相等操作符返回true，NaN == NaN => false
+ 如果两个操作符都为对象，则比较是否为同一个对象，如果都指向同一个对象，则相等操作符返回true。虽然存储的值相等，但是如果属于不同的内存区域，那么两者仍不相等

# isNaN和Number.isNaN函数的区别
+ 函数isNaN接收参数后，会尝试将这个参数转换为数值，任何不能被转换为数值的值都会返回true，因此传入非数字值也会返回true，会影响NaN的判断
+ 函数Number.isNaN会首先判断传入参数是否为数字，如果是再继续判断是否为NaN，不会进行数据类型的转换，这种方法对于NaN的判断更为准确

# 函数的尾调用
[内存的清道夫——函数的尾调用 - 掘金](https://juejin.cn/post/7125958517600550919?searchId=20230831143303D57B385B6C88D80201E6)

# setTimeout和setInterval的区别
<font style="color:rgb(37, 41, 51);">setTimeout是延迟执行，setTimeout() 方法用于在指定的毫秒数后调用函数或计算表达式，常用于延迟执行某些事务或方法</font>

<font style="color:rgb(37, 41, 51);">setInterval是定时执行，用于在指定的毫秒数间隔调用函数或计算表达式，一般用于一般定时刷新业务，用于业务数据同步等</font>

## <font style="color:rgb(37, 41, 51);">setTimeout()倒计时为什么会出现误差</font>
> [为什么 setTimeout 有最小时延 4ms ? - 掘金](https://juejin.cn/post/6846687590616137742)
>

setTimeout()只是将事件插入了任务队列，必须等当前代码（执行栈）执行完，主线程才会去执行他指定的回调函数，要是当前代码消耗时间很长，也有可能要等很久，所以并没有办法保证回调函数一定会在setTimeout()指定的时间执行，所以setTimeout（）的第二个参数表示的是最少时间，并非是确切是时间

HTML5标准规定了 setTimeout() 的第二个参数的最小值不得小于4毫秒，如果低于这个值，则默认是4毫秒。在此之前。老版本的浏览器都将最短时间设为10毫秒。另外，对于那些DOM的变动（尤其是涉及页面重新渲染的部分），通常是间隔16毫秒执行。这时使用 requestAnimationFrame() 的效果要好于 setTimeout()

# ajax&axios
## ajax——asynchronous javascript and xml
<font style="color:rgb(51, 51, 51);">主要用于实现客户端与服务端的异步通信效果，实现页面的局部刷新</font>

1. <font style="color:rgb(51, 51, 51);">创建xmlhttprequest异步调用对象</font>
2. <font style="color:rgb(51, 51, 51);">创建一个新的http请求，并指定http请求的方法，url及验证信息</font>
3. <font style="color:rgb(51, 51, 51);">设置响应http请求状态变化的参数</font>
4. <font style="color:rgb(51, 51, 51);">发送http请求</font>

```javascript
var xhr = new XMLHttpRequest();//创建对象
xhr.open('method','url'，'async');//配置请求，初始化设置请求方法和url
xhr.send();//构建请求体，发送到服务器
xhr.setRequestHeader('Content-Type','application/json');//设置请求头
//事件绑定，处理服务端返回的结果
xhr.onreadystatechange = function(){
  if(xhr.readyState === 4){[
    if(xhr.status >= 200 && xhr.status < 300){
    // some code         
  }
    ]}
}
```

<font style="color:rgb(51, 51, 51);">readyState的状态从0到4变化</font>

+ <font style="color:rgb(51, 51, 51);">0：请求未初始化，xmlhttprequest对象还没有完成初始化</font>
+ <font style="color:rgb(51, 51, 51);">1：服务器连接已建立，xmlhttprequest对象开始发送请求</font>
+ <font style="color:rgb(51, 51, 51);">2：请求已接收，xmlhttprequest对象的请求发送完成</font>
+ <font style="color:rgb(51, 51, 51);">3：请求处理中，xmlhttprequest对象开始读取服务器的响应</font>
+ <font style="color:rgb(51, 51, 51);">4：请求已完成且响应就绪，xmlhttprequest对象读取服务器响应结束</font>

### <font style="color:rgb(51, 51, 51);">promise封装ajax</font>
```javascript
const getJson = function(url){
  return new Promise(function(resolve,reject){
    const handler = fucntion(){
      if(this.readyState !== 4){
        return;
      }
      if(this.status == 200){
        resolve(this.response);
      }else{
        reject(new Error(this.statusText));
      }
    };
    const client = new XHRHttpRequest();
    client.open('GET',url);
    client.onreadystatechange = handler;
    client.responseType = 'json';
    client.setRequestHeader('Accept','application/json');
    client.send();
  });
}
getJson("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});
```

## axios
+ axios是通过promise实现的对ajax技术的一种封装，支持promise所有的api
+ 在服务端使用node.js http模块，在客户端中使用xmlhttprequests
+ 支持请求响应拦截器
+ 自动转换json数据
+ 客户端防止csrf

# 解构赋值
解构赋值的用途

+ 交换变量的值

```javascript
[x, y] = [y, x]
```

+ 从函数返回多个值

```javascript
// 返回一个数组
function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();
// 返回一个对象
function example() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();
```

+ 函数参数的定义

```javascript
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```

+ 提取json数据![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1693648366508-36adff13-0810-4aad-8228-a92cc988284b.png)
+ 函数参数的默认值

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1693648380997-ac5d0fc0-fd3f-4329-b437-a023dc5d2ad4.png)

+ 遍历map结构

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1693648402555-f4d24035-82b5-45e0-b3b2-ae0539de51a8.png)![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1693648420619-439fb839-3f52-4082-8379-b3a8656dbc06.png)

+ 输入模块的指定方法

```javascript
const { SourceMapConsumer, SourceNode } = require("source-map");
```

# <font style="color:#000000;">Number的一些方法</font>
## <font style="color:#000000;">Number.isNaN&Number.isFinite</font>
<font style="color:#000000;">传统的全局方法isFinite()和isNaN()的区别在于，传统方法先调用Number()将非数值的值转为数值，再进行判断，而这两个新方法只对数值有效，Number.isFinite()对于非数值一律返回false, Number.isNaN()只有对于NaN才返回true，非NaN一律返回false。</font>

## <font style="color:#000000;">Number.isInteger()</font>
用来判断一个数值是否为整数，如果参数不是数值，返回false

> <font style="color:rgb(13, 20, 30);">JavaScript 内部，整数和浮点数采用的是同样的储存方法，所以 25 和 25.0 被视为同一个值。</font>
>

<font style="color:rgb(13, 20, 30);">如果一个数值的绝对值小于Number.MIN_VAlUE（5E-324），即小于js能够分辨的最小值，会被自动转为0。</font>

## Number.EPSILON
es6在Number对象上面，新增的一个极小的常量Number.EPSLION。根据规格，它表示1与大于1的最小浮点数之间的差。Number.EPSLION实际上是js能够表示最小精度。误差如果小于这个值，就可以认为没有意义了，即不存在误差了

## 安全整数和Number.isSafeInteger()
js能够准确表示的整数范围在-2^53到2^53之间（不含两个端点），超过这个范围，无法精确表示这个值。

<font style="color:#000000;">Number.MAX_SAFE_INTEGER和Number.MIN_SAFE_INTEGER用来表示这个范围的上下限</font>![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1693659917156-942c8ccc-85cc-42a0-a773-e8792d59eac9.png)

Number.isSafeInteger()则是用来判断一个整数是否落在这个范围之内

> 验证运算结果是否落在安全整数的范围内，不要只验证运算结果，而要同时验证参与运算的每个值
>

# Math对象的扩展
## Math.trunc()
用于去除一个数的小数部分，返回整数部分。对于非数值，Math.trunc内部使用Number方法将其先转换为数值。对于空值和无法截取整数的值，返回NaN。对于没有部署这个方法的环境，可以用下面的代码模拟。

## Math.sign()
用来判断一个数到底是正数，负数还是零，对于非数值，会先将其转换为数值。返回值如下：

+ 参数为整数返回+1
+ 参数为负数，返回-1
+ 参数为0，返回0
+ 参数为-0，返回-0
+ 其他值，返回NaN

<font style="color:rgb(13, 20, 30);">如果参数是非数值，会自动转为数值。对于那些无法转为数值的值，会返回NaN</font>

## <font style="color:rgb(13, 20, 30);">Math.cbrt()</font>
<font style="color:rgb(13, 20, 30);">用于计算一个数的立方根.</font><font style="color:#000000;">对于非数值，Math.cbrt()方法内部也是先使用Number()方法将其转为数值</font>

# 函数扩展
## 函数的length属性
**函数的length属性的含义是该函数预期传入的参数个数，某个参数指定默认值以后，预期传入的参数个数就不包括这个参数了**

指定了默认值以后，函数的length属性，将返回没有指定默认值的参数个数，也就是说指定了默认值后，length属性将失真

```javascript
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2
```

> 将参数默认值设为undefined，表明这个参数是可以省略的
>

## 函数的rest参数
arguments对象不是数组，是一个类似数组的对象，所以当我们想使用数组的方法时必须使用Array.from先将其转为数组，rest参数本身就是一个真正的数组，数组特有的方法都可以使用

rest参数之后不能再有其他参数，否则会报错

函数的length属性，不包括rest参数

## 严格模式
只要函数参数使用了默认值，解构赋值，或者扩展运算符，那么函数内部就不能显式设定为严格模式，否则会报错。

原因：函数内部的严格模式，同时适用于函数体和函数参数，但是，函数执行的时候，先执行函数参数，再执行函数体。<font style="color:rgb(13, 20, 30);">这样就有一个不合理的地方，只有从函数体之中，才能知道参数是否应该以严格模式执行，但是参数却应该先于函数体执行。</font>

<font style="color:rgb(13, 20, 30);">解决方案：</font>

1. <font style="color:rgb(13, 20, 30);">全局设定严格模式</font>
2. <font style="color:rgb(13, 20, 30);">将函数包在一个无参数的立即执行函数里面</font>

```javascript
const doSomething = (function () {
  'use strict';
  return function(value = 42) {
    return value;
  };
}());
```

# 箭头函数
1. 箭头函数本身是没有prototype的，所以箭头函数本身没有this，箭头函数的this指向在定义的时候继承自外层第一个普通函数的this。（call，bind，apply等方法不能改变箭头函数的this指向） 
2. 箭头函数的this指向是定义时决定的，是静态作用域，但他的值是运行时决定的。
3. <font style="color:rgb(13, 20, 30);">不可以当作构造函数，也就是说，不可以对箭头函数使用</font>new<font style="color:rgb(13, 20, 30);">命令，否则会抛出一个错误</font>
4. <font style="color:rgb(13, 20, 30);"> 没有arguments，箭头函数中访问arguments实际上获取的是他外层函数的arguments  。如果要用，可以使用rest参数代替</font>
5. <font style="color:rgb(13, 20, 30);">没有super和new.target</font>
6. <font style="color:rgb(13, 20, 30);"> 定义对象的大括号{}是无法形   成单独的执行环境，他依旧是处于全局环境中 </font>
7. <font style="color:rgb(13, 20, 30);">不可以使用yield命令，因此箭头函数不能用作generator函数 </font>

<font style="color:rgb(13, 20, 30);">不适用的场合：</font>

+ <font style="color:rgb(13, 20, 30);">定义对象的方法，且该方法内部包括this</font>

```javascript
const cat = {
  lives: 9,
  jumps: () => {
    this.lives--;
  }
}
```

> 调用cat.jumps()时，如果是普通函数，该方法内部的this指向cat；如果写成上面那样的箭头函数，使得this指向全局对象，因此不会得到预期结果。这是因为对象不构成单独的作用域，导致jumps箭头函数定义时的作用域就是全局作用域。
>

+ 需要动态this的时候，也不应该使用箭头函数

```javascript
var button = document.getElementById('press');
button.addEventListener('click', () => {
  // 这里的this时全局对象
  // 如果是普通函数，this就会动态指向被点击的按钮对象
  this.classList.toggle('on');
});
```

# 函数柯里化
> [柯里化函数应用 | Heying Ye’s Personal Website](https://heyingye.github.io/2018/04/20/%E6%9F%AF%E9%87%8C%E5%8C%96%E5%87%BD%E6%95%B0%E5%BA%94%E7%94%A8/)
>
> [彻底搞懂闭包，柯里化，手写代码，金九银十不再丢分！ - 掘金](https://juejin.cn/post/6864378349512065038?searchId=20230906112843D340E3DC818AE65D6DB1#heading-27)
>

将多参数的函数转换成单参数的形式

作用及特点：

+ 参数复用——复用最初函数的第一个参数
+ 提前返回——返回接受余下的参数且返回结果的新函数
+ 延迟执行——返回新函数，等待执行

柯里化的应用

+ 兼容浏览器事件监听方法，只进行一次兼容判断

```javascript
var addEvent = function(ele, type, fn, isCapture) {
	if(window.addEventListener) {
		ele.addEventListener(type, fn, isCapture)
	} else if(window.attachEvent) {
		ele.attachEvent("on" + type, fn)
	}
}
// 柯里化后可以写成这样
var addEvent = (function() {
  if(window.addEventListener) {
    return function(ele, type, fn, isCapture) {
    	ele.addEventListener(type, fn, isCapture)
    }
  } else if(window.attachEvent) {
    return function(ele, type, fn) {
    	ele.attachEvent("on" + type, fn)
    }
  }
})()
```

+ 性能优化：防抖节流
+ 兼容低版本的IE的bind方法

封装简单柯里化函数

```javascript
function createCurry(fn){
  if(typeof fn !== 'function'){
    throw TyepError("fn is not function");
  }
  // 复用第一个参数
  var args = [].slice.call(arguments, 1);
  // 返回新函数
  return funciton (){
    // 收集剩余参数
    var _args = [].slice.call(arguments);
    // 返回结果
    return fn.apply(this, args.concat(_args));
  }
}
```

# http缓存
<font style="color:rgb(51, 51, 51);">缓存的</font>**<font style="color:rgb(51, 51, 51);">优点</font>**<font style="color:rgb(51, 51, 51);">：减少不必要的数据传输，减轻服务器压力；加快客户端展示网页的速度，用户体验更友好</font>

<font style="color:rgb(51, 51, 51);">缓存的</font>**<font style="color:rgb(51, 51, 51);">缺点</font>**<font style="color:rgb(51, 51, 51);">：资源如果有更改但是客户端不进行更新造成用户获取信息滞后</font>

<font style="color:rgb(51, 51, 51);">一般用http头信息控制缓存</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697720487586-2fd1b6db-655d-4c8e-be76-8967734edec4.png)

<font style="color:rgb(51, 51, 51);">强制缓存优先于协商缓存进行，若强制缓存(Expires和Cache-Control)生效则直接使用缓存，若不生效则进行协商缓存(Last-Modified / If-Modified-Since和Etag / If-None-Match)，协商缓存由服务器决定是否使用缓存，若协商缓存失效，那么代表该请求的缓存失效，重新获取请求结果，再存入浏览器缓存中；生效则返回304，继续使用缓存，主要过程如下：</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697720509106-189dc644-58c3-4e8d-a745-6f38fa353aa6.png)

缓存过程：

+ 浏览器首次加载资源成功时，服务器返回200，此时除了下载资源之外将response的header一并缓存
+ 下一次加载资源时，首先要经过强缓存的处理，cache-control的优先级最高，比如cache-control：no-cache,就直接进入到协商缓存的步骤了，如果cache-control：max-age=xxx,就会先比较当前时间和上一次返回200时的时间差，如果没有超过max-age，命中强缓存，不发请求直接从本地缓存读取该文件（这里需要注意，如果没有cache-control，会取expires的值，来对比是否过期），过期的话会进入下一个阶段，协商缓存
+ 协商缓存阶段，则向服务器发送header带有If-None-Match和If-Modified-Since的请求，服务器会比较Etag，如果相同，命中协商缓存，返回304；如果不一致则有改动，直接返回新的资源文件带上新的Etag值并返回200;
+ 协商缓存第二个重要的字段是，If-Modified-Since，如果客户端发送的If-Modified-Since的值跟服务器端获取的文件最近改动的时间，一致则命中协商缓存，返回304；不一致则返回新的last-modified和文件并返回200;

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697720541955-0b6afa81-d636-40e7-b648-76d69f00eacd.png)

## 强制缓存
<font style="color:rgb(51, 51, 51);">向浏览器缓存查找该请求结果，并根据该结果的缓存规则来决定是否使用该缓存结果的过程</font>

<font style="color:rgb(51, 51, 51);">浏览器直接从本地获取数据，不与服务器进行交互，命中时http返回码为200，size显示为from cache，其通过http返回头中的Expires或者cache-Control（优先级高）来控制</font>

+ <font style="color:rgb(51, 51, 51);"> Expires指缓存过期时间，是服务器的具体时间点，其返回一个代表资源失效的时间  
</font><font style="color:rgb(51, 51, 51);">缺点：失效时间是一个绝对时间，当客户端时间被修改或客户端与服务器时间偏差较大就会导致缓存混乱 </font>
+ <font style="color:rgb(51, 51, 51);"> Cache-Control为相对客户端的时间，服务器与客户端时间偏差也不会有影响，其拥有一些字段 </font>
    - <font style="color:rgb(51, 51, 51);">max-age：指定一个时间长度，在</font>**<font style="color:rgb(51, 51, 51);">时间长度</font>**<font style="color:rgb(51, 51, 51);">内资源有效（单位为s）</font>
    - <font style="color:rgb(51, 51, 51);">public：响应可以被任何对象（客户端，代理服务器）缓存</font>
    - <font style="color:rgb(51, 51, 51);">private：表明响应只能被单个用户缓存，代理服务器不能缓存</font>
    - <font style="color:rgb(51, 51, 51);">no-cache：该指令目的是为了</font>**<font style="color:rgb(51, 51, 51);">防止从缓存中返回过期的数据</font>**<font style="color:rgb(51, 51, 51);">，不是不缓存，客户端使用该指令表示客户端不接收缓存过的响应，缓存服务器必须把客户端请求发送给源服务器，服务器使用该指令时缓存服务器不能对该资源进行缓存</font>
    - <font style="color:rgb(51, 51, 51);">no-store：禁止缓存</font>
    - <font style="color:rgb(51, 51, 51);">Immutable：若页面命中强缓存，就算用户刷新页面，浏览器也不会发起服务请求</font>

<font style="color:rgb(51, 51, 51);">强制缓存的缓存规则：当浏览器向服务器发起请求时，服务器会将缓存规则放入HTTP响应报文的HTTP头中和请求结果一起返回给浏览器，控制强制缓存的字段分别是</font>**<font style="color:rgb(51, 51, 51);">Expires</font>**<font style="color:rgb(51, 51, 51);">和</font>**<font style="color:rgb(51, 51, 51);">Cache-Control</font>**<font style="color:rgb(51, 51, 51);">，其中Cache-Control优先级比Expires高。</font>

> [彻底理解浏览器的缓存机制 | Heying Ye’s Personal Website](https://heyingye.github.io/2018/04/16/%E5%BD%BB%E5%BA%95%E7%90%86%E8%A7%A3%E6%B5%8F%E8%A7%88%E5%99%A8%E7%9A%84%E7%BC%93%E5%AD%98%E6%9C%BA%E5%88%B6/)
>

## 协商缓存
协商缓存就是强制缓存失效后，浏览器携带缓存标识向服务器发起请求，由服务器根据缓存标识决定是否使用缓存的过程。协商缓存的标识也是在响应报文的HTTP头中和请求结果一起返回给浏览器的，控制协商缓存的字段分别有：**Last-Modified / If-Modified-Since和Etag / If-None-Match**，<font style="color:#DF2A3F;">其中Etag / If-None-Match的优先级比Last-Modified / If-Modified-Since高。</font>

浏览器发送请求到服务器，服务器根据http头信息中的条件判断文件是否更改，如果未发生改变则返回304，浏览器从缓存中加载资源，改变则返回文件与状态码200。其通过两对值来判断文件是否被改变

+ Last-modified/if-modified-since：

（1） Last-modified为浏览器向服务端发起请求后服务器放在响应头**返回的文件最后更改时间**

（2） If-modified-since即为被保存的last-modified值，其会在浏览器**协商缓存时被发送到服务器**，服务器通过读取这个字段并与文件最后改变时间比较，若相同则返回空，不同则将更新后的文件与文件最新更新时间返回至浏览器

**缺点：**last-modified保存的是精确到秒的绝对时间，如果资源在一秒内多次修改则无法识别。而且对于只修改了修改时间，并没有修改内容的文件也会去重新请求

+ 弱Etag/if-none-match：  //强etag无论资源发生多细微的变化也会改变

（1） Etag为每个文件唯一存在的hash值

（2） 在浏览器向服务器发起请求时if-none-match字段会被设置为该文件的etag值，服务器在接收该字段后将其与本地文件的etag值对比，若一致则代表文件内容没有被改变

**缺点：**由于要生成hash所以性能较低

总结：etag精度更高且优先级大于last-modified，但是性能差于后者。同时配置时服务器优先验证etag，一致后才会验证last-modified·

+ 协商缓存生效，返回304
+ 协商缓存失效，返回200和请求结果

# <font style="color:rgb(51, 51, 51);">内存缓存from memory cache和硬盘缓存from disk cache</font>
> <font style="color:rgb(119, 119, 119);">内存=》硬盘=》网络请求</font>
>

1. 先查找内存，如果内存中存在，从内存中加载；
2. 如果内存中未查找到，选择硬盘获取，如果硬盘中有，从硬盘中加载；
3. 如果硬盘中未查找到，那就进行网络请求；
4. 加载到的资源缓存到硬盘和内存；

内存缓存：快速读取和时效性

+ 快速读取：内存缓存会将编译解析后的文件，直接存入该进程的内存中，占据该进程一定的内存资源，以方便下次运行使用时的快速读取。
+ 时效性：硬盘缓存则是直接将缓存写入硬盘文件中，读取缓存需要对该缓存存放的硬盘文件进行I/O操作，然后重新解析该缓存内容，读取复杂，**速度比内存缓存慢**。

# 启发式缓存
<font style="color:rgb(51, 51, 51);">如果响应中未显示Expires，Cache-Control：max-age或Cache-Control：s-maxage，并且响应中不包含其他有关缓存的限制，缓存可以使用启发式方法计算新鲜度寿命。通常会根据响应头中的2个时间字段 Date 减去 Last-Modified 值的 10% 作为缓存时间。</font>

```javascript
// Date 减去 Last-Modified 值的 10% 作为缓存时间。
// Date：创建报文的日期时间, Last-Modified 服务器声明文档最后被修改时间
  response_is_fresh =  max(0,（Date -  Last-Modified)) % 10
```

# cookie、session、token
## cookie
1. cookie指的是某些网站为了识别用户身份而存储在用户本地终端的数据，cookie是服务端生成，客户端进行维护和存储的纯文本形式内容， 以**键值对形式**存在。
2. cookie是与特定域绑定的，服务端通过响应头里的**Set-Cookie**设置cookie后，默认情况下，domain被设置为设置cookie页面的主机名，cookie会与请求一起发送到创建它的域，这个限制能保证cookie中存储的信息只对被认可的接收者开放，不被其他域访问
3. 构成：
    - 名称：不区分大小写，需经过url编码
    - 值：存储在cookie里的字符串值，这个值必须经过url编码
    - 域：cookie有效的域，发送到这个域的所有请求都会包含对应的cookie
    - 路径：请求url中包含这个路径才会把cookie发送到服务器
    - 过期时间：表示何时删除cookie的时间戳
    - 安全标志：设置之后，只适用ssl安全连接的情况才会把cookie发送到服务器
4. 使用场景： 用户登录状态类的会话状态管理；自定义设置，主题等个性化设置；跟踪分析用户行为类的浏览器行为跟踪  
5. 缺点：
    - cookie不够大，大小限制在**4kb**左右，很多浏览器限制一个站点最多保存20个cookie（ie6或者更低版本），ie7和之后的版本最多可以有50个cookie，firefox也可以有50个cookie，chrome和safari没有硬性限制
    - **过多的cookie会带来性能浪费**：一旦服务器端向客户端发送了设置cookie的意图，除非cookie过期，否则客户端每次请求都会发送这些cookie到服务器端，一旦设置的cookie过多，将会导致报头较大，大多数的cookie并不需要每次都用上，会造成宽带的浪费
        * 减少cookie的大小
        * 为静态组件使用不同的域名

> 多个域名的优点：
>
> +  为不需要cookie的组件换个域名可以减少无效cookie的传输，所以很多网站的静态文件会有特别的域名，使得业务相关的cookie不影响静态资源  
> +  不仅可以减少cookie的发送，还可以突破浏览器下载线程数量的限制，因为域名不同，下载线程限制数量翻倍  
>
> 多个域名的缺点：
>
> +  将域名转换为IP需要进行DNS查询，多一个域名就多一次dns查询，页面的性能规则上就有一条：减少DNS查询，但是大多数浏览器都会进行DNS查 询，以削弱这个副作用的影响  
>

    - 不安全，http中cookie是明文传递的，所以具有安全问题
    - 每次http请求都会发送到服务器，影响效率
    - 需要自行封装增删改查的方法
6. cookie什么时候会被销毁？

Max-age：以秒为单位，cookie会在Max-age秒之后被删除，当Max-age为负数的时候，表示的是临时存储，不会生成cookie文件，只会存在浏览器内存中，且只会在打开浏览器窗口或者子窗口有效，一旦浏览器关闭就会消失，当Max-age为0时，就会删除cookie

7. 限制访问cookie：两种方法可以确保cookie被安全发送，并且不会被意外的参与者或脚本访问
    1. secure属性：标记为secure的cookie只应通过被**https协议加密过的请求发送给服务端**，他永远不会使用不安全的http发送，这意味着中间人攻击者无法轻松访问他，不安全的站点无法使用secure属性设置cookie，但是，secure不会阻止对cookie中敏感信息的访问
    2. httponly属性：**httponly不支持读写，浏览器不允许脚本操作document.cookie去更改cookie**，所以为避免跨域脚本攻击（xss），通过js的document.cookie API无法访问带有httponly标记的cookie，他们只应该发送给服务端，如果包含服务端session信息的cookie不想被客户端js脚本调用，那么就应该为其设置httponly标记
8. SameSite属性

用来限制第三方cookie，从而减少安全风险

    - strict：完全禁止第三方cookie，跨站点时，任何情况都不会发送cookie，**只有当前网页的url与请求目标一致，才会携带cookie**
    - lax：导航到目标网址的get请求除外，<font style="color:rgb(27, 27, 27);">这意味着 cookie 不会在跨站请求中被发送</font>

| 请求类型 | 正常情况 | 示例 | lax |
| --- | --- | --- | --- |
| **链接** | 发送cookie |  | 发送cookie |
| **预加载** | 发送cookie |  | 发送cookie |
| **get表单** | 发送cookie |  | 发送cookie |
| post表单 | 发送cookie |  | 不发送 |
| iframe | 发送cookie |  | 不发送 |
| ajax | 发送cookie | $.get("...") | 不发送 |
| image | 发送cookie |  | 不发送 |


    -  none：Chrome 计划将 Lax 变为默认设置。这时，网站可以选择显式关闭 SameSite 属性，将其设为 None 。不过，前提是必须同时设置 Secure 属性（Cookie 只能通过 HTTPS 协议发送），否则无效。  

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1694137428672-31e1b3d8-73fa-4778-9328-228fdedc1402.png)

>  只要两个 URL 的 eTLD+1 有效顶级域名相同即可，不需要考虑协议和端口。其中，eTLD 表示有效顶级域名。同源是协议域名端口，同站的话是只考虑域名，不考虑协议和端口  
>

## session
> 我们将一个用户对服务的访问成为会话
>

session是保存在服务器的一种数据结构，是用户的唯一标识，用于跟踪用户状态

当程序需要为某个客户端的请求创建一个session时，服务器首先检查这个客户端的请求里是否已包含了一个session标识（称为session id），如果已包含则说明以前已经为此客户端创建过session，服务器就按照session id把这个session检索出来使用（检索不到，会新建一个），如果客户端请求不包含session id，则为客户端创建一个session并且生成一个与此session相关联的session id，**session id的值应该是一个既不会重复，又不容易被找到规律以仿造的字符串**，这个session id将被在本次响应中返回给客户端保存。**保存这个session id的方式可以采用cookie**，这样在交互过程中浏览器可以自动的按照规则把这个标识发送给服务器。  



**泄漏后如何补救：**

1. **立即让所有受影响的用户重新生成SessionID:**** 停止使用原来的SessionID，让用户强制重新登陆**
2. **通知用户：**** 及时告知用户，提醒他们采取相应措施，例如：更改密码等等**
3. **检测异常行为：****检测到用户的异常行为后，即视采取错误，例如禁止某些操作等**

## token
访问资源接口时所需要的资源凭证

简单token的组成：uid（用户唯一的身份标识）、time（当前时间的时间戳）、sign（签名，token的前几位以哈希算法压缩成的一定长度的十六进制字符串）

## jwt——json web token
> <font style="color:rgb(18, 18, 18);">是一种开源标准(RFC 7519)，用来定义通信双方如何安全地交换信息的格式。</font>
>
> [JSON Web Token 入门教程 - 阮一峰的网络日志](https://ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)
>

放在http请求头信息的 authorization 字段里，使用bearer模式添加jwt，跨域的时候将jwt放在post请求的数据体里，通过url传输

**原理：**<font style="color:rgb(17, 17, 17);">服务器认证以后，生成一个 JSON 对象，发回给用户，以后用户与服务端通信的时候，都要发回这个json对象，服务器完全只靠这个对象认定用户身份，为了防止用户篡改数据，服务器在生成这个对象的时候会加上签名</font>

> <font style="color:rgb(17, 17, 17);">当服务端拿到JWT后，会解析出其中的 Header 、Playload、Signature 三部分，解析完成后根据服务端保存的密钥再次生成一个Signature。拿到新生成的Signature和解析出来的进行对比，如果一样则表示未被篡改即可使用Header和Playload中的信息</font>
>

 Authorization：Bearer token

> [authorization](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Authorization) 字段<font style="color:rgb(27, 27, 27);">用于提供服务器验证用户代理身份的凭据，允许访问受保护的资源</font>
>
> **<font style="color:rgb(51, 51, 51);">Authorization</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(248, 248, 248);">: </font><font style="color:rgb(51, 51, 51);"><</font><font style="color:rgb(51, 51, 51);background-color:rgb(248, 248, 248);">type</font><font style="color:rgb(51, 51, 51);">><</font>**<font style="color:rgb(51, 51, 51);">authorization</font>**<font style="color:rgb(51, 51, 51);">-</font><font style="color:rgb(51, 51, 51);background-color:rgb(248, 248, 248);">parameters</font><font style="color:rgb(51, 51, 51);">></font>
>
> [JWT授权为啥要在 Authorization标头里加个Bearer 呢 - 掘金](https://juejin.cn/post/7180234792313552933)
>
> <font style="color:rgb(51, 51, 51);">常见的type授权类型为：</font>
>
> + <font style="color:rgb(51, 51, 51);">Basic用于http-basic认证</font>
> + <font style="color:rgb(51, 51, 51);">Bearer常见于OAuth和JWT认证</font>
> + <font style="color:rgb(51, 51, 51);">Digest MD5哈希的http-basic认证（已弃用）</font>
> + <font style="color:rgb(51, 51, 51);">AWS4-HMAC-SHA256 AWS授权</font>
>

<font style="color:rgb(51, 51, 51);">jwt的三个部分依次如下：</font>

+ <font style="color:rgb(51, 51, 51);">Header： 头部</font>
+ <font style="color:rgb(51, 51, 51);">payload：负载</font>
+ <font style="color:rgb(51, 51, 51);">signature：签名</font>

特点：

1. JWT 默认是不加密，但也是可以加密的。生成原始 Token 以后，可以用密钥再加密一次
2. JWT 不加密的情况下，不能将秘密数据写入 JWT
3. JWT 不仅可以用于认证，也可以用于交换信息。有效使用 JWT，可以降低服务器查询数据库的次数
4. JWT 的最大缺点是，由于服务器不保存 session 状态，因此无法在使用过程中废止某个 token，或者更改 token 的权限。也就是说，一旦 JWT 签发了，在到期之前就会始终有效，除非服务器部署额外的逻辑
5. JWT 本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。**为了减少盗用，JWT 的有效期应该设置得比较短。对于一些比较重要的权限，使用时应该再次对用户进行认证**
6. 为了减少盗用，**JWT 不应该使用 HTTP 协议明码传输，要使用 HTTPS 协议传输**



**泄漏后如何补救：**

1. **使用黑名单或令牌撤销机制：**** 在服务器端维护一个黑名单，将已泄露或被盗的JWT加入其中，以阻止未经授权的访问。**
2. **重新颁发JWT：**** 要求用户或客户端重新登录以获取新的JWT令牌，这样之前泄露的JWT将无法继续使用。**

## cookie和session的区别
+ cookie存储在客户端，session存储在服务端
+ session是基于cookie实现的，session id会存储在客户端的cookie中，session比cookie安全
+ cookie只支持存字符串数据，想要设置其他类型的数需要将其转换成字符串，session可以存储任意数据类型
+ cookie可设置为长时间保持，session一般失效时间较短，客户端关闭（默认情况下）或者session超时都会失效
+ 存储大小不同，单个cookie保存的数据不能超过4kb，session可存储数据远高于cookie，但是当访问量过多，会占用过多的服务器资源

## session和token的区别
+ session是一种记录服务器和客户端会话状态的机制，使服务端有状态化，可以记录会话信息。而 Token 是令牌，访问资源接口（API）时所需要的资源凭证。Token 使服务端无状态化，不会存储会话信息。  
+ session和token并不矛盾，作为身份认证token安全性比session好， 因为每一个请求都有签名还 能防止监听以及重放攻击，而 Session 就必须依赖链路层来保障通讯安全了。**如果你需要实现有状态的会话，仍然可以增加 Session 来在服务器端保存一些状态。**

# SSO登录（单点登录）
一个大型系统中包含了n个子系统，用户在操作不同系统时，需要多次登录就会导致用户体验感不好，那么SSO就可以很好的解决这个问题，即在多个应用系统中，只需要登录一次即可访问其他相互信任应用系统 。 

我们以 tmall.com 和 taobao.com 为例，我们只需要登录其中一个系统，另一个系统就会默认登录

## 单点登录下的CAS验证流程：
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699432429165-a0a14624-a423-4c02-bdb6-4fd37f6ad250.png)

1. 客户端访问系统A ， 系统A发现用户并未登录 ，重定向至 CAS服务中心
2. CAS服务中心发现请求的cookie中并没有携带登录的票据证明（TGC），则CAS判定用户处于未登录状态，重定向至CAS的登录页面 
3. 用户在CAS登录页面输入账号密码进行认证 
4. CAS校验用户信息，生成TGC放入自己的Session中 ，并同时以 set-Cookie的形式写入 Domain为sso.com的域下，并生成一个授权令牌ST ，然后重定向到系统A的地址，并在url中携带ST
5. 系统A保存携带的ST（token）,然后再次携带token请求资源，CAS系统验证ST信息后告诉服务器已经登陆，服务器进行资源的响应。
6. 当我们此时访问系统B时，发现用户未登录，重定向至CAS服务中心
7. 客户端携带cookie中含有TGC，CAS服务中心校验成功后，生成一个新的token （含有ST）返还给客户端 
8. 客户端拿到token后，携带token发送请求，系统B会拿着token向CAS认证，校验成功后CAS返还给系统B一个成功标识，系统B返还数据 

## CAS 生成的票据：
+ **TGT（Ticket Grangting Ticket）** ：TGT 是 CAS 为用户签发的 登录票据，拥有了 TGT，用户就可以证明自己在 CAS 成功登录过。
+ **TGC：Ticket Granting ****Cookie****：** CAS Server 生成TGT放入自己的 Session 中，而 TGC 就是这个 Session 的唯一标识（SessionId），以 Cookie 形式放到浏览器端，是 CAS Server 用来明确用户身份的凭证。
+ **ST（Service Ticket）** ：ST 是 CAS 为用户签发的访问某个 Service 的票据。

# OAuth2.0（第三方登录）
OAuth是一种授权机制，数据的所有者告诉系统，同意授权第三方应用进入系统，获取这些数据 。 系统生成一个短期的进入Token，用来代替密码，提供给第三方使用 

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699432603987-4b05ae2b-dde3-48ee-9fd4-c398579c13fd.png)

+ 第三方应用发起微信授权登录请求后，用户授权登录第三方应用后，微信会重定向到第三方网站，并携带临时票据（code）  
code是用来获取 access_token，并且code具有时间限制
+ 通过code 加上AppId等参数，调用API获取 access_token
+ 通过携带token进行获取用户的基本数据资源或帮助用户实现基本操作

**Access token和refresh token**

> [Access Token & Refresh Token 详解以及使用原则 - 掘金](https://juejin.cn/post/6859572307505971213)
>

Access token（访问令牌）是一种用于认证和授权的令牌，用于访问受保护的资源。它通常由身份验证服务器颁发，并且具有一定的有效期。Access Token应该维持在较短有效期，过长不安全，过短也会影响用户体验，因为频繁去刷新带来没有必要的网络请求。可以参考我们常常在某些网站停止操作一段时间之后就会掉线，这个时间是Refresh Token的有效期，Access Token不应长过这个时间。

> Access Token（访问令牌）是在OAuth 2.0认证和授权框架中使用的一种令牌
>

Refresh token（刷新令牌）是一种用于认证和授权的令牌，通常与访问令牌（access token）一起使用。它是在用户进行身份验证后由身份验证服务器颁发的，并具有相对较长的有效期。Refresh Token的有效期就是允许用户在多久时间内不用重新登录的时间，可以很长，视业务而定。我们在使用某些APP的时候，即使一个月没有开过也是登录状态的，这就是Refresh Token决定的。授权服务在接到Refresh Token的时候还要进一步做客户端的验证，尽可能排除盗用的情况。

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699890827923-89c9077d-8750-4d9c-821a-6fa11f783966.png)

1. <font style="color:rgb(37, 41, 51);">客户端向授权服务器请求Access Token（整个认证授权的流程，可以是多次请求完成该步骤）</font>
2. <font style="color:rgb(37, 41, 51);">授权服务器验证客户端身份无误，且请求的资源是合理的，则颁发Access Token 和 Refresh Token，可以同时返回Access Token的过期时间等附加属性。</font>
3. <font style="color:rgb(37, 41, 51);">带着Access Token请求资源</font>
4. <font style="color:rgb(37, 41, 51);">资源服务器验证Access Token有效则返回请求的内容</font>
5. <font style="color:rgb(37, 41, 51);">上面的3、4步骤可以反复进行，直到Access Token过期。 如果客户端在请求之前就能判断Access Token已过期或临近过期（下发过期时间），就可以直接跳到步骤7。否则，就会再请求一次，也就产生了本步骤。</font>
6. <font style="color:rgb(37, 41, 51);">当Access Token无效的时候，资源服务器会拒绝响应资源并返回Token无效的错误。</font>
7. <font style="color:rgb(37, 41, 51);">客户端重新向授权服务器请求Access Token，但是这次只需带着Refresh Token即可，而不需要用户再执行认证和授权的流程。这样就可以做到用户无感。</font>
8. <font style="color:rgb(37, 41, 51);">授权服务器验证Refresh Token，如果有效，则签发新的Access Token（或者同时下发一个新的Refresh Token）</font>
9. Refresh Token的其中一个目的是让用户在较长的时间保持登录状态，那么可否直接让Access Token具有更长的有效期，从而可以省去许多没用的步骤。答案是不安全，理由参考上面问题的答案。

> <font style="color:rgb(37, 41, 51);">举个例子，某个用户登录成功，获得了一个可以发帖的Access Token，这时管理员发现他发布垃圾内容吊销了发帖权限，而这个信息一般属于授权服务管理，也就是说他下次向授权服务请求Access Token将不会得到发帖权限。但是如果用户之前拿到的Access Token是长期有效的，那么这个用户就可以发帖很长时间。如果Access Token在短时间内失效，那么他必须重新去授权服务请求，这时授权服务将不会颁发具备发帖权限的Access Token。</font>
>
> <font style="color:rgb(37, 41, 51);">第二个例子，如果Access Token具有较长的有效期，一旦被盗用，攻击者就可以拿Access Token使用很长时间。聪明的你可能会想到，攻击者可以同时盗取Refresh Token。</font>
>
> [<font style="color:rgb(37, 41, 51);">RFC6749</font>](https://link.juejin.cn?target=http%3A%2F%2Fwww.rfcreader.com%2F%23rfc6749)<font style="color:rgb(37, 41, 51);">第10节中有说明，授权服务</font>**必须维护Refresh Token与客户端的绑定关系**<font style="color:rgb(37, 41, 51);">，也就是说只有合法用户的客户端（可通过IP,UA等资料判断）来请求是可以通过的。退一步讲，如果攻击者模拟了客户端可以执行刷新请求，那么就要看谁先刷。由于授权服务</font>**可以设置Refresh Token一次有效**<font style="color:rgb(37, 41, 51);">，因此不管哪个先刷新，另一个人刷新就会报错。如果用户先刷新，攻击者以Access Token和Refresh Token的双重失效结束游戏。如果攻击者先刷新了，合法用户就会收到报错信息，授权服务会引导用户从上图的步骤1重新开始认证，从而把有效的Refresh Token拿回到合法用户这里。</font>
>

# 唯一登录
1. 用户在客户端A登录时，输入用户名和密码后，服务端校验用户名和密码，并创建出Token和一个已登录状态并保存，并将token返还给用户端 
2. 用户在客户端B登录时，服务端校验后清除服务端缓存的token，并生成一个新的token并重新保存一个已登陆状态
3. 用户重新在客户端A登录时，校验token已经过期，并提示客户端 

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699432645542-57938af0-874c-447f-acaf-9a1cd048a006.png)

# webstorage
<font style="color:rgb(51, 51, 51);">为了解决客户端存储不需要频繁发送回服务器的数据时使用cookie问题</font>

## <font style="color:rgb(51, 51, 51);">localStorage</font>
1. localstorage：永久存储机制，存储空间大，除非通过js删除，否则数据永远不会过期
2. <font style="color:rgb(51, 51, 51);">如何设置localstorage的过期时间？https://www.jianshu.com/p/50b4c89d3be3</font>

<font style="color:rgb(51, 51, 51);">检测某一个网页下localStoragr的剩余容量  
</font><font style="color:rgb(51, 51, 51);">使用json.stringify(localStorage).length和最大容量进行比较</font>

```javascript
if(window.localStorage) {
  var aa= 1024*1024*5 - 
    unescape(encodeURIComponent(JSON.stringify(localStorage))).length;
  console.log(aa);
}
```

    - <font style="color:rgb(51, 51, 51);">为localstorage设置过期时间</font>
    - <font style="color:rgb(51, 51, 51);">惰性删除：可以在每一次get的时候判断是否过期，过期就删除，但是可能有一些永远也不会用到，就永远不会删除。所以也可以采用刷新就删除。</font>
    - <font style="color:rgb(51, 51, 51);">刷新删除：每次刷新进入页面就调用一次删除过期localstorage的函数</font>

方法：

+ <font style="color:rgb(51, 51, 51);">removeItem：移除 localStorage 项</font>

> <font style="color:rgb(51, 51, 51);">存储新手引导问题：</font>
>
> 1. 确定要存储的问题和相关信息：首先，确定您想要存储的新手引导问题及其相关信息。这些信息可以包括问题的文本、提示、步骤、状态等。
> 2. 将问题和信息转换为JavaScript对象：将问题和相关信息组织成一个JavaScript对象，以便能够方便地进行存储和检索。例如：
>
>       var guideQuestion = {
>
>   	 question: "这是一个新手引导问题吗？",
>
>   	 hint: "请按照以下步骤操作...",
>
>   	 steps: ["第一步：...", "第二步：...", "第三步：..."],
>
>   	 status: "未完成"
>
>  };
>
> 3. 将对象存储到localStorage中：使用localStorage.setItem()方法将问题对象存储到localStorage中。
> 4. 从localStorage中检索问题对象：如果您需要在以后的时间点检索存储的问题对象，可以使用localStorage.getItem()方法。
>
> var storedQuestion = localStorage.getItem("guideQuestion");
>
>  var questionObject = JSON.parse(storedQuestion);
>
> 5. 更新问题对象的信息：如果用户完成了新手引导的某个步骤，您可以更新问题对象的相应信息，并将其重新存储到localStorage中。
>

## sessionstorage
<font style="color:rgb(51, 51, 51);">只存储会话信息，数据会保存到浏览器关闭，存在sesionstorage中的数据不受页面刷新影响，可以在浏览器崩溃后重启恢复</font>

# <font style="color:rgb(51, 51, 51);">indexdb</font>
> [浏览器数据库 IndexedDB 入门教程 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2018/07/indexeddb.html)
>

<font style="color:rgb(51, 51, 51);">用于</font>**<font style="color:rgb(51, 51, 51);">客户端存储大量结构化数据</font>**<font style="color:rgb(51, 51, 51);">，该api使用索引来实现对该数据的高性能搜索，IndexedDB 是一个运行在浏览器上的非关系型数据库理论上来说，IndexedDB 是没有存储上限的（一般来说不会小于 250M）。它不仅可以存储字符串，还可以存储</font>**<font style="color:rgb(51, 51, 51);">二进制</font>**<font style="color:rgb(51, 51, 51);">数据</font>

+ **采用键值对存储**
+ <font style="color:rgb(51, 51, 51);">异步，防止大量数据读写，拖慢网页</font>
+ <font style="color:rgb(51, 51, 51);">支持事务：这意味着一系列操作步骤之中，只要有一步失败，整个事务就都取消，数据库回滚到事务发生之前的状态，不存在只改写一部分数据的情况。</font>
+ **受到同源限制**
+ <font style="color:rgb(51, 51, 51);">存储空间大，理论上没有上限</font>
+ <font style="color:rgb(51, 51, 51);">支持二进制存储</font>

# 包装类
注：在通过new实例化引用类型后，得到的实例会在离开作用域时销毁，而自动创建的原始值包装对象则只存在于访问 

```javascript
var str="hello word";
// var str = new String("hello world"); // 1.创建出一个和基本类型值相同的对象
// var long = str.length; // 2.这个对象就可以调用包装对象下的方法，并且返回结给long变量
// str = null; // 3.之后这个临时创建的对象就被销毁了
var long=str.length; //因为str没有length属性 所以执行这步之前后台会自动执行以上三步操作
console.log(long); // （结果为:10）
 // 1.因为下面有输出创建出str.length 而str不应该具有length这个属性 所以再次开辟空间创建出一个和基本类型值相同的对象
// var str = new String("hello word");
// str.length=nudefined; // 2.因为包装对象下面没有length这个属性没有值，所以值是未定
// str = null; // 3.这个对象又被销毁了
console.log(str.length) // （结果为:undefined）
```

步骤：

1. 创建一个基本类型对应的对象
2. 调用该实例上的特定方法
3. 销毁该对象即该实例

<font style="color:rgb(51, 51, 51);">number,string,boolean构造函数可以与new操作符搭配，symbol，bigint使用call进行隐式装箱</font>

```javascript
function createSymbolObject(description){
  return function (){
    return this
  }.call(Symbol(description))
}
function createSymbolObject(description){
  return function (){
    return this
  }.call(BigInt(description))
}
```

# js内置对象
+ 本地对象：与宿主无关，独立于宿主环境的ecmascript实提供的对象，这些引用类型在本地运行过程中**需要通过new来创建所需的实例对象**

> <font style="color:rgb(51, 51, 51);">包含object，array，date，regexp，function，boolean，number，string等</font>
>

+ <font style="color:rgb(51, 51, 51);">内置对象：与宿主无关，独立于宿主环境的ecmascript实现提供的对象，本身就是实例化内置对象，包含</font>**<font style="color:rgb(51, 51, 51);">Global和Math，json</font>**
+ <font style="color:rgb(51, 51, 51);">宿主对象：由ecmascript实现的宿主环境提供的对象，包含两大类，素数提供以及自定义类对象，对于嵌入到网页中的js来说其宿主对象就是浏览器提供的对象例如window和document，所有的dom以及bom都属于宿主对象</font>

# <font style="color:rgb(51, 51, 51);">数组方法</font>
![画板](https://cdn.nlark.com/yuque/0/2023/jpeg/2366100/1698160120071-f81ce29f-54de-4140-b3c0-75d1fb2b6b26.jpeg)

# 数组拍平
1. 使用flat()拍平数组：该方法可传入一个数字作为拍平的层数，默认为1，设置为infinity后可实现全部拍平
2. 使用正则表达式：

```javascript
// 转为字符串后拼接为数组样式最后将其转换为数组对象
Json.parse(‘[’+ json.stringify(arr).replace(/[|]/g ,‘ ’) + ‘]’) 
```

3. <font style="color:rgb(51, 51, 51);">使用reduce：</font>

```javascript
let flat = arr => {
  return arr.reduce((prev,cur) => {
    return prev.concat(Array.isArray(cur) ? flat(cur) : cur)
  },[])
}
```

4. 递归

```javascript
const res = [];
const fns = arr => {
  for(let i = 0; i < arr.length; i++){
    if(Array.isArray(arr[i])){
      fns(arr[i])
    }else{
      res.push(arr[i])
    }
  }
}
```

# 数组去重
1. 基本去重

```javascript
Array.prototype.unique = function (){
  let temp = {};
  let arr = [];
  let len = this.length;
  for(let i = 0; i < len; i++){
    if(!temp[this[i]){
      temp[this[i]] = 'abc';
      arr.push(this[i])
    }
  }
  return arr;
}
```

2. indexOf()——返回某个指定的字符串值在字符串中首次出现的位置

```javascript
function unique(arr){
  if(!Array.isArray(arr)){
    console.log('type error');
    return ;
  }
  let res = [];
  let len = arr.length;
  for(let i = 0; i < len; i++){
    let cur = arr[i];
    if(res.indexOf(cur) === -1){
      res.push(cur);
    }
  }
  return res;
}
```

3. sort()先将数组进行排序，然后判断当前元素与前一个元素是否相同，相同说明重复，不相同就添加进res

```javascript
function unique(array){
    array = array.sort();
    let res = [array[0]]
    for(let i = 0;i<len;i++){
        if(array[i] !== array[i-1]){
            res.push(array[i]);
        }
    }
    return res;
}
```

4. 利用splice

```javascript
function unique(arr){
  let len = arr.length;
  for(let i = 0; i < len; i++){
    for(let j = i + 1; j < len; j++){
      if(arr[i] === arr[j]){
        arr.splice(j, l);
        j--;
      }
    }
    return arr;
  }
}
```

5. filter

```javascript
function unique(arr){
  return arr.filter(function (item, index, arr){
    return arr.indexOf(item, 0) === index;
  })
}
```

6. 利用set

```javascript
function unique(arr){
  return Array.from(new Set(arr))
}
```

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695214196651-68f57add-5074-4e22-91b3-a77d7bf7655c.png)

7. map

```javascript
function unique(arr){
  const seen = new Map();
  return arr.filter(a => !seen.has(a) && seen.set(a, 1))
}
```

# 语义版本控制
> [语义版本控制（Semver）-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/2115813)
>

版本控制：Major.Minor.Patch

+ Major：主版本号，当你做了不兼容的API修改
+ Minor：次版本号，当你做了向下兼容的功能性新增
+ Patch：修订号，当你做了向下兼容的问题修正

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698160372475-093234bd-754b-425f-ad19-3eb2db40e81c.png)

# ES6 Module
1. <font style="color:#000000;">ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。</font>
2. <font style="color:#000000;">ES6模块的好处</font>
    1. <font style="color:#000000;">不再需要UMD模块格式了，将来服务器和浏览器都会支持 ES6 模块格式。</font>
    2. <font style="color:#000000;">将来浏览器的新 API 就能用模块格式提供，不再必须做成全局变量或者navigator对象的属性</font>
    3. <font style="color:#000000;">不再需要对象作为命名空间（比如Math对象），未来这些功能可以通过模块提供。</font>
3. <font style="color:#000000;">模块module功能主要有两个命令构成：</font>
+ <font style="color:#000000;">export：用于规定模块的对外接口</font>
    - <font style="color:#000000;">可以使用as关键字进行重命名</font>

```javascript
function v1() { ... }
function v2() { ... }
export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
```

    - 导出写法，目前export命令能够对外输出的三种接口类型：函数，类，var/let/const声明的变量

```javascript
// 报错
export 1;
// 报错
var m = 1;
export m;
// 写法一
export var m = 1;
// 写法二
var m = 1;
export {m};
// 写法三
var n = 1;
export {n as m};
// 报错
function f() {}
export f;
// 正确
export function f() {};
// 正确
function f() {}
export {f};
```

+ <font style="color:#000000;">import：用于输入其他模块提供的功能</font>
    - <font style="color:#000000;">import命令具有提升效果，会提升到整个模块的头部，首先执行</font>
    - <font style="color:#000000;">import是静态执行，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构</font>
+ <font style="color:#000000;">特点：</font>
    - <font style="color:#000000;">很多现代浏览器可以使用</font>
    - <font style="color:#000000;">具有Commonjs的简单语法以及AMD的异步</font>
    - 得益于 ES6 的[静态模块结构](https://link.juejin.cn/?target=https%3A%2F%2Fexploringjs.com%2Fes6%2Fch_modules.html%23sec_design-goals-es6-modules)，可以进行 [Tree Shaking](https://link.juejin.cn/?target=https%3A%2F%2Fdevelopers.google.com%2Fweb%2Ffundamentals%2Fperformance%2Foptimizing-javascript%2Ftree-shaking%2F)
    - ESM 允许像 Rollup 这样的打包器，[删除不必要的代码](https://link.juejin.cn/?target=https%3A%2F%2Fdev.to%2Fbennypowers%2Fyou-should-be-using-esm-kn3)，减少代码包可以获得更快的加载

## import和import()的区别
import 是 ES6 中用于在静态环境中导入模块的关键字，它是在代码加载阶段执行的。import 的语法如下：

```javascript
import defaultExport from "module-name";
import * as name from "module-name";
import { export1 } from "module-name";
import { export1 as alias1 } from "module-name";
import { export1 , export2 } from "module-name";
import { export1 , export2 as alias2 , [...] } from "module-name";
import defaultExport, { export1 [ , [...] ] } from "module-name";
import defaultExport, * as name from "module-name";
```

import()是 ES6 中用于在动态环境中导入模块的函数，它是在运行时执行的，而不是在代码加载阶段执行。import() 函数的语法如下：

```javascript
import(moduleName)
  .then((module) => {
    // 使用模块中的内容
  })
  .catch((error) => {
    // 处理错误
  });
let module = await import('/modules/my-module.js');
```

import()函数返回一个promise，可以在 Promise 的 then 方法中使用导入的模块。与 import 不同，import() 可以动态地加载模块，即可以在运行时根据需要动态加载模块，而不需要在代码加载阶段就加载所有模块

# <font style="color:#000000;">CommonJS</font>
1. Commonjs是Nodejs专用的，语法上最明显的差异为<font style="color:#000000;">，CommonJS 模块使用require()和module.exports，ES6 模块使用import和export</font>

```javascript
// importing 
const doSomething = require('./doSomething.js'); 

// exporting
module.exports = function doSomething(n) {
  // do something
}
```

2. commonjs是同步导入模块
3. 当commonjs导入的时候，会给你一个导入对象的副本
4. commonjs不能在浏览器中工作，必须经过转换和打包
5. commonjs的四个变量：module,exports,require,global
6. **<font style="color:#000000;">CommonJS的加载原理</font>**

CommonJS的一个模块，就是一个脚本文件，require命令第一次加载该脚本，就会执行整个脚本，然后在内存生成一个对象。CommonJS模块无论加载多少次，都只会在第一次加载时运行一次，以后再加载，就返回第一次运行的结果，除非手动清除系统缓存

```javascript
{
  id: '...', // 模块名
  exports: { ... }, // 模块输出的各个接口
  loaded: true, // 表示该模块的脚本是否执行完毕
  ...
}
```

3. 一旦出现CommonJS模块被循环加载，就只输出已经执行的部分，还未执行的部分不会输出

```javascript
// a.js
exports.done = false;
var b = require('./b.js');
console.log('在 a.js 之中，b.done = %j', b.done);
exports.done = true;
console.log('a.js 执行完毕');
// b.js
exports.done = false;
var a = require('./a.js');
console.log('在 b.js 之中，a.done = %j', a.done);
exports.done = true;
console.log('b.js 执行完毕');
// main.js
var a = require('./a.js');
var b = require('./b.js');
console.log('在 main.js 之中, a.done=%j, b.done=%j', a.done, b.done);
// 执行main.js后的输出
在 b.js 之中，a.done = false
b.js 执行完毕
在 a.js 之中，b.done = true
a.js 执行完毕
在 main.js 之中, a.done=true, b.done=true
```

## commonjs缺点
![](https://cdn.nlark.com/yuque/0/2024/png/2366100/1712547384171-a15152f0-d407-4cee-b4e4-6b5883406e85.png)

## require文件加载流程
+ 首先像 fs ，http ，path 等标识符，会被作为 nodejs 的核心模块。核心模块的优先级仅次于缓存加载，在Node源码编译中，已被编译成二进制代码，所以加载核心模块，加载过程中速度最快。
+ ./和../作为相对路径的文件模块，/作为绝对路径的文件模块。将路径转换成真实路径，并以真实路径作为索引，将编译后的结果缓存起来，第二次加载的时候会更快。
+ 非路径形式也非核心模块的模块，将作为自定义模块。查找规则：
    - 在当前目录下的node_modules目录查找。
    - 如果没有，在父级目录的node_modules查找，如果没有在父级目录的父级目录的node_modules中查找。
    - 沿着路径向上递归，直到根目录下的node_modules目录。
    - 在查找过程中，会找package.json下 main 属性指向的文件，如果没有package.json，在 node 环境下会以此查找index.js，index.json，index.node

## require 模块引入与处理
CommonJS 模块同步加载并执行模块文件，CommonJS 模块在执行阶段分析模块依赖，采用深度优先遍历（depth-first traversal），执行顺序是父 -> 子 -> 父

## require加载原理
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1701185578651-3436d26b-7dd0-4675-a92d-8b0cdf0e80b3.png)

## require流程
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1701185602864-fe46593a-5b45-4e6b-a012-9356f0888b77.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1701185621900-b9e67233-e6f7-45f8-80a5-dd1785576b43.png)

# AMD
异步加载模块，模块的加载不影响后面语句运行，所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成后，这个回调函数才会运行，包装模块的函数是全局define的参数

AMD模块可以使用字符串表示指定自己的依赖，而AMD加载器会在所有依赖模块加载完毕后立即调用模块工厂函数。

```javascript
define(['dep1', 'dep2'], function (dep1, dep2) {
    //Define the module value by returning a value.
    return function () {};
});
// 或者
define(function (require) {
    var dep1 = require('dep1'),
        dep2 = require('dep2');
    return function () {};
});
```

# UMD
<font style="color:rgb(51, 51, 51);">UMD用于创建这两个系统都可以使用的模块代码。本质上，UMD定义的模块会在启动时检测要使用哪个模块系统然后进行配置，并把所有逻辑包装在一个立即调用函数表达式中</font>

```javascript
(function (root, factory) {
	 if (typeof define === "function" && define.amd) { 
		// AMD 规范 
		define(["b"], factory); }
   else if (typeof module === "object" && module.exports) { 
		// 类 Node 环境，并不支持完全严格的 CommonJS 规范，但是属于 CommonJS-like 环境，支持 module.exports 用法 
		module.exports = factory(require("b")); 
	 } else { 
	// 浏览器环境 root.returnExports = factory(root.b);
 }
 })(this, function (b) { 
	// 返回值作为 export 内容 
		return {}; 
});
```

# module，AMD，CommonJS
1. ES6的模块是尽量静态化的，编译时就能确定模块的依赖关系以及输入和输出的变量，但是CommonJS和AMD模块都只能在运行时确定这些东西
2. export语句输出的接口，与其对应的值是动态绑定的关系，输出的是值得引用；但是Commonjs模块输出的是值的缓存，是值得拷贝
3. commonjs模块的require()是同步加载模块，ES6模块的import命令是异步加载，<font style="color:#000000;">有一个独立的模块依赖的解析阶段</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1701740841762-7edfffab-e39e-40e2-981f-3ede65ada3d2.png)

# 严格模式
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1696947573543-8bba197d-9647-45a0-807b-9445c64fd39c.png)

# 单位
+ rem：根据网页的根元素来设置页面大小
+ em：根据父元素的字体大小来设置
+ vm/vh：很好的弥补了rem需要js辅助的缺点
+ 1vw：window.innerWidth的1%
+ 1vh：window.innerHeight的1%

# 懒加载&预加载
## 预加载
将所有所需资源提前请求加载到本地，这样后面再需要的时候就直接从缓存获取资源

作用：

<font style="color:rgb(51, 51, 51);">在网页全部加载之前，对一些主要内容进行加载，以提供给用户更好的体验，减少等待时间，如果一个页面的内容过庞大，没有使用预加载技术的页面就会长时间的展现为一片空白</font>

1. <font style="color:rgb(51, 51, 51);">图片预加载</font>
+ img标签：<font style="color:rgb(37, 41, 51);">img标签会在Html渲染解析到的时候，如果解析到img中src的值，则浏览器会立即开启一个线程去请求该资源，所以我们可以先将img标签隐藏但src写上对应的链接，这样皆可以把资源先请求回来了</font>
+ image对象，原理同上
2. js预加载：
    1. 添加async或defer属性
    2. 使用module：<font style="color:rgb(37, 41, 51);">浏览器会对其内部的 import 引用发起 HTTP 请求，获取模块内容。这时 script 的行为会像是 defer 一样，在后台下载，并且等待 DOM 解析</font>

```javascript
<script type="module">import { a } from './a.js'</script>
```

    3. link标签的preload属性：<font style="color:rgb(37, 41, 51);">用于提前加载一些需要的依赖，这些资源会优先加载;vue2 项目打包生成的 index.html 文件，会自动给首页所需要的资源，全部添加 preload，实现关键资源的提前加载</font>

```javascript
<link rel="preload" as="script" href="index.js">
```

    4. link标签的prefetch属性:<font style="color:rgb(37, 41, 51);">prefetch 是利用浏览器的空闲时间，加载页面将来可能用到的资源的一种机制；通常可以用于加载其他页面（非首页）所需要的资源，以便加快后续页面的打开速度</font>

```javascript
<link rel="prefetch" as="script" href="index.js">
```

## <font style="color:rgb(51, 51, 51);">懒加载</font>
> [3分钟搞定图片懒加载-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/1560388)
>

> [滑动验证页面](https://segmentfault.com/a/1190000019377624)
>
> 图片加载不出来的时候可以通过监听图片的error事件做出对应的操作
>
> ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699433741528-cfa1cd47-db3a-4b43-92b3-e1470ceb6f4e.png)
>

也叫做延迟加载，指的是在网页中延迟加载图像，是一种很好优化网页性能的方式

作用：

+ 提升用户体验
+ 减少无效资源的加载
+ 防止并发加载资源过度会阻塞js的加载

步骤：

+ 加载loading图片
+ 判断哪些图片要加载
+ 隐形加载图片
+ 替换真图片

一张图片就是一个img标签，浏览器是否发起请求是根据img的src属性，懒加载的关键是在图片没有进入可视区域时，先不给img的src赋值，即浏览器不会发送请求

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1694940623620-469afcc1-d63c-47d5-a556-2f79c380e8df.png)

> <font style="color:#000000;">window.innerHeight：是浏览器可视区的高度</font>
>
> <font style="color:#000000;">document.documentElement.scrollTop || </font><font style="color:#000000;">document.body.scrollTop：浏览器滚动过的距离</font>
>
> <font style="color:#000000;">imgs.offsetTop：元素顶部距离文档顶部的高度</font>
>
> <font style="color:#000000;">imgs.offsetHeight：图片自身高度</font>
>
> <font style="color:#000000;">内容到达显示区域：</font>
>
> <font style="color:#000000;">img.offsetTop < window.innerHeight + document.body.scrollTop && img.offsetTop + img.offsetHeight > document.body.scrollTop</font>
>

```javascript
// 等所有的资源文件加载完毕之后再绑定事件
window.onload = function (){
  // 获取图片列表，即img标签列表
  var imgs = document.querySelectorAll('img');
  function lazyLoad(imgs){
    // 可视区域高度
    let innerHeight = window.innerHeight
    // 滚动区域高度
    let scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
    for(let i = 0;i < imgs.length;i++){
      // 图片距离顶部的距离大于可视区域和滚动区域之和时懒加载
      if((innerHeight + scrollTop) > imgs[i].offsetTop){
        // 不加立即执行函数i会等于9
        (function(i){
          // 真实情况是页面开始有2秒空白，所以使用setTimeout定时2s
          setTimeout(function (){
            //创建一个临时图片，这个图片在内存中不会到页面上去。实现隐形加载
            let temp = new Image();
            //只会请求一次
            temp.src = imgs[i].getAttribute('data-src');
            temp.onload = function (){
              // 获取自定义属性data-src，用真图片替换假图片
              imgs[i].src = imgs[i].getAttribute('data-src');
            }
          },2000)
        })(i)
      }
    }
  }
  lazyLoad(imgs);
  window.onscroll = function(){
    lazyLoad(imgs)
  }
}
```

# rgba()透明度和opacity透明度
rgba()只作用于元素的颜色或其背景

opacity作用于元素以及元素内所有内容的透明度

# 客户端渲染与服务端渲染
客户端渲染的页面是js负责进行的，html仅仅作为静态文件，客户端在请求时，服务端不做任何处理，直接以源文件的形式返回客户端，然后根据html上的js生成dom插入html，而服务端渲染是服务器直接返回html让浏览器直接渲染，服务端在返回html之前，在特定的区域符号里用数据填充，再给客户端，客户端只负责解析html

| 客户端渲染 | 服务端渲染 |
| --- | --- |
| + 前后端分离，前端专注于UI，后端专注于逻辑<br/>+ 局部刷新，无需每次都请求完整页面，体验更好<br/>+ 节省服务器性能，部署简单<br/>+ 交互性好，可以实现各种效果 | + 首屏渲染快，客户端只负责解析html<br/>+ 利于SEO<br/>+ 可以生成存储片段，生成静态化文件<br/>+ 节能 |
| + SEO问题，爬虫看不到完整的呈现源码<br/>+ 首屏渲染慢，渲染前，需要下载一堆js和css文件 | + 用户体验较差<br/>+ 不容易维护，通常前端改了部分html或者css后端也需要更改 |


![画板](https://cdn.nlark.com/yuque/0/2023/jpeg/2366100/1694943112748-39dba27d-cf43-4b02-bd9e-25d982dff926.jpeg)

## 代码同构
+ 服务端生成html
+ 发送html给浏览器
+ 浏览器接到内容显示
+ 浏览器加载js文件
+ js代码执行并接管页面的操作

> + 浏览器对服务器发起请求
> + 服务器node server render
> + 拿到请求中的path
> + 根据path查找到具体的组件
> + 数据获取调用组件的getInitialProp方法
> + 数据注入组件，将数据注入到组件props或者通过context传递
> + 得到组件的html字符串结果
> + 数据注水，将预取的数据注入到页面，是浏览器可以访问到
> + 加载静态资源，服务端进行输出组件html和预取数据
> + 数据脱水，浏览器得到数据，将数据传入组件
> + ReactDOM.hydrate浏览器执行渲染并进行节点对比
> + 完成事件的绑定和交互处理
>

## 数据注水
将服务端的store数据注入到window全局环境中

## 数据脱水
将window上绑定的数据给到客户端的store

# 创建对象
## 工厂模式
可以解决创建多个类似对象的问题，但没有解决对象标识的问题，即不知道对象是什么类型——所有实例都指向一个原型

```javascript
function create(name, age, obj){
  let o = new Object();
  o.name = name;
  o.age = age;
  o.sayName = function(){
    console.log(this.name);
  }
  return o;
}
let person1 = create('tyx', 18);
let person2 = create('ftf', 19);
```

## 构造函数模式
确保实例被标识为特定类型，但定义的方法会在每个实例上都创建一遍

```javascript
function Person(name, age){
  this.name = name;
  this.age = age;
  this.sayName = function (){
    console.log(this.name);
  }
}
let person1 = new Person('tyx',18);
let person2 = new Person('txj',19);
// 为了解决的定义方法会在每个实例上都创建一个可以优化成如下代码
// 但如果需要多个方法，需要在全局作用域上定义多个函数
function Person(name, age){
  this.name = name;
  this.age = age;
  this.sayName = sayName;
}
function sayName(){
  console.log(this.name);
}
let person1 = new Person('tyx',18);
let person2 = new Person('txj',19);
```

与工厂模式的区别：

+ 没有显式的创建对象
+ 属性和方法都直接赋值给了this
+ 没有return

## 原型模式
原型对象上定义的属性和方法可以被对象实例共享，解决了构造函数中的问题，实例与构造函数的原型之间有直接的关系，但实例与构造函数之间没有，即person与Person.prorotype之间有关系，再通过对象访问属性时，会按照这个属性的名称开始搜索，搜索开始于对象实例本身，如果没有找到会在原型对象上继续查找

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1694952479443-65e93eaa-9924-401c-9de1-207896e472f1.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1694952529441-c73b1fd8-66df-4987-9a3c-6ad90bc2c7f2.png)

```javascript
function Person(){}
Person.prototype.name = 'tyx';
Person.prototype.age = 18
Person.prototype.job = 'teacher'
Person.prototype.sayName = function(){
    console,log(this.name);
}
let person1 = new Person();
let person2 = new Person();
person1.sayName();//'tyx'
person2.sayName();//'tyx'
```

## 组合使用原型模式和构造函数模式
原型模式创建方法，构造函数模式用于属性

缺点：代码封装性一般

```javascript
function Person(name, age){
  this.name = name;
  this.age = age;
}
Person.prototype = {
  constructor: Person
  sayName : function (){}
}
```

## 动态原型模式
**只有在sayname()不存在的时候才会将其放入原型**，减少了新建时的操作，不能使用对象字面量重写prototype属性，否则原先在原型上的属性与方法会被覆盖，但是可以用delete操作符去掉实例与原型的联系

```javascript
function Person(){
  Person.prototype.age = 18;
  Person.prototype.name = 'tyx';
  if(typeof this.sayname !== 'function'){
    Person.prototype.sayName = function(){
      console.log('tyx');
    }
  }
}
```

## 寄生构造函数模式
1. 返回的对象与构造函数或者构造函数的原型无关
2. 不能使用instanceof操作符判断对象类型

```javascript
function Create(name, age, job){
  let o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        console.log(this.name);
    }
    return o;
}
let person1 = new Create('tyx',18,'doctor');
let person2 = new Create('ftf',19,'teacher');
```

## 稳妥构造函数模式
没有公共属性，而且其方法也不引用this的对象。无法识别对象所属类型

```javascript
function Create(name,age,job){
    let o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        console.log(name)
    }
    return o;
}
let person1 = Create('tyx',18,'doctor');
let person2 = Create('ftf',19,'teacher');
```

# 继承
## 原型链继承
引用值共享，改变引用值会影响所有实例；<font style="color:rgb(51, 51, 51);">创建子类型实例时无法向超类型传参。</font>

**<font style="color:rgb(51, 51, 51);">构造函数.prototype = new 被继承构造函数</font>**

```javascript
function Parent(){
  this.name = 'tyx'
}
Parent.prototype.getName = function (){
  console.log(this.name);
}
function Child(){}
Child.prototype = new Parent();
var child1 = new Child();
console.log(child1.getName()) // 'tyx'
```

## 构造函数继承
方法都在构造函数中定义，无法复用函数，在超类型中定义的方法，子类型中不可见

```javascript
function Parent(){
  this.names = ['kevin','daisy'];
}
function Child(){
  Parent.call(this);
}
var child1 = new Child();
child1.names.push('ftf');
console.log(child.names); // ['kevin','daisy','ftf']
var child2 = new Child();
console.log(child2.names);
```

## 组合继承
会调用两次构造函数

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1701185012335-2c1568c8-6bb7-46e1-a91a-4a103e07dd34.png)

## 原型式继承
引用值共享

```javascript
function createObj(o){
  function F(){};
  F.prototype = o;
  return new F();
}
```

## 寄生式继承
```javascript
function createObj(o){
  var clone = Object.create(o);
  clone.sayName = function (){
    console.log('hi');
  }
  return clone;
}
```

## 寄生组合式继承
```javascript
function object(o){
  function F(){};
  F.prototype = o;
  return new F();
}
function prototype(child, parent){
  var prototype = object(parnet.prototype);
  prototype.constructor = child;
  child.prototype = prototype;
}
```

# 属性的可枚举型和遍历
对象的每个属性都有一个描述对象，用来控制该属性的行为。[Object.getOwnPropertyDescriptor](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)方法可以获取该属性的描述对象

目前四个操作会忽略enumerable为false的属性

+ for...in循环：之遍历对象自身的和继承的可枚举的属性（**<font style="color:#DF2A3F;">走原型链</font>**）
+ Object.keys()：返回对象自身的所有可枚举的属性的键名。（**<font style="color:#DF2A3F;">不会走原型链</font>**）
+ JSON.stringify()：只串行化对象自身的可枚举的属性
+ Object.assign()：忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性

可以遍历对象属性的方法：

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695310176194-5bd28aca-3088-4cbd-9572-07ae21ca5c20.png)

# super
super关键字指向当前**对象的原型对象**，只能用在对象的方法之中，用在其他地方会报错

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695310235214-fea17ca2-523d-4216-80ec-189f9e3f4bc1.png)

js引擎内部super.foo等同于Object.getPrototypeOf(this).foo或Object.getPrototypeOf(this).foo.call(this)

# AggregateError错误对象
<font style="color:rgb(13, 20, 30);">AggregateError 在一个错误对象里面，封装了多个错误。如果某个单一操作，同时引发了多个错误，需要同时抛出这些错误，那么就可以抛出一个 AggregateError 错误对象，把各种错误都放在这个对象里面。</font>

<font style="color:rgb(13, 20, 30);">AggregateError()可以接受两个参数：</font>

+ <font style="color:rgb(13, 20, 30);">errors：数组，它的每个成员都是一个错误对象。该参数是必须的。</font>
+ <font style="color:rgb(13, 20, 30);">message：字符串，表示 AggregateError 抛出时的提示信息。该参数是可选的</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1695310527589-a2c8eb60-333c-4c28-9cfd-c26cbfd531c7.png)

# 从输入url页面加载发生了什么即浏览器渲染机制
浏览器渲染过程

+  对url解析：url只能是字母或者数字还有一些其他特殊符号（-_.~ ! * ' ( ) ; : @ & = + $ , / ? # [ ]，不转义的话会出现歧义。比如 `http:www.baidu.com?key=value`,假如我的 `key`本身就包括等于 `=`符号，比如 `ke=y=value`，就会出现歧义，你不知道 `=`到底是连接 `key`和 `value`的符号，还是说本身 `key`里面就有 `=`。 
    - 对url进行编码，URL编码只是简单的在特殊字符的各个字节前加上%
    - url编码的格式采用的是ascii码
+  查找是否有缓存（强制缓存，协商缓存） 
+  DNS解析(网址=>ip地址)，计算机在互联网中唯一的标识是ip地址 

dns优化  
在html头部写入dns缓存地址 `<link rel="dns-prefetch" href="http://bdimg.share.baidu.com" />`

+ tcp连接(三次握手）
+ 发送http请求
+ 服务端处理请求，并且返回http报文
+ 浏览器解析和渲染页面：

浏览器工作流程：构建DOM -> 构建CSSOM -> 构建渲染树 -> 布局 -> 绘制。

    - 浏览器通过http协议请求到服务器，获取到html样式后，自上而下进行解析，**构建dom树，document.readystate = "loading"**——正在加载 

> 浏览器得到一部分就会开始构建dom，不会等到整个文档就位才开始渲染
>

    - 遇到link外部css，创建线程加载，并继续解析文档 
    - 遇到外部js，并且没有设置async或defer，浏览器阻塞并加载，等待js加载完成并执行该脚本，然后继续解析文档，对于设置async，defer的浏览器创建线程加载，并继续解析文档，**async脚本加载完后立即执行** 

> 多个带async属性的标签，不能保证加载的顺序，多个带defer属性的标签，按照加载顺序执行
>

    - 遇到img，先正常解析dom架构，然后 浏览器异步加载src，并继续解析文档 
    - **文档解析完成后，document.readystate =“interactive”**——可交互，并触发domcontentloaded事件，所有设置defer的脚本还会按顺序执行，dom树和cssom树进行关联形成渲染树——rendertree，并绘制在页面上 
    - **所有defer的脚本加载完成并执行后，img等加载完成，document.readystate ="complete"**——完成,window对象触发事件。 

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697533230618-c4ee7064-48ae-4225-b123-f159b9e3073f.png)

+ CSSOM会阻塞渲染，只有当CSSOM构建完毕后才会进入下一个阶段构建渲染树。通常情况下DOM和 CSSOM是并行构建的，但是当浏览器遇到一个不带defer或async属性的script标签时，DOM构建将暂 停，如果此时又恰巧浏览器尚未完成CSSOM的下载和构建，由于JavaScript可以修改CSSOM，所以需 要等CSSOM构建完毕后再执行JS，最后才重新DOM构建。  
+ 连接结束——四次挥手=>为什么三次不可以？简易理解：发完了，知道发完了，收完了，知道收完了 

由于tcp连接时全双工的，每个方向都需要单独进行关闭，收到一个fin后，意味着这个方向没有数 据流动，一个tcp连接在收到一个fin后仍能发送数据。首先进行关闭的一方执行主动关闭，另一方 执行被动关闭。防止服务器还在传输数据，不能断连  

将html文件转换为dom树：字节数据=>字符串=>Token=>Node=>Dom。token中会表示当前token是开始标签或结束标签或文本等信息。**有结束标签标识的Token不会创建节点对象**

构建cssom树：字节数据=>字符串=>Token=>Node=>cssom

CSSOM 提供了接口让JS动态操作 CSS，DOM 提供了接口让JS修改 HTML。cssom会阻塞页面渲染

document.readyState属性描述了document的加载状态  
三种状态：

1. loading：正在加载
2. interactive：文档已经被解析，正在加载状态结束，但是图像，样式表或框架类的子资源仍在加载
3. complete：文档和所有子资源已完成加载，表示load状态的事件即将被触发

# 根据浏览器渲染过程的优化：
[https://juejin.cn/post/6844904195707895816#heading-9](https://juejin.cn/post/6844904195707895816#heading-9)

+ **优化JS**：JavaScript文件加载会阻塞DOM树的构建，可以给 `<script>`标签添加异步属性async，这样浏览器的HTML解析就不会被js文件阻塞。
+ **优化CSS**：浏览器每次遇到 `<link>`标签时，浏览器就需要向服务器发出请求获得CSS文件，然后才继续构建DOM树和CSSOM树，可以合并所有CSS成一个文件，减少HTTP请求，减少关键资源往返加载的时间，优化渲染速度。
+ **加载部分HTML**浏览器先加载主要HTML初始化静态部分，动态变化的HTML内容通过Ajax请求加载。这样可以减少浏览器构建DOM树的工作量，让用户感觉页面加载速度很快。
+ **压缩**：对HTML、CSS、JavaScript这些文件去除冗余字符（例如不必要的注释、空格符和换行符等），再进行压缩，减小文件数据大小，加快浏览器解析文件编码。
+ **图片加载优化**  
1）小图标合并成雪碧图，进而减少img的HTTP请求次数；  
2）图片加载较多时，采用懒加载的方案，用户滚动页面可视区时再加载渲染图片。
+ **http缓存**
+ gzip压缩
+ 去除console.log
+ 预渲染：将浏览器解析 `javascript` 动态渲染页面的这部分工作，在打包阶段就完成了，（只构建了静态数据）换个说法在构建过程中，`webpack` 通过使用 `prerender-spa-plugin` 插件生成静态结构的 `html`
+ 资源预加载：提前加载资源，当用户需要查看时可直接从本地缓存中渲染。

# 防抖和节流
**防抖**

触发事件后在n秒内函数只能执行一次，如果在n秒内又触发了事件，则会重新计算函数执行时间。单位时间内，操作n次，选中最后一次。

特点：延迟==》无限后延，不断刷新定时器

```javascript
// 防抖
function debounce(fn, delay) {
	let timeout = null;
  return function () {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      fn.apply(this, arguments);
    }, delay)
  }
}
```

**节流**

连续触发事件但是在n秒内只执行一次函数，节流会稀释函数的执行频率。单位时间内，操作n次，执行第一次。

特点：只执行一次。设置标识位，看能不能触发事件

```javascript
function throttle(fn, delay) {
  let canrun = false;
  return function () {
    if(!canrun) return;
    canrun = false;
    setTimeout(() => {
      fn.apply(this, arguments);
      canrun = true;
    }, delay)
  }
}
```

# 同步异步
同步：一条线程按顺序执行指令，当客户端发送请求给服务端，在等待服务端响应的请求时，客户端不做其他的事情。当服务端做完了才返回到客户端。这样的话客户端需要一直等待。用户使用起来会有不友好。

异步：创建多条线程执行不同指令，当客户端发送给服务端请求时，在等待服务端响应的时候，客户端可以做其他的事情，这样节约了时间，提高了效率。

# this指向
this<font style="color:rgb(51, 51, 51);">指向执行环境也就是</font>**<font style="color:rgb(51, 51, 51);">指向最后调用它的那个对象</font>**<font style="color:rgb(51, 51, 51);">，在</font>**<font style="color:rgb(51, 51, 51);">标准函数</font>**<font style="color:rgb(51, 51, 51);">中，this引用的是把函数当成方法调用的上下文对象；在</font>**<font style="color:rgb(51, 51, 51);">箭头函数</font>**<font style="color:rgb(51, 51, 51);">中，this引用的是定义箭头函数的上下文。</font>**<font style="color:rgb(51, 51, 51);">当this遇到return时如果返回值是一个对象则this指向返回的对象，如果不是一个对象，this还是指向函数的实例，虽然null也是一个对象，但是其中的this还是指向函数的实例。</font>**<font style="color:rgb(51, 51, 51);">（在全局执行上下文中，this指向全局对象；在函数执行上下文中，this的值取决于该函数是如何调用的，如果被一个引用对象调用，那么this会被设置为那个对象，否则this的值被设置为全局对象或者undefined）</font>

改变this指向：

+ <font style="color:rgb(51, 51, 51);">可以通过call，bind，apply方式改变。 </font>
    - <font style="color:rgb(51, 51, 51);">call()，第一个参数为函数内this的值，参数需要一个一个列出来</font>
    - <font style="color:rgb(51, 51, 51);">apply(),第一个参数为函数内this的值, 第二个参数可以是Array的实例，也可以是arguents对象</font>
    - <font style="color:rgb(51, 51, 51);">bind()，会创建一个新的函数实例，其this值会被绑定到传给bind对象。例如，f.bind(obj)，实际上可以理解为obj.f()，这时，f函数体内的this自然指向的是obj，bind是创建一个新的函数，必须手动去调用</font>
+ <font style="color:rgb(51, 51, 51);">使用箭头函数</font>
+ <font style="color:rgb(51, 51, 51);">在函数内部使用_this = this，即使用变量保存下来</font>
+ <font style="color:rgb(51, 51, 51);">new实例化一个对象</font>

# <font style="color:rgb(51, 51, 51);">new有什么作用，如何用代码实现new</font>
1. 在内存中创建一个新对象
2. 这个新对象内部的[[prototype]]特性被赋值为构造函数的prototype属性
3. 构造函数内部的this被赋值为这个新对象(即this指向新对象)
4. 执行构造函数内部的代码（给新对象添加属性）
5. 如果构造函数返回空对象，则返回该对象；否则，返回刚创建的新对象

> <font style="color:rgb(119, 119, 119);">new一个构造函数，创建一个新对象</font>
>
> <font style="color:rgb(119, 119, 119);">将构造函数的原型对象绑定到新对象上，将构造函数的作用域给新对象——this指向这个新对象</font>
>
> <font style="color:rgb(119, 119, 119);">执行构造函数的代码</font>
>
> <font style="color:rgb(119, 119, 119);">返回新对象</font>
>

```javascript
Fucntion create(con, ...args){
  let obj = {};
  obj._proto_ = con.prototype;
  let result = con.apply(obj, args);
  return result instanceof object ? result : obj; 
}
let a = create(构造函数名称，需要传入的构造函数参数)
```

# js特点
+ js是一种**解释性脚本**语言，代码不进行预编译；
    - 编译型语言：在代码**运行前**编译器将人类可以理解的语言装换成集器可以理解的语言，会先转成可执行文件
    - 解释型语言：将人类可以理解的语言转换成机器可以理解的语言，但是是在**运行时**转换的
+  **跨平台**，在绝大多数浏览器的支持下，可以在多种平台下运行 
+  **弱类型**脚本语言：对使用的数据类型未作出严格要求，可以进行类型转换 
+  **单线程，事件驱动**：js对用户的响应，是以事件驱动的方式进行的。在网页中执行了某种操作中所产生的动作，被称为事件。事件发生后，可能会引起响应事件响应，执行某些对应的脚本，这种机制被称为事件驱动 
+  [**面向对象**](https://blog.csdn.net/jerry11112/article/details/79027834)：将一切都看成是对象，而对象一般都由属性和方法组成。
+  **安全性**：JavaScript是一种安全性语言，它不允许访问本地的硬盘，并不能将数据存入到服务器上，不允许对网络文档进行修改和删除，只能通过浏览器实现信息浏览或动态交互。从而有效地防止数据的丢失。

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697717135056-08c50e37-ca55-46ab-afc3-a9ca5e6b7358.png)

# 函数式编程和类编程
1. 函数式编程：<font style="color:rgb(0, 0, 0);">强调函数的纯粹性和不可变性，以及通过函数的组合和转换来处理数据。</font>

特点：

+ 纯函数：<font style="color:rgb(0, 0, 0);">函数式编程中的函数应该是纯函数，即给定相同的输入，永远产生相同的输出，并且没有副作用。纯函数不依赖于外部状态，这使得代码更加可靠、可测试和易于理解</font>
+ <font style="color:rgb(0, 0, 0);">不可变性：函数式编程倾向于使用不可变数据，即数据在创建后不能被修改。这样可以避免意外的数据修改和共享状态带来的问题，同时也方便并行处理和线程安全。</font>
+ <font style="color:rgb(0, 0, 0);">高阶函数：函数可以作为参数传递给其他函数，也可以作为返回值返回。高阶函数的使用使得代码更加抽象和灵活，可以更好地实现代码的重用和组合。</font>
+ <font style="color:rgb(0, 0, 0);">递归：函数式编程通常使用递归来进行循环和迭代，而不是使用可变状态的循环结构。递归可以提供一种简洁和优雅的方式来处理复杂的问题。</font>
2. 类编程（面向对象编程）：<font style="color:rgb(0, 0, 0);">以类和对象为核心，通过封装、继承和多态等机制来组织和管理代码</font>
+ <font style="color:rgb(0, 0, 0);">类和对象：类是对象的蓝图或模板，用于定义对象的属性和行为。对象是类的实例，通过创建对象来使用和操作数据。</font>
+ <font style="color:rgb(0, 0, 0);">封装：类提供了一种封装数据和方法的机制，将相关的数据和方法组织在一起，同时隐藏内部实现的细节。封装提供了一种信息隐藏和模块化设计的方式。</font>
+ <font style="color:rgb(0, 0, 0);">继承：类可以通过继承机制从其他类派生，继承可以实现代码的重用和扩展。子类可以继承父类的属性和方法，并可以添加自己的特定行为。</font>
+ <font style="color:rgb(0, 0, 0);">多态：多态允许不同的对象对同一消息做出不同的响应。通过多态，可以编写更加通用和灵活的代码，提高代码的可扩展性和可维护性。</font>
3. 面向过程编程：<font style="color:rgb(0, 0, 0);">以过程为中心的编程范式。在面向过程编程中，程序被划分为一个个的过程或函数，每个过程包含了一系列的操作步骤，通过顺序执行这些过程来完成任务。面向过程编程强调程序的执行顺序和控制流程，通常使用全局变量来共享数据。</font>

面向过程可以看作是蛋炒饭，面向对象可以看作是盖浇饭。 

+ 面向过程就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候一个一个依次调用就可以了；编程语言有：汇编语言，c语言等 
    - 优点：编码流程化，便于分析
    - 缺点：**代码重用性低，扩展性差**
+ 面向对象是把构成问题事务分解成各个对象，建立对象的目的不是为了完成一个步骤，而是为了描叙某个事物在整个解决问题的步骤中的行为。编程语言有java，js，python等 
    - 优点：代码结构分析，具备结构化；耦合度低，可重用，易扩展
    - 缺点：**性能低**(没有蛋炒饭香🤭)

# 高阶函数
高阶函数指的是能够接收函数作为参数或返回函数作为结果的函数

如 Array.prototype.map，Array.prototype.filter 和 Array.prototype.reduce 就是高阶函数的实现。

# v8怎么实现的
+  **parse将js代码转换成ast**（抽象语法树） 
+  **ignition[ɪɡˈnɪʃn] 会将ast转换为bytecode字节码同时会收集turbofan优化所需要的信息**(比如函数参数的类型信息) 
+  **turbofan是一个编译器，可以将字节码编译为cpu可以直接执行的机器码**。如果一个函数被多次调用，那么就会标记为热点函数，就会经过turbofan转换成优化的机器码，提高代码的执行性能；  
_机器码实际上也会被还原成bytecode，因为在后续执行函数的过程中，类型发生了变化，之前优化的机器码不能正确的处理，就会逆向转成字节码_ 

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697717233014-0c9a7d66-36a9-4219-8b8e-63d4ca1cd660.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697717242194-bc059a75-c8ff-4c2b-95b8-41318ed02520.png)

# <font style="color:rgb(51, 51, 51);">作用域和执行上下文</font>
<font style="color:rgb(51, 51, 51);">词法作用域：作用域是由书写代码时函数声明的位置决定的。js使用的是词法作用域</font>

<font style="color:rgb(51, 51, 51);">动态作用域：作用域链是基于调用栈的，而不是代码中的作用域嵌套</font>

<font style="color:rgb(51, 51, 51);">变量或函数的上下文决定它可以访问哪些数据，以及他们的行为。作用域则保护内部的变量与环境不被外部访问，定义了变量的代码使用范围，避免命名冲突，为模块化开发提供了便利。</font>

<font style="color:rgb(51, 51, 51);">在调用一个函数时，会为这个函数调用创建一个执行上下文，并创建一个作用域链。然后用arguments和其他命名参数来初始化这个函数的活动对象。外部函数的活动对象是内部函数作用域链上的第二个对象。这个作用域一直向外串起所有包含函数的活动对象，直到全局上下文才终止。</font>

<font style="color:rgb(51, 51, 51);">全局上下文中的叫变量对象，他会在代码执行期间始终存在，而函数局部上下文中的叫活动对象，只在函数执行期间存在。作用域链其实是一个包含指针的列表，每个指针分别指向一个变量对象，但物理上并不会包含相应的对象。</font>

<font style="color:rgb(51, 51, 51);">执行上下文：当前js代码被解析和执行时所在的环境</font>

# <font style="color:rgb(51, 51, 51);">闭包</font>
**<font style="color:rgb(51, 51, 51);">闭包指的是那些那些引用了另一个函数作用域中变量的函数。</font>**

<font style="color:rgb(51, 51, 51);">将执行环境想象为一个栈，栈底为全局执行环境，每检测到一个函数便将其执行环境入栈，由此栈顶的执行环境可以通过向栈底查找的方式寻找到本执行域总不存在的值。每执行一个函数便将其从栈中弹出，导致后续的函数无法访问被弹出函数内部的变量。</font>**<font style="color:rgb(51, 51, 51);">闭包就是将需要保存执行域的函数通过return的方式返回到外部并保存其函数执行域，使外部可以访问函数内部的值</font>**

解决：  
再退出函数之前，将不使用的全局变量全部删除，即手动接触引用赋值为null

<font style="color:rgb(51, 51, 51);">使用场景：</font>

+ <font style="color:rgb(51, 51, 51);">return 返回函数</font>
+ <font style="color:rgb(51, 51, 51);">函数作为参数</font>
+ <font style="color:rgb(51, 51, 51);">iife立即执行函数</font>
+ <font style="color:rgb(51, 51, 51);">定时器setTimeout</font>
+ <font style="color:rgb(51, 51, 51);">所有的回调函数</font>

<font style="color:rgb(51, 51, 51);">作用：</font>

+ <font style="color:rgb(51, 51, 51);">模拟块级作用域，使用立即执行函数IIFE即可</font>
+ <font style="color:rgb(51, 51, 51);">模块化，可以实现对私有变量的封装</font>
+ <font style="color:rgb(51, 51, 51);">可以在函数外部读取到函数内部的变量、</font>
+ <font style="color:rgb(51, 51, 51);">将内部变量始终保存在内存中</font>

注：



+ <font style="color:rgb(51, 51, 51);">引用变量可能会发生变化</font>
+ <font style="color:rgb(51, 51, 51);">this指向问题，闭包函数是在window作用域下执行的，this指向window</font>
+ **内存泄漏**<font style="color:rgb(51, 51, 51);">问题：解决需要主动释放不需要的闭包。</font>_对内存消耗有负面影响，闭包会导致原始作用域链不是放，造成内存泄露_
+ <font style="color:rgb(51, 51, 51);">会污染全局变量</font>

# <font style="color:rgb(51, 51, 51);">内存泄漏</font>
不在用到的内存，没有及时释放就叫做内存泄漏。

产生原因：

+ 意外声明全局变量。函数中的局部变量在函数执行结束之后这些变量已经不再被需要，所以垃圾回收会识别他们并释放，但是对于全局变量，垃圾回收器很难判断什么时候变量才不被需要，所以全局变量通常不会被回收。**在使用全局变量做持续存储大量数据的缓存时，需要设置存储上线并及时清理，不然的话数据量越来越大，内存压力也会越来越高**
+ 定时器的回调函数中通过闭包引用了外部变量，定时器存在，则会产生内存泄露
+ 使用js闭包
+ 没有清理的dom元素引用：DOM 元素的生命周期正常是取决于是否挂载在 DOM 树上，当从 DOM 树上移除时，也就可以被销毁回收了
+ 未清理的console.log输出

<font style="color:rgb(51, 51, 51);">内存泄漏带来的影响：</font>

+ <font style="color:rgb(51, 51, 51);">频繁GC——garbage collect，垃圾回收，gc会阻塞主进程的执行使得页面卡顿</font>
+ <font style="color:rgb(51, 51, 51);">当内存不足以为某些对象分配所需要的空间，会导致程序崩溃，造成体验差</font>

# <font style="color:rgb(51, 51, 51);">垃圾回收</font>
找出不再使用的变量，然后释放掉其占用的内存，但是这个过程不是时时的，因为其开销比较大，所以垃圾回收器会按照固定的时间间隔周期性的执行。

在chrome中，v8被限制了内存的使用（64位约1.4G/1464MB ， 32位约0.7G/732MB），限制原因：

+ v8最初是为浏览器设计的，不太可能会用到使用大量内存的情况
+ v8的垃圾回收的限制——如果清理大量的内存垃圾是很消耗时间的，会引起js线程暂停执行的时间，性能和应用直线下降

垃圾回收器都有一些必须定期完成的任务：

+ 标记：确定存活/死亡对象
+ 清除：回收/再利用死亡对象所占用的内存
+ 整理：压缩/整理内存

<font style="color:rgb(51, 51, 51);">执行环境负责在代码执行时管理内存，垃圾回收程序每个一定时间会自动运行，确定不再使用的变量并释放其内存。常见的两种标记策略：标记清理和引用计数。</font>

<font style="color:rgb(51, 51, 51);">weakset，weakmap是弱引用，即垃圾回收机制不考虑weakset对该对象的引用，如果其他对象都不在引用该对象，那么垃圾回收机制会自动回收改对象所占用的内存。</font>

<font style="color:rgb(51, 51, 51);">标记清理：当变量进入上下文，这个变量会被加上存在于上下文中的标记，当变量离开上下文时，也会被加上离开上下文的标记。eg：变量进入上下文时，反转某一位</font>

_<font style="color:rgb(51, 51, 51);">标记清除：垃圾回收器从根节点开始，标记根直接引用的对象，然后递归标记这些对象的直接引用对象。对象的可达性将作为是否“存活”的依据。</font>_

<font style="color:rgb(51, 51, 51);">引用计数：对每个值都记录他被引用的次数。声明变量并给他赋一个引用值时，这个值的引用数为1，如果同一个值又被赋给另外一个变量，引用数加1，如果保存该值引用的变量被其他值覆盖了，引用数减1。引用值为0时，就没办法再访问这个值了。</font>

<font style="color:rgb(51, 51, 51);">v8采用三色标记来识别内存垃圾，三种颜色通过两个标志位来区分，即白色（00）、灰色（10）、黑色（11）。最初，所有对象都是白色的。标记将从根节点出发，每遍历到一个节点，便将该节点变为灰色。如果某个灰色节点的所有直接子节点都遍历完成，该灰色节点将变为黑色。如果不再有新的灰色节点，则标记结束，剩余的白色节点不可访问，可以被安全回收。</font>

**<font style="color:rgb(51, 51, 51);">世代假说</font>**<font style="color:rgb(51, 51, 51);">（弱分代假说）</font>

<font style="color:rgb(51, 51, 51);">垃圾回收基于世代假说，将内存分为新生代和老生代；</font>

<font style="color:rgb(51, 51, 51);">新生代又分为Nursery和Intermediate</font>

<font style="color:rgb(51, 51, 51);">新生对象会被分配到新生代的Nursery子世代，若对象在第一次垃圾回收中存活，它的标志位将发生改变，就如逻辑上的Intermediate子世代，在物理存储上仍存在于新生代中，如果对象再下一次垃圾回收中再次存活，就会进入老生代。对象从新生代到老生代的过程叫做晋升</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697717559314-b66161d7-4b1d-425d-8488-71a394567d39.png)

v8在新生代使用Parallel scavenge[ˈpærəlel] [ˈskævɪndʒ]算法核心是复制算法——以空间换时间

在老生代中使用标记清除和标记整理进行垃圾回收

_标记整理能够让堆利用的更充分，但是需要额外的扫描时间和对象移动时间，花费的时间与堆的大小成正比_

执行垃圾回收时，不可避免会暂停 JavaScript 的执行。另一方面，为了页面流畅运行，我们通常希望页面能以每秒 60 帧的帧率运行，即每帧约 16ms 渲染间隔。这意味着如果在垃圾回收加上代码执行时间超过 16ms，用户将感受到卡顿的情况。Orinoco 利用了**并行、增量和并发**的技术进行垃圾回收，以释放主线程的压力，使其有更多的时间用于正常的 JavaScript 代码执行。v8使用的是并发

+ **并行**是指将垃圾回收任务分配成工作量大致相等的若干任务，交给主线程和辅助线程同时执行。由于执行过程没有 JavaScript 运行，所以实现较为简单，只需确保线程之间进行同步即可。

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697717581078-3720caac-72d8-4dda-86a2-f73a45cdd5b6.png)

+ **<font style="color:rgb(51, 51, 51);">增量</font>**<font style="color:rgb(51, 51, 51);">是指主线程将原本大量、集中的垃圾回收任务进行拆分，少量、多次间歇性地运行。</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697717605336-99653e05-7a51-4ab0-9edd-4421bb8a2a9a.png)

+ **<font style="color:rgb(51, 51, 51);">并发</font>**<font style="color:rgb(51, 51, 51);">是指主线程保持 JavaScript 执行不中断，辅助线程完全在后台执行垃圾回收。由于涉及主线程和辅助线程的读写竞争，是三种策略中最复杂的一种。</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697717623068-3c61684e-d3de-42ee-aa28-a8b107ffe8f8.png)

# 代码缓存
代码缓存分为cold，warm，hot三个等级

1. 用户首次请求js文件时，chrome将下载该文件并将其提供给**v8进行编译**，并将该文件**缓存到磁盘**中
2. 当用户第二次请求这个 JS 文件时（即 warm run），**chrome将从浏览器缓存中获取该文件，并将其再次交给v8进行编译**，在warm run阶段编译完成后，编译的代码会被**反序列化**，作为**元数据附加到缓存的脚本文件**中。
3. 当用户第三次请求这个文件时，chrome从缓存中获取文件和元数据，并将两者交给v8.v8将跳过编译阶段，直接反序列化元数据

# 尾调用优化
<font style="color:rgb(51, 51, 51);">尾调用指的是某个函数的最后一步是调用另一个函数，只能是直接调用。函数调用会在内存中产生调用帧，所有的调用帧会形成调用栈。尾调用优化即不保存之前的调用帧，只保存最内层的调用帧。</font>

<font style="color:rgb(51, 51, 51);">尾递归:函数调用自身称为递归，如果尾调用自身，则称为尾递归</font>

# <font style="color:rgb(51, 51, 51);">webworker</font>
[滑动验证页面](https://segmentfault.com/a/1190000014938305#:~:text=Web%20Workers%20%E6%9C%80%E4%BD%B3%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF%201%20%E5%B0%84%E7%BA%BF%E8%BF%BD%E8%B8%AA%20%EF%BC%9A%E5%B0%84%E7%BA%BF%E8%BF%BD%E8%B8%AA%E6%98%AF%E4%B8%80%E9%A1%B9%E9%80%9A%E8%BF%87%E8%BF%BD%E8%B8%AA%20%E5%85%89%E7%BA%BF%20%E7%9A%84%E8%B7%AF%E5%BE%84%E4%BD%9C%E4%B8%BA%E5%83%8F%E7%B4%A0%E6%9D%A5%E7%94%9F,3%20%E9%A2%84%E5%8F%96%E6%95%B0%E6%8D%AE%EF%BC%9A%E4%B8%BA%E4%BA%86%E4%BC%98%E5%8C%96%E7%BD%91%E7%AB%99%E6%88%96%E8%80%85%E7%BD%91%E7%BB%9C%E5%BA%94%E7%94%A8%E5%8F%8A%E6%8F%90%E5%8D%87%E6%95%B0%E6%8D%AE%E5%8A%A0%E8%BD%BD%E6%97%B6%E9%97%B4%EF%BC%8C%E4%BD%A0%E5%8F%AF%E4%BB%A5%20...%204%20%E6%B8%90%E8%BF%9B%E5%BC%8F%E7%BD%91%E7%BB%9C%E5%BA%94%E7%94%A8%EF%BC%9A%E5%8D%B3%E4%BD%BF%E5%9C%A8%E7%BD%91%E7%BB%9C%E4%B8%8D%E7%A8%B3%E5%AE%9A%E7%9A%84%E6%83%85%E5%86%B5%E4%B8%8B%EF%BC%8C%E5%AE%83%E4%BB%AC%E5%BF%85%E9%A1%BB%E5%BF%AB%E9%80%9F%E5%8A%A0%E8%BD%BD%E3%80%82...%205%20%E6%8B%BC%E5%86%99%E6%A3%80%E6%9F%A5%EF%BC%9A%E4%B8%80%E4%B8%AA%E5%9F%BA%E6%9C%AC%E7%9A%84%E6%8B%BC%E5%86%99%E6%A3%80%E6%B5%8B%E5%99%A8%E6%98%AF%E8%BF%99%E6%A0%B7%E5%B7%A5%E4%BD%9C%E7%9A%84%EF%BC%8D%E7%A8%8B%E5%BA%8F%E4%BC%9A%E8%AF%BB%E5%8F%96%E4%B8%80%E4%B8%AA%E5%8C%85%20)

<font style="color:rgb(51, 51, 51);">它允许在 Web 程序中并发执行多个 JavaScript脚本，每个脚本执行流都称为一个线程，彼此间互相独立，并且有浏览器中的 JavaScript引擎负责管理。这将使得线程级别的消息通信成为现实。使得在 Web 页面中进行多线程编程成为可能。</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697718996513-7959c065-6ca2-4306-93ee-c3f7b772422f.png)

+ 能够长时间运行（响应）
+ 快速启动和理想的内存消耗
+ 天然的沙箱环境

规范中有三种类型的web workers

+ Dedicated Workers：由主进程实例化并且只能与之进行通信
+ Shared Workrers：以被运行在同源的所有进程访问（不同的浏览的选项卡，内联框架及其它shared workers）。
+ Service Workers：是一个由事件驱动的 worker，它由源和路径组成。它可以控制它关联的网页，解释且修改导航，资源的请求，以及一种非常细粒度的方式来缓存资源以让你非常灵活地控制程序在某些情况下的行为（比如网络不可用）。

原理：

以加载 `.js` 文件的方式实现的，这些文件会在页面中异步加载。

为了在 Web Worker 和 创建它的页面间进行通信，你得使用 `postMessage` 方法或者一个[广播信道](https://link.segmentfault.com/?enc=CreYfJUK3po02NWzyuzn6g%3D%3D.TfwxqayoGOcSTKfLNBLKIKHH2oD7HV9d64%2FtVN5%2BDbZwYyg3MTCg207%2FAll8%2FN0%2FTFZOJzcsYzHZodxU4bG%2BeIjekFF0RKAwxjTlNU2WO7c%3D)。

**局限性：**（不能访问一些非常关键的元素）

+ dom
+ window对象
+ document对象
+ parent对象

# 浏览器内核
浏览器内核指的是浏览器的排版引擎，也称为浏览器引擎，页面渲染引擎或者样板引擎

其构成：

+ **GUI渲染线程：** 
    - HTMLparse解析html
    - css parser解析style数据
    - layout过程，为每个可见节点的几何信息
    - painting过程，遍历rendertree调用ui接口绘制每个节点
+ **js引擎线程**：负责解析js脚本，运行代码
+ **定时器触发线程**：浏览器定时计数器并不是由js引擎技术的，因为js引擎是单线程的，如果出于阻塞状态就会影响计时的准确，因此通过单独线程来即使并触发计时
+ **事件触发线程**：当一个事件被触发时该线程会把事件添加到处理队列的末尾，等待js引擎的处理
+ **异步http请求过程**：XMLHttpRequest 请求会在浏览器中新开一个线程请求， 将检测到状态变更时，如果设置有回调函数，异步线程就产生状态变更事件放到 JavaScript 引擎的处理队列中

**Gecko**：早期被netscape和mozilla firefox浏览器使用

**Trident**：微软开发，被IE4~IE11浏览器使用，但是edge浏览器已经转向blink。

**webkit**：苹果基于KHTML开发，开源的，用于safari，google chrome之前也在使用

**Blink**：是webkit的一个分支，google开发，目前应用于google chrome，edge，opera等

我们编写的js无论交给浏览器执行还是node执行，最后都是被cpu执行的，所以需要js引擎讲js代码翻译成cpu来执行

常见的js引擎：

+ **spiderMonkey**：第一款js引擎
+ **chakra**[ˈtʃʌkrə]：微软开发，用于it浏览器
+ **jscore**：webkit中的js引擎，apple公司开发
+ **v8**：google开发的js引擎

webkit由两部分组成：

+ webcore：负责解析html，布局，渲染等
+ jscore：解析，执行js代码

小程序中编写的js代码就是被jscore执行的

# js执行机制
> [js引擎的执行过程（一） | Heying Ye’s Personal Website](https://heyingye.github.io/2018/03/19/js%E5%BC%95%E6%93%8E%E7%9A%84%E6%89%A7%E8%A1%8C%E8%BF%87%E7%A8%8B%EF%BC%88%E4%B8%80%EF%BC%89/)
>

js引擎执行过程分为三个阶段：

1. **语法分析**：分析该js脚本代码块的语法是否正确，如果出现不正确，则向外抛出一个**语法错误（SyntaxError）**，停止该js代码块的执行，然后继续查找并加载下一个代码块；如果语法正确，则进入预编译
2. **预编译阶段**：创建go对象，找形参和变量声明，赋值为undefined；将实参和形参相统一；找函数声明并赋值函数体

运行环境： 

    - 全局环境：js代码加载完毕后，进入代码预编译即进入全局环境
    - 函数环境：函数调用时，进入该函数环境，不同的函数则函数环境不同
    - eval：不建议使用，由安全，性能等问题

函数调用栈：使用栈存取的方式进行管理运行环境 

创建执行上下文： 	

    1. **创建变量对象**：创建变量对象发生在预编译阶段，但尚未进入执行阶段，该变量对象都是不能访问的，因为此时的变量对象中的变量属性尚未赋值，值仍为undefined，只有进入执行阶段，变量对象中的变量属性进行赋值后，变量对象（Variable Object）转为活动对象（Active Object）后，才能进行访问，这个过程就是VO –> AO过程。vo：变量对象，存储了在上下文中定义的变量和函数声明，无法访问，必须是js中以var声明的变量才会记录在这里，let或者const声明的变量不会存在，必须是显式声明的函数，函数表达式不会被记录 
        * 创建arguments对象，检查当前上下文的参数，建立该对象的属性与属性值，仅在函数环境(非箭头函数)中进行，全局环境没有此过程
        * 检查当前上下文的函数声明
        * 检查当前上下文的变量声明 
    2. **建立作用域链**：作用域链由当前执行环境的变量对象（未进入执行阶段前）与上层环境的一系列活动对象组成，它保证了当前执行环境对符合访问权限的变量和函数的有序访问。作用域链的第一项永远是当前作用域(当前上下文的变量对象或活动对象)，最后一项永远是全局作用域（全局执行上下文的活动对象）
    3. **确定this指向** 	
3. **执行阶段**：

事件循环：

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1697719684957-93fe2962-811f-45a8-8f4a-0c89998879cf.png)

<font style="color:rgb(51, 51, 51);">首先，整体的script(作为第一个宏任务)开始执行的时候，会把所有代码分为同步任务，异步任务两部分，同步任务会直接进入主线程依次执行，异步任务会再分为宏任务和微任务，宏任务进入到Event Table中，并在里面注册回调函数，每当指定的事件完成时，Event Table会将这个函数移到Event Queue中。微任务也会进入到另一个Event Table中，并在里面注册回调函数，每当指定的事件完成时，Event Table会将这个函数移到Event Queue中。当主线程内的任务执行完毕，主线程为空时，会检查微任务的Event Queue，如果有任务，就全部执行，如果没有就执行下一个宏任务，该过程不断重复。event table：异步流程</font>

<font style="color:rgb(51, 51, 51);background-color:rgb(243, 244, 244);">await</font><font style="color:rgb(51, 51, 51);"> 以前的代码，相当于与 </font><font style="color:rgb(51, 51, 51);background-color:rgb(243, 244, 244);">new Promise</font><font style="color:rgb(51, 51, 51);"> 的同步代码，</font><font style="color:rgb(51, 51, 51);background-color:rgb(243, 244, 244);">await</font><font style="color:rgb(51, 51, 51);"> 以后的代码相当于 </font><font style="color:rgb(51, 51, 51);background-color:rgb(243, 244, 244);">Promise.then</font><font style="color:rgb(51, 51, 51);">的异步</font>

<font style="color:rgb(51, 51, 51);">常见微任务：process.nextTick ()-Node；Promise.then()；catch；finally；Object.observe；MutationObserver</font>

<font style="color:rgb(51, 51, 51);">常见宏任务：主代码块；setTimeout；setInterval；setImmediate ()-Node；requestAnimationFrame ()-浏览器</font>

```javascript
const { resolve } = require("path");

console.log('1');

setTimeout(function(){
    console.log('2');
    process.nextTick(function(){
        console.log('3');
    })
    new Promise(function(reslove){
        console.log('4');
        reslove();
    }).then(function(){
        console.log('5');
    })
});

process.nextTick(function(){
    console.log('6');
})

new Promise(function(reslove){
    console.log('7');
    resolve()
}).then(function(){
    console.log('8');
})

setTimeout(function(){
    console.log('9');
    process.nextTick(function(){
        console.log('10');
    })

    new Promise(function(reslove){
        console.log('11');
        resolve()
    }).then(function(){
        console.log('12');
    })
});
// 1 7 6 8 2 4 3 5 9 11 10 12
```

# <font style="color:rgb(51, 51, 51);">var,let与const</font>
+ var声明的变量存在变量提升的情况，let，const声明的变量不存在变量提升，只在let或const声明的代码块内有效
+ let不允许在相同作用域内重复声明,会报错
+ 存在暂时性死区，即在代码块内，**let，const之前使用变量会报错**
+ const声明之后需要初始化，const声明的变量即指向其地址，修改地址会出错

## <font style="color:rgb(51, 51, 51);">为什么let，const不能重复声明</font>
词法环境分为两个部分：  
环境记录以及对外部词法环境引用  
es6中存在全局环境变量记录Global Environment Records，其中包括两部分：

+ object environment record：对象式环境记录：主要用于with和global的词法环境。
+ declarative environment record：声明式环境记录。用来记录直接有标识符定义的元素，比如变量、常量、let、class、module、import以及函数声明。 
    - function environment record：函数环境记录
    - module environment record：模块环境记录

函数声明和使用var声明的变量会添加进入Object Enviroment Record中。

使用let声明和使用const声明的变量会添加入Declarative Enviroment Record中。  
使用了let和const声明时，引擎会同时检查Object Enviroment Record和Declarative Enviroment Record是否有该变量，如果有，则报错，否则将将变量添加入Declarative Enviroment Record中。

# <font style="color:rgb(51, 51, 51);">js性能优化</font>
1. 作用域链深层次对象局部化：如果某个跨作用域的值被引用一次以上，则将其存储于局部变量
2. 不改变执行时的作用域链：避免使用with语句，再catch语句中将错误委托给一个函数来处理，永远不要使用eval语句，如果一定要使用eval，用window.Function代替
3. 避免使用闭包
4. 对象深层次成员局部化：如果某个对象的属性被多次读取，则将属性值存储于局部变量
5. 适当在原型上增加方法：在实例调用方法时，若频繁创建实例，则应在原型上调用目标方法；若只创建一次，但需频繁执行内置方法，则应将目标方法创建于构造函数。
6. DOM方法代替innerHTML
7. 文档碎片增加节点：使用createDocumentFragment创建文档碎片，批量修改DOM，减少重排和重绘。
8. 事件绑定
9. 流程控制：4种循环类型：for，while，do-while，for-in，避免使用for-in，会搜索实例和原型，长生更多性能开销，唯一使用for-in的场景是迭代一个属性数量未知的对象 
    1. 使用forEach循环：数据量较少时，优先使用forEach，其次是for，便秘那使用for-in，大量数据使用for
    2. switch总比if-else快：条件数量大时用switch，数量少时用if-else
10. 避免使用+=，因为会在内存中创建一个临时字符串，进行字符串操作后讲结果赋值给目标变量
11. 数组项合并推荐使用join
12. 提高加载性能。因为js下载会阻塞其他资源的下载，尽管脚本下载不会相互影响，但是页面必须等待所有js代码下载并执行完成才能继续，推荐将所有js文件放在body标签底部以减少对整个页面的影响

# json
json是一种轻量级的数据交换格式，完全独立于语言的文本格式

json中有三种结构：

+ <font style="color:rgb(51, 51, 51);">简单值：字符串，数值，布尔值和null，undefined不可以</font>
+  对象： 

```javascript
var packjson = {
    "name" = "tyx",
    "age" = "18"
}
```

+  数组： 

```javascript
var packjson = [
    {
        "name":"tpf",
        "age":"19"
    },{
        "name":"tyx",
        "age":"18"
    }
]
```

json.parse(str)——字符串转对象

json.stringify(<font style="color:rgb(51, 51, 51);">要序列化的对象，过滤器，用于缩进结果的json字符串的选项</font>)——对象转字符串

<font style="color:rgb(51, 51, 51);">json.parse(json.stringify)——深克隆</font>

+ <font style="color:rgb(51, 51, 51);">对象中不能有函数，否则无法序列化，会被忽略</font>
+ <font style="color:rgb(51, 51, 51);">对象中不能有undefined，否则无法序列化，会被忽略</font>
+ <font style="color:rgb(51, 51, 51);">对象中不能有正则，否则无法序列化，克隆后为空</font>
+ <font style="color:rgb(51, 51, 51);">date数据类型会被转化成字符串类型</font>
+ <font style="color:rgb(51, 51, 51);">对象不能是环状结构，否则会报错</font>

# <font style="color:rgb(51, 51, 51);">dom事件流</font>
什么是流：流是对输入输出设备的抽象，程序的角度说流是有方向的数据

事件流所描述的是从页面中接收事件的顺序

dom事件流三个阶段：

+ 事件捕获
+ 到达目标
+ 事件冒泡

现在chrome使用的是事件冒泡处理事件流，如果使用事件捕获需将[addeventlistener](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener)最后的参数设置为true，默认为false事件冒泡

阻止事件冒泡：event.stopPropagation()

**事件委托**：一般来讲，会把一个或者一组元素的事件委托到它的浮层或者更外层元素上，真正绑定事件的是外层元素，当事件响应到需要绑定的元素上时，会通过事件冒泡机制从而触发它的外层元素的绑定事件上，然后在外层元素上去执行函数。

**事件冒泡**：event.stopPropagation()——阻止事件冒泡，事件的触发响应会从最底层目标一层层地向外到最外层（根节点）

**事件捕获**：事件会从最外层开始发生，直到最具体的元素。

# <font style="color:rgb(51, 51, 51);">dom0，dom2，dom3</font>
+ dom0是通过onclick卸载html中的事件

清理该事件只需要给事件赋值null，同一个元素的同种事件只能绑定一个函数

+ dom2是通过addeventlistener绑定的事件

清除时使用removeeventlistener  
参数一为事件名，二为事件处理函数，第三个参数true表示捕获阶段，false为冒泡阶段

+ dom3在dom2事件的基础上加了很多事件类型

# 跨域
<font style="color:rgb(51, 51, 51);">跨域是为了防止用户读取到另一个域名下的内容，ajax可以获取响应，浏览器认为不安全，所以拦截了响应。同源策略：是一种约定，浏览器最核心最基本的安全功能，如果缺少了同源策略，浏览器容易受到xss，csrf等攻击，同源指的是</font>**<font style="color:rgb(51, 51, 51);">协议+域名+端口</font>**<font style="color:rgb(51, 51, 51);">三者相同，即便两个不同的域名指向同一个ip地址，也非同源</font>

<font style="color:rgb(51, 51, 51);">限制内容：</font>

+ <font style="color:rgb(51, 51, 51);">Cookie、LocalStorage、IndexedDB 等存储性内容</font>
+ <font style="color:rgb(51, 51, 51);">DOM 节点</font>
+ <font style="color:rgb(51, 51, 51);">AJAX 请求发送后，结果被浏览器拦截了</font>

<font style="color:rgb(51, 51, 51);">但是有三个标签是允许跨域加载资源：</font>

+ <font style="color:rgb(167, 167, 167);"><link href=XXX></font>
+ <script src=XXX>

<font style="color:rgb(51, 51, 51);">注：</font>

1. <font style="color:rgb(51, 51, 51);">如果协议和端口造化的跨域问题前台是无能为力的</font>
2. <font style="color:rgb(51, 51, 51);">在跨域问题上，仅仅是通过url首部来识别而不会根据对应的ip地址是否相同来判断。url首部可以了理解为协议，域名和端口必须匹配</font>

<font style="color:rgb(51, 51, 51);">CORS支持所有类型的HTTP请求，是跨域HTTP请求的根本解决方案</font>

<font style="color:rgb(51, 51, 51);">JSONP只支持GET请求，JSONP的优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。</font>

<font style="color:rgb(51, 51, 51);">不管是Node中间件代理还是nginx反向代理，主要是</font>**<font style="color:rgb(51, 51, 51);">通过同源策略对服务器不加限制。</font>**

<font style="color:rgb(51, 51, 51);">日常工作中，用得比较多的跨域方案是cors和nginx反向代理</font>

## <font style="color:rgb(51, 51, 51);">jsonp</font>
<font style="color:rgb(51, 51, 51);">利用</font><font style="color:rgb(167, 167, 167);"><script></font><font style="color:rgb(51, 51, 51);">标签没有跨域限制的漏洞，网页可以得到其他来源的动态产生的json数据，jsonp请求一定需要对方的服务器做支持才可以jsonp实际上就是请求一段js脚本，将这段脚本的结果当作数据创建一个script标签，再把需要请求的api地址方法到src里</font>

<font style="color:rgb(51, 51, 51);">网页通过添加一个</font><font style="color:rgb(167, 167, 167);"><script></font><font style="color:rgb(51, 51, 51);">元素，向服务器请求json数据，服务器收到请求后，将数据放在指定的回调函数里传回来</font>

<font style="color:rgb(51, 51, 51);">jsonp与ajax相比：</font><font style="color:rgb(51, 51, 51);">都是客户端向服务端发送请求，从服务端获取数据的方式，但ajax属于同源策略，jsonp属于非同源策略</font>

<font style="color:rgb(51, 51, 51);">优缺点：</font><font style="color:rgb(51, 51, 51);">简单兼容性好，可用于解决主流浏览器的跨域数据访问的问题，缺点是仅支持get方法具有局限性，不安全可能会受到xss攻击，难以确定jsonp请求是否失败，可以通过使用定时器指定响应的允许时间，超出时间认为相应失败</font>

## <font style="color:rgb(51, 51, 51);">cors</font>
<font style="color:rgb(51, 51, 51);">它允许浏览器向跨源服务器，发出</font>[XMLHttpRequest](https://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html)<font style="color:rgb(51, 51, 51);">请求，从而克服了AJAX只能</font>[同源](https://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)<font style="color:rgb(51, 51, 51);">使用的限制。</font>

<font style="color:rgb(51, 51, 51);">服务端设置</font>**<font style="color:rgb(51, 51, 51);">Access-Control-Allow-Origin</font>**<font style="color:rgb(51, 51, 51);"> 就可以开启 CORS。盖属性表示哪些域名可以访问资源，如果设置通配符则表示所有网站都可以访问资源</font>

<font style="color:rgb(51, 51, 51);">简单请求：</font><font style="color:rgb(51, 51, 51);">条件一：使用get/post/head</font><font style="color:rgb(51, 51, 51);">条件3二：Content-Type的值为text/plain或multipart/form-data或application/x-www-form-urlencoded</font>

<font style="color:rgb(51, 51, 51);">复杂请求：</font><font style="color:rgb(51, 51, 51);">复杂请求得cors会在正式通信前，增加一次http请求，成为预检请求，该请求是option方法的，通过该请求来指导服务端是否允许跨域请求</font>

_<font style="color:rgb(51, 51, 51);">预检请求：浏览器先询问服务器，当前所在的域名是否在服务器的许可名单中，以及可以使用哪些http动词和头信息字段，只有得到肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则就报错。</font>_

+ <font style="color:rgb(51, 51, 51);">使用put或delete</font>
+ <font style="color:rgb(51, 51, 51);">发送json格式的数据</font>
+ **<font style="color:rgb(51, 51, 51);">请求中带有自定义头部</font>**

<font style="color:rgb(51, 51, 51);">Access-Control-Allow-Origin设置为*通配符时，表示接受任意域名的请求</font>

<font style="color:rgb(51, 51, 51);">Access-Control-Allow-Credentials值是一个布尔值，表示是否允许发送cookie，默认情况下，cookie不包括在cors请求之中。设为</font><font style="color:rgb(51, 51, 51);background-color:rgb(243, 244, 244);">true</font><font style="color:rgb(51, 51, 51);">，即表示服务器明确许可，Cookie可以包含在请求中，一起发给服务器。这个值也只能设为</font><font style="color:rgb(51, 51, 51);background-color:rgb(243, 244, 244);">true</font><font style="color:rgb(51, 51, 51);">，如果服务器不要浏览器发送Cookie，删除该字段即可。</font>

## <font style="color:rgb(51, 51, 51);">postmessage</font>
<font style="color:rgb(51, 51, 51);">postMessage是HTML5 XMLHttpRequest Level 2中的API，且是为数不多可以跨域操作的window属性之一，允许来自不同源得脚本采用异步方式进行有限的通信，可以实现跨文档，多窗口，跨域消息传递。它可用于解决以下方面的问题：</font>

+ <font style="color:rgb(51, 51, 51);">页面和其打开的新窗口的数据传递</font>
+ <font style="color:rgb(51, 51, 51);">多窗口之间消息传递</font>
+ <font style="color:rgb(51, 51, 51);">页面与嵌套的iframe消息传递</font>
+ <font style="color:rgb(51, 51, 51);">上面三个场景的跨域数据传递</font>

<font style="color:rgb(51, 51, 51);">otherWindow.postMessage(message, targetOrigin, [transfer]);</font>

## <font style="color:rgb(51, 51, 51);">websocket</font>
[一文吃透 WebSocket 原理 刚面试完，趁热赶紧整理 - 掘金](https://juejin.cn/post/7020964728386093093)

<font style="color:rgb(51, 51, 51);">h5提供的一种浏览器与服务器进行</font>**<font style="color:rgb(51, 51, 51);">全双工通信</font>**<font style="color:rgb(51, 51, 51);">的网络技术，属于</font>**<font style="color:rgb(51, 51, 51);">应用层</font>**<font style="color:rgb(51, 51, 51);">协议。</font>

1. <font style="color:rgb(51, 51, 51);">单向通信/单工通信：只能由一个方向的通信而没有反方向的交互</font>
2. <font style="color:rgb(51, 51, 51);">双向交替通信/半双工通信：通信双发都可以发送信息，但不能双方同时发送</font>
3. <font style="color:rgb(51, 51, 51);">双向同时通信/</font>**<font style="color:rgb(51, 51, 51);">全双工通信</font>**<font style="color:rgb(51, 51, 51);">：通信双发可以同时发送和接收信息</font>

<font style="color:rgb(51, 51, 51);">特点：</font>

<font style="color:rgb(51, 51, 51);">（1）</font>**<font style="color:rgb(51, 51, 51);">建立在 TCP 协议之上</font>**<font style="color:rgb(51, 51, 51);">，服务器端的实现比较容易。</font><font style="color:rgb(51, 51, 51);">（2）与 HTTP 协议有着良好的兼容性。默认端口也是80和443，并且握手阶段采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。</font>

<font style="color:rgb(51, 51, 51);">（3）数据格式比较轻量，性能开销小，通信高效。</font>

<font style="color:rgb(51, 51, 51);">（4）</font>**<font style="color:rgb(51, 51, 51);">可以发送文本，也可以发送二进制数据。</font>**

<font style="color:rgb(51, 51, 51);">（5）没有同源限制，客户端可以与任意服务器通信。</font>

<font style="color:rgb(51, 51, 51);">（6）协议标识符是ws（如果加密，则为wss），服务器网址就是 URL。</font>

<font style="color:rgb(51, 51, 51);">一些api：</font>

+ <font style="color:rgb(51, 51, 51);">webSocket.onopen：连接成功后的回调函数</font>
+ <font style="color:rgb(51, 51, 51);">webSocket.onclose：用于指定连接关闭后的回调函数</font>
+ <font style="color:rgb(51, 51, 51);">webSocket.onmessage:用于指定收到服务器数据后的回调函数</font>
+ <font style="color:rgb(51, 51, 51);">webSocket.send():用于向服务器发送数据</font>

### <font style="color:rgb(51, 51, 51);">原理</font>
<font style="color:rgb(51, 51, 51);">具体实现是通过http协议建立通道，然后再此基础上使用websocket协议进行通信。</font>

<font style="color:rgb(51, 51, 51);">过程：</font>

<font style="color:rgb(51, 51, 51);">客户端发起http请求，经过三次握手之后，建立起tcp链接，http请求里存放websocket支持的版本号信息，如：Upgrade、Connection、WebSocket-Version等；然后，服务器收到客户端的握手请求后，同样采用http协议反馈数据，最后，客户端收到连接成功的消息后，开始借助于tcp传输信道进行全双工通信。</font>

### <font style="color:rgb(51, 51, 51);">断线重连</font>
<font style="color:rgb(51, 51, 51);">当客户端第一次发请求至服务端时会携带唯一标识，以及时间戳，服务端到db或者缓存去查询该请求的唯一标识，如果不存在就存入db或者缓存中</font>

<font style="color:rgb(51, 51, 51);">第二次客户端定时再次发送请求依旧携带唯一标识，以及时间戳，服务端到db或者缓存中去查询该请求的唯一标识，如果存在就把上次的时间戳拿取出来，使用当前的时间戳减去上次的时间，得出的毫秒数判断是否大于指定的时间，小于的话就是在线，否则就是离线</font>

<font style="color:rgb(51, 51, 51);">断线原因：</font>

+ <font style="color:rgb(51, 51, 51);">websocket超时没有消息自动断开连接</font>
+ <font style="color:rgb(51, 51, 51);">websocket异常包括服务端出现中断，交互切屏等等客户端异常中断等等</font>
    - <font style="color:rgb(51, 51, 51);">解决方法:引入reconnecting-websocket.min.js</font>

**<font style="color:rgb(51, 51, 51);">解决：</font>**

+ <font style="color:rgb(51, 51, 51);">修改nginx配置信息</font>
+ <font style="color:rgb(51, 51, 51);">websocket发送心跳包</font>
    - <font style="color:rgb(51, 51, 51);">客户端每隔一个时间间隔发生一个探测包给服务器</font>
    - <font style="color:rgb(51, 51, 51);">客户端发包时启动一个超时定时器</font>
    - <font style="color:rgb(51, 51, 51);">服务器端接收到检测包，应该回应一个包</font>
    - <font style="color:rgb(51, 51, 51);">如果客户机收到服务器的应答包，则说明服务器正常，删除超时定时器</font>
    - <font style="color:rgb(51, 51, 51);">如果客户端的超时定时器超时，依然没有收到应答包，则说明服务器挂了</font>

## <font style="color:rgb(51, 51, 51);">node中间件代理</font>
<font style="color:rgb(51, 51, 51);">实现原理：同源策略式浏览器需要遵循的标准，而如果服务器向服务器请求就毋须遵循同源策略。代理服务器需要做以下几个步骤：</font>

1. <font style="color:rgb(51, 51, 51);">接收客户端请求</font>
2. <font style="color:rgb(51, 51, 51);">将请求转发给服务器</font>
3. <font style="color:rgb(51, 51, 51);">拿到服务器响应数据</font>
4. <font style="color:rgb(51, 51, 51);">将相应转发给客户端</font>

## <font style="color:rgb(51, 51, 51);">nginx反向代理</font>
<font style="color:rgb(51, 51, 51);">搭建一个中转ngnix服务器，用于转发请求，使用nginx反向代理实现跨域是最简单的跨域方式，只需要修改nginx的配置即可解决跨域问题，支持所有浏览器，支持session，不需要修改任何代码，并且不会影响服务器性能】</font>

<font style="color:rgb(51, 51, 51);">实现：</font><font style="color:rgb(51, 51, 51);">通过nginx配置一个代理服务器（域名与domain1相同，端口不同）做跳板机，反向代理访问domain2接口，并且可以顺便修改cookie中domain信息，方便当前域cookie写入，实现跨域登录。</font>

<font style="color:rgb(51, 51, 51);">使用反向代理最主要的两个原因：</font>

1. <font style="color:rgb(51, 51, 51);">安全及权限。可以看出，使用反向代理后，用户端将无法直接通过请求访问真正的内容服务器，而必须首先通过Nginx。可以通过在Nginx层上将危险或者没有权限的请求内容过滤掉，从而保证了服务器的安全。</font>
2. <font style="color:rgb(51, 51, 51);">负载均衡。例如一个网站的内容被部署在若干台服务器上，可以把这些机子看成一个集群，那么Nginx可以将接收到的客户端请求“均匀地”分配到这个集群中所有的服务器上（内部模块提供了多种负载均衡算法），从而实现服务器压力的负载均衡。</font>

<font style="color:rgb(51, 51, 51);">nginx还带有健康检查功能（服务器心跳检查），会定期轮询向集群里的所有服务器发送健康检查请求，来检查集群中是否有服务器处于异常状态，一旦发现某台服务器异常，那么在以后代理进来的客户端请求都不会被发送到该服务器上（直到后面的健康检查发现该服务器恢复正常），从而保证客户端访问的稳定性。</font>

## <font style="color:rgb(51, 51, 51);">window.name+iframe</font>
<font style="color:rgb(51, 51, 51);">name值在不同的页面加载后依旧存在，并且可以支持非常长的name值</font>

## <font style="color:rgb(51, 51, 51);">location.hash+iframe</font>
<font style="color:rgb(51, 51, 51);">实现原理：</font><font style="color:rgb(51, 51, 51);">a.html欲与c.html跨域相互通信，通过中间页b.html来实现，不同域之间利用iframe的location.hash船只，相同域之间直接js访问来通信</font>

## <font style="color:rgb(51, 51, 51);">document.domain+iframe</font>
<font style="color:rgb(51, 51, 51);">该方式值用于二级域名相同的情况下。</font><font style="color:rgb(51, 51, 51);">实现原理：两个页面都通过js强制设置document.domain为基础主语，就实现了同域</font>

## <font style="color:rgb(51, 51, 51);">samesite </font>
<font style="color:rgb(51, 51, 51);">cookie的samesite属性用来限制第三方cookie，从而减少安全风险</font>

<font style="color:rgb(51, 51, 51);">可以设置三个值：</font>

+ <font style="color:rgb(51, 51, 51);">strict：完全禁止第三方cookie,跨站点时，任何情况都不会发送cookie。只有当前网页的url与请求目标一致，才会携带上cookie</font>
+ <font style="color:rgb(51, 51, 51);">lax：导航到目标网址的get请求除外</font>

| **<font style="color:rgb(51, 51, 51);">请求类型</font>** | **<font style="color:rgb(51, 51, 51);">示例</font>** | **<font style="color:rgb(51, 51, 51);">正常情况</font>** | **<font style="color:rgb(51, 51, 51);">lax</font>** |
| :--- | --- | :--- | :--- |
| <font style="color:rgb(51, 51, 51);">链接</font> | <a href="..."></a> | <font style="color:rgb(51, 51, 51);">发送 Cookie</font> | <font style="color:rgb(51, 51, 51);">发送 Cookie</font> |
| <font style="color:rgb(51, 51, 51);">预加载</font> | <font style="color:rgb(167, 167, 167);"><link rel="prerender" href="..."/></font> | <font style="color:rgb(51, 51, 51);">发送 Cookie</font> | <font style="color:rgb(51, 51, 51);">发送 Cookie</font> |
| <font style="color:rgb(51, 51, 51);">get表单</font> | <font style="color:rgb(167, 167, 167);"><form method="GET" action="..."></font> | <font style="color:rgb(51, 51, 51);">发送 Cookie</font> | <font style="color:rgb(51, 51, 51);">发送 Cookie</font> |
| <font style="color:rgb(51, 51, 51);">post表单</font> | <font style="color:rgb(167, 167, 167);"><form method="POST" action="..."></font> | <font style="color:rgb(51, 51, 51);">发送 Cookie</font> | <font style="color:rgb(51, 51, 51);">不发送</font> |
| <font style="color:rgb(51, 51, 51);">iframe</font> | <font style="color:rgb(167, 167, 167);"><iframe src="..."></font><font style="color:rgb(167, 167, 167);"></iframe></font> | <font style="color:rgb(51, 51, 51);">发送 Cookie</font> | <font style="color:rgb(51, 51, 51);">不发送</font> |
| <font style="color:rgb(51, 51, 51);">ajax</font> | <font style="color:rgb(51, 51, 51);">$.get("...")</font> | <font style="color:rgb(51, 51, 51);">发送 Cookie</font> | <font style="color:rgb(51, 51, 51);">不发送</font> |
| <font style="color:rgb(51, 51, 51);">image</font> |  | <font style="color:rgb(51, 51, 51);">发送 Cookie</font> | <font style="color:rgb(51, 51, 51);">不发送</font> |


+ <font style="color:rgb(51, 51, 51);">none：Chrome 计划将</font><font style="color:rgb(51, 51, 51);background-color:rgb(243, 244, 244);">Lax</font><font style="color:rgb(51, 51, 51);">变为默认设置。这时，网站可以选择显式关闭</font><font style="color:rgb(51, 51, 51);background-color:rgb(243, 244, 244);">SameSite</font><font style="color:rgb(51, 51, 51);">属性，将其设为</font><font style="color:rgb(51, 51, 51);background-color:rgb(243, 244, 244);">None</font><font style="color:rgb(51, 51, 51);">。不过，前提是必须同时设置</font><font style="color:rgb(51, 51, 51);background-color:rgb(243, 244, 244);">Secure</font><font style="color:rgb(51, 51, 51);">属性（Cookie 只能通过 HTTPS 协议发送），否则无效。</font>

**<font style="color:rgb(51, 51, 51);">只要两个 URL 的 eTLD+1 有效顶级域名相同即可，不需要考虑协议和端口。其中，eTLD 表示有效顶级域名</font>**<font style="color:rgb(51, 51, 51);">。同源是协议域名端口，同站的话是只考虑域名，不考虑协议和端口</font>

# <font style="color:rgb(51, 51, 51);">深浅克隆</font>
[彻底讲明白浅拷贝与深拷贝](https://www.jianshu.com/p/35d69cf24f1f)

## <font style="color:rgb(51, 51, 51);">深克隆</font>
<font style="color:rgb(51, 51, 51);">主要是将一个对象的属性拷贝并存储至自己开辟的内存区域，不受外界干扰：</font>

+ <font style="color:rgb(51, 51, 51);">判断是不是原始值</font>
+ <font style="color:rgb(51, 51, 51);">判断是数组还是对象instanceof/toString/constructor</font>
+ <font style="color:rgb(51, 51, 51);">建立相应的而数组或对象</font>
+ <font style="color:rgb(51, 51, 51);">递归</font>

**<font style="color:rgb(51, 51, 51);">方法</font>**<font style="color:rgb(51, 51, 51);">：</font>

+ <font style="color:rgb(51, 51, 51);">json.parse(json.stringify)</font>

> json只支持object,array,string,number,true,false,null，其他的比如函数，undefined，Date，RegExp等数据类型都不支持。对于它不支持的数据都会直接忽略该属性
>

    - <font style="color:rgb(51, 51, 51);">对象中不能有函数，否则无法序列化，会被忽略</font>
    - <font style="color:rgb(51, 51, 51);">对象中不能有undefined，否则无法序列化，会被忽略</font>
    - <font style="color:rgb(51, 51, 51);">对象中不能有正则，否则无法序列化，克隆后为空</font>
    - <font style="color:rgb(51, 51, 51);">date数据类型会被转化成字符串类型</font>
    - <font style="color:rgb(51, 51, 51);">对象不能是环状结构，否则会报错</font>
+ <font style="color:rgb(51, 51, 51);">库函数lodash</font>
+ <font style="color:rgb(51, 51, 51);">手写递归</font>

> [这一次彻底掌握深拷贝 - 掘金](https://juejin.cn/post/6889327058158092302)
>

```javascript
function deepClone(target,cache = new Map()){
  if(cache.get(target)){
    return cache.get(target)
  }
  if(target instanceof Object){
    let dist ;
    if(target instanceof Array){
      // 拷贝数组
      dist = [];
    }else if(target instanceof Function){
      // 拷贝函数
      dist = function () {
        return target.call(this, ...arguments);
      };
    }else if(target instanceof RegExp){
      // 拷贝正则表达式
      dist = new RegExp(target.source,target.flags);
    }else if(target instanceof Date){
      dist = new Date(target);
    }else{
      // 拷贝普通对象
      dist = {};
    }
    // 将属性和拷贝后的值作为一个map
    cache.set(target, dist);
    for(let key in target){
      // 过滤掉原型身上的属性
      if (target.hasOwnProperty(key)) {
        dist[key] = deepClone(target[key], cache);
      }
    }
    return dist;
  }else{
    return target;
  }
}
```

## <font style="color:rgb(51, 51, 51);">浅拷贝</font>
<font style="color:rgb(51, 51, 51);">值复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享一块内存，但深拷贝会另外创造一个一模一样的对象，新对象与源对象不共享内存，修改新对象不会改到原对象。</font>

**<font style="color:rgb(51, 51, 51);">方法</font>**<font style="color:rgb(51, 51, 51);">：</font>

+ <font style="color:rgb(51, 51, 51);">object.assign()——当object只有一层时，是深拷贝。用于将所有可枚举属性的值从一个或多个元对象分配到目标对象，返回目标对象</font>
+ <font style="color:rgb(51, 51, 51);">Array.prototype.concat()</font>
+ <font style="color:rgb(51, 51, 51);">Array.prototype.slice()</font>

```javascript
//浅
function clone(origin,target){
  var target = target || {};//如果用户不提供子集提供
  for(let prop in origin){
    target[prop]=origin[prop];
  }
  return target;

}
```

# Restful
[RESTful API 一种流行的 API 设计风格](https://restfulapi.cn/)

[什么是REST | RESTful API 中文网](http://restful.p2hp.com/)  
 如果一个架构符合 REST 原则，就称它为 RESTful 架构。<font style="color:rgb(51, 51, 51);">RESTful 架构可以充分的利用 HTTP 协议的各种功能，是 HTTP 协议的最佳实践</font>

# <font style="color:rgb(51, 51, 51);">cdn（内容分发网络）</font>
> [CDN是什么？为什么使用CDN? - 掘金](https://juejin.cn/post/7008708776119894029)
>

CDN是构建在现有网络基础之上的智能虚拟网络，依靠部署在各地的**边缘服务器**，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，**提高用户访问响应速度和命中率。**

优点：

+ js体积变小，使用CDN的第三方资源的js代码，将不再打包到本地服务的js包中，减小本地js包的体积，提高加载速度
+ 给网页加载提速

## 访问过程
主要依赖于DNS的重定向技术，即将用户重定向至离用户最近的CDN边缘节点中。

1. 基于DNS调度

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699430311460-6851e202-2298-4721-83cd-cbf4de5f0879.png)

    1. 当终端用户向www.aliyundoc.com下的指定资源发起请求时，首先向Local DNS（本地DNS）发起请求域名www.aliyundoc.com对应的IP。
    2. Local DNS检查缓存中是否有www.aliyundoc.com的IP地址记录。如果有，则直接返回给终端用户；如果没有，则向网站授权DNS请求域名www.aliyundoc.com的解析记录。
    3. 当网站授权DNS解析www.aliyundoc.com后，返回域名的CNAME www.aliyundoc.com.example.com。
    4. Local DNS向阿里云CDN的DNS调度系统请求域名www.aliyundoc.com.example.com的解析记录，阿里云CDN的DNS调度系统将为其分配最佳节点IP地址。
    5. Local DNS获取阿里云CDN的DNS调度系统返回的最佳节点IP地址。
    6. Local DNS将最佳节点IP地址返回给用户，用户获取到最佳节点IP地址。
    7. 用户向最佳节点IP地址发起对该资源的访问请求。
        1. 如果该最佳节点已缓存该资源，则会将请求的资源直接返回给用户（步骤8），此时请求结束。
        2. 如果该最佳节点未缓存该资源或者缓存的资源已经失效，则节点将会向源站发起对该资源的请求。获取源站资源后结合用户自定义配置的缓存策略，将资源缓存到CDN节点并返回给用户（步骤8），此时请求结束。配置缓存策略的操作方法，请参见[配置缓存过期时间](https://help.aliyun.com/zh/cdn/user-guide/add-a-cache-rule#task-261642)。

存在的问题：

DNS的缓存时间较长，导致节点异常时自动调度延迟很大

2. 302调度
+ 访问url后，请求到了调度集群上，此时的算法和DNS调度的算法一样，但判断依据由本地域名服务器的IP变成了客户端的出口IP
+ 浏览器接收到302响应，根据location中的url继续发起http请求，此时的请求目标IP时CDN边缘节点

**302 调度的优势：**

    - 实时调度，因为没有 local DNS 缓存的，适合 CDN 的削峰处理，对于成本控制意义重大
    - 准确性高，直接获取客户端出口 IP 进行调度

  **302 调度的劣势：**

    - 每次都要跳转，对于延时敏感的业务不友好。一般只适用于大文件。
3. AnyCast BGP路由策略

## CDN预热数据
上面说的访问模式，都是基于Pull模式，由用户决策哪部分热点数据会最终存留在CDN缓存中;对于大促场景，我们往往需要预先将活动相关资源预热 到 边缘节点(L1),避免大促开启后，大量用户访问，造成源站压力过大。这时候采用的是 Push模式。

## CDN回源
当CDN本地缓存没有命中时，触发回源动作,

+ 一级缓存 访问二级缓存是否有相关数据，如果有，返回一级缓存。
+ 二级缓存 Miss，触发 二级缓存 回源请求，请求源站对应数据。获取结果后，缓存到本地缓存，返回数据到一级缓存。
+ 一级缓存 获取数据，缓存本地后，返回给用户。

# 虚拟列表
> [「前端进阶」高性能渲染十万条数据(虚拟列表) - 掘金](https://juejin.cn/post/6844903982742110216)
>

> <font style="color:rgb(37, 41, 51);">假设我们的长列表需要展示10000条记录，我们同时将10000条记录渲染到页面中。此时为了避免卡顿就可以使用虚拟列表。虚拟列表就是为了</font>**<font style="color:rgb(37, 41, 51);">同时加载大量数据</font>**
>

<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">虚拟列表</font><font style="color:rgb(37, 41, 51);">其实是按需显示的一种实现，即只对</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">可见区域</font><font style="color:rgb(37, 41, 51);">进行渲染，对</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">非可见区域</font><font style="color:rgb(37, 41, 51);">中的数据不渲染或部分渲染的技术，从而达到极高的渲染性能。</font>

## <font style="color:rgb(37, 41, 51);">实现</font>
<font style="color:rgb(37, 41, 51);">在首屏加载的时候，只加载</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">可视区域</font><font style="color:rgb(37, 41, 51);">内需要的列表项，当滚动发生时，动态通过计算获得</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">可视区域</font><font style="color:rgb(37, 41, 51);">内的列表项，并将</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">非可视区域</font><font style="color:rgb(37, 41, 51);">内存在的列表项删除。</font>

+ <font style="color:rgb(37, 41, 51);">计算当前可视区域起始数据索引</font>
+ <font style="color:rgb(37, 41, 51);">计算当前可视区域结束数据索引</font>
+ <font style="color:rgb(37, 41, 51);">计算当前可视区域的数据，并渲染到页面中</font>
+ <font style="color:rgb(37, 41, 51);">计算startIndex对应的数据在整个列表中的偏移位置</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">startOffset</font><font style="color:rgb(37, 41, 51);">并设置到列表上</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699600224702-bba41048-4cea-4d5c-93a7-0525bd9bf21e.png)

```html
// 可视区域的容器
<div class="infinite-list-container">
    //  容器内的占位，高度为总列表高度，用于形成滚动条   
  <div class="infinite-list-phantom"></div>
  	// 列表项的渲染区域
    <div class="infinite-list">
      <!-- item-1 -->
      <!-- item-2 -->
      <!-- ...... -->
      <!-- item-n -->
    </div>
</div>
```

接着监听infinite-list-container的scroll时间，获取滚动位置scrollTop

+ <font style="color:rgb(37, 41, 51);">假定</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">可视区域</font><font style="color:rgb(37, 41, 51);">高度固定，称之为</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">screenHeight</font>
+ <font style="color:rgb(37, 41, 51);">假定</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">列表每项</font><font style="color:rgb(37, 41, 51);">高度固定，称之为</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">itemSize</font>
+ <font style="color:rgb(37, 41, 51);">假定</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">列表数据</font><font style="color:rgb(37, 41, 51);">称之为</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">listData</font>
+ <font style="color:rgb(37, 41, 51);">假定</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">当前滚动位置</font><font style="color:rgb(37, 41, 51);">称之为</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">scrollTop</font>

<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">则可得到：</font>

+ <font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">列表总高度：</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">listHeight</font><font style="color:rgb(37, 41, 51);"> = listData.length * itemSize</font>
+ <font style="color:rgb(37, 41, 51);">可现实的列表项数：</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">visibleCount</font><font style="color:rgb(37, 41, 51);"> = Math.ceil(screenHeight / itemSize)</font>
+ <font style="color:rgb(37, 41, 51);">数据的起始索引：</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">startIndex</font><font style="color:rgb(37, 41, 51);"> = Math.floor(scrollTop / itemSize)</font>
+ <font style="color:rgb(37, 41, 51);">数据的结束索引：</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">endIndex</font><font style="color:rgb(37, 41, 51);"> = startIndex + visibleCount</font>
+ <font style="color:rgb(37, 41, 51);">列表显示数据为：</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">visibleData</font><font style="color:rgb(37, 41, 51);"> = listData.slice(startIndex,endIndex)</font>

<font style="color:rgb(37, 41, 51);">当滚动后，由于</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">渲染区域</font><font style="color:rgb(37, 41, 51);">相对于</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">可视区域</font><font style="color:rgb(37, 41, 51);">已经发生了偏移，此时我需要获取一个偏移量</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">startOffset</font><font style="color:rgb(37, 41, 51);">，通过样式控制将</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">渲染区域</font><font style="color:rgb(37, 41, 51);">偏移至</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">可视区域</font><font style="color:rgb(37, 41, 51);">中。</font>

+ <font style="color:rgb(37, 41, 51);">偏移量</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">startOffset</font><font style="color:rgb(37, 41, 51);"> = scrollTop - (scrollTop % itemSize);</font>

```html
<template>
  <div ref="list" class="infinite-list-container" @scroll="scrollEvent($event)">
    <div class="infinite-list-phantom" :style="{ height: listHeight + 'px' }"></div>
    <div class="infinite-list" :style="{ transform: getTransform }">
      <div ref="items"
        class="infinite-list-item"
        v-for="item in visibleData"
        :key="item.id"
        :style="{ height: itemSize + 'px',lineHeight: itemSize + 'px' }"
        >{{ item.value }}</div>
    </div>
  </div>
</template>

```

```javascript
export default {
  name:'VirtualList',
  props: {
    //所有列表数据
    listData:{
      type:Array,
      default:()=>[]
    },
    //每项高度
    itemSize: {
      type: Number,
      default:200
    }
  },
  computed:{
    //列表总高度
    listHeight(){
      return this.listData.length * this.itemSize;
    },
    //可显示的列表项数
    visibleCount(){
      return Math.ceil(this.screenHeight / this.itemSize)
    },
    //偏移量对应的style
    getTransform(){
      return `translate3d(0,${this.startOffset}px,0)`;
    },
    //获取真实显示列表数据
    visibleData(){
      return this.listData.slice(this.start, Math.min(this.end,this.listData.length));
    }
  },
  mounted() {
    this.screenHeight = this.$el.clientHeight;
    this.start = 0;
    this.end = this.start + this.visibleCount;
  },
  data() {
    return {
      //可视区域高度
      screenHeight:0,
      //偏移量
      startOffset:0,
      //起始索引
      start:0,
      //结束索引
      end:null,
    };
  },
  methods: {
    scrollEvent() {
      //当前滚动位置
      let scrollTop = this.$refs.list.scrollTop;
      //此时的开始索引
      this.start = Math.floor(scrollTop / this.itemSize);
      //此时的结束索引
      this.end = this.start + this.visibleCount;
      //此时的偏移量
      this.startOffset = scrollTop - (scrollTop % this.itemSize);
    }
  }
};
```

> 可以使用[IntersectionObserver](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FIntersectionObserver)替换监听scroll事件，IntersectionObserver可以监听目标元素是否出现在可视区域内，在监听的回调事件中执行可视区域数据的更新，并且IntersectionObserver的监听回调是异步触发，不随着目标元素的滚动而触发，性能消耗极低。
>

为了避免滚动过快的时候会出现短暂的白屏的现象的出现，可以在可见区域的上方和下方渲染额外的项目，在滚动时给予一些缓冲，所以将屏幕分为三个区域：

+ 可视区域上方： above
+ 可视区域：screen
+ 可视区域下方：below

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1699601951311-a1a5806d-7971-45dd-974b-2dd4d08ac1b9.png)

# fetch
> [https://github.com/yuanyuanbyte/Blog/issues/105](https://github.com/yuanyuanbyte/Blog/issues/105)
>

fetch()是xmlHttpRequest的升级版，用于在js里面发出http请求

```javascript
// 基本使用
fetch(url，options).then(response => response.json()) //解析为可读数据
 .then(data => console.log(data))//执行结果是 resolve就调用then方法
 .catch(err => console.log("Oh, error", err))//执行结果是 reject就调用catch方法
```

fetch(url,options):

+ 第一个参数：请求的url
+ 第二个参数：定制http请求 
    - 请求方法
    - 提交的json数据
    - 请求头

```javascript
const response = await fetch(url, {
  method: 'POST',
  headers: {
    "Content-type": "application/x-www-form-urlencoded; charset=UTF-8",
  },
  body: 'foo=bar&lorem=ipsum',
});
const json = await response.json();
```

**优点：**

+ 语法简介，语义化，业务逻辑更清晰
+ 基于标准promise实现，支持async/await
+ 通过数据流处理数据，可以分块读取，有利于提高网站性能表现，减少内存占用，对于请求大文件或者网速慢的场景相当有用，xmlhttpRequest对象不支持数据流，所有的数据必须放在缓存里，不支持分块读取，必须等待全部拿到后，再一次性吐出来

API：

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1698650724593-a8c2f9cc-f2ca-4edf-8425-0005aa05d6ec.png)

# 大文件切片上传
> [大文件上传的处理方法——切片上传（前端） - 掘金](https://juejin.cn/post/7088903942729367582)
>

获取文件后进行切片，切片整理好每个切片的参数并发请求即可

```vue
<template>
  <div>
    <input type="file" @change="handleFileChange" />
    <el-button @click="handleUpload">上传</el-button>
  </div>
</template>
<script>
const SIZE = 10 * 1024 * 1024; // 切片大小（10MB）

export default {
  data: () => ({
    // 存放文件信息
    container: {
      file: null
      hash: null
    }，
    data: [] // 用于存放加工好的文件切片列表
    hashPercentage: 0 // 存放hash生成进度
  }),
  methods: {
    // 获取上传文件
    handleFileChange(e) {
      const [file] = e.target.files;
      if (!file) {
        this.container.file = null;
        return;
      }
      this.container.file = file;
    },
        
    // 生成文件切片
    createFileChunk(file, size = SIZE) {
     const fileChunkList = [];
      let cur = 0;
      while (cur < file.size) {
        fileChunkList.push({ file: file.slice(cur, cur + size) });
        cur += size;
      }
      return fileChunkList;
    },
        
    // 生成文件hash    
    calculateHash(fileChunkList) {
      return new Promise(resolve => {
        this.container.worker = new Worker("/hash.js");
        this.container.worker.postMessage({ fileChunkList });
        this.container.worker.onmessage = e => {
          const { percentage, hash } = e.data;
          // 可以用来显示进度条
          this.hashPercentage = percentage;
          if (hash) {
            resolve(hash);
          }
        };
      });
    },

    // 切片加工（上传前预处理 为文件添加hash等）
    async handleUpload() {
      if (!this.container.file) return;
      // 切片生成
      const fileChunkList = this.createFileChunk(this.container.file);
      // hash生成
      this.container.hash = await this.calculateHash(fileChunkList);
      this.data = fileChunkList.map(({ file }，index) => ({
           chunk: file,
           // 这里的hash为文件名 + 切片序号，也可以用md5对文件进行加密获取唯一hash值来代替文件名
           hash: this.container.hash + "-" + index
      }));
      await this.uploadChunks();
    }
      
    // 上传切片
    async uploadChunks() {
     const requestList = this.data
     	// 构造formData
       .map(({ chunk，hash }) => {
         const formData = new FormData();
         formData.append("chunk", chunk);
         formData.append("hash", hash);
         formData.append("filename", this.container.file.name);
         return { formData };
       })
     	// 发送请求 上传切片
       .map(async ({ formData }) =>
         uploadRequest(formData) // 这里的uploadRequest是你封装好的上传文件切片接口请求方法
       );
     await Promise.all(requestList); // 等待全部切片上传完毕
     await merge(this.container.file.name) // 发送请求合并文件
    },
  }
};
</script>
```

## 实现方案
1. **读取文件计算hash值： **
    1. 使用组件或者`input`读取文件，使用`sparkMd5` 计算大文件的`hash`值 
    - 简单的做法：使用文件名 + 切片下标作为切片 hash，但这样做文件名一旦修改就失去了效果，而事实上只要文件内容不变，hash 就不应该变化
    - 根据文件内容生成hash：使用spark-md5，根据文件内容计算出文件的 hash 值，另外考虑到如果上传一个超大文件，读取文件内容计算 hash 是非常耗费时间的，并且会引起 UI 的阻塞，导致页面假死状态，所以我们使用 web-worker在 worker 线程计算 hash，这样用户仍可以在主界面正常的交互
    2. 由于file对象继承于Blob，即可以使用 `slice()`方法切片 
2. **创建文件上传列表： **
    1. 根据预设的切片大小，对文件进行切片并构建上传列表 

```tsx
interface RequestItem {
  	index :number // 下标值 
    fileSection : Blob  // 切片   
} 
type SectionList = RequestItem[];
```

3. **上传文件 ： **

发送请求（`文件hash值 `、`切片大小 `、` 文件类型` 、 `文件总大小 ` ）获取已经上传的切片的下标 

    1. 如果已经上传过该文件则后端直接返回url前端直接引用，实现秒传 
    2. 如果未上传过，则将构建的上传列表进行上传 
    3. 上传过部分切片则返回已经上传切片的下标值 ，根据该下标值筛选未上传的切片后进行上传 
4. **合并切片：**
    1. 当切片前端全部上传完毕且后端返回成功响应后，发送合并请求让后端对切片进行合并校验

## 优化方案
1. 构建上传列表后，上传列表时**控制并发量**减少浏览器负担 
2. 计算hash值时可以使用webworker开启新线程进行计算 
3. 抽样计算hash值  。 第一个和最后一个切片全部数据，中间的切片进行首、中、尾部进行定量的取样后对上述数据进行hash计算 。 

![](https://cdn.nlark.com/yuque/0/2023/png/27018002/1692589688823-a5842306-5bc3-47c5-b971-804040d631fb.png)

## 文件秒传
在文件上传之前先计算出文件的hash，然后发送给后端进行验证，看后端是否存在这个hash，如果存在，则证明这个文件上传过，则直接提示用户秒传成功

## <font style="color:rgb(37, 41, 51);">暂停上传</font>
将所有的切片存在一个数组中，每当一个切片上传完毕，从数组中移除，这样就可以实现用一个数组只保存上传中的文件。此外，因为要暂停上传，所以需要中断请求 axios中断请求可以利用AbortController

# deeplink
深度链接是一种能将用户直接从网页带到app<font style="color:rgb(18, 18, 18);">指定页面的技术，目前广义上的“深度链接”概念包含了 DeepLink 和 Deferred Deeplink，可以省去中间主要触发场景分为两种：</font>

+ <font style="color:rgb(18, 18, 18);">用户已安装目标App情况下：在web网页点击链接，就能直接跳转到App内指定页面。</font>
+ <font style="color:rgb(18, 18, 18);">用户未安装目标App情况下：在web网页点击链接，会先跳转应用商店，下载后首次打开App，会自动跳转到指定页面。</font>

## <font style="color:rgb(18, 18, 18);">原理</font>
<font style="color:rgb(18, 18, 18);">移动端深度链接（Deeplink）本质上就是通过web调用原生App，依赖URL实现</font>

<font style="color:rgb(18, 18, 18);">在这个过程中需要满足的前提条件是：</font>

+ **<font style="color:rgb(18, 18, 18);">社交平台或浏览器：</font>**<font style="color:rgb(18, 18, 18);">必须支持打开目标App，需要经过一些处理才能实现。比如京东App可以从微信上直接打开，淘宝App却不可以，这也是由于平台的选择性开放和限制。</font>
+ **<font style="color:rgb(18, 18, 18);">App本身：</font>**<font style="color:rgb(18, 18, 18);">必须能够获取参数，并且设置好唤醒地址，才能解析参数，定位到具体位置。</font>
1. <font style="color:rgb(18, 18, 18);">URL Scheme（iOS/Android都适用）</font>

<font style="color:rgb(18, 18, 18);">URL Scheme是实现Deeplink兼容性最高、也最简单的一项方法，原生App可以先向操作系统注册一个URL，其中Scheme的作用是从不同平台唤醒相应App。</font>

<font style="color:rgb(18, 18, 18);">由于涉及到需要打开页面的能力，用于接收从H5传递过来的参数，那么还需要一些配置：</font>

    - <font style="color:rgb(18, 18, 18);">Android：配置Action和category</font>
    - <font style="color:rgb(18, 18, 18);">iOS：原理一致，配置info</font>

<font style="color:rgb(18, 18, 18);">工作流程是：当用户点击此类深度链接时—>操作系统提供解析URL Scheme的能力—>判断属于哪个App、是否安装了App—>唤醒App并传递需要的参数。</font>

2. <font style="color:rgb(18, 18, 18);">Universal link（iOS 9.2及以上适用）</font>

<font style="color:rgb(18, 18, 18);">Universal Link是iOS 9以后苹果推出的通用链接技术，能够方便的</font>**<font style="color:rgb(18, 18, 18);">通过一个https链接来打开App指定页面，不需要额外的判断</font>**<font style="color:rgb(18, 18, 18);">；如果没有安装App，则跳转到自定义地址。</font>

<font style="color:rgb(18, 18, 18);">相对Scheme的优势在于，Universal Link是一个Web Link，因此少了很多麻烦：</font>

    - <font style="color:rgb(18, 18, 18);">当用户已安装该App时，不需要加载任何页面以及判断提示，能够立即唤醒App，用户未安装App，则跳去对应的web link（自定义页面）。</font>
    - <font style="color:rgb(18, 18, 18);">Universal Links支持从其他App中的UIWebView中跳转到目标App。</font>
    - <font style="color:rgb(18, 18, 18);">绝大多数平台都支持Universal Link，能被搜索引擎索引，iOS微信7.0.5版本也解除了对Universal Link的限制，</font>**<font style="color:rgb(18, 18, 18);">目前微信7.0.5以上版本已经能流畅运行Universal Link</font>**<font style="color:rgb(18, 18, 18);">。</font>

<font style="color:rgb(18, 18, 18);">在Deeplink的实现方案中，Universal Link相比Scheme无疑具有更优的用户体验，iOS9.2及以上的版本更推荐使用Universal Link唤醒App</font>

## 短链接
<font style="color:rgb(43, 43, 43);">在使用网址访问访问网站的时候，可能都会遇到一个很长的链接，多则可能几百个字符，这样的链接看起来非常的长</font>

> https://www.google.com/search?q=%E9%95%BF%E9%93%BE%E9%93%BE%E6%8E%A5&rlz=1C5GCEM_enCN1065&oq=%E9%95%BF%E9%93%BE%E9%93%BE%E6%8E%A5&aqs=chrome..69i57j0i13i512j0i10i13i512l2j0i13i30j0i10i13i30j0i13i30l4.4703j0j15&sourceid=chrome&ie=UTF-8
>
> <font style="color:rgb(43, 43, 43);">像以上这种，就是长链链接，看起来非常的臃肿，虽然说链接里面携带的一些参数使链接变长是不可避免的，但是我们可以使用另一种方法，既能使链接携带参数也能使链接为短链接，后面会讲到短链的原理和实现方式。</font>
>
> [<font style="color:rgb(43, 43, 43);">https://juejin.cn/post/7254039051588599864</font>](https://juejin.cn/post/7254039051588599864)
>
> <font style="color:rgb(43, 43, 43);">像以上这种就是短链，使用短链有以下几种优点。</font>
>
> 1. <font style="color:rgb(89, 89, 89);">相比长链更加简洁。</font>
> 2. <font style="color:rgb(89, 89, 89);">便于使用，粘贴复制分享给别人时较为便捷，有些平台对分享内容的长度有所限制（微博只能发140字），这个时候使用短链可以输入更多的内容。</font>
> 3. <font style="color:rgb(89, 89, 89);">短链生成的二维码更容易识别。</font>
>

**核心：**

> [转转短链平台设计与实现 - 掘金](https://juejin.cn/post/7265155568398434343?searchId=202311162243471023DDD16C803E9808C2)
>

链接映射表和302临时重定向。当我们用户用短链访问服务器时，服务器会将用户携带的短链在服务器中的链接映射表中寻找唯一与之对应的长链接，寻找到后返回重定向，重定向至长链接的网页，这样用户就可以不直接携带长链接，而是携带一个短链接，间接的去访问长链接，这个中间层就是服务器，服务器会在中间做一个链接映射和重定向的操作。

# web components
> [Web Components 入门实例教程 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2019/08/web_components.html)
>
> [案例实战，一文吃透 Web Components - 掘金](https://juejin.cn/post/7010595352550047752?searchId=2023112117150658947BE36D362993FCA1#heading-6)
>

1. 组成
+ custom element（自定义元素）：<font style="color:rgb(27, 27, 27);">一组 JavaScript API，允许你定义 custom elements 及其行为，然后可以在你的用户界面中按照需要使用它们</font>
+ <font style="color:rgb(27, 27, 27);">Shadow DOM（影子DOM）：一组 JavaScript API，用于将封装的“影子”DOM 树附加到元素（与主文档 DOM 分开呈现）并控制其关联的功能。通过这种方式，你可以保持元素的功能私有，这样它们就可以被脚本化和样式化，而不用担心与文档的其他部分发生冲突。</font>
+ <font style="color:rgb(27, 27, 27);">HTML template（HTML模板）：</font>[<font style="color:rgb(27, 27, 27);"><template></font>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/template)<font style="color:rgb(27, 27, 27);"> 和 </font>[<font style="color:rgb(27, 27, 27);"><slot></font>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/slot)<font style="color:rgb(27, 27, 27);"> 元素使你可以编写不在呈现页面中显示的标记模板。然后它们可以作为自定义元素结构的基础被多次重用。</font>
2. 实现方法：
    1. <font style="color:rgb(27, 27, 27);">创建一个类或函数来指定 web 组件的功能</font>
    2. <font style="color:rgb(27, 27, 27);">使用 </font>[CustomElementRegistry.define()](https://developer.mozilla.org/zh-CN/docs/Web/API/CustomElementRegistry/define)<font style="color:rgb(27, 27, 27);"> 方法注册你的新自定义元素，并向其传递要定义的元素名称、指定元素功能的类、以及可选的其所继承自的元素。</font>
    3. <font style="color:rgb(27, 27, 27);">如果需要的话，使用 </font>[Element.attachShadow()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/attachShadow)<font style="color:rgb(27, 27, 27);"> 方法将一个 shadow DOM 附加到自定义元素上。使用通常的 DOM 方法向 shadow DOM 中添加子元素、事件监听器等等。</font>
    4. <font style="color:rgb(27, 27, 27);">如果需要的话，使用 </font>[<template>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/template)<font style="color:rgb(27, 27, 27);"> 和 </font>[<slot>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/slot)<font style="color:rgb(27, 27, 27);"> 定义一个 HTML 模板。再次使用常规 DOM 方法克隆模板并将其附加到你的 shadow DOM 中。</font>
    5. <font style="color:rgb(27, 27, 27);">在页面任何你喜欢的位置使用自定义元素，就像使用常规 HTML 元素那样。</font>

# 设计模式
> [前端需要了解的9种设计模式 什么是设计模式？设计模式的类型一. 结构型模式（Structural Patterns）二. 创建型模式（Creat-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/1627336)
>

设计模式的三大类：

+ 结构型模式：外观模式，代理模式
+ 创建型模式： 工厂模式，单例模式
+ 行为型模式：策略模式，迭代器模式，<font style="color:#000000;">观察者模式，中介者模式</font>

## 外观模式
<font style="color:rgb(51, 51, 51);">为子系统中的一组接口提供一个统一的高层接口，使子系统更容易使用。简而言之外观设计模式就是把多个子系统中复杂逻辑进行抽象，从而提供一个更统一、更简洁、更易用的API。</font>

```javascript
// 绑定事件
function addEvent(element, event, handler) {
  if (element.addEventListener) {
    element.addEventListener(event, handler, false);
  } else if (element.attachEvent) {
    element.attachEvent('on' + event, handler);
  } else {
    element['on' + event] = fn;
  }
}
// 取消绑定
function removeEvent(element, event, handler) {
  if (element.removeEventListener) {
    element.removeEventListener(event, handler, false);
  } else if (element.detachEvent) {
    element.detachEvent('on' + event, handler);
  } else {
    element['on' + event] = null;
  }
}
```

## 代理模式
可以解决的问题：

1. <font style="color:rgb(51, 51, 51);">增加对一个对象的访问控制</font>
2. <font style="color:rgb(51, 51, 51);">当访问一个对象的过程中需要增加额外的逻辑</font>

<font style="color:rgb(51, 51, 51);">例如es6中的proxy</font>

## 单例模式
<font style="color:rgb(51, 51, 51);">属于创建型模式，它提供了一种创建对象的最佳方式，它确保一个类只有一个实例，并提供了一个全局访问点来访问该实例。</font>

<font style="color:rgb(51, 51, 51);">实现单例模式需要解决以下几个问题：</font>

1. <font style="color:rgb(51, 51, 51);">如何确定Class只有一个实例？</font>
2. <font style="color:rgb(51, 51, 51);">如何简便的访问Class的唯一实例？</font>
3. <font style="color:rgb(51, 51, 51);">Class如何控制实例化的过程？</font>
4. <font style="color:rgb(51, 51, 51);">如何将Class的实例个数限制为1？</font>

<font style="color:rgb(51, 51, 51);">我们一般通过实现以下两点来解决上述问题：</font>

1. <font style="color:rgb(51, 51, 51);">隐藏Class的构造函数，避免多次实例化</font>
2. <font style="color:rgb(51, 51, 51);">通过暴露</font>一个 getInstance() 方法来创建/获取唯一实例

## 策略模式
<font style="color:rgb(51, 51, 51);">策略模式简单描述就是：对象有某个行为，但是在不同的场景中，该行为有不同的实现算法。</font>

<font style="color:rgb(51, 51, 51);">最常见的使用策略模式的场景如登录鉴权，鉴权算法取决于用户的登录方式是手机、邮箱或者第三方的微信登录等等，而且登录方式也只有在运行时才能获取，获取到登录方式后再动态的配置鉴权策略。所有这些策略应该实现统一的接口，或者说有统一的行为模式。</font>

## <font style="color:rgb(51, 51, 51);">迭代器模式</font>
ES6中的迭代器 [Iterator](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FJavaScript%2FGuide%2FIterators_and_Generators&source=article&objectId=1627336) 相信大家都不陌生，迭代器用于遍历[容器](https://cloud.tencent.com/product/tke?from_column=20065&from=20065)（集合）并访问容器中的元素，而且无论容器的数据结构是什么（Array、Set、Map等），迭代器的接口都应该是一样的，都需要遵循 [迭代器协议](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FJavaScript%2FReference%2FIteration_protocols%23The_iterator_protocol&source=article&objectId=1627336)。

迭代器模式解决了以下问题：

1. 提供一致的遍历各种数据结构的方式，而不用了解数据的内部结构
2. 提供遍历容器（集合）的能力而无需改变容器的接口

一个迭代器通常需要实现以下接口：

+ <font style="color:#000000;">hasNext()：判断迭代是否结束，返回Boolean</font>
+ <font style="color:#000000;">next()：查找并返回下一个元素</font>

## <font style="color:#000000;">观察者模式（发布订阅模式）</font>
<font style="color:#000000;">比如给DOM元素绑定事件的 </font><font style="color:#000000;background-color:rgb(243, 245, 249);">addEventListener()</font><font style="color:#000000;"> 方法：Target就是被观察对象Subject，listener就是观察者Observer。</font>

```javascript
target.addEventListener(type, listener [, options]);
```

## 中介者模式
<font style="color:rgb(51, 51, 51);">在中介者模式中，中介者（Mediator）包装了一系列对象相互作用的方式，使得这些对象不必直接相互作用，而是由中介者协调它们之间的交互，从而使它们可以松散偶合。当某些对象之间的作用发生改变时，不会立即影响其他的一些对象之间的作用，保证这些作用可以彼此独立的变化。</font>**<font style="color:rgb(51, 51, 51);">中介者模式比较常见的应用比如聊天室，聊天室里面的人之间并不能直接对话，而是通过聊天室这一媒介进行转发</font>**

<font style="color:rgb(51, 51, 51);"></font>

# 微信小程序
> [微信小程序底层框架实现原理｜万字长文 - 掘金](https://juejin.cn/post/7140509513852911647#heading-53)
>

## 双线程架构
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700554087837-2817a122-571b-4a0a-8aef-66ab2261d706.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700556190492-de49ccf7-2113-4918-a16c-88bea22c616f.png)

逻辑层使用jscore运行js代码

渲染层使用webview进行渲染，小程序有多个webview页面，<font style="color:rgb(37, 41, 51);">所以渲染层存在多个webview</font>

<font style="color:rgb(37, 41, 51);">两个线程之间由Native 层之间统一处理，无论是线程之间的通信，还是数据的传递，网络请求都是由Native层做转发。</font>

> <font style="color:rgb(37, 41, 51);">由于小程序是双线程模型，即意味着任何数据传递都是线程间的通信，也就是都会有一定的延时，都是异步的。</font>
>

## <font style="color:rgb(37, 41, 51);">快速渲染设计原理</font>
<font style="color:rgb(37, 41, 51);">小程序采用多个webview渲染，更加接近原生App的用户体验。</font>

<font style="color:rgb(37, 41, 51);">如果为单页面应用，单独打开一个页面，需要先卸载当前页面结构，并重新渲染。</font>

<font style="color:rgb(37, 41, 51);">多页面应用，新页面直接滑动出来并且覆盖在旧页面上即可。这样用户体验非常好。</font>

### <font style="color:rgb(37, 41, 51);">数量限制</font>
<font style="color:rgb(37, 41, 51);">页面得载入是通过创建并插入webview 来实现的。</font>

<font style="color:rgb(37, 41, 51);">微信小程序做了限制，在微信小程序中打开的页面不能超过10个，达到10个页面后，就不能再打开新的页面。</font>

**<font style="color:rgb(37, 41, 51);">所以我们在开发中，要避免路由嵌套太深。</font>**

### <font style="color:rgb(37, 41, 51);">pageFrame</font>
我们在微信开发者工具中会发现两个webview

+ 一个是加载的当前的页面，<font style="color:rgb(37, 41, 51);">加载地址和当前页面路径一致</font>
+ <font style="color:rgb(37, 41, 51);">一个是instanceframe.html，这个是用来新渲染webview的模板</font>

### <font style="color:rgb(37, 41, 51);">快速启动</font>
在视图层内，每个页面都是一个webiew，当小程序启动时只有首页一个webview

执行wx.navigateTo新开一个页面的时候，就会创建一个新的webview并插入到视图层

wx.navigateBack则为销毁webview

小程序每个视图层页面内容都是通过pageframe.html模板来生成的。

+ 首页启动时，即第一次通过pageframe.html生成内容后，后台服务会缓存pageframe.html模板首次生成的html内容
+ 非首次新打开页面时，页面请求的pageframe.html内容直接走后台缓存
+ 非首次新打开页面时，pageframe.html页面引入的外链js资源走本地缓存

这样在后续新打开页面时，都会走缓存的pageframe的内容，避免重复生成，快速打开一个新页面。

### 首次打开新页面
+ 启动一个webview，src为空地址[http://127.0.0.1:${global.proxyPort}/aboutblank?${c}](https://link.juejin.cn?target=http%3A%2F%2F127.0.0.1%3A%24%257Bglobal.proxyPort%257D%2Faboutblank%3F%24%257Bc%257D)
+ webview 初始化完毕后，设置地址src 为pageframe.html，开始加载注入的预设样式和预设js 代码
+ pageframe.html在dom ready之后，触发注入并执行具体页面的相关代码

**小程序启动流程示意图（用户首次访问或小程序同步更新时，命中环境预加载）**

## 生命周期
### data
<font style="color:rgb(37, 41, 51);">逻辑层的data与view是相互绑定的，data是页面第一次渲染使用的初始数据。页面加载的时候，data将会以JSON字符串形式由逻辑层传至渲染层。因此data中的数据必须是可以转成JSON的类型：字符串，数字，布尔值，对象，数组。</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700555984129-8200c857-8242-4a50-a36f-c179c5f5c887.png)

### 生命周期
+ onload()：页面加载时触发，<font style="color:rgb(37, 41, 51);">一个页面只会调用一次，可以在onLoad的参数中获取打开当前页面路径中的参数。</font>
+ <font style="color:rgb(37, 41, 51);">onShow()：页面显示/切入前台时触发</font>
+ <font style="color:rgb(37, 41, 51);">onHide()：页面隐藏/切入后台时触发。 如 wx.navigateTo 或底部 tab 切换到其他页面，小程序切入后台等。</font>
+ <font style="color:rgb(37, 41, 51);">onReady()：页面初次渲染完成时触发。一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互。</font>
+ <font style="color:rgb(37, 41, 51);">onUnload()：页面卸载时触发</font>。如wx.redirectTo或wx.navigateBack到其他页面时

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700556099882-4519dfda-b5a9-426d-8fd3-a02e506fc3d8.png)

## wxss
相比于css扩展的特性有

+ 尺寸单位：rpx，<font style="color:rgb(37, 41, 51);">可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。</font>
+ 样式导入：<font style="color:rgb(37, 41, 51);">使用 </font>**<font style="color:rgb(37, 41, 51);">@import</font>**<font style="color:rgb(37, 41, 51);">语句可以导入外联样式表， </font>**<font style="color:rgb(37, 41, 51);">@import</font>**<font style="color:rgb(37, 41, 51);">后跟需要导入的外联样式表的相对路径，用;表示语句结束。</font>

## virtual dom渲染流程
微信开发者工具和微信客户端都无法直接运行小程序的源码，因此我们需要对小程序的源码进行编译。

代码编译过程包括本地预处理、本地编译和服务器编译。

为了快速预览，微信开发者工具模拟器运行的代码只经过本地预处理、本地编译，没有服务器编译过程，而微信客户端运行的代码是额外经过服务器编译的。

## 通讯系统设计
最上面提到，视图层和逻辑层通讯是通过Native层。

具体的手段就是

+ ios利用 WKWebView 的提供 messageHandlers 特性
+ android 是往webview的window对象注入一个原生方法

这两种会统一封装成weixinJSBridge，这和正常h5与客户端通讯手段一致

初始化过程中Native层理论上是微信客户端，分别在视图层和业务逻辑层注入了WeixinJSBridge

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700557848043-7992d302-a7b0-439a-bf3b-8887238bb791.png)

## <font style="color:rgb(37, 41, 51);">小程序为什么比普通h5快</font>
我们在对小程序的架构设计时的要求只有一个，就是要快，包括要渲染快、加载快等。当用户点开某个小程序时，我们期望体验到的是只有很短暂的加载界面，在一个过渡动画之后可以马上看到小程序的主界面。

+ 双线程，渲染层和逻辑层并行不阻塞
+ 多个webview，页面切换更流畅
+ webview 预加载
+ 安装包缓存
+ 以及微信做了大量的优化和看不见的操作

## 性能优化
### 小程序启动流程
1. 资源准备
    1. <font style="color:rgb(37, 41, 51);">小程序相关信息准备：</font><font style="color:rgb(37, 41, 51);">微信客户端需要从微信后台获取小程序的</font>**<font style="color:rgb(37, 41, 51);">头像、昵称、版本、配置、权限</font>**<font style="color:rgb(37, 41, 51);">等基本信息，这些信息会在本地缓存，并通过一定的机制进行更新。</font>
    2. <font style="color:rgb(37, 41, 51);">环境预加载：为了尽可能的降低运行环境准备对启动耗时的影响，微信客户端会根据用户的使用场景和设备资源的使用情况，依照一定策略在小程序</font>**<font style="color:rgb(37, 41, 51);">启动前</font>**<font style="color:rgb(37, 41, 51);">对运行环境进行部分地</font>**<font style="color:rgb(37, 41, 51);">预加载</font>**<font style="color:rgb(37, 41, 51);">，以降低启动耗时。但不一定命中。</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700556462333-e4e87a36-16f4-4e47-9628-202effaeaaeb.png)

    3. 代码包准备

从微信后台获取代码包地址，从 CDN 下载小程序代码包

小程序代码包会在本地缓存，并通过[更新机制](https://link.juejin.cn?target=https%3A%2F%2Fdevelopers.weixin.qq.com%2Fminiprogram%2Fdev%2Fframework%2Fruntime%2Fupdate-mechanism.html)进行更新。

同步下载/异步下载 强制更新/静默更新

为例降低代码包下载的耗时，微信做的一些优化

        * 代码包压缩
        * 增量更新
        * 优先使用QUIC 和HTTP/2
        * 预先建立连接：在下载发生前，提前和 CDN 建立连接，降低下载过程中 DNS 请求和连接建立的耗时
        * 代码包复用：对每个代码包都会计算 MD5 签名。即使发生了版本更新，如果代码包的 MD5 没有发生变化，则不需要重新进行下载。
2. 代码注入

<font style="color:rgb(37, 41, 51);">小程序启动时需要从代码包内读取小程序的配置和代码，并注入到 JavaScript 引擎中。</font>

<font style="color:rgb(37, 41, 51);">微信客户端会使用 V8 引擎的 </font>[Code Caching](https://link.juejin.cn/?target=https%3A%2F%2Fv8.dev%2Fblog%2Fcode-caching-for-devs)<font style="color:rgb(37, 41, 51);"> 技术对代码编译结果进行缓存，降低非首次注入时的编译耗时</font>

> <font style="color:rgb(37, 41, 51);">code cac</font>he：<font style="background-color:rgb(248, 248, 248);">V8 会把编译和解析的结果缓存下来，等到下次遇到相同的文件，直接跳过这个过程，把直接缓存好的数据拿来使用</font>
>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700556773452-54179330-0d2f-4e0f-9634-7e904b6a6c6f.png)

### 启动时性能优化
+ 控制代码包体积
    - 推荐所有小程序使用分包加载
    - 避免非必要使用全局自定义组件和插件
        * 会影响按需注入的效果和小程序代码注入的耗时
    - 控制资源文件
        * 建议开发者在代码包内的图片一般应只包含一些体积较小的图标，避免在代码包中包含或在 WXSS 中使用 base64 内联过多、过大的图片等资源文件。
        * 这类文件应尽可能部署到 CDN，并使用 URL 引入。
+ 代码注入优化
    - <font style="color:rgb(37, 41, 51);">推荐所有小程序使用按需注入</font>
    - <font style="color:rgb(37, 41, 51);">用时注入</font>
        * <font style="color:rgb(37, 41, 51);">为自定义组件配置 </font>[占位组件](https://link.juejin.cn/?target=https%3A%2F%2Fdevelopers.weixin.qq.com%2Fminiprogram%2Fdev%2Fframework%2Fcustom-component%2Fplaceholder.html)<font style="color:rgb(37, 41, 51);">，组件就会自动被视为用时注入组件</font>
    - <font style="color:rgb(37, 41, 51);">启动过程中减少同步 API 的调用</font>
        * 建议优先使用拆分后的 getSystemSetting/getAppAuthorizeSetting/getDeviceInfo/getWindowInfo/getAppBaseInfo 按需获取信息，或使用使用异步版本 [getSystemInfoAsync](https://link.juejin.cn?target=https%3A%2F%2Fdevelopers.weixin.qq.com%2Fminiprogram%2Fdev%2Fapi%2Fbase%2Fsystem%2Fwx.getSystemInfoAsync.html)
        * <font style="color:rgb(37, 41, 51);">getStorageSync/setStorageSync 应只用来进行数据的持久化存储，不应用于运行时的数据传递或全局状态管理</font>
+ <font style="color:rgb(37, 41, 51);">首屏渲染优化</font>
    - <font style="color:rgb(37, 41, 51);">启用</font>[初始渲染缓存](https://developers.weixin.qq.com/miniprogram/dev/framework/view/initial-rendering-cache.html)
        * <font style="color:rgb(37, 41, 51);">启用初始渲染缓存，可以使视图层不需要等待逻辑层初始化完毕，而直接提前将页面初始 data 的渲染结果展示给用户，这可以使得页面对用户可见的时间大大提前</font>
    - <font style="color:rgb(37, 41, 51);">提前首屏数据请求</font>
        * <font style="color:rgb(37, 41, 51);">预拉取能够在小程序冷启动的时候通过微信后台提前向第三方服务器拉取业务数据，当代码包加载完时可以更快地渲染页面，减少用户等待时间，从而提升小程序的打开速度</font>
        * <font style="color:rgb(37, 41, 51);">周期性更新能够在用户未打开小程序的情况下，也能从服务器提前拉取数据，当用户打开小程序时可以更快地渲染页面，减少用户等待时间，增强在弱网条件下的可用性。</font>
    - <font style="color:rgb(37, 41, 51);">缓存请求数据</font>
    - <font style="color:rgb(37, 41, 51);">骨架屏</font>

### <font style="color:rgb(37, 41, 51);">运行时性能优化</font>
+ 合理使用setData：<font style="color:rgb(37, 41, 51);">控制频率，范围，内容</font>
+ <font style="color:rgb(37, 41, 51);">页面渲染优化：</font>
    - <font style="color:rgb(37, 41, 51);">适当监听scroll 事件</font>
    - <font style="color:rgb(37, 41, 51);">控制 WXML 节点数量和层级：源码中一个页面dom 数目超过16000，肯定会报错</font>
    - <font style="color:rgb(37, 41, 51);">data层级不要过深，因为需要深度遍历</font>
    - <font style="color:rgb(37, 41, 51);">使用 IntersectionObserver 监听元素曝光</font>
+ <font style="color:rgb(37, 41, 51);">页面切换优化：</font>
    - <font style="color:rgb(37, 41, 51);">避免在 onHide/onUnload 执行耗时操作</font>
        * <font style="color:rgb(37, 41, 51);">页面切换时，会先调用前一个页面的 onHide 或 onUnload 生命周期，然后再进行新页面的创建和渲染</font>
    - <font style="color:rgb(37, 41, 51);">提前发起数据请求</font>
        * 进行页面跳转时（例如 wx.navigateTo），可以提前为下一个页面做一些准备工作。页面之间可以通过 [EventChannel](https://link.juejin.cn?target=https%3A%2F%2Fdevelopers.weixin.qq.com%2Fminiprogram%2Fdev%2Freference%2Fapi%2FPage.html%23%25E9%25A1%25B5%25E9%259D%25A2%25E9%2597%25B4%25E9%2580%259A%25E4%25BF%25A1) 进行通信。类似postMessage
        * <font style="color:rgb(37, 41, 51);">例如，在页面跳转时，可以同时发起下一个页面的数据请求，而不需要等到页面 onLoad 时再进行，从而可以让用户更早的看到页面内容。</font>
    - <font style="color:rgb(37, 41, 51);">控制预加载下个页面的时机</font>
        * <font style="color:rgb(37, 41, 51);">程序页面加载完成后，会预加载下一个页面。默认情况下，小程序框架会在当前页面 onReady 触发 200ms 后触发预加载。</font>
        * <font style="color:rgb(37, 41, 51);">预加载会阻塞当前页面setData，我们可以对单个页面的配置增加， handleWebviewPreload 选项，来控制预加载下个页面的时机。</font>

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1700557591834-f3f08fee-c131-4677-b299-0d6bd4c3b8ab.png)

+ 资源加载优化：控制页面大小
+ 内存优化：
    - <font style="color:rgb(37, 41, 51);">合理分包，既能减少耗时，也能降低内存占用</font>
    - <font style="color:rgb(37, 41, 51);">事件监听，定时器记得清除</font>



# js文件相互引用有什么问题？如何解决
当 JavaScript 文件相互引用时，可能会导致以下问题：

1. 循环依赖：如果 A 文件引用了 B 文件，而 B 文件又引用了 A 文件，就形成了循环依赖。这会导致代码执行顺序混乱，可能导致未定义的行为或错误。
2. 加载顺序问题：如果文件 A 依赖文件 B，但是文件 B 的加载顺序不正确，可能会导致 A 文件无法正常运行，因为它依赖的代码尚未加载。

为了解决这些问题，可以采取以下方法：

1. 重构代码结构：检查代码结构，避免循环依赖。如果发现循环依赖，考虑将共享的功能提取到独立的模块中，以避免相互引用的问题。
2. 使用模块加载器：使用模块加载器，例如 RequireJS、Webpack 或 Rollup 等，可以更好地管理模块之间的依赖关系。这些工具提供了模块加载和依赖管理的功能，可以自动解决加载顺序和循环依赖的问题。
3. 异步加载：异步加载模块是解决加载顺序问题的一种方法。可以使用动态加载的方式，根据需要异步加载依赖的模块，确保它们在需要时可用。
4. 使用命名空间或模块化规范：使用命名空间或模块化规范（如 CommonJS、ES Modules）来管理代码的命名和作用域，避免全局变量的冲突和相互引用的问题。
5. 将依赖提取到顶层：将共享的依赖提取到顶层文件，确保它们在被其他文件引用之前已经加载和执行。
6. 构建工具优化：使用构建工具（如 Webpack）对代码进行优化和打包，可以自动解决模块之间的依赖关系，并生成适当的加载顺序。

