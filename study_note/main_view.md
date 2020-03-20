# 绘制主页

## 页面布局

主要使用css与elementUI组件去绘制页面框架

## axios拦截器进行token验证

在main.js中拦截请求,获取sessionStorage中的token值

## 动态绑定左侧导航菜单

登录成功后,主页发起请求请求menu列表

在一级菜单中,使用v-for标签循环menulist列表

## 侧边栏折叠与展开

添加折叠按钮,通过按钮动态绑定侧边栏的isCollapse值,改变控件的collapse属性

## 侧边栏路由设置

在内容展示主页设置路由占位符,在循环的菜单中使用 :index 绑定路由地址,最后在router模块下的Index.js中注册各个模块的组件即可

