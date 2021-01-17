# html5



# css/css3

# js

### 1，作用域

函数执行时，会创建一个执行期上下文的内部对象，定义了一个函数执行时的环境

### 2，立即执行函数

### 3，闭包

A函数执行时保存并返回了B函数，并且B函数在A函数的外面执行形成了闭包，所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。闭包可以用在许多地方。它的最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。

由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。

### 4，原型，原型链

一个对象的原型，每一个通过该构造函数构造出来的对象都具有原型上的属性，

原型链的顶端是Object.protoptype，即绝大多数对象最终都会继承自Object.prototype,

而通过Object.cretea(原型)创造出来的对象的原型指向括号里的参数



### 5，call/apply

改变this指向

```
        function test(num){
            //默认this指向window
            console.log(this)
            console.log(num)
        }
        //当方法使用call调用时，可以改变this指向，this指向第一个参数
        test.call({},6)
        test.apply({},[8])
```

call和apply 的区别传参列表不同，第一个参数相同，后面的参数apply接受一个数组

### 6，this

### 7,深拷贝/浅拷贝

### 8，正则表达式

### 9，跨域

### 10,js数组去重

1.es6方法 

```
//Array.from()方法就是将一个类数组对象或者可遍历对象转换成一个真正的数组。
function unique (arr) {
  return Array.from(new Set(arr))
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
 //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {}, {}]
```

### 11， window.onload和document.ready的区别？哪一个先执行？

一般情况一个页面响应加载的顺序是，域名解析-加载html-加载js和css-加载图片等其他信息。
**window.onload**是在DOM文档树加载完和所有文件加载完之后执行一个函数，也就是在页面响应加载的顺序中的“加载图片等其他信息”之后，可以操作DOM。只能执行一次，如果有多个，那么第一次的执行会被覆盖
**document.ready**是在DOM加载完成后就可以可以对DOM进行操作，也就是在在“加载js和css”和“加载图片等其他信息”之间，就可以操作DOM了。可以执行多次
所以，document.ready函数只需对 DOM 树的等待，而无需对图像或外部资源加载的等待，从而执行起来更快

# vue

### 1，组件之间的通信方式

父子通信：父组件通过props向子组件传递属性，子组件通过$emit向父组件传递属性

兄弟通信：vuex，父子传递

隔代通信：父子通信，$attrs/$listeners

通过$ref可获取子组件实例，$parent/$chriden可获取父组件/子组件实例

### 2,v-model的原理

view层输入影响data的属性值，通过给dom节点添加oninput事件，更新data的属性值

```js
    // 假如node是遍历到的input节点
    node.addEventListener("input",function(e){
        vm.name=e.target.value;
    })  
```

data属性值发生变化时影响view，通过defineProperty监听每个属性的get,set方法，当某个属性重新赋值时，更新view

```
    Object.defineProperty(data,"name",{
        get(){
            return data["name"];
        },
        set(newVal){
            let val=data["name"];
            if (val===newVal){
                return;
            }
            data["name"]=newVal;
            // 监听到了属性值的变化,假如node是其对应的input节点
            node.value=newVal;
        }    
    })
```

### 3，vue.component和vue.extend

vue.component用于注册全局组件

vue.extend返回一个组件构造函数，可使用vue.component对其实例化

### 4，computed,watch,methods三者的区别

computed: 计算属性，A属性依赖于B属性时，或依赖于几个属性，B属性经过计算返回A属性，没有异步操作

watch:监听属性，监听某个属性，这个属性值发生改变时经行一系列的操作，可以有异步操作

methods:

### 5，template,render函数，jsx三者的使用

### 6,data和peops的区别

data中返回的属性可读可写

props一般用户父子组件通信上，props里的属性是只读的

### 7，keep alive有哪些作用

keep-alive是vue内置的一个组件，可以是组件保留状态，避免组件被重新渲染，例如：从列表页组件切换到详情页组件时，添加keep-alive使列表页组件缓存起来，防止页面进行无必要的初始化

```
<keep-alive>
  <component>
    <!-- 该组件将被缓存！ -->
  </component>
</keep-alive>
```

### 8，如何在vue项目中实现动画

使用vue内置组件transition

```
<!-- 将要使用动画的内容放在transition里即可 -->
<transition name="fade">
  <div v-show="show"></div>
</transition>


.fade-enter-active,
.fade-leave-active {
  transition: opacity .5s
}
.fade-enter,
.fade-leave-active {
  opacity: 0
}
```

使用animate.css插件

### 9，key属性有什么作用

用于 管理可复用的元素。因为Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。

### 10,vue-router有几种路由方式

1.hash模式

首先来了解一下Vue中的哈希路由的实现原理，其实根本原理涉及到了BOM中的location对象，其中对象中的location.hash储存的是路由的地址、可以赋值改变其URL的地址。而这会触发hashchange事件，而通过window.addEventListener监听hash值然后去匹配对应的路由、从而渲染页面的组件

2.history模式

原理是**history.pushState({}, ‘’, url)**

### 11，vuex相关内容

Vuex 是一个专为 Vue.js 应用程序开发的状态管理插件。它采用集中式存储管理应用的所有组件的状态，而更改状态的唯一方法是提交mutation，例`this.$store.commit('SET_VIDEO_PAUSE', video_pause`，`SET_VIDEO_PAUSE`为mutations属性中定义的方法 。

核心属性分别是 state、getters、mutations、actions、modules 。

### 12，vue.use的使用

注册全局对象的时候使用，传入一个function或者是object，必须要有install方法

### 13，axios相关内容

# react

# webpack

### 1.有哪些常见的Loader？他们是解决什么问题的？

- file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件
- url-loader：和 file-loader 类似，但是能在文件很小的情况下以 base64 的方式把文件内容注入到代码中去
- source-map-loader：加载额外的 Source Map 文件，以方便断点调试
- image-loader：加载并且压缩图片文件
- babel-loader：把 ES6 转换成 ES5
- css-loader：加载 CSS，支持模块化、压缩、文件导入等特性
- style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。
- eslint-loader：通过 ESLint 检查 JavaScript 代码

# 计算机网络

### 1，一个页面从输入 URL 到页面加载显示完成，这个过程中都发生了什么？

1.浏览器根据url交给DNS域名解析，找到真实的IP地址

DNS域名解析过程：

1）浏览器搜索**自己的 DNS 缓存**，缓存中维护一张域名与 IP 地址的对应表；

2）若没有，则搜索**操作系统的 DNS 缓存**；

3）若没有，则操作系统将域名发送至**本地域名服务器**（递归查询方式），本地域名服务器查询自己的 DNS 缓存，查找成功则返回结果，否则，通过以下方式迭代查找：

- 本地域名服务器向根域名服务器发起请求，根域名服务器返回 com 域的顶级域名服务器的地址；
- 本地域名服务器向 com 域的顶级域名服务器发起请求，返回权限域名服务器地址
- 本地域名服务器向权限域名服务器发起请求，得到 IP 地址

4）本地域名服务器将得到的 IP 地址返回给操作系统，同时自己将 IP 地址缓存起来

5）操作系统将 IP 地址返回给浏览器，同时自己也将 IP 地址缓存起来

至此，浏览器已经得到了域名对应的 IP 地址。

2.浏览器根据 IP 地址向服务器发起 TCP 连接，与浏览器建立 TCP 三次握手

3.浏览器发送HTTP请求
浏览器根据 URL 内容生成 HTTP 请求，请求中包含请求文件的位置、请求文件的方式等等；

4.服务器处理请求并返回HTTP报文（HTTP响应报文也是由三部分组成: 状态码, 响应报头和响应报文。）：
a…服务器接到请求后，会根据 HTTP 请求中的内容来决定如何获取相应的 HTML 文件；
b.服务器将得到的 HTML 文件发送给浏览器；
c.在浏览器还没有完全接收 HTML 文件时便开始渲染、显示网页；
d在执行 HTML 中代码时，根据需要，浏览器会继续请求图片、CSS、JavsScript等文件，过程同请求 HTML 。
5.断开连接

### 2，TCP和UDP的区别及特点

TCP/UDP是传输层的协议，运输层(transport layer)的主要任务就是负责向**两台主机进程之间的通信**提供通用的数据传输服务。

UDP 在传送数据之前不需要先建立连接**，远程主机在收到 UDP 报文后，不需要给出任何确认。虽然 UDP **不提供可靠交付**，但在某些情况下 UDP 确是一种最有效的工作方式（一般用于即时通信），比如： QQ 语音、 QQ 视频 、直播等等

TCP 提供**面向连接**的服务。在传送数据之前必须先建立连接，数据传送结束后要释放连接。

### 3，TCP三次握手四次挥手

进行三次握手的主要作用就是为了确认双方的接收能力和发送能力是否正常、指定自己的 **初始化序列号(Init Sequense Number, `ISN`)** 为后面的可靠性传输做准备。

[https://veal98.github.io/CS-Wiki/#/%E8%AE%A1%E7%AE%97%E6%9C%BA%E5%9F%BA%E7%A1%80/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/5-%E4%BC%A0%E8%BE%93%E5%B1%82](https://veal98.github.io/CS-Wiki/#/计算机基础/计算机网络/5-传输层)

# es6

# 微信小程序