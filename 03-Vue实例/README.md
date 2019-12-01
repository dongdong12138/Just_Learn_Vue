# 创建一个 Vue 实例
- 之前学 js 的时候，怎么创建一个实例对象呢？
    ```js
    var date = new Date()  // 创建了一个 Date 实例对象
    ```
- 现在创建一个 Vue 实例对象也是一样的：
    ```js
    var vm = new Vue()  // 创建了一个 Vue 实例对象
    ```
- 但是 Vue 构造函数里边呢，是可以传一个对象作为参数的：
    ```js
    var vm = new Vue({
        // 对象里面就可以定义一些选项啦，比如 el、data、mounted、methods 等
    })
    ```
- 写一个带有选项的 Vue 实例给你看
    ```js
    var vm = new Vue({
        // el 选项用来挂载到id为app的DOM元素，就像js里面的document.getElementById('app')
        // 也可以根据class来挂载元素，比如：el: '.app'，这就相当于js里面的 document.getElementsByClassName('app')[0]
        el: '#app',  
        data: {
            // data 选项用来定义数据
        },
        mounted: function () {
            // mounted 是一个生命周期函数，后面还会介绍到
        },
        methods: {
            // methods 里面可以写一些方法(函数)
        }
    })
    ```
- 文档中会经常使用 vm 这个变量名来表示一个 Vue 实例。
- 这里主要需要知道：**所有的 Vue 组件都是 Vue 实例，所有的 Vue 组件都是 Vue 实例，所有的 Vue 组件都是 Vue 实例。**

# 数据与方法
- 数据
    - data 对象中定义的数据是**响应式**的，也就是说，**只要 data 中的数据发生改变，页面上显示的东西也会随之改变。**比如：
        ```html
        <div id="app">
            <p>{{ a }}</p>
            <p>{{ b }}</p>
            <p>{{ c }}</p>
        </div>
        ```
        ```js
        var vm = new Vue({ 
            el: '#app',
            data: {
                a: 123,
                b: '我想说其实你很好',
                c: false
            }
        })
        ```
        现在打开浏览器的控制台，改变 data 中的数据：
        ```js
        // 这里有三行，一行一行地写进去哦
        vm.a = 789
        vm.b = '你自己却不知道'
        vm.c = true
        ```
        你会看到，页面上渲染的数据也跟着改变了。
    - **当 data 中的数据改变时，视图会进行重新渲染。**（相当于页面会刷新一遍）
    - 值得注意的是，只有当 Vue 实例被创建时，就已经存在于 data 中的数据，才是**响应式**的。也就是说，在实例被创建后，再给 data 添加新数据的话，这个数据就不是响应式的了。比如你在控制台输入下面一句：
        ```js
        vm.x = '我是瑶瑶'
        ```
        你会发现页面上不会发生变化。
    - 那怎么办呢？如果我就是想后期动态地新增一个数据，那岂不是做不到了？其实，你可以**先在 data 里面定义一个初始值，**哪怕这个初始值是个空字符串或者 null 都没有关系。
    - 比如上面，你想后期定义一个 x 属性，你可以先在 data 中定义一个初始值：
        ```html
        <div id="app">
            <p>{{ a }}</p>
            <p>{{ b }}</p>
            <p>{{ c }}</p>
            <p>{{ x }}</p>
        </div>
        ```
        ```js
        var vm = new Vue({ 
            el: '#app',
            data: {
                a: 123,
                b: '我想说其实你很好',
                c: false,
                x: ''
            }
        })
        ```
        然后再来改变它试试：
        ```js
        vm.x = '我是瑶瑶'
        ```
    - 我不希望你是响应式的，我希望，**你是我的！**哈哈~~
    - 看看 Vue 文档是如何为 data 定义初始值的：
        ```js
        data: {
            newTodoText: '',  // 初始值可以是空字符串
            visitCount: 0,    // 可以是 0
            hideCompletedTodos: false,  // 可以是 false
            todos: [],        // 可以是空数组
            error: null       // 可以是 null
        }
        ```
    - 这里有一个唯一的例外，就是使用了 **[Object.freeze()](https://wangdoc.com/javascript/stdlib/attributes.html#objectfreeze)**，这会阻止修改现有的属性，也意味着响应系统无法再追踪变化。
        ```js
        var obj = {
            foo: 'bar'
        }    

        Object.freeze(obj)  // freeze 有结冰、冻结的意思，这里就是把 obj 这个对象冻结了，它不能被改变了

        new Vue({
            el: '#app',
            data: obj
        })
        ```
        ```html
        <div id="app">
            <p>{{ foo }}</p>
            <button v-on:click="foo = 'baz'">Change it!</button>
        </div>
        ```
        如果你把 `Object.freeze(obj)` 注释掉，会发现数据又可以被改变了。

- 方法  
    - 除了上面说的数据属性，Vue 还给我们提供了一些有用的实例属性和实例方法。它们都带有 $ 前缀，以便与用户定义的属性区分开。比如：
        ```js
        var data = { a: 1 }
        var vm = new Vue({
            el: '#app',
            data: data
        })

        vm.$el === document.getElementById('app')  // true
        // 还记得上面说过，el 可以用来挂载元素，就相当于 js 的 document.getElementById() 吗？
        vm.$data === data  // true

        // $watch 是一个实例方法
        vm.$watch('a', function (newValue, oldValue) {
            // 这个回调函数将在 vm.a 改变后调用
            // 这个 watch 不明白没关系，后面还会说到的！
        })
        ``` 
    - 以后你可以在 [API 参考](https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B%E5%B1%9E%E6%80%A7) 中查阅到完整的实例属性和方法的列表。

# 实例生命周期钩子
- 一句话解释什么是 “生命周期钩子” ：不同的时期做不同的事！  
- 就像你的生命，也是在不同的时期做不同的事。比如，小学的时候，是个熊孩子；中学的时候，是个熊孩子；大学的时候，还是个熊孩子；遇见我的时候，变成小可爱；最后我们一起吃饭、逛街、旅游，一起生活，直到慢慢老去......
- Vue 实例也有它自己的生命，它也会在不同的时期做不同的事。比如创建的时候、挂载的时候、更新的时候、销毁的时候等等。同时，Vue 也给我们提供了一些函数，让我们可以控制它的生命，控制它在指定的时期做指定的事，这些函数，被称作 “钩子” 。
- 现在来看看 Vue 文档中是怎么说的吧：每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。
- 那 Vue 提供了哪些 “钩子函数” 呢？（要背的哦）
  - beforeCreate
  - created
  - beforeMount
  - mounted
  - beforeUpdate
  - updated
  - beforeDestroy
  - destroyed
- 这些钩子又有什么用呢？下面分成 4 组来简单介绍，一定要理解哦！
    ```js
    new Vue({
        
        1. 创建：比如做房子，要先把地基做好
        beforeCreate() {  // 创建前
            // 在 Vue 实例创建以前，可以做一些基础的工作，比如 loading
        },
        created() {  // 创建后
            // Vue 实例创建以后，data 已经有了，但是还没有 $el
            // 在这里，你可以请求一些数据，为 mounted 做准备
        },

        2. 挂载
        beforeMount() {  // 挂载前
            // vue 实例的 $el 和 data 都初始化了，但还是虚拟的 dom 节点
            // 也就是说，在这里，还没有办法去操作 DOM
        },
        mounted() {  // 挂载后
            // Vue 实例已经被挂载到页面上，这里就可以开始操作 DOM 了
        },

        3. 更新：一般来说，页面中的数据不会是死的，它是会发生变化的
        beforeUpdate() {  // 更新前
            // 在数据( data )更新之前做的一些操作，在这里可以访问现有的 DOM
            // 比如手动移除已添加的事件监听器
        },
        updated() {  // 更新后
            // 数据更新以后做的操作，比如把新数据重新渲染到 DOM 中
        },

        4. 销毁：就像我们需要打扫房间，如果不打扫，房间就太乱太杂了
        beforeDestroy() {  // 销毁前
            // 实例销毁之前调用，在这里，Vue 实例还可以正常使用
        },
        destroyed() {// 销毁后
            // 实例销毁之后调用，在这里，Vue 解除了事件监听，以及和 DOM 的绑定（无响应了），但是 DOM 节点依然存在
        }  
    })
    ```
- 有一点需要注意：不要在选项属性或回调上使用箭头函数，因为箭头函数没有 this ，那么 this 就会一级一级向上查找，直到找到为止，所以可能会导致一些错误。
    ```js
    var vm = new Vue({
        // 错误
        created: () => {
            console.log(this.a)  // this 不指向 vm 这个 Vue 实例
        },

        // 正确
        created: function () {   // ES5 写法
            console.log(this.a)  // this 指向 vm
        },
        created() {              // ES6 写法
            console.log(this.a)  // this 指向 vm
        }
    })
    ```
    ```js
    // 错误
    vm.$watch('a', newValue => this.myMethods())  // this 不指向 vm
    
    // 正确
    vm.$watch('a', function (newValue) {
        this.myMethods()  // this 指向 vm
    })
    ```

# 生命周期图示
下面这张图现在不用看懂，随便看看就行，以后慢慢就懂了！  
![生命周期图示](./lifecycle.png)