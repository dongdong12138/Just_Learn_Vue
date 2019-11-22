# Vue.js 是什么
- Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。（了解一下就好啦，不用背）

# 起步
- 安装方法
  ```js
    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  ```
  或者：
  ```js
    <!-- 生产环境版本，优化了尺寸和速度 -->
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
  ```

# 声明式渲染
1. 文本插值  
   下面是一个小 demo，要跟着敲哦！
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
    - 有没有发现这里是直接 app.message 呀？怎么不是 app.data.message 呢？中间隔了一个 data 难道不要吗？对啦，data 里面的数据，可以直接通过 app 获取哒，比如 app.message。但是如果不是 data 里面的东西，就不可以这样获取了哦，后面会说到的。这一点要记住哦！

2. 绑定元素特性  
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
   - 啊哦！上面的 v-bind 貌似没见过呢。不要着急，那叫做 指令。带有前缀 v- 的，就是 Vue 给我们提供的一些指令。那指令有什么用呢？它可以在相应的 DOM 元素上，做一些指定的事情。
   - 比如你写的上面的 v-bind，就像是在说：“本公主要在这个 span 元素上绑定一个 title 属性，并且这个 title 属性的值(tip)要和 data 里面定义的 tip 保持一致！”
   - 这样代码就会乖乖听你的话啦，就像我也会乖乖听你的话。
   - 如果你再次打开浏览器的控制台，修改 app.tip 的值，你就会看到页面上渲染的数据又变化了。比如你可能想这么修改：
    ```js
      app.tip = '你才不听我的话呢！'
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
   
   