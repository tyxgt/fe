# <font style="color:#000000;">面试题</font>
> [「2022」TypeScript最新高频面试题指南 - 掘金](https://juejin.cn/post/7162011064819777567?searchId=202312031321312E7E4EBFB5B47C9DEEE4)
>
> [TypeScript 高级用法 - 掘金](https://juejin.cn/post/6926794697553739784#heading-28)
>

## <font style="color:#000000;">什么是ts</font>
<font style="color:#000000;">Typescript 是一个强类型的 JavaScript 超集，支持ES6语法，支持面向对象编程的概念，如类、接口(interface)、继承、泛型等。Typescript并不直接在浏览器上运行，需要编译器编译成纯Javascript来运行。</font>

<font style="color:#000000;">优点：</font>

1. <font style="color:#000000;">杜绝手误导致的变量名写错;</font>
2. <font style="color:#000000;">类型可以一定程度上充当文档;</font>
3. <font style="color:#000000;">IDE自动填充，自动联想;</font>

## ts中的类型有哪些
number，string，null，undefined，boolean，void，enums，interface，array，tuple，class

## <font style="color:#000000;">const和readonly的区别</font>
+ <font style="color:#000000;">const可以防止变量的值被修改，readonly可以防止变量的属性被修改</font>
+ <font style="color:#000000;">const在运行时检查，readonly在编译时检查</font>
+ <font style="color:#000000;">使用const变量保存的数组，可以使用push，pop等方法。但是如果使用Readonly Array声明的数组不能使用push，pop等方法</font>

## <font style="color:#000000;">枚举和常量枚举</font>
> [TS入门篇 | 详解 TypeScript 枚举类型 - 掘金](https://juejin.cn/post/6998318291420708900)
>

<font style="color:#000000;">常量枚举只能使用常量枚举表达式，并且不同于常规的枚举，它们在编译阶段会被删除。 常量枚举成员在使用的地方会被内联进来。 之所以可以这么做是因为，常量枚举不允许包含计算成员。</font>

> <font style="color:#000000;">在TypeScript中，定义了枚举值之后，编译成 JavaScript 的代码会创建一个对应的对象，这个对象可以在程序运行时使用。但是如果使用枚举只是为了让程序可读性好，并不需要编译后的对象呢？这样会增加一些编译后的代码量。TypeScript 中有一个const enum(常量枚举)，在定义枚举的语句之前加上const关键字，这样编译后的代码不会创建这个对象，只是会从枚举里拿到相应的值进行替换：（编译完如右图所示）</font>
>
> ![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1701579780745-0ef8023b-444a-43c2-9da6-f1e3f186bc9d.png)![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1701579793782-bd9b23bb-9d40-4b4b-bc01-bbd2778a8ee6.png)
>

## <font style="color:#000000;">interface可以给Function/Array/Class做声明嘛</font>
```javascript
/* 可以 */
// 函数声明
interface Say {
 (name: string): void;
}
let say: Say = (name: string):void => {}
// Array 声明
interface NumberArray { 
 [index: number]: number; 
} 
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
// Class 声明
interface PersonalIntl {
 name: string
 sayHi (name: string): string
}
```

## <font style="color:#000000;">type(类型别名)和interface的区别</font>
+ <font style="color:#000000;">两者都可以用来描述对象或者函数的类型，类型别名还可以用于其他类型</font>
+ <font style="color:#000000;">type可以定义基本类型别名，可以声明联合类型，可以声明元组，但是interface无法定义</font>

```javascript
type userName = string // 基本类型别名
type Student = {stuNo: number} | {classId: number} // 联合类型
type Data = [number, string]; // 元组类型
```

+ <font style="color:#000000;">type语句中还可以使用typeof获取实例类型进行赋值</font>

```javascript
// 当你想获取一个变量的类型时，使用 typeof
let div = document.createElement('div');
type B = typeof div
```

+ <font style="color:#000000;">interface可以做到声明合并, type声明两个同样的会发生报错</font>

```javascript
interface Person { name: string }
interface Person { age: number }
let user: Person = {
  name: "Tolu",
  age: 0,
};
```

+ <font style="color:#000000;">扩展：interface继承：</font>**<font style="color:#000000;">使用extends；</font>**<font style="color:#000000;">type继承，</font>**<font style="color:#000000;">使用&进行交叉</font>**
    - <font style="color:#000000;">interface继承interface</font>

```javascript
interface Person{
  name:string
}
interface Student extends Person { stuNo: number }
```

    - <font style="color:#000000;">interface继承type</font>

```javascript
type Person{
  name:string
}
interface Student extends Person { stuNo: number }
```

    - <font style="color:#000000;">type继承type</font>

```javascript
type Person{
  name:string
}
type Student = Person & { stuNo: number }
```

    - <font style="color:#000000;">type继承interface</font>

```javascript
interface Person{
  name:string
}
type Student = Person & { stuNo: number }
```

## <font style="color:#000000;">implements和extends的区别</font>
<font style="color:#000000;">impleme</font><font style="color:#000000;">nts：实现，一个新的类，从父类或者接口实现所有的属性和方法，同时可以重写属性和方法，包含一些新的功能</font>

<font style="color:#000000;">extends：继承，一个新的接口或者类，从父类或者接口继承所有的属性和方法，不可以重写属性，但可以重写方法</font>

## <font style="color:#000000;">any的作用</font>
<font style="color:#000000;">为编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。</font>

<font style="color:#000000;">any类型使用小tips </font>

1. <font style="color:#000000;">如果是类型不兼容报错导致你使用any，考虑使用类型断言替代</font>
2. <font style="color:#000000;">如果是类型太复杂导致你不想声明而使用any，考虑将这一处的类型去断言为你需要的最简类型</font>
3. <font style="color:#000000;">如果你是想表达一个未知类型，更合理的方式是使用unknown</font>

## <font style="color:#000000;">ts中any，never，unknown，null，undefined和void有什么区别</font>
> [一文看懂any，never，void和unknown的区别 - 掘金](https://juejin.cn/post/7151926103324983304)
>

+ <font style="color:#000000;">any：动态的变量类型（失去了类型检查的作用）</font>
+ <font style="color:#000000;">never：表示什么都没有，当函数抛出一个错误时，可以限制返回值类型为never。</font>**<font style="color:#000000;">never则表示永远都不会出现的值。</font>**
+ <font style="color:#000000;">unknown：任何类型的值都可以赋给 unknown 类型，但是 unknown 类型的值只能赋给 unknown 本身和 any 类型，并且不可以访问上面的任何属性</font>

> <font style="color:#000000;">使用场景：</font>
>
> <font style="color:#000000;">由于unknown基本可以替代any，所以在任何any适用的场景，都应该优先使用unknown。使用了unknown后，我们既允许某个对象储存任意类型的变量，同时也要求别人在使用这个对象的时候一定要先进行类型推断。</font>
>

+ <font style="color:#000000;">null和undefined：默认情况下 null 和 undefined 是所有类型的子类型。 就是说你可以把 null 和 undefined 赋值给 number 类型的变量。当你指定了 --strictNullChecks 标记，null 和 undefined 只能赋值给 void 和它们各自。null表示这里有值，但是个空值。undefined表示这里没有值。</font>
+ <font style="color:#000000;">void：没有任何类型，</font>**<font style="color:#000000;">void表示空值。void用于描述一个内部没有return语句，或者没有显示return一个值的函数的返回值</font>**

## <font style="color:#000000;">keyof和typeof</font>
keyof可以获取一个类型所有键值，返回一个联合类型

```javascript
type Person = {
  name: string;
  age: number;
}
// PersonKey得到的类型为 'name' | 'age'
type PersonKey = keyof Person;  
```

typeof是获取一个对象/实例的类型

```javascript
const me: Person = { name: 'gzx', age: 16 };
type P = typeof me;  // { name: string, age: number | undefined }
const you: typeof me = { name: 'mabaoguo', age: 69 }  // 可以通过编译
```

## <font style="color:#000000;">泛型是什么</font>
语法格式简单总结为

```javascript
类型名<泛型列表> 具体类型定义
```

它能够创建可以使用多种数据类型而不是单一数据类型的组件。 而且，它在不影响性能或生产率的情况下提供了类型安全性。 泛型允许我们创建泛型类，泛型函数，泛型方法和泛型接口。

在泛型中，类型参数写在左括号（<）和右括号（>）之间，这使它成为强类型集合。 它使用一种特殊的类型变量来表示类型

```javascript
function identity<T>(arg: T): T {
  return arg;
}
let output1 = identity<string>("CoderBin");
let output2 = identity<number>( 117 );
console.log(output1);
console.log(output2);
```

### 泛型工具
1. Partial<T>：将泛型中全部属性变为可选的

```javascript
type Partial<T> = {
	[P in keyof T]?: T[P]
}
// 例如
type Animal = {
  name: string,
  category: string,
  age: number,
  eat: () => number
}
type PartOfAnimal = Partial<Animal>;
const ww: PartOfAnimal = { name: 'ww' }; 
```

2.  Record<K,T>：将k中所有属性值转化为T类型，我们常用它来声明一个普通object对象

```javascript
type Record<K extends keyof any,T> = {
  [key in K]: T
}
// 例如
const obj: Record<string, string> = { 
  'name': 'zhangsan', 
  'tag': '打工人' 
}
```

3. exclude<T,U>：<font style="color:rgb(37, 41, 51);">此工具是在 T 类型中，去除 T 类型和 U 类型的交集，返回剩余的部分。</font>

```javascript
type Exclude<T, U> = T extends U ? never : T
// 例如
type T1 = Exclude<"a" | "b" | "c", "a" | "b">;   // "c"
type T2 = Exclude<string | number | (() => void), Function>; // string | number
```

4. Omit<T,K>：<font style="color:rgb(37, 41, 51);">适用于键值对对象的 Exclude，它会去除类型 T 中包含 K 的键值对。</font>

```javascript
type Omit = Pick<T, Exclude<keyof T, K>>
// 例如
const OmitAnimal:Omit<Animal, 'name'|'age'> = { category: 'lion', eat: () => { console.log('eat') } }
```

5. Required<T>：<font style="color:rgb(37, 41, 51);">此工具可以将类型 T 中所有的属性变为必选项</font>

```javascript
type Required<T> = {
  [P in keyof T]-?: T[P]
}
```

6. Pick<T,K>：<font style="color:rgb(37, 41, 51);">将 T 类型中的 K 键列表提取出来，生成新的子键值对类型。</font>

```javascript
type Pick<T, K extends keyof T> = {
  [P in K]: T[P]
}
// 例如
const bird: Pick<Animal, "name" | "age"> = { name: 'bird', age: 1 }
```

## <font style="color:#000000;">ts中?., ??, !, !., _, **的含义</font>
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1701584535211-07d0f618-e3ef-436d-9ff0-917f2497a194.png)

## <font style="color:#000000;">协变、逆变、双变、抗变</font>
<font style="color:#000000;">协变：</font><font style="color:#000000;">X = Y ，</font><font style="color:#000000;">Y 类型可以赋值给 X 类型的情况就叫做协变，也可以说是 X 类型兼容 Y 类型</font>

```javascript
interface X { name: string; age: number; } 
interface Y { name: string; age: number; hobbies: string[] }
let x: X = { name: 'xiaoming', age: 16 }
let y: Y = { name: 'xiaohong', age: 18, hobbies: ['eat'] }
x = y
```

逆变：<font style="color:#000000;">printY = printX 函数X 类型可以赋值给函数Y 类型，因为函数Y 在调用的时候参数是按照Y类型进行约束的，但是用到的是函数X的X的属性和方法，ts检查结果是类型安全的。这种特性就叫做逆变，函数的参数有逆变的性质。</font>

```javascript
let printY: (y: Y) => void
printY = (y) => { console.log(y.hobbies) }
let printX: (x: X) => void
printX = (x) => { console.log(x.name) }
printY = printX
```

<font style="color:#000000;">双变（双向协变）：</font><font style="color:#000000;">X = Y；Y = X</font><font style="color:#000000;">父类型可以赋值给子类型，子类型可以赋值给父类型，既逆变又协变，叫做“双向协变”</font>

> <font style="color:#000000;">ts2.x 之前支持这种赋值，之后 ts 加了一个编译选项 strictFunctionTypes，设置为 true 就只支持函数参数的逆变，设置为 false 则支持双向协变</font>
>

<font style="color:#000000;">抗变（不变）：非父子类型之间不会发生型变，只要类型不一样就会报错</font>

## <font style="color:#000000;">ts的tsconfig.json文件中有哪些配置项信息</font>
> [tsconfig.json](https://www.tsdev.cn/tsconfig-json.html)
>

```javascript
{
  // 用来指定被编译的文件列表，只有编译少量文件才使用
  "files": ["1.ts"],
  // 用来指定哪些文件需要被编译
  "include": [
    // ** : 任意目录 ， * : 任意文件
    "./src/**/*"
  ],
  // 用来指定哪些文件不需要被编译 ：默认node_module
  "exclude": [
    "./src/hello/**/*"
  ],
  // 可以让IDE在保存文件的时候根据tsconfig.json重新生成文件
  "compileOnSave": false,
  // 用来指定继承的配置文件
  "extends": "",
  // 编译器的选项是配置文件中非常重要也是非常复杂的配置选项
  "compilerOptions": {
    "baseUrl": ".", 
    "paths": { 
       "@helper/*": ["src/helper/*"], 
       "@utils/*": ["src/utils/*"], 
       ... 
    } 
  }
}
```

## <font style="color:#000000;">简述工具类型</font>**<font style="color:#000000;">Exclude、Omit、Merge、Intersection、Overwrite的作用</font>**
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1701590582381-37fda2b9-a31c-4836-b140-a1adb860630d.png)

```javascript
interface Todo {
  title: string
  description: string
  completed: boolean
  createdAt: number
}
type TodoPreview = Omit<Todo, "description">
```

# <font style="color:#000000;">基础知识</font>
## <font style="color:#000000;">原始类型与对象类型</font>
1. <font style="color:#000000;"> js中null表示这里有值但是是个空值，undefined表示这里没有值；而在ts中null和undefined都是有具体意义的值，在没有开启strictNullChecks检查的情况下，会被视作其他类型的子类型 </font>
2. <font style="color:#000000;"> ts中void用于描述一个内部没有return语句，或者没有显示return一个值的函数的返回值 </font>
3. <font style="color:#000000;"> 元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。具名元组可以为元组元素打上类似属性的标记 </font>

```javascript
const arr7: [name: string, age: number, male: boolean] = ['linbudu', 599, true];
```

4. <font style="color:#000000;"> 数组与元组的区别 </font>
    - <font style="color:#000000;">只能将整个数组/元组标记为只读，而不能像对象那样标记某个属性为只读</font>
    - <font style="color:#000000;">一旦被标记为只读，则这个只读数组/元组的类型上，将不再具有push，pop等会修改原数组的方法</font>
5. <font style="color:#000000;"> readonly关键字：防止对象的属性被再次赋值 </font>
6. <font style="color:#000000;"> 在ts中Object包含所有的数据类型；object代表所有非原始类型的类型（数组，对象，函数）；{}也包含所有类型，但是不能对这个变量进行任何赋值操作 </font>
7. <font style="color:#000000;"> 在js中symbol代表一个唯一的值的类型，类似于字符串类型，可以作为对象的属性名，但是在ts中，symbol类型指的都是ts中的同一个类型。在ts中支持了unique symbol这一类型声明，他是symbol类型的子类型，每一个unique symbol类型都是独一无二的。在ts中，如果想要引用已创建的unique symbol类型，则需要适用类型查询操作符typeof </font>

```javascript
declare const uniqueSymbolFoo: unique:symbol;
const uniqueSymbolBaz: typeof uniqueSymbolFoo = uniqueSymbolFoo
```

## <font style="color:#000000;">字面量类型和枚举</font>
1. <font style="color:#000000;"> 字面量类型主要包括 字符串字面量类型 、 数字字面量类型 、布尔字面量类型和对象字面量类型 </font>
2. <font style="color:#000000;"> 联合类型 | ,代表了一组类型的可用集合 </font>
    1. <font style="color:#000000;">对于联合类型中的函数类型，需要使用括号（）包裹起来</font>
    2. <font style="color:#000000;">函数类型并不存在字面量类型，因此这里的 </font>`<font style="color:#000000;">(() => {})</font>`<font style="color:#000000;"> 就是一个合法的函数类型</font>
    3. <font style="color:#000000;">你可以在联合类型中进一步嵌套联合类型，但这些嵌套的联合类型最终都会被展平到第一级中</font>
3. <font style="color:#000000;"> 无论是原始类型还是对象类型的字面量类型，它们的本质都是类型而不是值 。它们在编译时同样会被擦除，同时也是被存储在内存中的类型空间而非值空间 </font>
4. <font style="color:#000000;"> 枚举 </font>
    1. <font style="color:#000000;"> 如果你使用了延迟求值，那么没有使用延迟求值的枚举成员必须放在使用常量枚举值声明的成员之后（如上例），或者放在第一位 </font>

```javascript
enum Items {
  Foo = returnNum(),
  Bar = 599,
  Baz
}
// 或者
enum items {
  Baz,
  Foo = returnNum(),
  Bar = 599,
}
```

    2. <font style="color:#000000;"> 枚举和对象的区别：对象是单向映射的，我们只能从键映射到键值。</font>**<font style="color:#000000;">而枚举是双向映射的。 </font>**
    3. <font style="color:#000000;"> 仅有值为数字的枚举成员才能够进行这样的双向枚举，字符串枚举成员仍然会进行单次映射 </font>
    4. <font style="color:#000000;"> 如果你没有声明枚举的值，它会默认使用数字枚举，并且从 0 开始，以 1 递增 </font>

```plain
enum Items {
  Foo, // 0
  Bar, // 1
  Baz  // 2
}
```

## <font style="color:#000000;">函数</font>
1. <font style="color:#000000;"> 函数类型签名，下列代码中(name: string) => number称为函数类型签名 </font>

```javascript
const foo:(name: string) => number = function (name){
  return name.length;
}
```

<font style="color:#000000;">下面两种方式是等价的 </font>

```javascript
// 方式一：不建议使用，因为代码可读性比较差
const foo:(name: string) => number = function (name){
  return name.length;
}
// 方式二
const foo = (name: string): number => {
  return name.length;
}
```

2. <font style="color:#000000;"> 我们可以使用interface来进行函数声明 </font>

```javascript
interface FuncFooStruct {
  (name: string): number
}
```

3. <font style="color:#000000;"> 可选参数必须位于必选参数之后，因为js中函数的入参是按照位置，而不是按照参数名进行传递 </font>
4. 不传入参数时函数会使用此参数的默认值

```javascript
// 直接为可选参数声明默认值
function foo2(name: string, age: number = 18): number {
  const inputAge = age;
  return name.length + inputAge
}
```

5. <font style="color:#000000;"> 重载：函数或者方法有相同的名称但是参数列表不相同的情形 </font>
6. <font style="color:#000000;"> class中的setter方法不允许进行返回值饿类型标注，可以理解为setter的返回值并不会被消费，这只是一个关注过程的函数 </font>
7. <font style="color:#000000;"> 修饰符 </font>
    1. <font style="color:#000000;">public：访问性修饰符。此类成员在类、类的实例、子类中都能被访问</font>
    2. <font style="color:#000000;">private：访问性修饰符。此类成员仅能在类的内部被访问</font>
    3. <font style="color:#000000;">protected：访问性修饰符。此类成员仅能在类与子类中被访问</font>
    4. <font style="color:#000000;">readonly：操作性修饰符</font>
8. <font style="color:#000000;"> 静态成员：在ts中可以使用static关键字来标识一个成员为静态成员。在类的内部静态成员无法通过this来访问，如下图所示，我们只能通过Foo.statichandler这种形式进行访问。静态成员不会被实例继承</font>![](image/ts/1667828594128.png)<font style="color:#000000;"> </font>
9. <font style="color:#000000;"> ts中使用extends实现继承。基类中的那些成员能够被派生类访问，完全是由其访问性修饰符决定的 </font>

```javascript
class base {} // 基类
class Derived extends Base {}  // 派生类
```

9. <font style="color:#000000;"> ts 4.3中新增了override关键字，来确保派生类尝试覆盖的方法一定在基类中存在定义。下图中会报错，因为尝试覆盖的方法并未在基类中声明</font>![](image/ts/1667829468682.png)<font style="color:#000000;"> </font>
10. <font style="color:#000000;"> 抽象类：一个抽象类描述了一个类中应当有哪些成员，一个抽象方法描述了这一方法在实际实现中的结构。抽线方法其实描述的就是这个方法的入参类型和返回值类型。抽象类中的成员也需要使用abstract关键字才能被视为抽象类成员。 </font>
11. <font style="color:#000000;"> 私有构造函数：类的构造函数使用private进行标记时，只允许在类的内部访问，即我们不能实例化这个类。但是我们将类作为utils方法时，此时 Utils 类内部全部都是静态成员，我们也并不希望真的有人去实例化这个类。此时就可以使用私有构造函数来阻止它被错误地实例化 </font>
12. <font style="color:#000000;"> </font>[<font style="color:#000000;">SOLID原则</font>](https://juejin.cn/book/7086408430491172901/section/7100487738012467212)<font style="color:#000000;"> （该页最后部分内容） </font>
    1. <font style="color:#000000;">S：单一功能原则，一个类应该仅具有一种职责，这也意味着只存在一种原因使得需要修改类的代码。如对于一个数据实体的操作，其读操作和写操作也应当被视为两种不同的职责，并被分配到两个类中。更进一步，对实体的业务逻辑和对实体的入库逻辑也都应该被拆分开来。</font>
    2. <font style="color:#000000;">O：开放封闭原则，一个类应该是可扩展但不可修改的。</font>
    3. <font style="color:#000000;">L：里氏替换原则，一个派生类可以在程序的任何一处对其基类进行替换 。这也就意味着，子类完全继承了父类的一切，对父类进行了功能地扩展（而非收窄）。</font>
    4. <font style="color:#000000;">I：接口分离原则，类的实现方应当只需要实现自己需要的那部分接口</font>
    5. <font style="color:#000000;">D：依赖倒置原则，这是实现开闭原则的基础，它的核心思想即是对功能的实现应该依赖于抽象层，即不同的逻辑通过实现不同的抽象类</font>

## <font style="color:#000000;">内置类型：any，unknown，never</font>
1. <font style="color:#000000;">any表示任意类型，any类型使用小tips </font>
    1. <font style="color:#000000;">如果是类型不兼容报错导致你使用any，考虑使用类型断言替代</font>
    2. <font style="color:#000000;">如果是类型太复杂导致你不想声明而使用any，考虑将这一处的类型去断言为你需要的最简类型</font>
    3. <font style="color:#000000;">如果你是想表达一个未知类型，更合理的方式是使用unknown</font>
2. <font style="color:#000000;">一个unknown类型的变量可以再次赋值为任意其他类型，但只能赋值给any与unknownn类型的变量</font>
3. <font style="color:#000000;">any放弃了所有的类型检查，unknown并没有，当对一个设置了unknown的变量进行点属性的时候，会报错</font>
4. <font style="color:#000000;">never类型表示什么都没有，当函数抛出一个错误时，可以限制返回值类型为never</font>
5. <font style="color:#000000;">类型断言能够显示告知类型检查程序当前这个变量的类型，可以进行类型分析的修正、类型。其实就是一个将变量的已有类型更改为新指定类型的操作，基本语法为 as NewType，可以将any/known类型断言到一个具体的类型。正确使用方式为</font>**<font style="color:#000000;">在ts类型分析不正确或不符合预期的时候将其断言为此处的正确类型。除了as也可以使用<></font>**
6. <font style="color:#000000;">非空断言使用!语法的形式标记前面的一个声明一定是非空的，即剔除了null和undefined</font>
7. <font style="color:#000000;">类型层级关系 </font>
    - <font style="color:#000000;">最顶级的类型，any 与 unknown</font>
    - <font style="color:#000000;">特殊的 Object ，它也包含了所有的类型，但和 Top Type 比还是差了一层</font>
    - <font style="color:#000000;">String、Boolean、Number 这些装箱类型</font>
    - <font style="color:#000000;">原始类型与对象类型</font>
    - <font style="color:#000000;">字面量类型，即更精确的原始类型与对象类型嘛，需要注意的是 null 和 undefined 并不是字面量类型的子类型</font>
    - <font style="color:#000000;">最底层的 never</font>

## <font style="color:#000000;">类型工具</font>
1. <font style="color:#000000;"> 可以使用type关键字声明类型别名 </font>
2. <font style="color:#000000;"> 工具类同样基于类型别名，只是多了个泛型 </font>

```plain
type Fatory<T> = T | number |string
```

3. <font style="color:#000000;"> 我们常用的工具类型 </font>

```javascript
type MaybeArray<T> = T | T[];
type MaybeNull<T> = T | null;
```

4. <font style="color:#000000;"> 交叉类型使用&</font>

### <font style="color:#000000;">索引类型</font>
1. <font style="color:#000000;"> 索引签名类型：主要指的是在接口或类型别名中</font>
+ <font style="color:#000000;"> 在js中，对于object[prop]形式的访问会将数字索引访问转换为字符串索引访问，即obj[123]和obj['123']效果是一样的 </font>
+ <font style="color:#000000;"> 索引签名类型的一个常见场景是在重构 JavaScript 代码时，为内部属性较多的对象声明一个 any 的索引签名类型，以此来暂时支持 </font>**<font style="color:#000000;">对类型未明确属性的访问</font>**<font style="color:#000000;"> ，并在后续一点点补全类型</font>
2. <font style="color:#000000;">索引类型查询 keyof操作符。</font>
+ <font style="color:#000000;"> 可以将对象中所有键转换为对应字面量类型，然后再组成联合类型。</font>**<font style="color:#000000;">这里并不会将数字类型的简明转换为字符串类型字面量，而是仍然保持为数字类型字面量</font>**<font style="color:#000000;"> </font>
+ <font style="color:#000000;"> keyof的产物必定是个联合类型</font>
3. <font style="color:#000000;">索引类型访问 obj[expression]</font>
+ <font style="color:#000000;"> expression表达式会先被执行，然后使用返回值来访问属性 </font>
+ <font style="color:#000000;"> 索引类型访问的本质是通过键的字面量类型访问这个键对应的键值类型</font>

### <font style="color:#000000;">映射类型</font>
1. <font style="color:#000000;"> 下图所示例子会接受一个对象类型，使用keyof获得这个对象类型的键名组成字面量联合类型，然后通过映射类型将这个联合类型的每一个成员映射出类，并将其键值类型设置为string</font>

### <font style="color:#000000;">类型查询操作符</font>
<font style="color:#000000;">ts中typeof返回的是一个ts类型</font>

## <font style="color:#000000;">泛型</font>
1. <font style="color:#000000;"> 泛型可以理解为变量</font>
2. <font style="color:#000000;"> Partial的实现 </font>

```javascript
type Partial<T> = {
  [P in keyof T]?: T[P];
}
// 使用举例
interface IFoo {
  prop1: string;
  prop2: number;
  prop3: boolean;
  prop4: () => void;
}

type PartialIFoo = Partial<IFoo>;

// 等价于
interface PartialIFoo {
  prop1?: string;
  prop2?: number;
  prop3?: boolean;
  prop4?: () => void;
}
```

3. <font style="color:#000000;"> 在ts中，泛型参数存在默认约束，这个月数值在TS 3.9版本以前是any，3.9版本以后是unknown </font>
4. <font style="color:#000000;"> 多泛型参数其实就像接受更多参数的函数，其内部的运行逻辑会更加抽象，表现在参数需要进行的逻辑运算会更加复杂</font>![](image/ts/1667992089162.png)<font style="color:#000000;"> </font>
5. <font style="color:#000000;"> tsx文件中泛型的<>尖括号可能会造成报错，我们需要使其长得更像泛型</font>![](image/ts/1667993337173.png)<font style="color:#000000;"> </font>

## <font style="color:#000000;">结构化类型系统</font>
1. <font style="color:#000000;"> 鸭子类型：这个名字来源于鸭子测试，核心理念为如果A是M，A和B具有相同的属性则判断B也是M</font>![](image/ts/1668057739458.png)<font style="color:#000000;"> </font>
2. <font style="color:#000000;"> 标称类型系统：标称类型系统要求，两个可兼容的类型，其名称必须是完全一致的。对于标称类型系统，父子类型关系只能通过显示的继承来实现，称为标称子类型。</font>![](image/ts/1668058085240.png)<font style="color:#000000;"></font>
3. <font style="color:#000000;"> 类型，类型系统与类型检查： </font>
    - <font style="color:#000000;">类型：限制了数据的可用操作、意义、允许的值的集合，总的来说就是</font>**<font style="color:#000000;">访问限制</font>**<font style="color:#000000;">与 </font>**<font style="color:#000000;">赋值限制</font>**<font style="color:#000000;"> 。在 TypeScript 中即是原始类型、对象类型、函数类型、字面量类型等基础类型，以及类型别名、联合类型等经过类型编程后得到的类型。</font>
    - <font style="color:#000000;">类型系统：一组为变量、函数等结构分配、实施类型的规则，通过显式地指定或类型推导来分配类型。同时类型系统也定义了如何判断类型之间的兼容性：在 TypeScript 中即是结构化类型系统。</font>
    - <font style="color:#000000;">类型检查：确保 </font>**<font style="color:#000000;">类型遵循类型系统下的类型兼容性</font>**<font style="color:#000000;"> ，对于静态类型语言，在</font>**<font style="color:#000000;">编译时</font>**<font style="color:#000000;">进行，而对于动态语言，则在</font>**<font style="color:#000000;">运行时</font>**<font style="color:#000000;">进行。TypeScript 就是在编译时进行类型检查的</font>

## <font style="color:#000000;">类型系统层级</font>
**<font style="color:#000000;">never <</font>**<font style="color:#000000;"> </font>**<font style="color:#000000;">字面量类型 < 包含此字面量类型的联合类型（同一基础类型） < 对应的原始类型 < 原始类型对应的装箱类型 < Object 类型 < any/known</font>**![](image/ts/1668062118952.png)

+ <font style="color:#000000;"> 对于基类和派生类，通常情况下派生类会完全保留基类的结构，而只是自己新增新的属性与方法，在结构化类型的比较下，其类型自然会存在子类型关系。更不用说派生类本身就是 extends 基类得到的。 </font>
+ <font style="color:#000000;"> 对于联合类型的比较，我们只需要比较一个联合类型是否可以被视为另一个联合类型的子集，即这个联合类型中所有成员在另一个联合类型中都能找到</font>

## <font style="color:#000000;">条件类型与infer(目前有点看不太懂)</font>
1. <font style="color:#000000;">infer关键字：infer是inference的缩写，infer R中R表示待推断的类型。infer只能在条件类型中使用</font>![](image/ts/1668069682674.png)

## <font style="color:#000000;">内置工具类型基础</font>
1. <font style="color:#000000;"> 工具类型的分类： </font>
    - <font style="color:#000000;">属性修饰工具类型：对对象属性和数组元素的可选/必选，只读/可写</font>
    - <font style="color:#000000;">结构工具类型：对既有类型的裁剪、拼接、转换等</font>
    - <font style="color:#000000;">集合工具类型：对集合（即联合类型）的处理，即交集、并集、差集、补集</font>
    - <font style="color:#000000;">模式匹配工具类型：基于 infer 的模式匹配，即对一个既有类型特定位置类型的提取</font>
    - <font style="color:#000000;">模板字符串工具类型：模板字符串专属的工具类型</font>
2. <font style="color:#000000;"> 属性修饰工具类型 </font>

```javascript
 // Partial 标记属性为可选
type Partial<T> = {
  [P in keyof T]?: T[P]
}
// Required 如果原本属性上有？这个标记，则移除
type Required<T> = {
  [P in keyof T]-?: T[P];
}
type Readonly<T> = {
  readonly [P in keyof T]: T[P]
}
```

3. <font style="color:#000000;"> 结构工具类型： </font>
    - <font style="color:#000000;"> 结构声明：快速声明一个结构，比如Record,我们常使用 </font>`<font style="color:#000000;">Record<string, unknown></font>`<font style="color:#000000;">和 </font>`<font style="color:#000000;">Record<string, any></font>`<font style="color:#000000;">代替object </font>

```javascript
type Record<K extends keyof any, T> = {
    [P in K]: T;
};
// 键名均为字符串，键值类型未知
type Record1 = Record<string, unknown>;
// 键名均为字符串，键值类型任意
type Record2 = Record<string, any>;
```

    - <font style="color:#000000;"> 结构处理工具类型：Pick和Omit。Pick是保留这些传入的键，Omit是移除这些传入的键 </font>

```javascript
type Pick<T,K extends keyof T> = {
   [P in K]: T[P]
}
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>
```

4. <font style="color:#000000;"> 集合工具类型 </font>
    - <font style="color:#000000;">交集: </font>`<font style="color:#000000;">type Extract<T,U> = T extends U ? T : never</font>`<font style="color:#000000;">;</font>
    - <font style="color:#000000;">差集: </font>`<font style="color:#000000;">type Exclude<T,U> = T extends U ? never : T</font>`<font style="color:#000000;">;</font>
    - <font style="color:#000000;">并集: </font>`<font style="color:#000000;">type Concurrence<A, B> = A | B;</font>`
    - <font style="color:#000000;">补集: </font>`<font style="color:#000000;">type Complement<A, B extends A> = Exclude<A, B></font>`

## <font style="color:#000000;">上下文类型</font>
<font style="color:#000000;">上下文类型的核心理念：基于位置的类型推导，基于已定义的类型来规范开发者的使用</font>

## <font style="color:#000000;">协变与逆变</font>
<font style="color:#000000;">协变：随着某一个量的变化随之变化一致的即称为协变  
</font><font style="color:#000000;">逆变：随着某一个量的变化随着变化相反的即称为逆变</font>

## <font style="color:#000000;">在React中愉快的使用Ts：内置类型和泛型坑位</font>
### <font style="color:#000000;">组件声明：我们声明一个 React 组件的方式</font>
### <font style="color:#000000;">泛型坑位：React API 中预留出的泛型坑位</font>
### <font style="color:#000000;">内置类型定义</font>
## 命名空间和模块
两个的使用场景：

+ 命名空间：当我们在程序内部，防止全局污染的时候或者想把相关的内容放在一起
+ 模块：
    - 当我们封装了一个工具或者库，想适用于一个模块系统引入的时候
    - 实际上模块也有防止全局污染的作用，所以大多数情况下我们都是用模块



