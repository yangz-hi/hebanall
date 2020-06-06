## VUE基础-day05

### 01-每日回顾

vue-router

- 动态路由：当你的一个路由规则需要匹配到多个不同的路由路径的时候
  - `{path:'/article/:id', name:'article', component:'组件配置对象'}`
- router-link组件的to属性写法（路由传参）
  - /user?id=100&name=tom  键值对传参
    - `:to="{path:'/user',query:{id:100,name:'tom'}}"`
  - /user/:id/:name  ===  /user/100/tom   路径传参
    - `:to="{name:'user',params:{id:100,name:'tom'}}"`

- 编程式导航：在js代码中进行路由的跳转
  - 在vue-router的实例中，有一个函数push可以进行跳转
  - 在vue实例中有一个属性`$router`就是路由实例，`this.$router.push(地址)`
  - 还可以传对象，规则和to属性的规则是一致的。
- 重定向：当你访问 地址A的时候，需要自动跳转的地址B 
  - `{path:'/', redirect: '/home'}`
- 导航守卫：当你需要在路由跳转的时候做一些事件
  - 例如：访问权限的控制
  - 路由实例提供了一个函数  beforeEach
  - `router.beforeEach((to,from,next)=>{ //处理逻辑 })`
    - next()   放行   next('/login')  拦截跳转
- 路由嵌套：多级页面的局部更新
  - router-view组件需要嵌套
  - 路由规则也需要呈现出嵌套的关系，规则对象中：children属性

vue-cli

- 安装 `npm i -g @vue/cli`
- 创建项目  `vue create 项目名称`
- 目前使用默认的创建方式 default
- 启动项目：在项目的根目录下执行 `npm run serve`

通过main.js找项目依赖的所有资源---->把这些资源打包在一起----->注入自动添加到index.html页面---->在localhost:8080的服务预览

main.js称为：**入口文件**



代码：$mount() 和 el 作用一致，指定跟根容器

代码：render函数，把App.vue组件渲染在，渲染在根容器，替换式渲染。

- 就是把App.vue中的内容当中根容器内容。
- 以后其他组件都是在App.vue组件中使用。



### 02-ES6模块化

> 目标：掌握ES6模块化开发的，导入和导出。

前置提醒：ES6 commonJS 模块化的实现不能再前端浏览器中使用，浏览器不支持。

在vue-cli中，会把ES6的模块化实现，通过vue-cli实现成浏览器能够支持的模块化代码。



第一种写法：默认导出和默认导入

```js
// 使用默认导出
// 只能默认导出一次
// 可以导出任何东西
const str = '默认导出的数据'
export default str
```

```js
// ES6默认导入
// import 变量名称(接收默认导出的内容)  from  '路径|包名称'
import myStr from './myModule'
console.log(myStr)
```

第二种写法：指定成员导出和指定成员导入（按需导出和按需导入）

```js
// a 属于模块内部变量
const a = 10
// 按需导出 b 成员
export const b = 20
// 按需导出 c 成员
export const c = a + b 
```

```js
// ES6按需导入写法
// 1. 如果接收所有的按需导出的成员
// import * as all from './myModule'
// console.log(all) === {b:20,c:30}
// 2. 只想接收其中某一个成员
// 强调：这里不是解构赋值，这是模块化的语法
import { b } from './myModule'
import { b as otherB } from './myModule'
console.log(b)
console.log(otherB)
// as 是取别名的意思
```

混合写法

```js
// a 属于模块内部变量
const a = 10
// 按需导出 b 成员
export const b = 20
// 按需导出 c 成员
export const c = a + b 

export default '默认的'
```

```js
// 混合写法
// 得到默认成员 得c成员  
import str, { c } from './myModule'
console.log(str,c)
```





### 03-单文件组件





### 04-hero案例-项目介绍

 ![img](https://gitee.com/zhoushugang/hm-vue-base-96/raw/master/%E9%9A%8F%E5%A0%82%E7%AC%94%E8%AE%B0/docs/media/1587095356562.png) 





### 05-hero案例-静态界面





### 06-hero案例-公用组件





### 07-hero案例-实现路由





### 08-hero案例-路由激活样式





###  09-hero案例-提取路由模块





###  10-hero案例-列表组件





### 11-hero案例-时间过滤





### 12-hero案例-删除功能