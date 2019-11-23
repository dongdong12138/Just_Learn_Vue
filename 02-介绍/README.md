# Vue.js 是什么
> Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。（了解一下就好啦，不用背）

# 起步
- 安装方法
  ```html
  <!-- 开发环境版本，包含了有帮助的命令行警告 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  ```
  或者：
  ```html
  <!-- 生产环境版本，优化了尺寸和速度 -->
  <script src="https://cdn.jsdelivr.net/npm/vue"></script>
  ```

# 声明式渲染
1. 文本插值  
   下面是一个小 demo，**要跟着敲哦！**
    ```html
    <div id="app">
        {{ message }}  <!-- 这里的 message，就是下面定义的 message 啦 -->
                        <!-- 这里用的双大括号，叫做文本插值 -->
    </div>
    ```
    ```js
    var app = new Vue({
        el: '#app',
        data: {
            message: '你好呀，小可爱'
        }
    })
    ```
    - 那什么是声明式渲染呢？你看 js代码 里边是不是声明了一个 message 变量，然后这个变量被渲染到 html代码（也就是DOM） 里边啦。
    - 现在数据和 DOM 已经被建立了关联，所有东西都是响应式的。也就是说，只要 js 里边定义的 message 发生了变化，DOM 里边的 message 也会跟着发生变化啦。
    - 不相信吗？那你可以在浏览器打开控制台，并修改 app.message 的值，你就会看到页面会更新啦。比如你可以这样修改：
    ```js
    app.message = '我是董董'
    ```
    - 有没有发现这里是直接 app.message 呀？怎么不是 app.data.message 呢？中间隔了一个 data 难道不要吗？对啦，**data 里面的数据，可以直接通过 app 获取**哒，比如 app.message。这一点要记住哦！

2. 绑定元素特性( v-bind )  
   ```html
   <div id="app">
       <span v-bind:title="tip">鼠标悬停在这里就能看到提示信息啦</span>
   </div>
   ```
   ```js
   var app = new Vue({
       el: '#app',
       data: {
           tip: '天知道我有多喜欢你呀！'
       }
   })
   ```
   - 啊哦！上面的 v-bind 貌似没见过呢。不要着急，那叫做 **指令**。带有前缀 **v-** 的，就是 Vue 给我们提供的一些指令。那指令有什么用呢？**它可以在相应的 DOM 元素上，做一些指定的事情。**
   - 比如你写的上面的 v-bind，就像是在说：“本公主要在这个 span 元素上绑定一个 title 属性，并且这个 title 属性的值(tip)要和 data 里面定义的 tip 保持一致！”
   - 这样代码就会乖乖听你的话啦，**就像我也会乖乖听你的话。**
   - 如果你再次打开浏览器的控制台，修改 app.tip 的值，你就会看到页面上渲染的数据又变化了。比如你可能想这么修改：
    ```js
    app.tip = '哼，你才不听我的话呢！'
    ```

# 条件与循环
demo 又来了哦，要保持跟着敲代码的习惯哦！  

1. 条件( v-if )  
   这是一个控制元素显示或隐藏的 demo：
    ```html
    <div id="app">
        <p v-if="seen">你看到我啦！</p>
    </div>
    ```
    ```js
    var app = new Vue({
        el: '#app',
        data: {
            seen: true
        }
    })
    ```
    - 如果你在控制台输入 `app.seen = false` ，你会发现那个 p 元素隐藏啦。

2. 循环( v-for )  
   这是一个循环遍历数组的数据，并将它们渲染到 DOM 中的 demo：
   ```html
   <div id="app">
       <ol>
           <li v-for="fruit in fruits">{{ fruit }}</li>
       </ol>
   </div>
   ```
   ```js
   var app = new Vue({
       el: '#app',
       data: {
           fruits: ['苹果', '香蕉', '西瓜', '菠萝']
       }
   })
   ```
   - 如果你在控制台输入 `app.fruits.push('董董')`，你会发现我也变成水果被加进去啦。

# 处理用户输入
1. 绑定事件( v-on )  
    为了让用户和你的应用进行交互，我们可以用 v-on 指令添加一个事件监听器（也就是绑定事件啦），通过它调用在 Vue 实例中定义的方法：
    ```html
    <div id="app">
        <p>{{ message }}</p>
        <button v-on:click="reverseMessage">反转消息</button>
    </div>
    ```
    ```js
    var app = new Vue({
        el: '#app',
        data: {
            message: '你是耀眼的星河'
        },
        methods: {
            reverseMessage: function () {
                this.message = this.message.split('').reverse().join('')
            }
        }
    })
    ```
    现在点击按钮，看看效果吧。
2. 双向绑定( v-model )  
    Vue 还提供了 `v-model` 指令，它能轻松实现表单输入和应用状态之间的**双向绑定**。
    ```html
    <div id="app">
        <p>{{ message }}</p>
        <input v-model="message">
    </div>
    ```
    ```js
    var app = new Vue({
        el: '#app',
        data: {
            message: '很爱很爱你'
        }
    })
    ```
    - 那什么是双向绑定呢？就是，现在 data 里面的 message 被绑定到了 input 中，同时，input 中的值也绑定到了 data 中的 message。换句话说，只要 data 中的 message 改变，input 中的值就会改变；或者只要 input 中的值改变，data 中的 message 也会改变。
    - 你可以自己试试呀，如果还是不理解的话，就来问我哦！

# 组件化构建应用
- 先来解释一下这个标题：什么是组件化，什么是构建，什么是应用。
    - 先来玩一个游戏吧，用积木搭一个机器人。首先我们需要一个说明书，说明书上有机器人的样子，有教我们如何安装这个机器人。然后我们开始动手，把一块一块的积木，安装起来，最终装成说明书上的样子。
    - **什么是组件化**：上面的一块块的积木，就可以理解为组件啦。因为有了这些积木（组件），我们可以很快地就把机器人搭起来了。
    - **什么是构建**：我们用积木搭建机器人的这个过程，就可以理解为构建啦。
    - **什么是应用**：我们最终搭建完成的，和说明书上长得一样的机器人，就是应用啦。
    - 注意：实际开发中，也许有一些已经写好了的组件，你可以直接拿过来用；但是因为需求的不确定，不可能所有的组件都提前给你准备好，这时就需要你自己，根据设计稿，根据需求，做一个组件出来啦。最终你写了好几个组件，然后把这些组件安装起来，就变成了一个应用。
    - 是不是和用积木搭机器人有点像呀！
- 下面是 Vue 文档里的一张图，这张图把 “组件化构建应用” 更形象地表示了出来。
![组件化构建应用](./components.png)
这张图可以这么看：在这个页面中，由三个大组件（浅灰色部分）组合而成，其中第二个大组件包括了两个小组件，第三个大组件包括了三个小组件。正是这些组件的组合，最终构成了一个复杂的页面。
- 那我们要怎么做一个组件并使用它呢：
    - 注册组件
        ```js
        Vue.component('my-component', {  // 这样会注册一个全局的组件哦
            template: '<li>我是一个 li 元素</li>'
        })
        ```
    - 使用组件
        ```html
        <ol>
            <my-component></my-component>  <!-- 这就相当于是上面 template 里面的 li 元素啦 -->
        </ol>
        ```
    - Tip：你会不会觉得这样很麻烦呀？我明明只要在 ol 里直接写 li 就行了，为什么还要注册一个组件，然后在 ol 里写这么长一个组件的名字，这不是多此一举吗？小可爱，你要知道，这只是一个 demo，template 里面只写了一点点东西，实际开发中，template 里边可能会写很多很多东西哦，这样一来，你在 ol 里面写的 `<my-component></my-component>` 就代表了很多很多东西，并且你在其他地方想使用这些东西的话，也可以直接写个 `<my-component></my-component>` 就行啦。
- 但是像上面这个注册组件有个问题，模板( template )里面的内容都是固定的，我在其他地方使用这个组件时，只会渲染出同样的东西，如果我希望它渲染不同的东西，怎么办呢？那就要用到 **props** 啦！**（props 可以让组件接收外面传进来的东西）**
    - html 里边使用组件：
        ```html
        <div id="app">
            <ol>
                <my-component 
                    v-for="item in fruits"
                    v-bind:fruit="item"
                ></my-component>
            </ol>
        </div>
        ```
    - js 里边注册组件和定义数据：
        ```js
        // 要首先注册组件哦
        Vue.component('my-component', {
            props: ['fruit'],
            template: '<li>{{ fruit }}</li>'
        })
        
        // 然后定义数据
        var app = new Vue({
            el: '#app',
            data: {
                fruits: ['苹果', '香蕉', '草莓', '西瓜']
            }
        })
        ```
    上面这段代码你现在理解起来可能有些困难，但是没关系哒，不用管它，后面还会说到的！或者你可以把问题记下来，我回到家就可以教你啦，但是代价是你要亲我一分钟哦，哈哈！
- 回过头来想一想，上面都是只有一个地方用到了一个组件，可实际开发中肯定会用到很多组件呀，那会是什么样子呢？Vue 文档给我们贴心的写了一段代码，可以让我们看到使用多个组件的样子。
    ```html
    <div id="app">
        <app-nav></app-nav>    <!-- 一个导航栏组件 -->
        <app-view>    <!-- 一个不知道是什么的组件，大概是个包裹的容器吧 -->
            <app-sidebar></app-sidebar>    <!-- 一个侧边栏组件 -->
            <app-content></app-content>    <!-- 一个内容区域的组件 -->
        </app-view>
    </div>
    ```
    看，这里一下子用了 4 个组件，但实际开发中，可能会用到更多组件。

### 如果上面有大部分看不懂的话，没有关系，这章本来就是介绍Vue的一些核心功能呢！下面我们真正开始学习啦！
   
   