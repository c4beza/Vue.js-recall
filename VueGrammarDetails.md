- [模板插值](#模板插值)
  - [Mustache 语法](#mustache-语法)
  - [`v-once`](#v-once)
  - [`v-html`](#v-html)
  - [`v-text`](#v-text)
  - [`v-pre`](#v-pre)
  - [`v-cloak`](#v-cloak)
- [绑定属性](#绑定属性)
  - [`v-bind` 缩写为 `:`](#v-bind-缩写为-)
  - [v-bind 动态绑定 class 的两种方法](#v-bind-动态绑定-class-的两种方法)
  - [v-bind 动态绑定 style 的两种方法](#v-bind-动态绑定-style-的两种方法)
- [计算属性](#计算属性)
  - [`computed`](#computed)
  - [计算属性的复杂计算](#计算属性的复杂计算)
  - [计算属性的 setter 和 getter](#计算属性的-setter-和-getter)
  - [methods 和 computed 的区别](#methods-和-computed-的区别)
- [事件监听 `v-on`](#事件监听-v-on)
  - [`v-on`基础，简写为@](#v-on基础简写为)
  - [`v-on`参数](#v-on参数)
  - [`v-on` 事件修饰符](#v-on-事件修饰符)
- [条件判断](#条件判断)
  - [`v-if v-else-if v-else`](#v-if-v-else-if-v-else)
  - [`v-if v-show`的区别](#v-if-v-show的区别)
- [循环遍历](#循环遍历)
  - [`v-for`遍历数组](#v-for遍历数组)
  - [`v-for`遍历对象](#v-for遍历对象)
  - [组件的 key 属性](#组件的-key-属性)
  - [检测数组更新的响应式方法](#检测数组更新的响应式方法)
- [`v-model`](#v-model)
  - [`v-model`的基本使用](#v-model的基本使用)
  - [`v-model`结合 radio 使用](#v-model结合-radio-使用)
  - [`v-model`结合 checkbox 使用](#v-model结合-checkbox-使用)
  - [`v-model`结合 select 使用](#v-model结合-select-使用)
  - [`v-model` 事件修饰符](#v-model-事件修饰符)
- [组件化开发](#组件化开发)
  - [Vue 组件化思想](#vue-组件化思想)
  - [注册组件](#注册组件)
  - [全局组件和局部组件](#全局组件和局部组件)
  - [父组件和子组件](#父组件和子组件)
  - [注册组件语法糖](#注册组件语法糖)
  - [组件数据的存放](#组件数据的存放)
- [父子组件的通信](#父子组件的通信)
  - [父传子](#父传子)
  - [子传父](#子传父)
- [slot 插槽](#slot-插槽)
  - [slot 的基本使用](#slot-的基本使用)
  - [具名插槽 slot](#具名插槽-slot)
  - [作用域插槽](#作用域插槽)

# 模板插值

## Mustache 语法

-   即为双大括号，用于将 data 中的数据渲染进 html 中。
-   `{{ data.msg }}`

## `v-once`

-   为 Vue 当中的指令，直接使用在 html 标签中，不需要跟任何表达式。
-   该指令表示元素和组件只渲染一次，不会随着数据的改变而改变

```
<div id="app">
  <p v-once>{{msg}}</p>
  <!-- //msg不会改变 -->
  <p>{{msg}}</p>
  <p>
    <input type="text" v-model = "msg">
  </p>
</div>

<script src="../js/vue.js"></script>
<script type="text/javascript">
  let vm = new Vue({
    el : '#app',
    data : {
      msg : "hello"
    }
  });
</script>
```

## `v-html`

-   可以使数据按照 html 格式来解析
-   `<h2 v-html = "link"></h2>`

## `v-text`

-   作用和 Mustachea 一致，但直接放于 html 标签中。值为 string 型
-   `<h2 v-text = "messgage"></h2>` === `{{ message }}`

## `v-pre`

-   为 Vue 的指令，直接使用在 html 标签中，用于显示原本的 Mustache 语法
-   会跳过这个元素和它子元素的编译过程，一些静态的内容不需要编辑加这个指令可以加快编辑
-   `<h2 v-pre> {{ message }}</h2>` => `{{ message }}`

## `v-cloak`

-   可以使用 v-cloak 指令设置样式，这些样式会在 Vue 实例编译结束时，从绑定的 HTML 元素上被移除。
-   当网络较慢，网页还在加载 Vue.js ，而导致 Vue 来不及渲染，这时页面就会显示出 Vue 源代码。我们可以使用 v-cloak 指令来解决这一问题。
-   通过在 css 中调用 v-cloak 可对渲染之前的模块调整样式。

```
  <div id="app" v-cloak>
  <h2>{{message}}</h2>
</div>

<script src="../js/vue.js"></script>
<script>
  // 在vue解析之前, div中有一个属性v-cloak
  // 在vue解析之后, div中没有一个属性v-cloak
  setTimeout(function () {
    const app = new Vue({
      el: '#app',
      data: {
        message: '你好啊'
      }
    })
  }, 1000)
</script>

<style>
    [v-cloak]{
    display: none;
}
</style>
```

# 绑定属性

## `v-bind` 缩写为 `:`

-   可实现属性的动态绑定，或者向另一个组件传递 props 值
    -   比如动态绑定 a 标签的 href 属性
    -   比如动态绑定 img 元素的 src 属性

## v-bind 动态绑定 class 的两种方法

-   使用对象语法
    -   用法一：直接通过{}绑定一个类 </br>
        `<h2 :class = "{ 'active' = isActive}"> Hello World </h2>`
    -   用法二：也可以通过判断，传入多个值 </br>
        `<h2 :class = "{ 'active' = isActive, 'line' : isLine}"> Hello World </h2>`
    -   用法三：和普通的类同时存在并不冲突，即会有 title、active、line 三个类</br>
        `<h2 class = "title" :class = "{ 'active' = isActive, 'line' : isLine}"> Hello World </h2>`
    -   用法四：如果过于复杂，可以放在 methods 或者 computed 中。注：classes 是个计算属性</br>
        `<h2 class = "title :class = "classes"> Hello World </h2>`
-   使用数组语法,适用于无判断类绑定
    -   用法一：直接通过[]绑定一个类 </br>
        `<h2 :class = "['active']"> Hello World </h2>`
    -   用法二：也可以通过判断，传入多个值 </br>
        `<h2 :class = "['active', 'line']"> Hello World </h2>`
    -   用法三：和普通的类同时存在并不冲突，即会有 title、active、line 三个类</br>
        `<h2 class = "title" :class = "['active', 'line']"> Hello World </h2>`
    -   用法四：如果过于复杂，可以放在 methods 或者 computed 中。注：classes 是个计算属性</br>
        `<h2 class = "title :class = "classes"> Hello World </h2>`

## v-bind 动态绑定 style 的两种方法

-   使用对象语法，style 后面跟的是一个对象类型

    -   对象的 key 是 css 属性名，可用 camelCase => fontSize，也可用 kebab-case => 'font-size'
    -   对象的 value 可以使具体赋值（必须加上单引号, 否则是当做一个变量去解析），也可以是来自于 data 中的属性。
    -   如果样式过于复杂，可以调用 methods 中返回一个样式对象。

    ```
    <h2 :style="{key(属性名): value(属性值)}">{{message}}</h2>

    <!--'50px'必须加上单引号, 否则是当做一个变量去解析-->
    <h2 :style="{fontSize: '50px'}">{{message}}</h2>

    <!--finalSize当成一个变量使用-->
    <h2 :style="{fontSize: finalSize}">{{message}}</h2>
    <h2 :style="{fontSize: finalSize + 'px', backgroundColor: finalColor}">{{message}}</h2>
    <h2 :style="getStyles()">{{message}}</h2>

      data: {
        message: '你好啊',
        finalSize: 100,
        finalColor: 'red',
      },
      methods: {
        getStyles: function () {
          return {fontSize: this.finalSize + 'px', backgroundColor: this.finalColor}
        }
      }
    ```

-   使用数组语法
    -   style 后面跟的是一个数组。多个样式值以逗号进行分割。
    ```
      <h2 :style="[baseStyle, baseStyle1]">{{message}}</h2>
        data: {
        message: '你好啊',
        baseStyle: {backgroundColor: 'red'},
        baseStyle1: {fontSize: '100px'},
      }
    ```

# 计算属性

## `computed`

-   在某些情况，需要对数据进行一些转化后再进行显示，或者需要多个数据结合起来进行显示。
-   ```
      <div id="app">
          <h2>{{firstName + ' ' + lastName}}</h2>
          <h2>{{firstName}} {{lastName}}</h2>
          <h2>{{getFullName()}}</h2>
          <h2>{{fullName}}</h2>
      </div>
        data: {
        firstName: 'Lebron',
        lastName: 'James'
      },
      // computed: 计算属性()
      computed: {
        fullName: function () {
          return this.firstName + ' ' + this.lastName
        }
      },
      methods: {
        getFullName() {
          return this.firstName + ' ' + this.lastName
        }
    ```

## 计算属性的复杂计算

-   ```
    <h2>总价格: {{totalPrice}}</h2>
    <h2>总价格: {{getTotalPrice()}}</h2>
    <script>
    const app = new Vue({
        el: '#app',
        data: {
        books: [
            {id: 110, name: 'Unix编程艺术', price: 119},
            {id: 111, name: '代码大全', price: 105},
            {id: 112, name: '深入理解计算机原理', price: 98},
            {id: 113, name: '现代操作系统', price: 87},
        ]
        },
        methods: {
        getTotalPrice: function () {
            let result = 0
            for (let i=0; i < this.books.length; i++) {
            result += this.books[i].price
            }
            return result
        }
        },
        computed: {
        totalPrice: function () {
            let result = 0
            for (let i=0; i < this.books.length; i++) {
            result += this.books[i].price
            }
            return result}
        }
        }
    )
    ```

## 计算属性的 setter 和 getter

-   每个计算属性都包含一个 getter 和 setter，getter 负责读取，setter 负责赋值。
-   在 template 中，我们可以看到，input 是直接绑 v-model=“fullName”，如果我们这里直接修改了 fullName 的值，那么就会触发 setter，同时也会触发 getter 以及 updated 函数。其执行顺序是 setter -> getter -> updated，如下：
-   这里需要注意的是，并不是触发了 setter 也就会触发 getter，他们两个是相互独立的。我们这里修改了 fullName 会触发 getter 是因为 setter 函数里有改变 firstName 和 lastName 值的代码。也就是说我们如果注释掉上边的 setter 中修改 firstName 和 lastName 的代码后就不会执行 getter。
-   ```
      <div id="app">
      <h2>{{fullName}}</h2>
      <input type = "text" v-model = "fullName">
      <input type = "text" v-model = "firstName">
      <input type = "text" v-model = "lastName">
      </div>

      <script>
      const app = new Vue({
          el: '#app',
          data: {
          firstName: 'Kobe',
          lastName: 'Bryant'
          },
          computed: {
              fullName: {
                  get(){
                      console.log('computed getter...')
                      return this.firstName + ' ' + this.lastName
                  },
                  set(newValue){
                      console.log('computed setter...')
                      var names = newValue.split(' ')
                      this.firstName = names[0]
                      this.lastName = names[names.length - 1]
                      return this.firstName + ' ' + this.lastName
                  }
              },
          },
          updated(){
              console.log('updated')
          }
      })
      </script>
    ```

## methods 和 computed 的区别

-   计算属性会进行缓存，如果多次使用时，计算属性只会调用一次。

# 事件监听 `v-on`

## `v-on`基础，简写为@

-   写在 html 标签中，可以将事件指向一个在 methods 中定义的函数。

## `v-on`参数

-   当通过 methods 中定义方法，以供@click 调用时，需要注意参数问题
    -   情况一： 如果该方法不需要额外参数，那么方法后的（）可以不添加。但是注意如果方法本身中有一个参数，那么会默认将原生时间 event 参数传递进去。
    -   情况二： 如果需要传入某个参数，同时需要 event 时，可以通过$event 传入事件。

```
    <div id="app">
    <!--1.事件调用的方法没有参数-->
    <button @click="btn1Click()">按钮1</button>
    <button @click="btn1Click">按钮1</button>

    <!--2.在事件定义时, 写方法时省略了小括号, 但是方法本身是需要一个参数的, 这个时候, Vue会默认将浏览器生产的event事件对象作为参数传入到方法-->
    <!--<button @click="btn2Click(123)">按钮2</button>-->
    <!--<button @click="btn2Click()">按钮2</button>-->
    <button @click="btn2Click">按钮2</button>

    <!--3.方法定义时, 我们需要event对象, 同时又需要其他参数-->
    <!-- 在调用方式, 如何手动的获取到浏览器参数的event对象: $event-->
    <button @click="btn3Click(abc, $event)">按钮3</button>
    </div>

    <script src="../js/vue.js"></script>
    <script>
    const app = new Vue({
        el: '#app',
        data: {
        message: '你好啊',
        abc: 123
        },
        methods: {
        btn1Click() {
            console.log("btn1Click");
        },
        btn2Click(event) {
            console.log('--------', event);
        },
        btn3Click(abc, event) {
            console.log('++++++++', abc, event);
        }
        }
    })

    // 如果函数需要参数,但是没有传入, 那么函数的形参为undefined
    // function abc(name) {
    //   console.log(name);
    // }
    //
    // abc()
    </script>
```

## `v-on` 事件修饰符

-   .stop - 调用 event.stopPropagation()，阻止事件继续传播
-   .prevent - 调用 event.preventDefault()，阻止标签默认行为
-   .{keyCode|keyAlias} - 只当事件是从特定键触发时才触发回调。
    -   你还可以通过全局 config.keyCodes 对象自定义按键修饰符别名：
    -   // 可以使用 `v-on:keyup.f1`
    -   Vue.config.keyCodes.f1 = 112
-   .native - 监听组件根元素的原生事件。用处：对于 elementUI 的 input，我们需要在后面加上.native, 因为 elementUI 对 input 进行了封装，原生的事件不起作用。
-   .once - 事件将只会触发一次
-   .capture - 使用事件捕获模式，即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理
-   .self - 只当在 event.target 是当前元素自身时触发处理函数
-   .passive - 告诉浏览器你不想阻止事件的默认行为
-   使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击。

# 条件判断

## `v-if v-else-if v-else`

-   这三个指令与 javascript 的条件语句 if、else、else if 类似。
-   vue 的条件指令可以根据表达式的值在 DOM 中进行渲染和销毁元素和组件。
-   v-if 后面的条件为 false 时，对应的元素以及子元素不会被渲染。也就是根本没有对应的标签出现在 DOM 中

```
<div id="app">
  <h2 v-if="score>=90">优秀</h2>
  <h2 v-else-if="score>=80">良好</h2>
  <h2 v-else-if="score>=60">及格</h2>
  <h2 v-else>不及格</h2>

  <h1>{{result}}</h1>
</div>
```

## `v-if v-show`的区别

-   `v-if`条件为 false 时，压根不会有对应的元素在 DOM 中。
-   `v-show`条件为 false 时，仅仅是将元素的 display 属性设置为 none 而已。
-   在开发中，如果需要在显示隐藏之间切换很频繁时，使用 v-show
-   只有一次切换时，使用 v-if

# 循环遍历

## `v-for`遍历数组

-   语法：`v-for = "movie in movies"` 或者 `v-for = "(item,index) in items"`

    ```
    <div id="app">
    <!--1.在遍历的过程中,没有使用索引值(下标值)-->
    <ul>
        <li v-for="item in names">{{item}}</li>
    </ul>

    <!--2.在遍历的过程中, 获取索引值-->
    <ul>
        <li v-for="(item, index) in names">
        {{index+1}}.{{item}}
        </li>
    </ul>
    </div>
    ```

## `v-for`遍历对象

-   语法：`v-for = "(value,key,index) in info"`

    ```
    <div id="app">
    <!--1.在遍历对象的过程中, 如果只是获取一个值, 那么获取到的是value-->
    <ul>
      <li v-for="item in info">{{item}}</li>
    </ul>


    <!--2.获取key和value 格式: (value, key) -->
    <ul>
      <li v-for="(value, key) in info">{{value}}-{{key}}</li>
    </ul>


    <!--3.获取key和value和index 格式: (value, key, index) -->
    <ul>
      <li v-for="(value, key, index) in info">{{value}}-{{key}}-{{index}}</li>
    </ul>
      </div>
    ```

## 组件的 key 属性

-   在使用 v-for 时，需要给对应的元素或组件添加上一个 :key 属性。与 Vue 的虚拟 DOM 的 Diff 算法有关。
-   当我们使用 key 来给做一个唯一标识时，diff 算法就可以正确的识别节点，并找到正确的位置区插入新的节点。
-   key 的作用是为了高效的更新虚拟 DOM。

## 检测数组更新的响应式方法

-   vue 通过原型拦截的方式重写了数组的 7 个方法，首先获取到这个数组的 Observer。如果有新的值，就调用 observeArray 对新的值进行监听，然后调用 notify，通知 render watcher，执行 update

-   核心：arrayMethods 首先继承了 Array，然后对数组中所有能改变数组自身的方法，如 push、pop 等这些方法进行重写。重写后的方法会先执行它们本身原有的逻辑，并对能增加数组长度的 3 个方法 push、unshift、splice 方法做了判断，获取到插入的值，然后把新添加的值变成一个响应式对象，并且再调用 ob.dep.notify() 手动触发依赖通知，这就很好地解释了之前的示例中调用 vm.items.splice(newLength) 方法可以检测到变化。

```
        // 1.push方法
        // this.letters.push('aaa')
        // this.letters.push('aaaa', 'bbbb', 'cccc')

        // 2.pop(): 删除数组中的最后一个元素
        // this.letters.pop();

        // 3.shift(): 删除数组中的第一个元素
        // this.letters.shift();

        // 4.unshift(): 在数组最前面添加元素
        // this.letters.unshift()
        // this.letters.unshift('aaa', 'bbb', 'ccc')

        // 5.splice作用: 删除元素/插入元素/替换元素
        // 删除元素: 第二个参数传入你要删除几个元素(如果没有传,就删除后面所有的元素)
        // 替换元素: 第二个参数, 表示我们要替换几个元素, 后面是用于替换前面的元素
        // 插入元素: 第二个参数, 传入0, 并且后面跟上要插入的元素
        // this.letters.splice(1, 3, 'm', 'n', 'l', 'x')
        // this.letters.splice(1, 0, 'x', 'y', 'z')

        // 6.sort()
        // this.letters.sort()

        // 7.reverse()
        // this.letters.reverse()

        // 注意: 通过索引值修改数组中的元素
        // this.letters[0] = 'bbbbbb';
        // this.letters.splice(0, 1, 'bbbbbb')
        // set(要修改的对象, 索引值, 修改后的值)
        Vue.set(this.letters, 0, 'bbbbbb')
```

# `v-model`

## `v-model`的基本使用

-   Vue 中使用 v-model 指令来实现表单元素和数据双向绑定
-   vue2.x 实现双向数据绑定的原理是利用了 Object.defineProperty() 这个方法重新定义了对象获取属性值(get)和设置属性值(set)的操作来实现的。
    -   Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。它接收三个参数，要操作的对象，要定义或修改的对象属性名，属性描述符。重点就是最后的属性描述符。属性描述符是一个对象，主要有两种形式：数据描述符和存取描述符。
    -   这两种对象只能选择一种使用，不能混合两种描述符的属性同时使用。上面说的 get 和 s et 就是属于存取描述符对象的属性。然后我们可以通过在存取描述符中的 get 和 set 方法内写入自定义的逻辑来实现对象获取属性和设置属性时的行为。
-   vue3.x 用 ES6 的语法 Proxy 对象来实现的，也可以实现数据的劫持。
    -   defineProperty 只能监听某个属性，不能对全对象监听
    -   可以省去 for in、闭包等内容来提升效率（直接绑定整个对象即可）
    -   可以监听数组，不用再去单独的对数组做特异性操作
    -   vue3.x 可以检测到数组内部数据的变化

## `v-model`结合 radio 使用

```
    <div id="app">
    <label for="male">
        <input type="radio" id="male" value="男" v-model="sex">男
    </label>
    <label for="female">
        <input type="radio" id="female" value="女" v-model="sex">女
    </label>
    <h2>您选择的性别是: {{sex}}</h2>
    </div>
    <script>
    const app = new Vue({
        el: '#app',
        data: {
        message: '你好啊',
        sex: '女'
        }
    })
    </script>
```

## `v-model`结合 checkbox 使用

-   单个勾选框，v-model 为布尔值，此时 input 的 value 并不影响 v-model 的值
-   多个复选框，v-model 对应的 data 属性为一个数组，当勾中某个选项时，就会将 input 的 value 添加到数组中。

```
    <div id="app">
    <h2>您的爱好是: {{hobbies}}</h2>

    <label v-for="item in originHobbies" :for="item">
        <input type="checkbox" :value="item" :id="item" v-model="hobbies">{{item}}
    </label>
    </div>

    <script src="../js/vue.js"></script>
    <script>
    const app = new Vue({
        el: '#app',
        data: {
        message: '你好啊',
        isAgree: false, // 单选框
        hobbies: [], // 多选框,
        originHobbies: ['篮球', '足球', '乒乓球', '羽毛球', '台球', '高尔夫球']
        }
    })
    </script>
```

## `v-model`结合 select 使用

-   单选，v-model 绑定的是一个值，当我们选中 option 中的一个时，会将它对应的 value 赋值到 data 中
-   多选，v-model 绑定的是一个数组，当选中多个值时，会将选中的 option 添加到 data 的数组中

```
<div id="app">
  <!--1.选择一个-->
  <select name="abc" v-model="fruit">
    <option value="苹果">苹果</option>
    <option value="香蕉">香蕉</option>
    <option value="榴莲">榴莲</option>
    <option value="葡萄">葡萄</option>
  </select>
  <h2>您选择的水果是: {{fruit}}</h2>

  <!--2.选择多个-->
  <select name="abc" v-model="fruits" multiple>
    <option value="苹果">苹果</option>
    <option value="香蕉">香蕉</option>
    <option value="榴莲">榴莲</option>
    <option value="葡萄">葡萄</option>
  </select>
  <h2>您选择的水果是: {{fruits}}</h2>
</div>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊',
      fruit: '香蕉',
      fruits: []
    }
  })
</script>
```

## `v-model` 事件修饰符

-   lazy - 默认情况下，v-model 同步输入框的值和数据。可以通过这个修饰符，转变为在数据失去焦点或者回车时才会更新。
-   number - 默认情况下，在输入框中无论我们输入的事字母还是数字，都会被当做字符串类型进行处理。当我们希望处理的是数字类型，可用 number 修饰符，自动将用户的输入值转化为数值类型
-   trim - 自动过滤用户输入的首尾空格

```
  <!--2.修饰符: number-->
  <input type="number" v-model.number="age">
  <h2>{{age}}-{{typeof age}}</h2>

  <!--3.修饰符: trim-->
  <input type="text" v-model.trim="name">
  <h2>您输入的名字:{{name}}</h2>
</div>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊',
      age: 0,
      name: ''
    }
  })
  var age = 0
  age = '1111'
  age = '222'
</script>
```

# 组件化开发

## Vue 组件化思想

-   如果我们将一个页面中所有的处理逻辑全部放在一起，处理起来就会变得非常复杂，而且不利于后续的管理以及扩展。
-   但如果我们将一个页面拆分成一个个小的功能块，每个功能块完成属于自己这部分独立的功能，那么之后整个页面的管理和维护就变得非常容易了。

## 注册组件

-   组件的使用分成三个步骤

    -   创建组件构造器
        -   在调用 Vue.extend()创建组件构造器时，传入 template 代表我们自定义组件的模板，该模板包含要显示的 html 代码
    -   注册组件
        -   调用 Vue.component()是将刚才的组件构造器注册为一个组件，并且给它起一个组建的标签名称。
        -   所以需要传递两个参数：1、注册组件的标签名 2、组件构造器
    -   使用组件
        -   将组件挂在到 Vue 实例下

    ```
    <script src="../js/vue.js"></script>
      <script>
      // 1.创建组件构造器对象
      const cpnC = Vue.extend({
          template: `
          <div>
              <h2>我是标题</h2>
              <p>我是内容, 哈哈哈哈</p>
              <p>我是内容, 呵呵呵呵</p>
          </div>`
      })

      // 2.注册组件
      Vue.component('my-cpn', cpnC)

      const app = new Vue({
          el: '#app',
          data: {
          message: '你好啊'
          }
      })
      </script>
    ```

## 全局组件和局部组件

-   当我们通过调用 Vue.component()注册组件时，组件的注册是全局的。意味着该组件可以在任意 Vue 实例下使用

```
<div id="app">
  <cpn></cpn>
</div>

<div id="app2">
  <cpn></cpn>
</div>

<script>
  // 1.创建组件构造器
  const cpnC = Vue.extend({
    template: `
      <div>
        <h2>我是标题</h2>
        <p>我是内容,哈哈哈哈啊</p>
      </div>
    `
  })

  // 2.注册组件(全局组件, 意味着可以在多个Vue的实例下面使用)
  // Vue.component('cpn', cpnC)

  // 疑问: 怎么注册的组件才是局部组件了?

  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    components: {
      // cpn使用组件时的标签名
      cpn: cpnC
    }
  })

  const app2 = new Vue({
    el: '#app2'
  })
</script>
```

## 父组件和子组件

-   当子组件注册到父组件的 components 时，Vue 会编译好父组件的模块。
-   该模块的内容已经决定了父组件将要渲染的 html（相当于父组件中已经有了子组件的内容了）
-   `<child-cpn></child-cpn>`是只能在父组件中被识别的

```
<div id="app">
  <parent-cpn></parent-cpn>
  <!--<child-cpn></child-cpn>-->
</div>

<script src="../js/vue.js"></script>
<script>
  // 1.创建第一个组件构造器(子组件)
  const cpnC1 = Vue.extend({
    template: `
      <div>
        <h2>我是标题1</h2>
        <p>我是内容, 哈哈哈哈</p>
      </div>
    `
  })

  // 2.创建第二个组件构造器(父组件)
  const cpnC2 = Vue.extend({
    template: `
      <div>
        <h2>我是标题2</h2>
        <p>我是内容, 呵呵呵呵</p>
        <child-cpn></child-cpn>
      </div>
    `,
    components: {
      child-cpn: cpnC1
    }
  })

  // root组件
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    components: {
      parent-cpn: cpnC2
    }
  })
</script>
```

## 注册组件语法糖

-   为了简化注册组件的这个过程，Vue 提供了注册的语法糖
-   主要是省去了调用 Vue.extend()的步骤，而是可以直接使用一个对象来代替。

```
<div id="app">
  <cpn1></cpn1>
  <cpn2></cpn2>
</div>

<script src="../js/vue.js"></script>
<script>
  // 1.全局组件注册的语法糖
  // 1.创建组件构造器
  // const cpn1 = Vue.extend()

  // 2.注册组件
  Vue.component('cpn1', {
    template: `
      <div>
        <h2>我是标题1</h2>
        <p>我是内容, 哈哈哈哈</p>
      </div>
    `
  })

  // 2.注册局部组件的语法糖
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    components: {
      'cpn2': {
        template: `
          <div>
            <h2>我是标题2</h2>
            <p>我是内容, 呵呵呵</p>
          </div>
    `
      }
    }
  })
</script>
```

-   或者可以使用模板分离写法

```
<div id="app">
  <cpn></cpn>
  <cpn></cpn>
  <cpn></cpn>
</div>

<!--1.script标签, 注意:类型必须是text/x-template-->
<!--<script type="text/x-template" id="cpn">-->
<!--<div>-->
  <!--<h2>我是标题</h2>-->
  <!--<p>我是内容,哈哈哈</p>-->
<!--</div>-->
<!--</script>-->

<!--2.template标签-->
<template id="cpn">
  <div>
    <h2>我是标题</h2>
    <p>我是内容,呵呵呵</p>
  </div>
</template>

<script src="../js/vue.js"></script>
<script>

  // 1.注册一个全局组件
  Vue.component('cpn', {
    template: '#cpn'
  })

  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    }
  })
</script>
```

## 组件数据的存放

-   组件不可以访问 Vue 实例中的数据，组件有自己的数据保存地方
-   组件对象也有个一个 data 属性，以及 methods 等属性
-   data 属性是一个函数，返回一个保存着数据的对象
-   data 是一个函数的原因是：因为组件是可以多次调用的，如果 data 是一个对象的话，多次调用会相互影响。

```
  // 1.注册一个全局组件
  Vue.component('cpn', {
    template: '#cpn',
    data() {
      return {
        title: 'abc'
      }
    }
  })
```

# 父子组件的通信

-   通过 props 向子组件传递数据
-   通过事件向父组件发送消息

## 父传子

-   props 的基本用法
    -   在组件中，可以使用 props 来声明需要从父组件接收到的数据。
    -   方式一：字符串数组，数组中的字符串就是传递时的名称
    -   方式二：对象，对象可以设置传递时的类型，也可以设置默认值等
    -   在调用子组件时，使用 v-bind:[key] = "value"，然后在子组件构造器 props 属性中定义 key
-   props 数据验证
    -   当需要对 props 进行类型等验证时，就需要用到对象写法

```
<div id="app">
  <!--<cpn v-bind:cmovies="movies"></cpn>-->
  <!--<cpn cmovies="movies" cmessage="message"></cpn>-->

  <cpn :cmessage="message" :cmovies="movies"></cpn>
</div>



<template id="cpn">
  <div>
    <ul>
      <li v-for="item in cmovies">{{item}}</li>
    </ul>
    <h2>{{cmessage}}</h2>
  </div>
</template>

<script src="../js/vue.js"></script>
<script>
  // 父传子: props
  const cpn = {
    template: '#cpn',
    // props: ['cmovies', 'cmessage'],
    props: {
      // 1.类型限制
      // cmovies: Array,
      // cmessage: String,

      // 2.提供一些默认值, 以及必传值
      cmessage: {
        type: String,
        default: 'aaaaaaaa',
        required: true
      },
      // 类型是对象或者数组时, 默认值必须是一个函数
      cmovies: {
        type: Array,
        default() {
          return []
        }
      }
    },
    data() {
      return {}
    },
    methods: {

    }
  }

  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊',
      movies: ['海王', '海贼王', '海尔兄弟']
    },
    components: {
      cpn
    }
  })
</script>
```

## 子传父

-   子级向父级传递
    -   当子组件需要想父组件传递数据时，要用到自定义事件。
    -   v-on 不仅仅可以用于监听 DOM 事件，还可以用于组件间的自定义事件。
-   自定义事件流程
    -   在子组件中，通过`$emit()`来触发事件。`this.$emit([eventName]，value)`
    -   在父组件中，通过 v-on 来监听子组件事件。`@eventName = "func"`

```

<!--父组件模板-->
<div id="app">
  <cpn @item-click="cpnClick"></cpn>
</div>

<!--子组件模板-->
<template id="cpn">
  <div>
    <button v-for="item in categories"
            @click="btnClick(item)">
      {{item.name}}
    </button>
  </div>
</template>

<script src="../js/vue.js"></script>
<script>

  // 1.子组件
  const cpn = {
    template: '#cpn',
    data() {
      return {
        categories: [
          {id: 'aaa', name: '热门推荐'},
          {id: 'bbb', name: '手机数码'},
          {id: 'ccc', name: '家用家电'},
          {id: 'ddd', name: '电脑办公'},
        ]
      }
    },
    methods: {
      btnClick(item) {
        // 发射事件: 自定义事件
        this.$emit('item-click', item)
      }
    }
  }

  // 2.父组件
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    components: {
      cpn
    },
    methods: {
      cpnClick(item) {
        console.log('cpnClick', item);
      }
    }
  })
</script>
```

# slot 插槽

## slot 的基本使用

-   在子组件中使用特殊的元素`<slot></slot>`就可以为子组件开启一个插槽
-   该插槽插入什么内容取决于父组件如何使用

```
<body>
    <div id="app">
        <cpn></cpn> //显示插槽中默认内容 即为button
        <cpn><input type="text"></cpn> //替换插槽中的button为一个输入框
    </div>
    <template id="cpn">
        <div>
            <h2>我是组件</h2>
            <p>我是组件，哈哈哈</p>
            <slot><button>按钮</button></slot>
        </div>
    </template>
    <script>

        const cpn = {
            template:'#cpn'
        }
        var vm = new Vue({
            el: '#app',
            data: {},
            methods: {},
            components:{
                cpn
            }
        });
    </script>
</body>
```

## 具名插槽 slot

-   当子组件的功能复杂时，子组件的插槽可能并非一个。这时需要给各个插槽起名字来区分它们。
-   给 slot 元素一个 name 属性就为具名插槽。`<slot name="mySlot"></slot>`

```
<body>

    <div id="app">
        <cpn v-show="isShow"><span slot="center">标题</span></cpn>
    </div>
    <template id="cpn">
        <div>
            <slot><span>左边</span></slot>
            <slot name="center"><span>中间</span></slot>
            <slot><span>右边</span></slot>
        </div>
    </template>
    <script>
/**
父组件模板的所有东西都会在父级作用域内编译，子组件模板所有的东西都会在子级
作用域编译
*/
<body>
    <div id="app">
        <cpn v-show="isShow"><span slot="center">标题</span></cpn> //左边 标题 中间
    </div>
    <template id="cpn">
        <div>
            <slot><span>左边</span></slot>
            <slot name="center"><span>中间</span></slot>
            <slot><span>右边</span></slot>
        </div>
    </template>
    /**
    父组件模板的所有东西都会在父级作用域内编译，子组件模板所有的东西都会在子级
    作用域编译
    */
    <script>
        const cpn = {
            template:'#cpn',
            data(){
                return{
                    isShow:true
                }
            }
        }
        var vm = new Vue({
            el: '#app',
            data: {
                isShow:false
            },
            methods: {},
            components:{
                cpn
            }
        });
    </script>
</body>
```

## 作用域插槽

-   父组件替换插槽的标签，但是内容由子组件来提供，也就是说子组件中定义数据，并且绑定到 slot 上

```

<body>

    <div id="app">
        <cpn></cpn>
        <cpn>
            <br>
            <br>
           <!-- 目的是获取子组件的pLanguage -->
          <template slot-scope="slot">
              <span v-for="item in slot.data">{{item}} - </span>
              <br>
            <span>{{slot.data.join(' - ')}}</span>
          </template>
        </cpn>
    </div>
    <template id="cpn">
        <div>
           <slot :data="pLanguages"><li v-for="item in pLanguages">{{item}}</li></slot>
        </div>
    </template>
    <script>
/**
父组件替换插槽的标签，但是内容是由子组件来提供
*/
<body>
    <div id="app">
        <cpn></cpn>
        <cpn>
           <!-- 目的是获取子组件的pLanguage -->
          <template slot-scope="slot">
            <span v-for="item in slot.data">{{item}} - </span> //javascript-c++-go-java-
            <span>{{slot.data.join(' - ')}}</span> //javascript-c++-go-java
          </template>
        </cpn>
    </div>
    <template id="cpn">
        <div>
           <slot :data="pLanguages"><li v-for="item in pLanguages">{{item}}</li></slot>
        </div>
    </template>
    <script>
        const cpn = {
            template:'#cpn',
            data(){
                return{
                   pLanguages: ['javascript','c++','go','java']  //子组件中定义数据，并且绑定到slot上
                }
            }
        }
        var vm = new Vue({
            el: '#app',
            data: {
                isShow:false
            },
            methods: {},
            components:{
                cpn
            }
        });
    </script>
</body>

```
