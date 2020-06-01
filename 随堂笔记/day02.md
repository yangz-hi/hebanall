## VUE基础-day02

### 01-每日回顾

vue是什么

- 它就js框架，有自己的开发规则。

vue做什么

- 适合做SPA类型的系统

vue核心

- 数据驱动视图，MVVM模式，组件化开发

vue的配置选项

- el   指定vue实例管理的视图容器
- data   声明响应式数据
- methods   定义函数

术语

- 插值表达式 `{{}}`
- 指令  `v-*` 的属性

指令

- v-text    更新替换标签内容，文本
- v-html    更新替换标签内容，html
- v-show  显示隐藏
- v-if  移除创建
- v-on  绑定事件
- v-bind  绑定属性
- v-for  进行遍历



![1590974037720](docs/media/1590974037720.png)

准备静态页面：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>案例</title>
  <link rel="stylesheet" href="./bootstrap.min.css">
</head>
<body>
  <div id="app" class="container" style="padding-top: 100px;">
    <table class="table table-bordered">
      <thead>
        <tr>
          <th>编号</th>
          <th>品牌名称</th>
          <th>创建时间</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>1</td>
          <td>宝马</td>
          <td>2010-10-10 10:10:10</td>
          <td>
            <a href="#">删除</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <script src="./vue.js"></script>
</body>
</html>
```



### 02-案例-渲染列表

实现的大致步骤：

- 准备表格所需数据
- 使用v-for指令进行遍历 （tbody--->tr）

落地的代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>案例</title>
  <link rel="stylesheet" href="./bootstrap.min.css">
</head>
<body>
  <div id="app" class="container" style="padding-top: 100px;">
    <table class="table table-bordered">
      <thead>
        <tr>
          <th>编号</th>
          <th>品牌名称</th>
          <th>创建时间</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <!-- v-for="每个选项变量名称 in  data数据中的数组" -->
        <tr v-for="brand in brandList" :key="brand.id">
          <td>{{brand.id}}</td>
          <td>{{brand.brandName}}</td>
          <td>{{brand.createTime}}</td>
          <td>
            <a href="#">删除</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <script src="./vue.js"></script>
  <script>
    // 初始化
    const vm = new Vue({
      el: '#app',
      data: {
        // 品牌列表数据
        brandList: [
          {id:1,brandName:'宝马',createTime:'2020-06-01 12:12:12'},
          {id:2,brandName:'宝骏',createTime:'2021-06-01 12:12:12'},
          {id:3,brandName:'保时捷',createTime:'2020-06-01 12:12:12'},
          {id:4,brandName:'奥迪',createTime:'2025-06-01 12:12:12'}
        ]
      }
    })
  </script>
</body>
</html>
```





### 03-案例-完成删除





### 04-指令-v-bind绑定class





### 05-指令-v-bind绑定style





### 06-指令-v-model





### 07-指令-v-cloak





### 08-指令-v-once





###  09-案例-添加品牌





### 10-案例-梳理其它功能





### 11-vue定义过滤器





### 12-vue操作dom





### 13-vue自定义指令





