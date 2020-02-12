# [十四、功能优化](https://lxst9l.coding-pages.com/#/14-%E5%8A%9F%E8%83%BD%E4%BC%98%E5%8C%96?id=%e5%8d%81%e5%9b%9b%e3%80%81%e5%8a%9f%e8%83%bd%e4%bc%98%e5%8c%96)

## [处理 token 过期](https://lxst9l.coding-pages.com/#/14-%E5%8A%9F%E8%83%BD%E4%BC%98%E5%8C%96?id=%e5%a4%84%e7%90%86-token-%e8%bf%87%e6%9c%9f)

```js
// 响应拦截器
request.interceptors.response.use(
  // 响应成功进入第1个函数
  // 该函数的参数是响应对象
  function (response) {
    // Any status code that lie within the range of 2xx cause this function to trigger
    // Do something with response data
    return response
  },
  // 响应失败进入第2个函数，该函数的参数是错误对象
  async function (error) {
    // Any status codes that falls outside the range of 2xx cause this function to trigger
    // Do something with response error
    // 如果响应码是 401 ，则请求获取新的 token

    // 响应拦截器中的 error 就是那个响应的错误对象
    console.dir(error)
    if (error.response && error.response.status === 401) {
      // 校验是否有 refresh_token
      const user = store.state.user

      if (!user || !user.refresh_token) {
        // router.push('/login')
+        redirectLogin()

        // 代码不要往后执行了
        return
      }

      // 如果有refresh_token，则请求获取新的 token
      try {
        const res = await axios({
          method: 'PUT',
          url: 'http://ttapi.research.itcast.cn/app/v1_0/authorizations',
          headers: {
            Authorization: `Bearer ${user.refresh_token}`
          }
        })

        // 如果获取成功，则把新的 token 更新到容器中
        console.log('刷新 token  成功', res)
        store.commit('setUser', {
          token: res.data.data.token, // 最新获取的可用 token
          refresh_token: user.refresh_token // 还是原来的 refresh_token
        })

        // 把之前失败的用户请求继续发出去
        // config 是一个对象，其中包含本次失败请求相关的那些配置信息，例如 url、method 都有
        // return 把 request 的请求结果继续返回给发请求的具体位置
        return request(error.config)
      } catch (err) {
        // 如果获取失败，直接跳转 登录页
        console.log('请求刷线 token 失败', err)
        // router.push('/login')
+        redirectLogin()
      }
    }

    return Promise.reject(error)
  }
)

+ function redirectLogin () {
+   // router.currentRoute 当前路由对象，和你在组件中访问的 this.$route 是同一个东西
    // query 参数的数据格式就是：?key=value&key=value
+   router.push('/login?redirect=' + router.currentRoute.fullPath)
+ }

点击复制复制失败复制成功
```

## [登录成功跳转回原来页面](https://lxst9l.coding-pages.com/#/14-%E5%8A%9F%E8%83%BD%E4%BC%98%E5%8C%96?id=%e7%99%bb%e5%bd%95%e6%88%90%e5%8a%9f%e8%b7%b3%e8%bd%ac%e5%9b%9e%e5%8e%9f%e6%9d%a5%e9%a1%b5%e9%9d%a2)

首先在响应拦截器中：

```js
// 响应拦截器
request.interceptors.response.use(
  // 响应成功进入第1个函数
  // 该函数的参数是响应对象
  function (response) {
    // Any status code that lie within the range of 2xx cause this function to trigger
    // Do something with response data
    return response
  },
  // 响应失败进入第2个函数，该函数的参数是错误对象
  async function (error) {
    // Any status codes that falls outside the range of 2xx cause this function to trigger
    // Do something with response error
    // 如果响应码是 401 ，则请求获取新的 token

    // 响应拦截器中的 error 就是那个响应的错误对象
    console.dir(error)
    if (error.response && error.response.status === 401) {
      // 校验是否有 refresh_token
      const user = store.state.user

      if (!user || !user.refresh_token) {
        // router.push('/login')
+        redirectLogin()

        // 代码不要往后执行了
        return
      }

      // 如果有refresh_token，则请求获取新的 token
      try {
        const res = await axios({
          method: 'PUT',
          url: 'http://ttapi.research.itcast.cn/app/v1_0/authorizations',
          headers: {
            Authorization: `Bearer ${user.refresh_token}`
          }
        })

        // 如果获取成功，则把新的 token 更新到容器中
        console.log('刷新 token  成功', res)
        store.commit('setUser', {
          token: res.data.data.token, // 最新获取的可用 token
          refresh_token: user.refresh_token // 还是原来的 refresh_token
        })

        // 把之前失败的用户请求继续发出去
        // config 是一个对象，其中包含本次失败请求相关的那些配置信息，例如 url、method 都有
        // return 把 request 的请求结果继续返回给发请求的具体位置
        return request(error.config)
      } catch (err) {
        // 如果获取失败，直接跳转 登录页
        console.log('请求刷线 token 失败', err)
        // router.push('/login')
+        redirectLogin()
      }
    }

    return Promise.reject(error)
  }
)

+ function redirectLogin () {
+   // router.currentRoute 当前路由对象，和你在组件中访问的 this.$route 是同一个东西
    // query 参数的数据格式就是：?key=value&key=value
+   router.push('/login?redirect=' + router.currentRoute.fullPath)
+ }
点击复制复制失败复制成功
```

然后在登录成功以后：

```js
const redirect = this.$route.query.redirect || "/";
this.$router.push(redirect);点击复制复制失败复制成功
```

## [组件缓存](https://lxst9l.coding-pages.com/#/14-%E5%8A%9F%E8%83%BD%E4%BC%98%E5%8C%96?id=%e7%bb%84%e4%bb%b6%e7%bc%93%e5%ad%98)

默认组件在切换的时候会销毁，有了组件缓存就可以帮我们把组件缓存起来而不销毁。

> 参考文档：
>
> - [在动态组件上使用 `keep-alive`](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#%E5%9C%A8%E5%8A%A8%E6%80%81%E7%BB%84%E4%BB%B6%E4%B8%8A%E4%BD%BF%E7%94%A8-keep-alive)

```html
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  动态组件
</keep-alive>点击复制复制失败复制成功
```

> 注意：`<keep-alive>` 要求被切换到的组件都有自己的名字，不论是通过组件的 `name` 选项还是局部/全局注册。

1、在 App.vue 对根路由组件启用组件缓存

```html
<keep-alive>
  <router-view />
</keep-alive>点击复制复制失败复制成功
```

2、在 `views/tabbar/index.vue` 对子路由也启用组件缓存

```html
<keep-alive>
  <router-view />
</keep-alive>点击复制复制失败复制成功
```

### [禁用组件缓存](https://lxst9l.coding-pages.com/#/14-%E5%8A%9F%E8%83%BD%E4%BC%98%E5%8C%96?id=%e7%a6%81%e7%94%a8%e7%bb%84%e4%bb%b6%e7%bc%93%e5%ad%98)

> 参考文档：
>
> - <https://cn.vuejs.org/v2/api/#keep-alive>

手动销毁的方式需要修改组件内部的代码，这里介绍一种更灵活的方式来配置哪些组件缓存，哪些组件不缓存。

`include` 和 `exclude` 属性允许组件有条件地缓存。二者都可以用逗号分隔字符串、正则表达式或一个数组来表示：

```html
<!-- 逗号分隔字符串 -->
<keep-alive include="a,b">
  <component :is="view"></component>
</keep-alive>

<!-- 正则表达式 (使用 `v-bind`) -->
<keep-alive :include="/a|b/">
  <component :is="view"></component>
</keep-alive>

<!-- 数组 (使用 `v-bind`) -->
<keep-alive :include="['a', 'b']">
  <component :is="view"></component>
</keep-alive>点击复制复制失败复制成功
```

匹配首先检查组件自身的 `name` 选项，如果 `name` 选项不可用，则匹配它的局部注册名称 (父组件 `components` 选项的键值)。匿名组件不能被匹配。

总结：

- 根据需要设置需要缓存的组件
- 缓存需要占用更大的内容，当缓存组件过多的时候容易造成页面卡顿
- 把不需要缓存的组件排除缓存

### [动态缓存](https://lxst9l.coding-pages.com/#/14-%E5%8A%9F%E8%83%BD%E4%BC%98%E5%8C%96?id=%e5%8a%a8%e6%80%81%e7%bc%93%e5%ad%98)