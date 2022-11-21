---
title: Vue基础
date: 2021-7-20
categories: VUE 
---

## 一、Vue和React（基于自我可以理解的程度）

### 区别

1.在 React 应用中，当某个组件的状态发生变化时，它会以该组件为根，重新渲染整个组件子树。在 Vue 应用中，组件的依赖是在渲染过程中自动追踪的，所以系统能精确知晓哪个组件确实需要被重渲染。

2.在 React 中，一切都是 JavaScript。不仅仅是 HTML 可以用 JSX 来表达，现在的潮流也越来越多地将 CSS 也纳入到 JavaScript 中来处理。Vue 的整体思想是拥抱经典的 Web 技术，整体上还是html，js，css三部分

3.在 React 中，所有的组件的渲染功能都依靠 JSX渲染函数。JSX 是使用 XML 语法编写 JavaScript 的一种语法糖。事实上 Vue 也提供了[渲染函数](https://cn.vuejs.org/v2/guide/render-function.html)，甚至[支持 JSX](https://cn.vuejs.org/v2/guide/render-function.html#JSX)。然而，我们默认推荐的还是模板。

4.更抽象一点来看，我们可以把组件区分为两类：一类是偏视图表现的 (presentational)，一类则是偏逻辑的 (logical)。我们推荐在前者中使用模板，在后者中使用 JSX 或渲染函数。

5.Vue 提供了 [CLI 脚手架](https://github.com/vuejs/vue-cli)，能让你通过交互式的脚手架引导非常容易地构建项目。React 在这方面也提供了 [create-react-app](https://github.com/facebookincubator/create-react-app)，但是现在还存在一些局限性：它不允许在项目生成时进行任何配置；它只提供一个构建单页面应用的默认选项，而 Vue 提供了各种用途的模板

6.React Native 能使你用相同的组件模型编写有本地渲染能力的 APP (iOS 和 Android)，能同时跨多平台开发。Vue 和 [Weex](https://weex.apache.org/) 会进行官方合作，Weex 是阿里巴巴发起的跨平台用户界面开发框架，Weex 允许你使用 Vue 语法开发不仅仅可以运行在浏览器端，还能被用于开发 iOS 和 Android 上的原生应用的组件。在现在，Weex 还在积极发展，成熟度也不能和 React Native 相抗衡。

### 相同之处：

1.使用Virtual DOM

2.将注意力集中保持在核心库，而将其他功能如路由和全局状态管理交给相关的库。

3.都是单向数据流

## 二、VUE

### 1.双向数据绑定和单向数据流

这两个是不同的概念，双向绑定是model改变view自动更新，view改变model自动更新；而单向数据流指的父组件传值给子组件，子组件不能修改这个值，而父组件修改这个值的话子组件也会受影响，这个影响是单向的，只能从父组件流向子组件，不能反向。

#### v-model

表单的v-model，用户对View层的更改会直接同步到Model层。实际上v-model只是v-bind:value 和 v-on:input的语法糖，我们也可以采取类似react的单向绑定。

### 2.计算属性

如果模板不在是简单声明，而是复杂的逻辑，就需要计算属性computed，例如：

```vue
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```

Vue 知道 `vm.reversedMessage` 依赖于 `vm.message`，因此当 `vm.message` 发生改变时，所有依赖 `vm.reversedMessage` 的绑定也会更新。

#### 2.1计算属性和方法

我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是**计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 `message` 还没有发生改变，多次访问 `reversedMessage` 计算属性会立即返回之前的计算结果，而不必再次执行函数。

#### 2.2计算属性和侦听属性

当你有一些数据需要随着其它数据变动而变动时，通常更好的做法是使用计算属性而不是命令式的 `watch`

#### 2.3计算属性的setter

计算属性默认只有 getter，不过在需要时你也可以提供一个 setter

```vue
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```

#### 2.4侦听器

虽然计算属性在大多数情况下更合适，但当需要在数据变化时执行异步或开销较大的操作时还需要侦听器。

### 3.v-if和v-show

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。

### 4.v-if和v-for

当 `v-if` 与 `v-for` 一起使用时，`v-for` 具有比 `v-if` 更高的优先级。VUE3中vi-if优先级高于v-for，使用时可能会报错

### 5.全家桶

Vue+Vue-router+Vuex+axios

### 6.route和router的区别

#### $route对象

 $route对象表示当前的路由信息，包含了当前 URL 解析得到的信息。包含当前的路径，参数，query对象等。

 **1.**  **$route.path**   字符串，对应当前路由的路径，总是解析为绝对路径，如"/foo/bar"。

  **2.**  **$route.params**   一个 key/value 对象，包含了 动态片段 和 全匹配片段，如果没有路由参数，就是一个空对象。name

  **3.**  **$route.query**   一个 key/value 对象，表示 URL 查询参数。   例如，对于路径 /foo?user=1，则有$route.query.user == 1，   如果没有查询参数，则是个空对象。path

  **4.**  **$route.hash**   当前路由的hash值 (不带#) ，如果没有 hash 值，则为空字符串。锚点*

  **5.**  **$route.fullPath**   完成解析后的 URL，包含查询参数和hash的完整路径。

  **6.**  **$route.matched**   数组，包含当前匹配的路径中所包含的所有片段所对应的配置参数对象。

  **7.**  **$route.name**  当前路径名字

  **8.**  **$route.meta** 路由元信息

#### **$router对象**

$router对象是全局路由的实例，是router构造方法的实例。

##### **1、push**

  (1)字符串this.$router.push('home')

  (2)对象this.$router.push({path:'home'})

  (3)命名的路由this.$router.push({name:'user',params:{userId:123}})

  (4)带查询参数，变成 /register?plan=123this.$router.push({path:'register',query:{plan:'123'}})

​       push方法其实和<router-link :to="...">是等同的。

  注意：push方法的跳转会向 history 栈添加一个新的记录，当我们点击浏览器的返回按钮时可以看到之前的页面。

##### **2、go**

   页面路由跳转，前进或者后退this.$router.go(-1) // 后退

##### 3、replace

  push方法会向 history 栈添加一个新的记录，而replace方法是替换当前的页面，

  不会向 history 栈添加一个新的记录

### 7.vue.js的两个核心是什么

1.数据驱动，也叫双向数据绑定。

2.组件系统，vue组件的核心选项:

- 1、模板（template）：模板声明了数据和最终展现给用户的DOM之间的映射关系。

- 2、初始数据（data）：一个组件的初始数据状态。对于可复用的组件来说，这通常是私有的状态。
- 3、接受的外部参数(props)：组件之间通过参数来进行数据的传递和共享。
- 4、方法（methods）：对数据的改动操作一般都在组件的方法内进行。
- 5、生命周期钩子函数（lifecycle hooks）：一个组件会触发多个生命周期钩子函数，最新2.0版本对于生命周期函数名称改动很大。

- 6、私有资源（assets）：Vue.js当中将用户自定义的指令、过滤器、组件等统称为资源。一个组件可以声明自己的私有资源。私有资源只有该组件和它的子组件可以调用。

### 8.单页面和多页面

**单页面应用**（SPA)

一个项目中只有一个完整的html页面，其他的都是部分的html片段组成。页面跳转只是局部刷新，不会重新加载全部资源。片段之间的切换快，比较容易实现转场动画。

eg：Vue，React

优点：

1.没有页面之间的切换，就不会出现“白屏现象”，也不会出现假死并有“闪烁”现象，页面显示流畅，web应用更具响应性和更令人着迷。

2.良好的前后端工作分离模式：后端不再负责模板渲染、输出页面工作，后端API通用化，即同一套后端程序代码，不用修改就可以用于Web界面、手机、平板等多种客户端。

3.减轻服务器压力：单页应用相对服务器压力小，服务器只用出数据就可以，不用管展示逻辑和页面合成，吞吐能力会提高几倍。

缺点：

1.首屏加载慢：如果不对路由进行处理，在加载首页的时候，就会将所有组件全部加载，并向服务器请求数据，这必将拖慢加载速度；

解决方案：

​	1.Vue-router懒加载：只有当路由被访问时才会加载对应的组件，而不是在加载首页的时候就加载，

​	2.使用CDN加速

2.不利于seo

3.导航不可用，如果一定要导航需要自行实现前进、后退。（由于是单页面不能用浏览器的前进后退功能，所以需要自己建立堆栈管理）

**多页面应用**（MPA)

一个项目是由多个完整的html页面组成，页面跳转所有的资源都要重新加载，页面之间的切换会出现卡顿空白的问题，不容易实现切换动画等

eg：Vue配置打包多入口文件

{% asset_img 1687094-20190513234650178-610052132.png 这是一张图片 %}

### 9.SSR（服务端渲染）

#### 客户端渲染：

在服务端放了一个html 页面，里面有客户端发起请求，服务端把页面（响应的是字符串）发送过去，客户端从上到下依次解析，如果在解析的过程中，发现ajax请求，再次像服务器发送新的请求，客户端拿到ajax 响应结果，模板引擎渲染。过程至少和服务端发起两次请求

#### SSR：服务端渲染

sever 端页面+数据，服务端过程：1.读取index.html 2.模版进行渲染，在发送给客户端之前，在服务端已经把index.html 渲染处理了。var 渲染结果 = tempeter.render(模板字符串，｛解析替换对象｝) response.end(渲染结果)，服务端响应的就是最总的结果

服务端只请求一次

多数网站既有服务端渲染又有客户端渲染

服务端渲染和客户端渲染的区别

- 客户端渲染不利于 SEO 搜索引擎优化
- 服务端渲染是可以被爬虫抓取到的，客户端异步渲染是很难被爬虫抓取到的
- 所以你会发现真正的网站既不是纯异步也不是纯服务端渲染出来的，而是两者结合来做的
- 例如京东的商品列表就采用的是服务端渲染，目的了为了 SEO 搜索引擎优化
- 而它的商品评论列表为了用户体验，而且也不需要 SEO 优化，所以采用是客户端渲染

### 10.Vue **不能检测**数组和对象的变化

#### 数组

vue不能检测数组中的以下变动

1.当你利用索引直接设置一个数组项时，例如：`vm.items[indexOfItem] = newValue`

2.当你修改数组的长度时，例如：`vm.items.length = newLength`

```
data: {
    items: ['a', 'b', 'c']
}
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
```

解决第一类问题可以使用

```
1.Vue.set(vm.items, indexOfItem, newValue)
2.vm.items.splice(indexOfItem, 1, newValue)
3.vm.$set(vm.items, indexOfItem, newValue)
```

解决第二类问题

```
vm.items.splice(newLength)
```

#### 对象

Vue 无法检测 property 的添加或移除。由于 Vue 会在初始化实例时对 property 执行 getter/setter 转化，所以 property 必须在 `data` 对象上存在才能让 Vue 将它转换为响应式的。

对于已经创建的实例，Vue 不允许动态添加根级别的响应式 property。但是，可以使用 `Vue.set(object, propertyName, value)` 方法向嵌套对象添加响应式 property。例如，对于：

```
Vue.set(vm.someObject, 'b', 2)
```

您还可以使用 `vm.$set` 实例方法，这也是全局 `Vue.set` 方法的别名：

```
this.$set(this.someObject,'b',2)
```

有时你可能需要为已有对象赋值多个新 property，比如使用 `Object.assign()` 或 `_.extend()`。但是，这样添加到对象上的新 property 不会触发更新。在这种情况下，你应该用原对象与要混合进去的对象的 property 一起创建一个新的对象。

```
// 代替 `Object.assign(this.someObject, { a: 1, b: 2 })`
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
```

### 11.Vue深度监听对象

```js
watch:{
  obj:{ //监听的对象
    deep:true, //深度监听设置为 true
    handler:function(newV,oldV){
      console.log('watch中：',newV)
    }
  }
}
```

### 12.父组件改变子组件的样式

父组件

```html
<template>
    <div class="cont">
        <footEr></footEr>
    </div>
</template>
<script>
    import footEr from '../../components/foot.vue'
        components: {footEr}
</script>    
<style scoped>
    .cont >>> .index {　　　　/*cont是父组件包裹子组件的类名，index是子组件中要修改样式的类名*/
        color: #2a82e4;
    }
</style>
```

```html
//如果是有些像Sass、less之类的预处理器无法正确解析 >>> 。这种情况下你可以使用 /deep/ 操作符取而代之——这是一个 >>> 的别名
<style scoped lang="scss">
.cont{
  /deep/ .index{
    color: #2a82e4;
  }
}
</style>
```

子组件

```html
<template>
    <div>
        <!-- 底部 -->
        <div class="footer">
            <div class="index">
                <p>菜单1</p>
            </div>
            <div>
                <p>菜单2</p>
            </div>
        </div>
    </div>
</template>
```

### 13.Vue.use()

安装 Vue.js 插件。如果插件是一个对象，必须提供 `install` 方法。如果插件是一个函数，它会被作为 install 方法。install 方法调用时，会将 Vue 作为参数传入。

### 14.VUE的优化

1.v-if和v-show的区分使用，频繁切换使用v-show。

2.computed和watch的区分使用。computed是依赖值发生变化后才会重新计算，watch可以实现数据变化时执行异步操作。

3.v-for遍历必须添加key值，设置唯一 key 值，方便 Vue.js 内部机制精准找到该条列表数据。当 state 更新时，新的状态值和旧的状态值对比，较快地定位到 diff 。

v-for 比 v-if 优先级高，如果每一次都需要遍历整个数组，将会影响速度，尤其是当之需要渲染很小一部分的时候，必要情况下应该替换成 computed 属性。

```vue
//推荐：
<ul>
  <li
    v-for="user in activeUsers"
    :key="user.id">
    {{ user.name }}
  </li>
</ul>
computed: {
  activeUsers: function () {
    return this.users.filter(function (user) {
     return user.isActive
    })
  }
}
//不推荐：
<ul>
  <li
    v-for="user in users"
    v-if="user.isActive"
    :key="user.id">
    {{ user.name }}
  </li>
</ul>
```

4.利用冻结数据

Vue 会通过 Object.defineProperty 对数据进行劫持，来实现视图响应数据的变化，然而有些时候我们的组件就是纯粹的数据展示，不会有任何改变，我们就不需要 Vue 来劫持我们的数据，在大量数据展示的情况下，这能够很明显的减少组件初始化的时间

```js
export default {
  data: () => ({
    users: {}
  }),
  async created() {
    const users = await axios.get("/api/users");
    this.users = Object.freeze(users);
  }
};
```

5.事件销毁：js监听事件的销毁，定时器的销毁

```js
beforeDestroy() {
  removeEventListener('click', this.click, false)
}
```

6.图片资源懒加载

将页面内未出现在可视区域内的图片先不做加载， 等到滚动到可视区域后再去加载，们在项目中使用 Vue 的 vue-lazyload 插件

```html
1.npm install vue-lazyload --save-dev
2.在main.js中全局引入
import VueLazyLoad from 'vue-lazyload'
Vue.use(VueLazyLoad, {
  preLoad: 1,
  error: require('./assets/img/error.jpg'),
  loading: require('./assets/img/homePage_top.jpg'),
  attempt: 2,
 })
3.lazyloadDemo.vue（页面中）使用：
<img v-lazy="图片地址" :key="图片地址" alt="" style="width: 768px;"> //img标签中使用懒加载：v-lazy 代替 v-bind:src
//:key="" 必须要加，否则就会出现，页面刷新，其他内容都刷新了，但是只有图片不刷新的情况。因为key可能相同，所以页面不会更新！
<li v-for="(item,index) in imgList" v-lazy:background-image="item"></li> //背景图片
```

7.路由懒加载

```
1.非懒加载
import Login from '@/components/login'
{
    path: "/login",
    name: "Login",
    component: Login,
},
2.懒加载
{
    path: "/index", // 首页
    name: "Index",
    component: (resolve) => require(["../views/pages/index"], resolve),
    meta: {
      title: "首页"
    }
},
或者（按组件分块）
const Foo = () => import('./Foo.vue')
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})
```

8.使用keep-alive，用来对组件进行缓存，从而节省性能。

### 15.beforeRouteEnter

不能获取组件实例this，因为当前守卫执行前，组件实例还未创建，next(vm => {}）进行回调访问，在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。

```js
beforeRouteEnter(to, from, next) {
    if (from.name === 'weightDetail') {
      next()
    } else {
      next(vm => {
        vm.shuka()
      })
    }
  },
```

### 16.nextTrick()

在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

```
1.DOM 更新循环是指什么？
2.下次更新循环是什么时候？
3.修改数据之后使用，是加快了数据更新进度吗？
4.在什么情况下要用到？
```

#### 异步

Vue 实现响应式并**不是数据发生变化之后 DOM 立即变化**，而是按一定的策略进行 DOM 的更新。

```
（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
（4）主线程不断重复上面的第三步。
```

#### 事件循环

简单来说，Vue 在修改数据后，视图不会立刻更新，而是等**同一事件循环**中的所有数据变化完成之后，再统一进行视图更新。

```
//改变数据
vm.message = 'changed'

//想要立即使用更新后的DOM。这样不行，因为设置message后DOM还没有更新
console.log(vm.$el.textContent) // 并不会得到'changed'

//这样可以，nextTick里面的代码会在DOM更新后执行
Vue.nextTick(function(){
    console.log(vm.$el.textContent) //可以得到'changed'
```

{% asset_img bV17xC-1626001240832 这是一张图片 %}

第一个 tick（图例中第一个步骤，即'本次更新循环'）：

1. 首先修改数据，这是同步任务。同一事件循环的所有的同步任务都在主线程上执行，形成一个执行栈，此时还未涉及 DOM 。
2. Vue 开启一个异步队列，并缓冲在此事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会被推入到队列中一次。

第二个 tick（图例中第二个步骤，即'下次更新循环'）：

同步任务执行完毕，开始执行异步 watcher 队列的任务，更新 DOM 。Vue 在内部尝试对异步队列使用原生的 Promise.then 和 MessageChannel 方法，如果执行环境不支持，会采用 setTimeout(fn, 0) 代替。

第三个 tick（图例中第三个步骤）：

此时就是文档所说的

> 下次 DOM 更新循环结束之后

此时通过 Vue.nextTick 获取到改变后的 DOM 。通过 setTimeout(fn, 0) 也可以同样获取到。

------

简单总结事件循环：

同步代码执行 -> 查找异步队列，推入执行栈，执行Vue.nextTick[事件循环1] ->查找异步队列，推入执行栈，执行Vue.nextTick[事件循环2]...

总之，异步是单独的一个tick，不会和同步在一个 tick 里发生，也是 DOM 不会马上改变的原因。

#### 用途

> 应用场景：需要在视图更新之后，基于新的视图进行操作。

#### created、mounted

需要注意的是，在 created 和 mounted 阶段，如果需要操作渲染后的试图，也要使用 nextTick 方法。

官方文档说明：

> 注意 mounted 不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用 vm.$nextTick 替换掉 mounted

```
mounted: function () {
  this.$nextTick(function () {
    // Code that will run only after the
    // entire view has been rendered
  })
}
```

#### 其他应用场景

其他应用场景如下三例：

例子1：

点击按钮显示原本以 v-show = false 隐藏起来的输入框，并获取焦点。

```
showsou(){
  this.showit = true //修改 v-show
  document.getElementById("keywords").focus()  //在第一个 tick 里，获取不到输入框，自然也获取不到焦点
}
```

修改为：

```
showsou(){
  this.showit = true
  this.$nextTick(function () {
    // DOM 更新了
    document.getElementById("keywords").focus()
  })
}
```

### 17.Vue父子组件传值

1.props/$emit/ref/$parent/$child

2.provide/inject

vue组件的深层次传值，允许一个祖先组件向其所有[子孙后代](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.baidu.com%2Fs%3Fwd%3D%E5%AD%90%E5%AD%99%E5%90%8E%E4%BB%A3%26tn%3D24004469_oem_dg%26rsv_dl%3Dgh_pl_sl_csd)注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效
 *provide 选项允许我们指定我们想要提供给后代组件的数据/方法*
 provide 选项应该是一个对象或返回一个对象的函数

```jsx
provide: function () {
  return {
    getMap: this.getMap
  }
}
// 或者 
provide: {
    foo: 'bar'
}
```

**然后在任何后代组件里，我们都可以使用 inject 选项来接收指定的我们想要添加在这个实例上的属性：**
 *inject 选项应该是：*
 一个字符串数组，或一个对象，对象的 key 是本地的绑定名，value 是：
 在可用的注入内容中搜索用的 key (字符串或 Symbol)，或
 一个对象，该对象的：
 from 属性是在可用的注入内容中搜索用的 key (字符串或 Symbol)
 default 属性是降级情况下使用的 value:

```bash
inject: ['getMap']
```

3.eventBus事件总线

`EventBus` 又称为事件总线。在Vue中可以使用 `EventBus` 来作为沟通桥梁的概念，就像是所有组件共用相同的事件中心，可以向该中心注册发送事件或接收事件，所以组件都可以上下平行地通知其他组件，但也就是太方便所以若使用不慎，就会造成难以维护的“灾难”，因此才需要更完善的Vuex作为状态管理中心，将通知的概念上升到共享状态层次。

自己写或者npm包

https://zhuanlan.zhihu.com/p/72777951

### 18.vue修饰符

1..sync译为同步

使用前提：

首先，我们需要明确的是，子父组件之间通讯，子组件是不能直接改变父组件的值的。（父组件是大佬，小弟不能改变大佬的值，但是父组件可以改变子组件的值）

作用：

通过某种方式，子组件可以”直接“改变父组件的值。

方法：

场景：控制弹框的显示与关闭。在父组件中打开子组件弹框，然后在点击子组件中的按钮关闭弹框。

事件修饰符：

1.  .stop阻止单击事件继续传播
2.  .prevent阻止默认事件
3.  .capture事件优先处理，先在此处处理
4.  .once事件只执行一次 
5.  .self自身触发事件

按键修饰符：

.enter、.tab、.space、.up、.down、.left、.right

### 19.vue-router的钩子函数

路由钩子函数有三种：

​      1：全局钩子： beforeEach、 afterEach、beforeResolve

​      2：单个路由里面的钩子：  beforeEnter（写在路由配置中）

​      3：组件路由：beforeRouteEnter、 beforeRouteUpdate、 beforeRouteLeave

### 20.created和watch谁先执行

如果watch 加了 immediate: true, 就是watch先执行，否则就是created 先执行

### 21.v-for为何要绑定唯一的key值

使用v-for更新已渲染的元素列表时,默认用就地复用策略;列表数据修改的时候,他会根据key值去判断某个值是否修改,如果修改,则重新渲染这一项,否则复用之前的元素。

key值一般不建议使用索引值index，因为如果在中插入了某一项的话，后边的数据值就不能复用，需要重新渲染，一般会选择唯一去确定某一项的id值。

### 22.webpack集成脚手架以及常用loader，是什么样的一个东西

### 23.px2rem基于什么的插件

### 24.wacth用在哪种场景，可以监听哪里的，除了data

watch试用于一个数据影响多个数据，而computed是一个数据受多个数据的影响。

父子组件传值，子组件监听父组件值的变化

vue中的watch监听，除了监听data中的数据 。还可以监听 props 、$route 、$emit 、computed

## 25.vue自定义指令展示模态框

https://www.jb51.net/article/145514.htm

## 26.路由传参

```
//路由配置文件中 配置动态路由
{
     path: '/detail/:id',
     name: 'Detail',
     component: Detail
   }
//动态路由一般传一个参数居多
var id = 1;
this.$router.push('/detail/' + id)
```

```
//页面刷新参数不会丢失，路由路径，传一个或多个
this.$router.push({
  path: '/detail',
  query: {
    name: '张三'，
    id: 1,
  }
})
```

```
//页面刷新，参数丢失，路由名称，传一个或者多个
this.$router.push({
  name: 'Detail',
  params: {
    name: '张三'，
    id: 1,
  }
})
```

