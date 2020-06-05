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





### 02-★vue-router-动态路由



### 03-★vue-router-属性to



### 04-★vue-router-编程式导航



### 05-★vue-router-重定向



### 06-★vue-router-导航守卫



### 07-★vue-router-路由嵌套



### 08-vue-cli-介绍



### 09-vue-cli-安装



### 10-vue-cli-创建项目



### 11-vue-cli-目录结构



