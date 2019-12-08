# 插值

## # 文本
- 数据绑定最常见的形式就是使用**文本插值**，即 {{ }}，如：
    ```html
    <p>Message: {{ msg }}</p>
    ```
    ```js
    new Vue({
        data: {
            msg: '我不要阳光，我只要你'
        }
    })
    ```
    - 上面代码中，p 标签中的 msg ，就是 data 中定义的 msg。
    - 如果 data 中的 msg 改变，那么 p 标签中的 msg 也会跟着改变。
- 你也可以使用 **v-once** 指令，让 data 中的某个数据**只渲染一次**，以后 data 中这个数据发生变化的时候，页面中对应的数据不会发生变化，如：
    ```html
    <p v-once>Message: {{ msg }}</p>
    ```
    ```js
    var vm = new Vue({
        data: {
            msg: '我不要阳光，我只要你'
        }
    })
    ```
    - 上面代码中，因为 p 标签上写了 v-once 指令，所以它只会被渲染一次，如果你在控制台修改 data 中 msg 的值，会发现页面中不会发生变化，如：
        ```js
        vm.msg = '我要阳光，不要你'
        ```
    - **小傻瓜，我怎么会舍得呢 ^_^**
    - 但是要注意哦，v-once 会导致该节点上绑定的其他数据也不会发生改变，因为它们都只会渲染一次，以后不会再被渲染了。

## # 原始 HTML
- 上面的 {{ }} 会将数据解释成普通文本，但如果我们是想输出一个 HTML 标签，我们就可以使用 **v-html** 指令，如：
    ```html
    <p>我是文本：{{ rawHtml }}</p>
    <p>我是标签：<span v-html="rawHtml"></span></p>
    ```
    ```js
    new Vue({
        data: {
            rawHtml: '<span style="color: red;">细水长流</span>'
        }
    })
    ```
    - 上面代码中，第一个 p 标签中，会以文本的形式输出 span 标签；第二个 p 标签中，则会将 span 标签正常渲染出来。
- 注意，动态渲染 HTML 是非常危险的，因为它很容易导致 **XSS** 攻击。请只对可信内容使用 HTML 插值，绝不要对用户提供的内容使用 v-html 。因为用户可能输入一段 script 脚本，如果这个脚本中包含恶意代码，那你就凉凉了。总之，**绝不要相信用户的任何操作！**

## # 特性（属性）
- 对于 HTML 元素的属性，比如 id、title、class 等，我们可以用 **v-bind** 来操纵它。如：
  ```html
  <div v-bind:id="dynamicId"></div>
  ```
  ```js
  new Vue({
      data: {
          dynamicId: 'dongdong'
      }
  })
  ```
  以上代码中，div 元素的 id 属性的值并不是 "dynamicId" 哦！而是 "dongdong" 呢！这是因为 div 元素的 id 属性被 **动态绑定( v-bind )** 了，这样一来，div 标签中的 dynamicId 就变成了一个变量，那这个变量的值是什么呢？就是 data 中定义的 "dongdong" 了。所以，div 元素的 id 属性的属性值是 "dongdong"。
- 如果是要绑定布尔属性（布尔属性只要存在，就意味着 true）的话，那么 v-bind 工作起来就略有不同了。如：
  ```html
  <button v-bind:disabled="isButtonDisabled">按钮</button>
  ```
  ```js
  new Vue({
      data: {
          isButtonDisabled: true
      }
  })
  ```
  以上代码中，button 元素的 disabled 属性的值是 true，表示 button 元素不可用。如果你把 data 中的 isButtonDisabled 的值改成其他 falsy 值，那么按钮就可以用了，**disabled 属性甚至不会被包含在渲染出来的 button 元素中。**

## # 使用 JavaScript 表达式
- 上面我们在绑定数据的时候，无论是 {{ }} 或者 v-bind，都只绑定了一些简单的属性值。但实际上，对于所有的数据绑定，我们都可以使用 **js表达式** 来操作。如：
  ```html
  <p>{{ numer + 1 }}</p>
  <p>{{ ok ? 'YES' : 'NO' }}</p>
  <p>{{ message.split('').reverse().join('') }}</p>
  <div v-bind:id="'list-' + id"></div>
  ```
- 需要注意的是，每个绑定都只能包含**单个表达式**，不可以是语句，如：
  ```html
  <!-- 这是语句，不是表达式 -->
  <p>{{ var a = 1 }}</p>

  <!-- 这是流程控制语句，不是表达式 -->
  <p>{{ if (ok) { return message } }}</p>
  ```


# 指令

# 缩写