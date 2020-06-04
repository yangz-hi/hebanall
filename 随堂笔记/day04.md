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





### 03-★组件-体验





###  04-★组件-全局注册





###  05-★组件-局部注册





###  06-★组件-组件嵌套





###  07-★组件-命名规则





###  08-★组件-组件传值





###  09-SPA-特点介绍





###  10-SPA-前端路由





###  11-★vue-router-使用步骤



