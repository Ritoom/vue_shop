# Vue基础知识

## 声明式渲染

使用模板语法声明数据,例如

```vue
<div id="app">
  {{ message }}
</div>

var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

## VUE生命周期

新建实例
   ↓ 开始初始化
beforeCreate
   ↓ 初始化校验
created
   ↓
是否指定元素选项
   ↓
是否指定模板选项
   ↓
将template编译到render函数中/将el外部的html作为template编译
   ↓
beforeMount
   ↓ 
mounted
   ↓ 挂载完毕
beforeUpdate
   ↓ 虚拟DOM重新渲染并应用更新
updated
   ↓
beforeDestroy
   ↓ 解除绑定销毁子组件以及事件监听器
destroyed

