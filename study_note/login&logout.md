# 登录与登出

1. 客户端与服务端不跨域
通过cookie在客户端记录状态
通过session在服务端记录状态
2. 客户端与服务端跨域
使用token维持状态

### token原理分析(登录过程分析)

1. 客户端输入用户名密码发送到服务端
2. 服务端验证通过后生成用户token并返回
3. 客户端存储该token
4. 客户端之后发送的请求自动携带该token
5. 服务端验证token是否通过

## 路由

src目录中的router目录代表路由组件,index.js是组件入口

在index.js中的routers数组中定义路由

例如
```js
{
    path: '/login',
    component: Login
}
```

以上代码就定义了一个指向 Login 组件的路由地址 '/login'

component指向了一个定义好的组件信息,需要使用import导入到index.js中

```js
import Login from '../components/Login.vue'
import Home from '../components/Home.vue'
```

### 重定向

```js
{
    path: '/',
    redirect: '/login'
}
```

以上两行代码代表访问 '/' 地址时,路由重定向到 '/login' 地址

### 导航守卫

设置导航守卫的原因: 用户在不带token的情况下请求需要带token验证的页面,是可以访问到的.因此需要用户在发起请求是,对token进行检测,不携带token访问不能访问权限页面.

```js
router.beforeEach((to, from, next) => {
  if (to.path === '/login') return next() // 用户直接访问login页面,直接放行
  const tokenStr = window.sessionStorage.getItem('token')
  if (!tokenStr) return next('/login') // 用户请求是未携带token,跳转至Login页面
  next() //用户请求携带token,放行
})
```

## 数据绑定

在Element-UI中,需在Form表单中设置一个实体对象,使用ref属性

```js
<el-form ref="loginFormRef"></el-form>
```

这样就可通过该属性获取Form表格中的username与password

```js
this.$refs.loginFormRef
```

### VUE中绑定数据

在Form表单中设置 :model

在Form的Item中设置 v-model

在JS中绑定数据

```html
<el-form ref="loginFormRef" :model="loginForm" :rules="loginFromRules" label-width="0px" class="login_form">
    <el-form-item prop="username">
        <el-input v-model="loginForm.username" prefix-icon="el-icon-user"></el-input>
    </el-form-item>
    <el-form-item prop="password">
        <el-input v-model="loginForm.password" type="password" prefix-icon="el-icon-view"></el-input>
    </el-form-item>
    <el-form-item class="btns">
        <el-button @click="login" type="primary">登录</el-button>
        <el-button @click="resetLoginForm" type="info">重置</el-button>
    </el-form-item>
</el-form>
```

```js
  data () {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      }
    }
  },
```

## 按钮事件与请求

使用@click=""绑定按钮事件,在js中定义该方法

```js
  methods: {
    resetLoginForm () {
      this.$refs.loginFormRef.resetFields()
    },
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败')
        this.$message.success('登录成功')
        window.sessionStorage.setItem('token',res.data.token)
        this.$router.push('/home')
      })
    }
  }
```

methods与data()平级