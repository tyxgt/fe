# VUE2
## .sync修饰符
> [修饰符.sync(面试重点) - 掘金](https://juejin.cn/post/6996257854663426056?searchId=20231130104438D2681E867ED16DBE152C)
>

绑定一个属性并为其添加v-on监听事件

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1691723075179-544dc363-14ac-4e0e-84d3-e550f6c0eb01.png)

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1691723104620-5842cc6b-9612-4f17-9b32-ba0f692240e5.png)

## 混入mixin
[https://v2.cn.vuejs.org/v2/guide/mixins.html](https://v2.cn.vuejs.org/v2/guide/mixins.html)

1. 当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行合并，<font style="color:rgb(48, 68, 85);">比如，数据对象在内部会进行递归合并，并在发生冲突时以组件数据优先</font>
2. <font style="color:rgb(48, 68, 85);">混入对象的钩子将在组件自身钩子</font>**<font style="color:rgb(39, 56, 73);">之前</font>**<font style="color:rgb(48, 68, 85);">调用</font>
3. <font style="color:rgb(48, 68, 85);">全局注册混入的话，将影响每一个之后创建的Vue实例</font>

## <font style="color:#000000;">自定义指令</font>
[https://v2.cn.vuejs.org/v2/guide/custom-directive.html](https://v2.cn.vuejs.org/v2/guide/custom-directive.html)

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

# VUE3
## 模板语法
![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1692326691323-31a13b8b-c9d3-4e98-98dc-b1d286832a4d.png)

## 响应式状态
## 响应式
### 声明ref()——作为声明响应式状态的主要API
1. 组合式API中，推荐使用ref()函数来声明响应式状态，ref()接收参数，并将其包裹在一个带有.value属性的ref对象中返回

```javascript
const count = ref(0)

console.log(count) // { value: 0 }
console.log(count.value) // 0

count.value++
console.log(count.value) // 1
```

2. 在模板中使用 ref 时，我们**不**需要附加 .value。为了方便起见，当在模板中使用时，ref 会自动解包
3. 为什么要使用ref？

当我们在模板中使用了一个ref，然后改变了这个ref的值时，vue会自动检测到这个变化，并且相应的更新dom，这是依赖追踪的响应式系统实现的，当一个组件首次渲染时，vue会追踪在渲染过程中使用的每一个ref，然后，当一个ref被修改时，它会触发追踪它的组件的一个重新渲染

4. 深层响应性：ref可以持有任何类型的值，包括深层嵌套的对象、数组或者js内置的数据结构。非原始值将通过reactive()转换为响应式代理，也可以通过[shallow ref](https://cn.vuejs.org/api/reactivity-advanced.html#shallowref)来放弃深层响应性，<font style="color:#000000;">对于浅层 ref，只有 </font><font style="color:#000000;">.value</font><font style="color:#000000;"> 的访问会被追踪。浅层 ref 可以用于避免对大型数据的响应性开销来优化性能、或者有外部库管理其内部状态的情况。</font>

#### DOM更新时间
当修改了响应式状态时，dom会被自动更新，但是更新是不同步的，vue会在nexttick更新周期中缓冲所有状态的修改，以确保不管你进行了多少次状态修改，每个组件都只会被更新一次，要等待dom更新完成后再执行额外的代码，可以使用nextTick()全局API

### reactive()
1. 与ref不同的是，reactive()将使对象本身具有响应性。响应式对象是Proxy，其行为和普通对象一样，不同的是，vue能够拦截<font style="color:#000000;">对响应式对象所有属性的访问和修改，以便进行依赖追踪和触发更新。</font>
2. <font style="color:#000000;">reactive()将深层的转换对象：当访问嵌套对象时，他们也会被reactive()包装，当ref的值是一个对象时，ref()也会在内部调用它。与浅层ref类似，这里也有一个</font>[<font style="color:#000000;">shallowReactive()</font>](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreactive)<font style="color:#000000;"> API 可以选择退出深层响应性</font>

> <font style="color:#000000;">只有代理对象是响应式的，更改原始对象不会触发更新，因此，使用vue的响应式系统的最佳实践是仅使用你生命对象的代理版本</font>
>

3. **<font style="color:#000000;">为保证访问代理的一致性，对同一个原始对象调用reactive()会总是返回同样的代理对象，而对一个已存在的代理对象调用reactive()会返回其本身</font>**
4. <font style="color:#000000;">缺点：</font>
    - <font style="color:#000000;">有限的值类型：只能用于对象类型（对象，数组，map，set这样的集合类型），不能持有原始类型</font>
    - <font style="color:#000000;">不能替换整个对象</font>
    - <font style="color:#000000;">对解构操作不友好：当我们将响应式对象的原始类型属性结构为本地变量时，或者将该属性传递给函数时，我们将丢失响应式连接</font>

### ref和reactive的区别——todo
## 计算属性
1. computed()方法期望接收一个getter函数，返回值为一个计算属性ref。计算属性默认是只读的。

```javascript
const bookMessage = computed(() => {
  return author.books.length > 0 ? 'Yes' : 'No'
})
```

上述代码块中，可以直接通过访问bookMessage.value访问计算结果。计算属性ref也会在模板中自动解包。

2. 计算属性默认是只读的，当尝试修改一个计算属性时会收到一个运行时警告。从计算属性返回的值是派生状态，是一个临时快照，所以当数据源变化的时候，就会创建一个新的快照，所以，更改快照是没有意义的。只在某些特殊场景中<font style="color:rgb(33, 53, 71);">可能才需要用到“可写”的属性，可以通过同时提供 getter 和 setter 来创建</font>
3. <font style="color:rgb(33, 53, 71);">计算属性的getter不应该有副作用，应只做计算。</font>

> 副作用是指让一个函数变得不再纯净的东西，一个纯净的函数，无论何时何地执行，都会得到稳定的结果。
>
> 常见的副作用包括：对外部可变数据或变量的修改，外部接口的调用尤其是IO，异常的抛出。
>
> eg：
>
> + <font style="color:rgb(51, 51, 51);">对外部可变数据或变量的修改: 全局变量 / 闭包变量 / dom对象 / bom对象的读写操作</font>
> + <font style="color:rgb(51, 51, 51);">外部接口的调用尤其是IO：dom对象 / bom对象的方法调用; xhr / fetch这样的网络IO；console / LocalStorage这样的磁盘IO</font>
> + <font style="color:rgb(51, 51, 51);">异常的抛出：函数中的某些代码可能会抛出异常或者执行出错</font>
>
> [<u><font style="color:rgb(51, 51, 51);">https://juejin.cn/post/6905234297360220174</font></u>](https://juejin.cn/post/6905234297360220174)
>

## 生命周期
完整生命周期钩子函数

+ onBeforeMount：挂载之前可以调用
+ onMount：挂载后调用
+ onBeforeUpdate：当响应数据改变，且重新渲染前调用
+ onUpdated：重新渲染后调用
+ onBeforeUnmount：Vue实例销毁前调用
+ onUnmounted：实例销毁后调用
+ onActivated：当keep-alive组件被激活后调用
+ onDeactivated：当keep-alive组件取消激活时调用
+ onErrorCaptured：从子组件中捕获错误时调用

## 侦听器
1. watch的第一个参数可以是不同形式的数据源：他可以是一个ref（包括计算属性）、一个响应式对象、一个getter函数，或多个数据源组成的数组，**不能直接侦听响应式对象的属性值，可以返回该属性的getter函数**

```javascript
const obj = reactive({ count: 0 })
// 错误，因为 watch() 得到的参数是一个 number
watch(obj.count, (count) => {
  console.log(`count is: ${count}`)
})
// 提供一个 getter 函数
watch(
  () => obj.count,
  (count) => {
    console.log(`count is: ${count}`)
  }
)
```

2. 直接给watch(）传入一个响应式对象，会隐式的创建一个深层侦听器，该回调函数在所有嵌套的变更时都会被触发，给watch显示添加deep选项，强制转换成深层侦听器
3. watch默认是懒执行的：仅当数据源变化时，才会执行回调。但在某些场景中，我们希望再创建侦听器时，立即执行一遍回调。我们可以通过传入immediate：true选项来强制侦听器的回调立即执行。
4. watchEffect()允许我们自动跟踪回调的响应式依赖。

```javascript
const todoId = ref(1)
const data = ref(null)

watch(todoId, async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  data.value = await response.json()
}, { immediate: true })

// 上下两种方式等效，
// 下面这种方式不需要指定immediate: true，在执行期间，它会自动追踪todoId.value作为依赖
// 每当 todoId.value 变化时，回调会再次执行
watchEffect(async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  data.value = await response.json()
})
```

### watch和watchEffect的异同
watch和watchEffect都能响应式地执行有副作用的回调，他们之间的主要区别是追踪响应式依赖的方式：

+ watch只追踪明确侦听的数据源，他不会追踪任何在回调中访问到的东西。另外只在数据源确实改变时才会触发回调，watch会避免在发生副作用时追踪依赖，因此，我们能更加精确地控制回调函数的触发时机
+ watchEffect则会在副作用发生期间追踪依赖，他会在同步执行过程中，自动追踪所有能访问到的响应式属性，这更方便，而且代码往往更简洁，但有时其响应性依赖关系会不那么明确

### 回调的触发时机
默认情况下，**用户创建的侦听器回调，都会在vue组件更新之前被调用**，这意味着在侦听器回调中访问的dom将是被vue更新之前的状态。如果想在侦听器回调中能访问被vue更新之后的dom，需要指明flush: 'post'选项

**后置刷新的watchEffect() 有个更方便的别名watchPostEffect()，他会在vue更新之后执行**

### 停止侦听器
侦听器必须用同步语句创建：如果用异步回调创建一个侦听器，那么它不会绑定到当前组件上，必须手动停止它，以防内存泄漏。手动停止一个侦听器，可以调用watch或watchEffect返回的函数

> 需要一步创建侦听器的情况很少，请尽可能选择同步创建，如果需要等待一些异步数据，你可以使用条件式的侦听逻辑
>

## 访问模板引用
为了通过组合式API获得模板引用，我们需要声明同名的ref

```javascript
<script setup>
import { ref, onMounted } from 'vue'

// 声明一个 ref 来存放该元素的引用
// 必须和模板里的 ref 同名
const input = ref(null)

onMounted(() => {
  input.value.focus()
})
</script>

<template>
  <input ref="input" />
</template>
```

只可以在组件挂在后才能访问模板引用，如果想在模板表达式上访问input，在初次渲染的时候会是null，因为初次渲染前这个元素还不存在，**如果需要侦听一个模板引用ref的变化，确保考虑到其值为null的情况**

### v-for中的模板引用
当在v-for中使用模板引用时，对应的ref中包含的值是一个数组，它将在元素被挂在后包含对应整个列表的所有元素，**ref数组并不保证与源数组相同的顺序**

### 组件上的ref
使用了<script setup>的组件是默认私有的：一个父组件无法访问到一个使用了<script setup>的子组件中的任何东西，除非子组件在其中通过defineExpose宏显示暴露

## 传递props
组合式API中需要使用defineProps编译宏命令。defineProps会返回一个对象，其中包含了可以传递给组件的所有props；

选项是API中和vue2中的props声明方式相同

## 监听事件
我们可以使用defineEmits宏来声明需要抛出的事件，defineEmits仅可用于<script setup>之间，它返回一个等同于$emit方法的emit函数

子组件

```javascript
<template>
  <div class="blog-post">
    <h4>{{ title }}</h4>
    <button @click="$emit('enlarge-text')">Enlarge text</button>
  </div>
</template>
<script setup>
defineProps(['title'])
defineEmits(['enlarge-text'])
</script>
```

父组件

```javascript
<BlogPost
  ...
  @enlarge-text="postFontSize += 0.1"
 />
```

## 组件注册
### 全局注册&局部注册
1. 使用全局注册的情况下，并没有被使用的组件无法在生产打包时被自动移除（tree-shaking）。如果全局注册了一个组件，即使并没有被实际使用，仍然会出现在打包后的js文件中
2. 全局注册在大型项目中使项目的依赖关系不明确，在父组件中使用子组件时，不太容易定位子组件的实现，和使用过多的全局变量一样，可能会影响应用长期的可维护性

## 组件v-model
1. 原生元素上的v-model

```javascript
<input v-model="searchText" />
// 等价于 
<input
  :value="searchText"
  @input="searchText = $event.target.value"
/>
```

2. 组件元素上的v-model

```javascript
<CustomInput v-model="searchText" />
// 等价于
<CustomInput
  :modelValue="searchText"
  @update:modelValue="newValue => searchText = newValue"
/>
    
<!-- CustomInput.vue -->
<script setup>
defineProps(['modelValue'])
defineEmits(['update:modelValue'])
</script>

<template>
  <input
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>
```

### 处理v-model修饰符
自定义组件的v-model支持自定义的修饰符

+ 组件的v-model上所添加的修饰符，可以通过modelModifiers prop在组件内访问到

```javascript
<MyComponent v-model.capitalize="myText" />
<script setup>
const props = defineProps({
  modelValue: String,
  modelModifiers: { default: () => ({}) }
})

defineEmits(['update:modelValue'])

console.log(props.modelModifiers) // { capitalize: true }
</script>

<template>
  <input
    type="text"
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>
```

### 带参数的v-model修饰符
对于又有参数又有修饰符的v-model绑定，生成的prop名将是arg+"Modifiers"

```javascript
<MyComponent v-model:title.capitalize="myText">
// 相应的声明是
const props = defineProps(['title', 'titleModifiers'])
defineEmits(['update:title'])
console.log(props.titleModifiers) // { capitalize: true }
```

## 透传Attributes
透传attributes指的是传递给一个组件，却没有被该组件声明为props或emits的attribute或者v-on事件监听器，最常见的例子是class，style，id

### 禁用attributes继承
如果不想要一个组件自动地继承attribute，可以在组件选项中设置inheritAttrs：false。在<script setup>中使用defineOptions，透传进来的attribute可以在模板的表达式中直接用$attrs访问到。这个$attrs对象包含了除组件所声明的props和emits之外的所有其他attribute

```javascript
<script setup>
defineOptions({
  inheritAttrs: false
})
// ...setup 逻辑
</script>
```

注：

+ 和props不同的是，透传attributes在js中保留了他们原始的大小写，所以像foo-bar这样的一个attribute需要通过$attrs['foo-bar']来访问
+ 像@click这样的一个v-on事件监听器将在此对象下被暴露为一个函数$attrs.onClick

当我们想要所有像class和v-on监听器这样的透传attribute都应用在内部的<button>上而不是外层的<div>上，我们可以通过设定inheritAttrs：false和使用v-bind="attrs"来实现

```javascript
<div class="btn-wrapper">
  <button class="btn" v-bind="$attrs">click me</button>
</div>
```

Tip：没有参数的v-bind会将一个对象的所有属性都作为attribute应用到目标元素上

### 在js中访问透传attributes
我们可以在<script setup>中使用useAttrs()API来访问一个组件的所有透传attribute

```javascript
<script setup>
import { useAttrs } from 'vue'
const attrs = useAttrs()
</script>
```

如果没有使用<script setup>，attrs会作为setup()上下文对象的一个属性暴露。

> 这里的attrs对象总是反应为最新的透传attribute，但它并不是响应式的，不能通过侦听器去监听它的变化，<font style="color:rgb(33, 53, 71);">如果你需要响应性，可以使用 prop，或者使用onUpdated()是的在每次更新时结合最新的attrs执行副作用</font>
>

```javascript
export default {
  setup(props, ctx) {
    // 透传 attribute 被暴露为 ctx.attrs
    console.log(ctx.attrs)
  }
}
```

## 插槽
### 作用域插槽
> [插槽 Slots | Vue.js](https://cn.vuejs.org/guide/components/slots.html#scoped-slots)
>

插槽的内容无法访问到子组件的状态，但是某些场景下插槽的内容可能想要同时使用父组件域内和子组件域内的数据，我们需要一种方法来让子组件在渲染时将一部分数据提供给插槽

```javascript
<!-- <MyComponent> 的模板 -->
<div>
  <slot :text="greetingMessage" :count="1"></slot>
</div>
  
<MyComponent v-slot="slotProps">
  {{ slotProps.text }} {{ slotProps.count }}
</MyComponent>
```



## 依赖注入
prop逐级透传，可能Footer并不关心该组件但是仍然需要向下传递，组件链路非常长，影响到的组件比较多。

![](https://cdn.nlark.com/yuque/0/2023/png/2366100/1693531812551-d92386e5-2134-4756-b027-b6646d36418d.png)

provide和inject可以帮助我们解决这一问题。一个父组件相对于其所有的后代组件会作为依赖提供者。任何后代的组件树，无论层级有多深，都可以注入有负组件提供给整条链路的依赖

### provide
要为组件后代提供数据，需要使用到provide(注入名，提供的值)函数，如果不使用<script setup>请确保provide()是在setup()同步调用的。

> 注入名：可以是一个字符串或是一个Symbol
>
> 提供的值：值可以是任何类型，包括响应式的状态，比如一个ref
>

#### 应用层provide
我们还可以在整个应用层面提供依赖，在应用级别提供的数据在该应用内的所有组件中都可以注入，这在编写插件时会特别有用。

### inject
注入上层组件提供的数据

#### 注入默认值
如果在注入一个值时不要求必须有提供者，那么我们应该声明一个默认值，和props类似

```javascript
// 如果没有祖先组件提供 "message"
// `value` 会是 "这是默认值"
const value = inject('message', '这是默认值')
```

某些情况下，默认值可能需要通过调用一个函数或初始化一个类来取得，为了避免在用不到默认值的情况下进行不必要的计算或产生副作用，我们可以使用工厂函数来创建默认值，第三个参数表示默认值应该被当做一个工厂函数

```javascript
const value = inject('key', () => new ExpensiveClass(), true)
```

## 组合式函数
组合式函数只能在<script setup>或setup()钩子中被调用，在这些上下文中，他们也只能被同步调用，在某些情况下，也可以在像onMounted()这样的生命周期钩子中调用

以上限制很重要，因为这些是vue用于确定当前活跃的组件实例的上下文，访问活跃的组件实例很有必要，这样才能：

+ 将生命周期钩子注册到该组件实例上
+ 将计算属性和监听器注册到该组件实例上，以便在该组件被卸载时停止监听，避免内存泄漏

### 和mixin的对比
mixins的缺点：

+ 不清晰的数据来源：<font style="color:rgb(33, 53, 71);">当使用了多个 mixin 时，实例上的数据属性来自哪个 mixin 变得不清晰，这使追溯实现和理解组件行为变得困难。这也是我们推荐在组合式函数中使用 ref + 解构模式的理由：让属性的来源在消费组件时一目了然</font>
+ <font style="color:rgb(33, 53, 71);">命名空间冲突：多个来自不同作者的 mixin 可能会注册相同的属性名，造成命名冲突。若使用组合式函数，你可以通过在解构变量时对变量进行重命名来避免相同的键名。</font>
+ <font style="color:rgb(33, 53, 71);">隐式的跨mixin交流：多个 mixin 需要依赖共享的属性名来进行相互作用，这使得它们隐性地耦合在一起。而一个组合式函数的返回值可以作为另一个组合式函数的参数被传入，像普通函数那样。</font>

> <font style="color:rgb(33, 53, 71);">我们不推荐在vue3中继续使用mixin，保留该功能只是为了项目迁移的需求和照顾熟悉它的用户。</font>
>

### <font style="color:rgb(33, 53, 71);">和无渲染组件的对比</font>
组合式函数相对于无渲染组件的主要优势是：组合式函数不会产生额外的组件实例开销，当在整个应用中使用时，由无渲染组件产生的额外组件实例会带来无法忽视的性能开销

**推荐在纯逻辑复用时使用组合式函数，在需要同时复用逻辑和视图布局时使用无渲染组件**

### 和react hooks的对比
组合式API的一部分灵感正来自于react hooks，vue的组合式函数在逻辑组合能力上与react hooks相似，但是vue的组合式函数是基于vue细粒度的响应式系统，这和react hooks的执行模型有本质的不同。

## 自定义指令
> 组件是主要的构建模块，组合是函数侧重于有状态的逻辑，自定义指令主要是为了重用设计普通元素的底层dom访问的逻辑
>

一个自定义指令由一个包含类似组件生命周期钩子的对象来定义

+ el：指令绑定到的元素们可以用于直接操作dom
+ binding：一个对象
    - value
    - oldValue：仅在beforeUpdate和updated中可用，无论值是否更改，他都可用
    - arg：传递给指令的参数
    - modifiers：一个包含修饰符的对象
    - instance：使用该指令的组件实例
    - dir：指令的定义对象
+ vnode：代表绑定元素的底层Vnode
+ prevNode：代表之前的渲染中指令所绑定元素的VNode，仅在beforeUpdate和updated钩子中可用

```javascript
const myDirective = {
  // 在绑定元素的 attribute 前
  // 或事件监听器应用前调用
  created(el, binding, vnode, prevVnode) {},
  // 在元素被插入到 DOM 前调用
  beforeMount(el, binding, vnode, prevVnode) {},
  // 在绑定元素的父组件
  // 及他自己的所有子节点都挂载完成后调用
  mounted(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件更新前调用
  beforeUpdate(el, binding, vnode, prevVnode) {},
  // 在绑定元素的父组件
  // 及他自己的所有子节点都更新后调用
  updated(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件卸载前调用
  beforeUnmount(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件卸载后调用
  unmounted(el, binding, vnode, prevVnode) {}
}
```

钩子函数会接收到指令所绑定元素作为其参数。在setup中，任何以v开头的驼峰式命名的变量都可以被用作一个自定义指令。

```javascript
<script setup>
// 在模板中启用 v-focus
const vFocus = {
  mounted: (el) => el.focus()
}
</script>
<template>
  <input v-focus />
</template>
```

在没有使用<script setup>的情况下，自定义指令需要通过directives选项注册

```javascript
export default {
  setup() {},
  directives: {
    // 在模板中启用 v-focus
    focus: {}
  }
}
```

## 插件
插件是一种能为Vue添加全局功能的工具代码，一个插件可以拥有install()方法的对象，也可以直接是一个安装函数本身。安装函数会接收到安装它的应用实例和传递给app.use()的额外选项作为参数

```javascript
import { createApp } from 'vue'
const app = createApp({})
app.use(myPlugin, {
  /* 可选的选项 */
})
const myPlugin = {
  install(app, options) {
    // 配置此应用
  }
}
```

### 常见场景
+ 通过app.component()和app.directive()注册一到多个全局组件或自定义指令
+ 通过app.provide()使一个资源可被注入进整个应用
+ 向app.config.globalProperties中添加一些全局实例属性或方法
+ 一个可能上述三种都包含了的功能库（例如vue-router）

## 内置组件
### Transition
1. 当一个<Transition>组件中的元素被插入或移除时，会发生下面这些事情
    1. Vue会自动检测目标元素是否应用了css过渡或动画，如果是，则一些CSS过渡class会在适当的时机被添加和移除
    2. 如果有作为监听器的js钩子，这些钩子函数会在适当的时机被调用
    3. 如果没有探测到css过渡或动画，也没有提供js钩子，那么dom的插入、删除操作将在浏览器的下一个动画帧后执行
2. 基于css的过渡效果
    1. v-enter-from：进入动画的起始状态，在元素插入之前添加，在元素插入完成后的下一帧移除
    2. v-enter-active：<font style="color:rgb(33, 53, 71);">进入动画的生效状态。应用于整个进入动画阶段。在元素被插入之前添加，在过渡或动画完成之后移除。这个 class 可以被用来定义进入动画的持续时间、延迟与速度曲线类型。</font>
    3. v-enter-to：<font style="color:rgb(33, 53, 71);">进入动画的结束状态。在元素插入完成后的下一帧被添加 (也就是</font>v-enter-from<font style="color:rgb(33, 53, 71);">被移除的同时)，在过渡或动画完成之后移除。</font>
    4. <font style="color:rgb(33, 53, 71);">v-leave-from：离开动画的起始状态。在离开过渡效果被触发时立即添加，在一帧后被移除。</font>
    5. <font style="color:rgb(33, 53, 71);">v-leave-active：离开动画的生效状态。应用于整个离开动画阶段。在离开过渡效果被触发时立即添加，在过渡或动画完成之后移除。这个 class 可以被用来定义离开动画的持续时间、延迟与速度曲线类型。</font>
    6. <font style="color:rgb(33, 53, 71);">v-leave-to：离开动画的结束状态。在一个离开动画被触发后的下一帧被添加 (也就是 </font>v-leave-from<font style="color:rgb(33, 53, 71);"> 被移除的同时)，在过渡或动画完成之后移除。</font>
3. <font style="color:rgb(33, 53, 71);">过渡效果的命名</font>
    1. <font style="color:rgb(33, 53, 71);">在<Transition name="name">组件上使用name属性对过渡组件进行命名</font>
    2. <font style="color:rgb(33, 53, 71);">对于有名字的过渡效果，起作用的过渡class会以其名字而不是v作为前缀，上面对应的六个状态会对应为name-enter-to等</font>
4. <font style="color:rgb(33, 53, 71);"> 原生css动画与css transition 的应用方式基本上是相同的，只有一点不同，那就是 </font>*-enter-from<font style="color:rgb(33, 53, 71);"> 不是在元素插入后立即移除，而是在一个 </font>animationend<font style="color:rgb(33, 53, 71);"> 事件触发时被移除。</font>

### Teleport
<Teleport>是一个内置组件，他可以将一个组件内部的一部分模板传送到该组件的dom结构外层的位置去

### Suspense
1. <font style="color:rgb(33, 53, 71);">用来在组件树中协调对异步依赖的处理。它让我们可以在组件树上层等待下层的多个嵌套异步依赖项解析完成，并可以在等待时渲染一个加载状态</font>
2. <font style="color:rgb(33, 53, 71);"><Suspense>可以等待的异步依赖有两种：</font>
    - <font style="color:rgb(33, 53, 71);">带有异步setup()钩子的组件，这也包含了使用<script setup>时有顶层await表达式的组件</font>
    - [异步组件](about:blank)
3. 异步组件：<font style="color:rgb(33, 53, 71);">这意味着如果组件关系链上有一个 </font><Suspense><font style="color:rgb(33, 53, 71);">，那么这个异步组件就会被当作这个 </font><Suspense><font style="color:rgb(33, 53, 71);"> 的一个异步依赖。在这种情况下，加载状态是由 </font><Suspense><font style="color:rgb(33, 53, 71);"> 控制，而该组件自己的加载、报错、延时和超时等选项都将被忽略。</font>
4. 异步组件也可以通过在选项中指定suspensible: false表明不用Suspense控制，并让组件始终自己控制其加载状态
5. 事件
    1. pending事件是在进入挂起状态时触发
    2. resolve是在default插槽完成获取新内容时触发
    3. fallback事件则是在fallback插槽的内容显示时触发

## 单文件组件（SFC）
使用SFC必须使用构建工具，其优点为：

+ 使用熟悉的html，css和js语法编写模块化的组件
+ 让本来就强相关的关注点自然内聚
+ 预编译模板，避免运行时的编译开销
+ 组件作用域的css
+ 在使用组合式API时语法更简单
+ 使用交叉分析模板和逻辑代码能进行更多编译时优化
+ 更好的IDE支持，提供自动补全和对模板中表达式的类型检查
+ 开箱即用的模块热更新支持

使用场景：

+ 单页面应用
+ 静态站点生成
+ 任何值得引入构建步骤以获得更好的开发体验的项目

# Vue Router
## 导航守卫
### 导航解析流程
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

### 全局前置守卫：beforeEach
**<font style="color:rgb(60, 60, 67);">当一个导航触发时，全局前置守卫按照创建顺序调用</font>**

两个参数：

+ to：即将要进入的目标
+ from：当前导航正要离开的路由
+ next：可选，放行，中断当前导航，执行新的导航

```javascript
const router = createRouter({ ... })
router.beforeEach((to, from) => {
  // ...
  // 返回 false 以取消导航
  return false
})
```

### 全局解析守卫：beforeResolve
**每次导航时都会触发，****<font style="color:rgb(60, 60, 67);">不同的是，解析守卫刚好会在导航被确认之前、所有组件内守卫和异步路由组件被解析之后调用</font>**

<font style="color:rgb(60, 60, 67);">获取数据或执行任何其他操作（如果用户无法进入页面时你希望避免执行的操作）的理想位置。</font>

#### <font style="color:rgb(60, 60, 67);">全局后置钩子：afterEach</font>
<font style="color:rgb(60, 60, 67);">这些钩子不会接受 </font>next<font style="color:rgb(60, 60, 67);"> 函数也不会改变导航本身</font>

<font style="color:rgb(60, 60, 67);">对分析、更改页面标题、声明页面很有用</font>

<font style="color:rgb(60, 60, 67);">反映了navigation faliures作为第三个参数</font>

### <font style="color:rgb(60, 60, 67);">路由独享守卫：beforeEnter</font>
只在进入路由时触发，不会在params，query或者hash改变时触发

### 组件内的守卫
#### beforeRouteEnter
在渲染该组件的对应路由被验证前调用，不能获取组件实例this！

#### beforeRouteUpdate
在当前路由改变，但是该组件被复用时调用，导航守卫可以访问组件实例this

#### beforeRouteLeave
在导航离开渲染该组件的对应路由时调用，导航守卫可以访问组件实例this

