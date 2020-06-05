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

> 目标：掌握to属性的各种用法

to属性可以写很多种形式的路径。

例如：

- `to="/article"`    静态的to属性
- `:to="{path:'/article',query:{id:101}}"`  === `to="/article?id=101"`
- `:to="{name:'article',params:{articleId:101}}"`  === `to="/article/101"`
  - 路由规则的名称  ` {name:'article', path: '/article/:articleId', component: Article}`

代码演示：

键值对

```html
      // 参数方式：键值对  /article?id=101
      const List = {
        data () {
          return {
            id: 10010
          }
        },
        template: `<div>
          <h1>列表</h1>
          <!--<router-link to="/article?id=101">键值对传参</router-link>--> 
          <!--<router-link :to="'/article?id='+id">键值对传参</router-link>-->
          <router-link :to="{path:'/article',query:{articleId:id}}">键值对传参</router-link>
        </div>`
      }
      const Item = {
        // $route  vue实例下的数据，代表路由信息对象，例如传参信息
        // 获取键值对传参  $route.query
        template: `<div>
          <h1>选项 {{$route.query.articleId}}</h1>
        </div>`
      }

      const router = new VueRouter({
        routes: [
          {path: '/', component: List},
          {path: '/article', component: Item}
        ]
      })
```

路径上

```html
// 参数方式：路径上  /article/101
       const List = {
        data () {
          return {
            id: 10010
          }
        },
        template: `<div>
          <h1>列表</h1>
          <!--<router-link to="/article/10010">路径上传参</router-link>-->
          <!--<router-link :to="'/article/'+id">路径上传参</router-link>-->
          <router-link :to="{name:'article',params:{id:id}}">路径上传参</router-link>
        </div>`
      }
      const Item = {
        template: `<div>
          <h1>选项 {{$route.params.id}}</h1>
        </div>`
      }

      const router = new VueRouter({
        routes: [
          {path: '/', component: List},
          {name:'article', path: '/article/:id', component: Item}
        ]
      })
```

总结：

- 怎么样通过to属性的对象写法，传递键值对参数   获取 `$route.query`
- 怎么样通过to属性的对象写法，传递路径上参数   获取 `$route.params`



### 04-★vue-router-编程式导航

> 目标：掌握不使用router-link进行跳转，通过js的方式进行跳转。

以前：

- 使用 A 标签进行跳转，如果没有 A 标签使用 location.href = '地址'

现在：

- 使用 router-link 进行跳转路由，如果没有 router-link 标签使用  `$router.push('静态地址'|对象)`

概念：

- 使用js的方式进行路由的跳转，就叫：编程式导航

区别 `$route` 和 `$router`  作用：

- `$route`  获取路由信息的（找数据）
- `$router`  获取路由实例的 ( 调方法 )
- 以上两个对象均可以通过 VUE实例去访问



场景：

- 异步的登录成功后，需要从登录跳转到首页。
- 这个时候就可以使用编程式导航，通过js进行跳转。



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
    <router-view></router-view>
  </div>
  <script src="./vue.js"></script>
  <script src="./vue-router.js"></script>
  <script>

    // 路由实例对象
    const router = new VueRouter({
      // 路由规则
      routes: [
        {
          path: '/login', component: {
            template: `<div>登录页面  <button @click="login()">登录</button></div>`,
            methods: {
              login () {
                // 进行登录
                // 登录成功
                // 跳转首页
                // $router 是在vue实例上的 路由对象，其中有一个函数push函数可以跳转
                this.$router.push('/home')
                // this.$router.push({path:'/home',query:{}})
                // this.$router.push({name:'home',params:{}})
                // push当中的对象形式 和to属性中的对象写法规则一致
              }
            }
          }
        },
        {
          path: '/home', name:'home', component: {
            template: `<div>首页页面  欢迎您！！！</div>`
          }
        }
      ]
    })

    const vm = new Vue({
      el: '#app',
      router
    })
  </script>
</body>

</html>
```



总结：

- 如何通过js进行路由跳转 `$router.push()`



### 05-★vue-router-重定向

> 目标：掌握前端路由的重定向功能。

回忆：

- 在nodejs中大家使用的是express框架，其中有 response 对象 ， 函数redirect函数进行重定向。

```JS
app.get('/login',(req,res)=>{
  // 返回一个页面
  // res.send('页面')  res.json()
  // 重定向函数  跳转到其他页面
  // res.redirect('/admin')
})
```

概念：

- 当你访问的是 a 路径的时候，跳转的却是 b 路径。

业务场景：

- 我们路由默认的地址是 / 路径
- 但是我的首页 /home 路径
- 那么在访问 / 路径的时候需要自动跳转到 /home 路径 （需要使用重定向）

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

      // 路由实例对象
    const router = new VueRouter({
      // 路由规则
      routes: [
        // 重定向
        { path: '/', redirect: '/home'  },
        {
          path: '/home', name:'home', component: {
            template: `<div>首页页面  欢迎您！！！</div>`
          }
        }
      ]
    })

      const vm = new Vue({
        el: '#app',
        router
      })
    </script>
  </body>
</html>
```

总结：

- 在vue-router是使用重定向功能 `{ path: '/', redirect: '/home'  },`



### 06-★vue-router-导航守卫

> 目标：能掌握路由中导航守卫的用法。

导航守卫概念：

- 在路由跳转的时候，可以介入。在跳转的过程中可以做其他的事情。

画图：

![1591328540880](docs/media/1591328540880.png)

业务需求：

- 后台管理系统的（所有页面），必须要登录后才能访问。
- 如果没有登录，直接访问后台的页面，拦截到登录页面即可。
- 如果已经登录，放行即可。

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

      const router = new VueRouter({
        routes: [
          {path: '/login', component: { template: '<div>登录页面</div>' }},
          {path: '/', component: { template: '<div>管理系统的首页页面</div>' }},
          {path: '/setting', component: { template: '<div>管理系统的设置页面</div>' }}
        ]
      })

      // 使用导航守卫，监听所有的路由跳转
      router.beforeEach((to,from,next)=>{
        // 这个回调函数在路由跳转的时候会执行
        // 模拟一下当前的登录状态
        const isLogin = false
        // 判断登录的状态
        //console.log(to)
        // to 是跳转的目标路由对象  to.path 目标路径
        // console.log(from)
        // from 是来自的目标路由对象  from.path 来自路径
        // next 是一个下一步执行函数：
        // next() 放行
        // next(路径) 拦截到哪里
        // 如果不是访问登录，且此时没有登录，那么跳转登录页面
        if (to.path!=='/login' && !isLogin) return next('login')
        // 如果其他情况一律放行
        next()
      })


      const vm = new Vue({
        el: '#app',
        router
      })
    </script>
  </body>
</html>
```



总结：

- 可以使用导航守卫实现访问权限的控制。



### 07-★vue-router-路由嵌套



### 08-vue-cli-介绍



### 09-vue-cli-安装



### 10-vue-cli-创建项目



### 11-vue-cli-目录结构



