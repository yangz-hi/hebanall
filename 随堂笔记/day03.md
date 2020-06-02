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



### 04-计算属性



### 05-computed和methods区别



### 06-案例-完善功能



### 07-工具-json-server



### 08-接口规则-RESTful



### 09-插件-axios



### 10-接口版案例-列表渲染



### 11-接口版案例-删除品牌



### 12-接口版案例-添加品牌



### 13-接口版案例-搜索品牌



