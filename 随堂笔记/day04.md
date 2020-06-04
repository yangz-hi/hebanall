## VUE基础-day04



### 01-每日回顾

- 计算属性(为了得到数据)

  - 场景：你想依赖data里面的数据，经过逻辑处理，得到一项行新的数据
  - 语法：`new Vue({computed:{'计算属性名称':处理函数(){ 返回的就是新数据 }}})`

- 自定义指令

  - 场景：你想扩展标签的原有的功能
  - 全局：`Vue.directive('指令名称',指令配置对象{ inserted(){ 扩展标签功能 } })`
  - 局部：`new Vue({ directives:{ '指令名称':指令配置对象{ inserted(){ 扩展标签功能 }   } })`

- 侦听器（监听数据变化）

  - 场景：当你数据变化的时候，需要做异步操作或开销较大操作的时候。
  - 补充：其实只要想监听数据的变化去做其他事情，都可以使用侦听器。
  - 语法： `new Vue({watch:{ 'data中数据的字段名称',处理函数 }})`

- 接口开发

  - json-server

    - 模拟接口的一个基于nodejs命令行工具
    - 安装后在 json 中定义接口相关的数据
    - 启动接口服务即可

  - restful

    - get    查询
    - post   添加
    - delete  删除
    - put   全部修改
    - patch   局部修改

  - axios

    - axios.get()   键值对传参   get的第二个参数 `{params:{//对象数据}}`
    - axios.post()   请求体传参   post的第二个参数 {//请求体对象}
    - axios.delete()
    - axios.put()
    - axios.patch()
    - 综合的请求函数  axios({ // 请求配置对象 })
      - method  请求类型
      - url 请求地址
      - params  键值对参数对象
      - data  请求体参数对象
      - headers 请求头参数对象

    ```js
    axios({
      method: 'post',
      url: '接口地址',
      data: { /*请求体传参*/ },
      params: {  /*请求体传参*/ },
      headers: {
        'Content-Type': 'application/json'
      }
    })
    ```

- 接口-品牌列表案例

  - 列表
  - 删除
  - 添加
  - 搜索



###  02-★组件-概念

> 目标：知道组件的一些概念，组件优点

![1591234310017](docs/media/1591234310017.png)

概念：

- JS模块：对一个逻辑的封装，说白了就是一个js文件（模块）

- vue组件：把网页分成若干块，针对界面功能的封装，一个组件包含（HTML,CSS,JS）
- vue组件也是一个vue实例，在定义组件的时候可以使用`vue的配置选项` ,例如你可以使用 data method ... 这样的配置选项，有一个选项不能使用 `el` 

优点：

- 组件与组件之间是相互独立的，拥有自己的数据data、函数methods...，可维护性提高。
- 可复用





### 03-★组件-体验

> 目的：体验组件的好处。

- 三个一致的功能，点击一个按钮可以累加10，显示累加的结果。

不使用组件：

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
      <!-- 累加10的功能，三个一样的 -->
      <div>结果：{{count}} <button @click="add()">累加10</button></div>
      <div>结果：{{count1}} <button @click="add1()">累加10</button></div>
      <div>结果：{{count2}} <button @click="add2()">累加10</button></div>
    </div>
    <script src="./vue.js"></script>
    <script>
      const vm = new Vue({
        el: '#app',
        data: {
          count: 0,
          count1: 0,
          count2: 0
        },
        methods: {
          add () {
            this.count = this.count + 10
          },
          add1 () {
            this.count1 = this.count1 + 10
          },
          add2 () {
            this.count2 = this.count2 + 10
          }
        }
      })
    </script>
  </body>
</html>
```

使用组件：

```html
    <div id="app">
      <btn-add></btn-add>
      <btn-add></btn-add>
      <btn-add></btn-add>
    </div>
    <script src="./vue.js"></script>
    <script>

      // 定义组件
      Vue.component('btn-add',{
        template: `<div>结果：{{count}} <button @click="add()">累加10</button></div>`,
        data () {
          return {
            count: 0
          }
        },
        methods: {
          add () {
            this.count = this.count + 10
          }
        }
      })

      const vm = new Vue({
        el: '#app'
      })
    </script>
```



###  04-★组件-全局注册

> 目标：掌握全局组件注册的写法

全局注册的意思：在任何vue实例下都可以使用。

具体语法：

- 定义 `Vue.component(组件名称,组件配置对象)`

- 组件配置对象 和  vue实例配置对象  是几乎一样，没有el选项。

- template选项，声明组件结构的（组件自己的模板）。有且只有一个根标签。必须有这个选项

- data选项，必须指定一个函数，函数的返回对象是用来声明数据的。

使用组件：

- 在vue实例管理的视图中#app，把 组件名字 当中一个标签来使用，组件的名字不能用原生的标签来命名。
- 例如：div span a  article  footer header section ... 原生标签。

演示代码：

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
      <btn-add></btn-add>
    </div>
    <hr>
    <div id="app2">
      <btn-add></btn-add>
    </div>
    <script src="./vue.js"></script>
    <script>

      // 全局注册
      Vue.component('btn-add',{
        // 这是一个字符串，模板字符的原因是：可换html的结构清晰些
        // 细节：需要有且只有一个根标签
        template: `<div>
            结果：{{count}} 
            <button @click="add()">累加</button>
          </div>`,
        // 在组件中data必须是一个函数，返回的对象中来声明数据
        data () {
          return {
            count: 0
          }
        },
        methods: {
          add () {
            this.count ++
          }
        }
      })

      const vm = new Vue({
        el: '#app',
      })
      const vm2 = new Vue({
        el: '#app2',
      })
    </script>
  </body>
</html>
```






###  05-★组件-局部注册

> 目标：掌握局部组件注册的写法

局部注册的意思：仅仅在注册组件的当前vue实例管理的视图中才可以使用。

具体语法：

- 语法：`new Vue({ components:{组件的名字:组件配置对象} })`
- 组件配置对象 和  vue实例配置对象  是几乎一样，没有el选项。
- template选项，声明组件结构的（组件自己的模板）。有且只有一个根标签。必须有这个选项

- data选项，必须指定一个函数，函数的返回对象是用来声明数据的。

使用组件：

- 在vue实例管理的视图中#app，把 组件名字 当中一个标签来使用，组件的名字不能用原生的标签来命名。
- 例如：div span a  article  footer header section ... 原生标签。

演示代码：

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
      <btn-add></btn-add>
    </div>
    <div id="app2">
      <btn-add></btn-add>
    </div>
    <script src="./vue.js"></script>
    <script>
      const vm = new Vue({
        el: '#app',
        // 局部注册组件
        components: {
          'btn-add': {
            template: '<div>结果：{{count}} <button @click="add()">累加</button></div>',
            data () {
              return {
                count: 0
              }
            },
            methods: {
              add () {
                this.count ++
              }
            }
          }
        }
      })
      const vm2 = new Vue({
        el: '#app2'
      })
    </script>
  </body>
</html>
```





###  06-★组件-组件嵌套

> 目标：指定组件时如何嵌套使用

![1591239483136](docs/media/1591239483136.png)

演示代码：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title></title>
  </head>
  <body>
    <!-- <div id="app">
      <com-parent></com-parent>
    </div>
    <script src="./vue.js"></script>
    <script>
      // 嵌套：在父组件中使用子组件
      Vue.component('com-parent',{
        template: '<div>我是父组件 <com-child></com-child></div>'
      })
      Vue.component('com-child',{
        template: '<div>我是子组件</div>'
      })
      const vm = new Vue({
        el: '#app',
      })
    </script> -->
    <div id="app">
      <com-parent></com-parent>
    </div>
    <script src="./vue.js"></script>
    <script>
      // 嵌套：在父组件中使用子组件
      // 局部：局部定义的组件仅仅只能在当前注册的vue实例管理的模板（视图）中使用。
      const vm = new Vue({
        el: '#app',
        components: {
          'com-parent': {
            template: '<div>我是父组件 <com-child></com-child></div>',
            components: {
              'com-child': {
                template: '<div>我是子组件</div>'
              },
            }
          }
        }
      })
    </script>
  </body>
</html>
```



注意：局部定义的组件仅仅只能在当前注册的vue实例管理的模板（视图）中使用。





###  07-★组件-命名规则

> 目的：掌握组件名称的规范，为将来项目中使用组件做个铺垫。

有两种规范写法

- 写法一：
  - 例如 `com-parent`  `btn-add`  `com-child`   
  - 规则 小写单词加中线分割，多个单词拼接
  - 使用  按照组件的名称当做标签使用即可。
- 写法二：
  - 例如 `ComParent`  `BtnAdd`  `ComChild`
  - 规则  单词的首字母大写，多个单词拼接
  - 使用 
    - 可以转成成  小写单词加中线的写法  当做标签使用即可
    - 直接用组件的名称 当做标签使用即可
      - 只能在 template 指定的视图中使用（只能在组件中使用）
      - 在根vue实例下是不能使用的。
- 总结：**使用的时候一律使用  小写单词加中线  这种方式，万无一失。**

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
      <!-- <btn-add></btn-add> -->
      <!-- 以下写法  根容器 #app 容器下不可以使用，它是通过vue实例管理的 -->
      <!-- <BtnAdd></BtnAdd> -->
      <my-com></my-com>
    </div>
    <script src="./vue.js"></script>
    <script>

      // 写法一 
      // Vue.component('btn-add',{
      //   template: '<div>我是一个btn-add组件</div>'
      // })
      // 写法二
      Vue.component('BtnAdd',{
        template: '<div>我是一个btn-add组件</div>'
      })

      Vue.component('my-com',{
        template: '<div>我是一个my-com组件 <BtnAdd></BtnAdd></div>'
      })

      const vm = new Vue({
        el: '#app'
      })
    </script>
  </body>
</html>
```







###  08-★组件-组件传值





###  09-SPA-特点介绍





###  10-SPA-前端路由





###  11-★vue-router-使用步骤



