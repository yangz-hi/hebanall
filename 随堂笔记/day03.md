## VUE基础-day03



### 01-每日回顾

- v-bind指令
  - class属性
    - 对象 `:class="{'类名':'是否使用这个类名',...}"`
    - 数组 `:class="['类名1','类名',...]"`
  - style属性
    - 对象 `:style="{fontSize:'100px',...}"`
    - 数组 `:style="[{css属性},{},...]"`
- v-model指令
  - 语法糖（某些代码简写）原理
    - `:value`  `@input`   关键字
  - 其他表单元素的双向绑定
- v-cloak指令
  - 插值表达式闪烁问题
  - 配合自己写一段样式来实现
- v-once
  - 只渲染一次
- vue的过滤器
  - 转换输出数据格式的
  - 全局 `Vue.filter('过滤器名称',处理函数)`
  - 局部 `new Vue({filters:{过滤器名称:处理函数}})`
- vue中获取dom
  - ref属性标识容器
  - 通过$refs来获取



### 02-每日作业

- 实现时间格式转换
- 实现自动获取焦点

```diff
+    <input type="text" class="form-control" placeholder="输入搜索关键字" style="margin-bottom: 15px;width:195px">
    <table class="table table-bordered ">
```



使用过滤器来实现时间格式转换：

```js
    // 过滤器
    Vue.filter('formatDate',(val)=>{
      // val 是使用过滤器的时候，管道符前的js表达式的执行结果
      // val 可能是字符串 Date对象，转换成date对象
      const myDate = new Date(val)
      const y = myDate.getFullYear()
      const m = String(myDate.getMonth() + 1).padStart(2,0)
      const d = String(myDate.getDate()).padStart(2,0)
      const hh = String(myDate.getHours()).padStart(2,0)
      const mm = String(myDate.getMinutes()).padStart(2,0)
      const ss = String(myDate.getSeconds()).padStart(2,0)
      return `${y}-${m}-${d} ${hh}:${mm}:${ss}`
    })
```

```html
<td>{{brand.createTime|formatDate}}</td>
```



使用ref来实现自动获取焦点

- 有个H5属性 `autofocus`  即可自动获取焦点.
  - 但是模板会被vue解析，所以不会自动获取焦点。
- 可以在模板解析后，通过dom去获取焦点。
  - 获取焦点 `inputDom.focus()`  

```html
<input ref="inputDom" type="text" class="form-control" 
```

```js
    // window.onload = function(){}  等页面资源（css,js,img...）全部加载完毕执行
    // $(function(){}) 等页面文档（html结构）加载完成

    // 等模板解析完成，获取焦点
    setTimeout(()=>{
      // 意思：等所有代码执行完毕后，在来执行定时器中的代码
      vm.$refs.inputDom.focus()
    },0)
    // 不建议这种方式，不规范,投机取巧。
    // 补充：后面会学习vue的模板渲染完成的回调函数。
```





### 03-自定义指令

> 目标：vue提供的指令是有限的，实现的功能有限，尝试自己封装指令（自定义指令）

作用：

- 通过vue来封装指令（directive），从而去扩展标签原本的功能。

语法：

- 全局指令
  - 语法：`Vue.directive(指令名称,指令的配置对象)`
  - 指令名称：定义的时候不需要`v-`，但是使用的时候加上`v-`
  - 指令的配置对象：{inserted(el){}}  等使用该指令的元素渲染完毕后（dom生成后）执行
    - 在dom生成后才可为该dom扩展功能
    - el 就是使用指令的dom对象
- 局部指令
  - 语法：`new Vue({directives:{指令名称:指令的配置对象,...}})`
  - 指令名称：定义的时候不需要`v-`，但是使用的时候加上`v-`
  - 指令的配置对象：{inserted(el){}}  等使用该指令的元素渲染完毕后（dom生成后）执行
    - 在dom生成后才可为该dom扩展功能
    - el 就是使用指令的dom对象

代码：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title></title>
  </head>
  <body>
    <div id="app">
      <!-- 需求：v-focus指令来为该input标签实现自动获取焦点功能 -->
      <input type="text" v-focus="{h:100,w:100}">
    </div>
    <script src="./vue.js"></script>
    <script>
      // 全局指令
      // Vue.directive('focus',{
      //   // inserted 函数使用该指令的元素渲染完毕后执行
      //   inserted (el) {
      //     // el 使用该指令的DOM
      //     // 获取焦点
      //     // 对dom扩展任意功能
      //     el.style.height = '200px'
      //     el.focus()
      //   }
      // })

      const vm = new Vue({
        el: '#app',
        data: {},
        methods: {},
        // 局部 定义自定义指令
        directives: {
          // 属性名：指令的名称
          // 属性值：指令配置对象
          focus: {
            inserted (el, binding) {
              // binding 指令的信息对象
              // 其中有一个 value 就是指令的值
              el.style.width = binding.value.w + 'px'
              el.style.height = binding.value.h + 'px'
              el.focus()
            }
          }
        }
      })
    </script>
  </body>
</html>
```

补充：

- 指令的参数怎么接收 `inserted(el,binding){}`  binding就是指令信息



### 04-计算属性

> 目标：掌握通过计算属性来降低模板复杂度，提高模板的清晰度，可读性。

作用：

- 根据data当中的数据，经过一定的逻辑处理，得到一项新数据（计算属性）。
- 当data中的数据发生变化的时候，计算属性也会更新。
- 计算属性也是响应式数据，改变的时候也会驱动视图的更新。
- 当多次获取计算属性的时候，处理逻辑不会重新执行，因为有缓存。

定义：

- 语法：`new Vue({computed:{ 书写计算属性 }})`
- 书写计算属性：
  - `myMsg () { // 处理逻辑  return ‘处理后的数据’ }`
- 使用：和data中的数据一致

代码：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title></title>
  </head>
  <body>
    <div id="app">
      <h1>{{message}}</h1>
      <!-- 逻辑复杂，可读性差，违背初心（使用简单的js表达式） -->
      <h1>{{ message.split('').reverse().join('') }}</h1>
      <!-- 通过计算属性来优化 -->
      <hr>
      <h1>{{reverseMsg}}</h1>
    </div>
    <script src="./vue.js"></script>
    <script>
      const vm = new Vue({
        el: '#app',
        // 数据
        data: {
          message: 'hi vue'
        },
        methods: {},
        // vue的配置选项：computed
        // 计算属性
        computed: {
          // 属性名 计算属性的名称
          // 属性值 计算属性的处理函数
          // reverseMsg : function () {
          reverseMsg () {
            // 依赖data中的数据，进行一定的逻辑处理，得到一个新数据
            const newMsg = this.message.split('').reverse().join('')
            // 必须将新数据返回出去
            return newMsg
          }
          // reverseMsg 就是数据名称，在模板中使用data中数据一致
        }
      })
    </script>
  </body>
</html>
```



总结：

- 使用场景：当你依赖data里面的数据，经过较为复杂的逻辑处理，得到一个新的数据，此时可以使用计算属性来实现。



### 05-computed和methods区别

> 目的：指定计算属性和函数的区别，知道计算属性的有优点：缓存

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title></title>
  </head>
  <body>
    <div id="app">
      <h1>{{reverseFn()}}</h1>
      <h1>{{reverseFn()}}</h1>
      <h1>{{reverseFn()}}</h1>
      <h1>{{reverseMsg}}</h1>
      <h1>{{reverseMsg}}</h1>
      <h1>{{reverseMsg}}</h1>
    </div>
    <script src="./vue.js"></script>
    <script>
      const vm = new Vue({
        el: '#app',
        data: {
          message: 'hi vue'
        },
        methods: {
          // 反转字符的函数  模板中使用{{reverseFn()}}
          reverseFn () {
            console.log('reverseFn')
            return this.message.split('').reverse().join('')
          }
          // 每使用一次，会调用一次函数，重新执行内部逻辑，得到数据
        },
        computed: {
          // 反转字符计算属性 模板中使用{{reverseMsg}}
          reverseMsg () {
            console.log('reverseMsg')
            return this.message.split('').reverse().join('')
          }
          // 当多次调用的时候，之后执行一次逻辑，或走缓存
        }
      })
    </script>
  </body>
</html>
```

总结：

- 函数也可以实现数据逻辑处理得到新数据，但是多没使用一次执行一次逻辑，性能不优。
- 计算属性，在多次使用的时候，会走缓存，性能更好。
- 将来对数据进行（较为复杂）逻辑处理，建议使用计算属性。



### 06-案例-完善功能



### 07-工具-json-server



### 08-接口规则-RESTful



### 09-插件-axios



### 10-接口版案例-列表渲染



### 11-接口版案例-删除品牌



### 12-接口版案例-添加品牌



### 13-接口版案例-搜索品牌



