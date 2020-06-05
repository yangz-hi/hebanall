## VUE基础-day05



### 01-每日回顾

**组件**

- 概念：针对界面功能（HTML,CSS,JS）的封装，相互独立更好维护，提高复用度。
- 注册
  - 全局 `Vue.component(组件名称,组件配置对象)`
  - 局部 `new Vue({ components:{组件名称:组件配置对象} })`
- 命名：注册的时候 `btn-add`  `BtnAdd`  使用的时候 `btn-add`
- 传值（重要）
  - 父传子：
    - 使用子组件的时候，绑定属性   `<com-child :msg="父组件数据">`
    - 定义子组件的时候，使用props来接收属性值  `props:['msg']`   仅读
  - 子传父：
    - 使用子组件的时候，绑定自定义事件  `<com-child @abcd="$event就是触发自定义事件的传参">`
    - 定义子组件的时候，触发自定义事件 `子组件的vue实例.$emit('abcd',子组件的数据)`

例如：非父子传值，单文件组件  后面会使用讲解。



**前端路由**

- 概念：地址栏改变但是不刷新页面（不会请求HTML），根据地址栏的不同更新界面内容（切换业务场景）
- 改地址：
  - location.hash       hash实现模式
  - history.pushState()    history实现模式
- 理解hash实现的原理
- 知道vue-router插件的使用，实现的是：地址  映射  组件。
- 使用步骤：
  - 引入
  - 创建组件配置对象
  - 配置路由规则
  - 初始化路由
  - 挂载到vue根实例
  - 使用router-link代理a标签
  - 使用router-view指定组件渲染位置





### 02-★vue-router-动态路由

> 目标：能掌握动态路由的规则使用。

概念：不同的路由地址，需要指向同一个组件，这样的路由规则的实现，就叫动态路由。

换一句话：你定义的路由规则，需要匹配到不同的地址。

业务场景：

![1591321317470](docs/media/1591321317470.png)

代码演示：

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
      <router-view></router-view>
    </div>
    <script src="./vue.js"></script>
    <script src="./vue-router.js"></script>
    <script>

      // 文章列表组件配置对象
      const List = {
        template:`<div>
          <h1>文章列表</h1>
          <router-link to="/article/101">文章1</router-link>  
          <router-link to="/article/102">文章2</router-link>  
          <router-link to="/article/103">文章3</router-link>  
        </div>`
      }
      // 文章详情组件配置对象
      const Article = {
        // $route 是vue实例上的数据对象,和data中数据的使用时一样的
        // $route.params 获取路径传参的（动态路由传参的）这是一个对象
        // $route.params 对象中的数据：键 根据path中的参数名称  值 地址栏上的参数数据了
        template: `<div><h1>文章详情</h1> {{$route.params.articleId}}</div>`
      }

      // 路由规则
      const routes = [
        {path: '/', component: List},
        {path: '/article/:articleId', component: Article}
      ]

      // 初始化
      const router = new VueRouter({ routes })

      const vm = new Vue({
        el: '#app',
        // 挂载
        router
      })
    </script>
  </body>
</html>
```

总结：

- 规则 `{path: '/article/:articleId', component: Article}`
- 参数 `$route.params.articleId`



### 03-★vue-router-属性to



### 04-★vue-router-编程式导航



### 05-★vue-router-重定向



### 06-★vue-router-导航守卫



### 07-★vue-router-路由嵌套



### 08-vue-cli-介绍



### 09-vue-cli-安装



### 10-vue-cli-创建项目



### 11-vue-cli-目录结构



