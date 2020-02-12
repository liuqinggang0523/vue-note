# [三、Token 处理](https://lxst9l.coding-pages.com/#/03-Token%E5%A4%84%E7%90%86?id=%e4%b8%89%e3%80%81token-%e5%a4%84%e7%90%86)

## [Token 介绍](https://lxst9l.coding-pages.com/#/03-Token%E5%A4%84%E7%90%86?id=token-%e4%bb%8b%e7%bb%8d)

### [HTTP 是无状态的](https://lxst9l.coding-pages.com/#/03-Token%E5%A4%84%E7%90%86?id=http-%e6%98%af%e6%97%a0%e7%8a%b6%e6%80%81%e7%9a%84)

![HTTP 是无状态的](https://lxst9l.coding-pages.com/assets/v2-dde997503ed9d450e2f39042d53d4307_hd.png)

### [基于 Cookie 的状态保持](https://lxst9l.coding-pages.com/#/03-Token%E5%A4%84%E7%90%86?id=%e5%9f%ba%e4%ba%8e-cookie-%e7%9a%84%e7%8a%b6%e6%80%81%e4%bf%9d%e6%8c%81)

![img](https://lxst9l.coding-pages.com/assets/v2-85622297a93f493c891ffb90b67fd5e0_hd.png)

![img](https://lxst9l.coding-pages.com/assets/v2-1f49734871c5e2da2d264d28ac310a65_hd.png)

Cookie 在客户端本地，用户可以操作它，所以不适合存储对安全性要求比较高的数据。

### [基于 Session 的状态保持](https://lxst9l.coding-pages.com/#/03-Token%E5%A4%84%E7%90%86?id=%e5%9f%ba%e4%ba%8e-session-%e7%9a%84%e7%8a%b6%e6%80%81%e4%bf%9d%e6%8c%81)





Session 不是新东西，它是基于 Cookie 的一个服务端技术，用一个生活中的例子来解释就好比我们逛超市存包。

- 你：客户端
- 超市：服务器
- 存物柜：服务端的 Session Store
  - 存储用户的状态等对安全性比较高的数据
- 你拿的那个凭据：访问服务端 Session 数据的凭证
  - 凭证是服务端给你的
  - 凭证是唯一的

Session 其实就是把对安全性要求比较高的状态数据放到了服务端，把访问数据的钥匙放到了用户的本地（Cookie）。

用户的登录状态肯定是安全性要求比较高的，所以通常会使用 Session 来存储用户的登录状态。

![img](https://lxst9l.coding-pages.com/assets/v2-0b02fa4a73a8072eb03cdf78270235e1_hd.png)

1、用户向服务器发送用户名和密码。

2、服务器验证通过后，在当前对话（session）里面保存相关数据，比如用户角色、登录时间等等。

3、服务器向用户返回一个 session_id，写入用户的 Cookie。

4、用户随后的每一次请求，都会通过 Cookie，将 session_id 传回服务器。

5、服务器收到 session_id，找到前期保存的数据，由此得知用户的身份。

这种模式的问题在于，扩展性（scaling）不好。单机当然没有问题，如果是服务器集群，或者是跨域的服务导向架构，就要求 session 数据共享，每台服务器都能够读取 session。

举例来说，A 网站和 B 网站是同一家公司的关联服务。现在要求，用户只要在其中一个网站登录，再访问另一个网站就会自动登录，请问怎么实现？

一种解决方案是 session 数据持久化，写入数据库或别的持久层。各种服务收到请求后，都向持久层请求数据。这种方案的优点是架构清晰，缺点是工程量比较大。另外，持久层万一挂了，就会单点失败。

另一种方案是服务器索性不保存 session 数据了，所有数据都保存在客户端，每次请求都发回服务器。

### [基于 Token（令牌、凭据、凭证） 的状态保持](https://lxst9l.coding-pages.com/#/03-Token%E5%A4%84%E7%90%86?id=%e5%9f%ba%e4%ba%8e-token%ef%bc%88%e4%bb%a4%e7%89%8c%e3%80%81%e5%87%ad%e6%8d%ae%e3%80%81%e5%87%ad%e8%af%81%ef%bc%89-%e7%9a%84%e7%8a%b6%e6%80%81%e4%bf%9d%e6%8c%81)

基于 Token 的的思路是，服务器认证以后，生成一个加密数据（令牌），发回给用户，数据内容就像下面这样。

```json
{
  "姓名": "张三",
  "角色": "管理员",
  "到期时间": "2018年7月1日0点0分"
}点击复制复制失败复制成功
```

以后，用户与服务端通信的时候，都要发回这个加密数据（令牌）。服务器通过解密获取明文数据就知道了用户身份。

服务器就不保存任何 session 数据了，也就是说，服务器变成无状态了，从而比较容易实现扩展。

基于 `token` 的鉴权机制类似于 HTTP 协议也是无状态的，它不需要在服务端去保留用户的认证信息或者会话信息。这就意味着基于 `token` 认证机制的应用不需要去考虑用户在哪一台服务器登录了，这就为应用的扩展提供了便利。

![img](https://lxst9l.coding-pages.com/assets/ezgif-5-ae2b96ba00b5.png)

1、用户使用用户名密码来请求服务器

2、服务器进行验证用户的信息

3、服务器通过验证发送给用户一个 token

4、客户端存储 token，并在每次请求时附送上这个 token 值

5、服务端验证 token 值，并返回数据

以上就是基于 Token 认证的思路。

### [JWT（JSON Web Token）](https://lxst9l.coding-pages.com/#/03-Token%E5%A4%84%E7%90%86?id=jwt%ef%bc%88json-web-token%ef%bc%89)

有了基于 Token 认证的思路了，该如何实现呢？例如

- 如何生成 Token？
- 如何加密？
- 如何解密？
- 如何规定数据格式？

如果大家都各自搞一套，太麻烦。所以社区中制定了一个具体实现：[JSON Web Token](https://jwt.io/)。

- Java
- PHP
- Ruby
- Python
- Node.js
- 。。。。

该方案主要是对 JSON 格式数据进行加解密以及数据格式的规范，方便前后端交互。

![image-20200108012948915](https://lxst9l.coding-pages.com/assets/image-20200108012948915.png)

Json web token (JWT), 是为了在网络应用环境间传递声明而执行的一种基于JSON的开放标准（[(RFC 7519](https://link.jianshu.com/?t=https://tools.ietf.org/html/rfc7519)).该token被设计为紧凑且安全的，特别适用于分布式站点的单点登录（SSO）场景。JWT的声明一般被用来在身份提供者和服务提供者间传递被认证的用户身份信息，以便于从资源服务器获取资源，也可以增加一些额外的其它业务逻辑所必须的声明信息，该token也可直接被用于认证，也可被加密。

JWT 的一些特点如下：

（1）JWT 默认是不加密，但也是可以加密的。生成原始 Token 以后，可以用密钥再加密一次。

（2）JWT 不加密的情况下，不能将秘密数据写入 JWT。

（3）JWT 不仅可以用于认证，也可以用于交换信息。有效使用 JWT，可以降低服务器查询数据库的次数。

（4）JWT 的最大缺点是，由于服务器不保存 session 状态，因此无法在使用过程中废止某个 token，或者更改 token 的权限。也就是说，一旦 JWT 签发了，在到期之前就会始终有效，除非服务器部署额外的逻辑。

（5）JWT 本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。为了减少盗用，JWT 的有效期应该设置得比较短。对于一些比较重要的权限，使用时应该再次对用户进行认证。

（6）为了减少盗用，JWT 不应该使用 HTTP 协议明码传输，要使用 HTTPS 协议传输。

> 扩展阅读
>
> - <http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html>

## [存储 Token](https://lxst9l.coding-pages.com/#/03-Token%E5%A4%84%E7%90%86?id=%e5%ad%98%e5%82%a8-token)

![image-20200109192157006](https://lxst9l.coding-pages.com/assets/image-20200109192157006.png)

我们项目中哪里需要使用呢？

- 请求需要权限的接口
- 底部导航栏中也需要使用判断登录状态给出不同的提示
  - 已登录：我的
  - 未登录：未登录
- 我的页面也要使用，判断登录状态给出不同的页面展示
- ...

往哪儿存？

- 本地存储（持久化）
  - 获取麻烦
  - 不是响应式
- Vuex 容器（推荐）
  - 获取方便
  - 响应式的

思路：

- 登录成功，将 token 存储到本地存储
- 为了持久化，还需要把 token 放到本地存储

下面是具体实现。

一、使用 Vuex 容器存储 token

1、在 `src/store/index.js` 中

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    // 登录用户，一个对象，包含 token 信息
+    user: null
  },
  mutations: {
+    setUser (state, data) {
+      state.user = data
+    }
  },
  actions: {
  },
  modules: {
  }
})
点击复制复制失败复制成功
```

2、登录成功以后将后端返回的 token 相关数据存储到容器中

```js
async onLogin () {
  // const loginToast = this.$toast.loading({
  this.$toast.loading({
    duration: 0, // 持续时间，0表示持续展示不停止
    forbidClick: true, // 是否禁止背景点击
    message: '登录中...' // 提示消息
  })

  try {
    const res = await login(this.user)

    // res.data.data => { token: 'xxx', refresh_token: 'xxx' }
+    this.$store.commit('setUser', res.data.data)

    // 提示 success 或者 fail 的时候，会先把其它的 toast 先清除
    this.$toast.success('登录成功')
  } catch (err) {
    console.log('登录失败', err)
    this.$toast.fail('登录失败，手机号或验证码错误')
  }

  // 停止 loading，它会把当前页面中所有的 toast 都给清除
  // loginToast.clear()
}点击复制复制失败复制成功
```

完了，在浏览器中点击登录，通过 VueDevTools 查看数据是否能正常的存储到 Vuex 容器中。

二、持久化 token 存储

Vuex 容器中的数据只是为了方便在其他任何地方能获取登录状态数据，但是页面刷新还是会丢失数据状态，所以我们还要把数据进行持久化中以防止页面刷新丢失状态的问题。

前端持久化常见的方式就是：

- 本地存储
- Cookie

这里我们以使用本地存储持久化用户状态为例。

为了方便，这里先封装一个用于操作本地存储的工具模块。

1、创建 `src/utils/storage.js`

```js
/**
 * 封装操作本地存储的工具方法模块
 */

export const getItem = name => {
  const data = window.localStorage.getItem(name)
  try {
    return JSON.parse(data)
  } catch (err) {
    console.log('转换失败', err)
    return data
  }
}

export const setItem = (name, value) => {
  const data = typeof value === 'object'
    ? JSON.stringify(value)
    : value
  window.localStorage.setItem(name, data)
}

export const removeItem = name => {
  window.localStorage.removeItem(name)
}
点击复制复制失败复制成功
```

2、然后在容器中使用使用本地存储持久化 token 数据

```js
import Vue from 'vue'
import Vuex from 'vuex'
+ import { setItem, getItem } from '@/utils/storage'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    // 登录用户，一个对象，包含 token 信息
+    user: getItem('user')
    // user: null
  },
  mutations: {
    setUser (state, data) {
      state.user = data

      // 为了防止刷新丢失 state 中的 user 状态，我们把它放到本地存储
+      setItem('user', state.user)
    }
  },
  actions: {
  },
  modules: {
  }
})
点击复制复制失败复制成功
```

测试：重新登录，查看本地存储是否有 token 数据，刷新浏览器，查看 Vuex 容器是否有 token 数据。

经过以上处理之后，接下来我们项目中所有需要使用 token 的业务，直接找 Vuex 容器拿，不需要关心数据从哪儿来的。

## [解析 Token](https://lxst9l.coding-pages.com/#/03-Token%E5%A4%84%E7%90%86?id=%e8%a7%a3%e6%9e%90-token)

有时候我们需要使用到登录用户的相关信息，例如用户 ID，正常的话建议由后端返回，但是如果后端没有提供的话，我们也可以通过解析 JWT token 来获取。

> 提示：token 中一定有表示用户身份的信息，例如 ID。

这里主要使用到一个第三方工具包：[jwt-decode](https://github.com/auth0/jwt-decode)。

1、安装

```bash
npm i jwt-decode点击复制复制失败复制成功
```

2、然后在容器中

```js
import decodeJwt from 'jwt-decode'点击复制复制失败复制成功
setUser (state, data) {
  // 解析 JWT 中的数据（需要使用用户ID）
  if (data && data.token) {
    data.id = decodeJwt(data.token).user_id
  }

  state.user = data

  // 为了防止刷新丢失 state 中的 user 状态，我们把它放到本地存储
  setItem('user', state.user)
},点击复制复制失败复制成功
```

之后就可以直接通过 `store.state.user.id` 来访问使用了。

## [发送 Token](https://lxst9l.coding-pages.com/#/03-Token%E5%A4%84%E7%90%86?id=%e5%8f%91%e9%80%81-token)

很多接口都需要提供 token（用户的登录状态） 才能访问。

方式一：在每次请求的时候手动添加（麻烦）

```js
axios({
  method: "",
  url: "",
  headers: {
    token数据
  }
});点击复制复制失败复制成功
```

方式二：使用请求拦截器统一添加（推荐，更方便）

在 `src/utils/request.js` 中添加拦截器统一设置 token：

```js
import store from '@/store'点击复制复制失败复制成功
// 请求拦截器
request.interceptors.request.use(function (config) {
  // config 请求配置对象，我们可以通过修改 config 来实现统一请求数据处理
  const { user } = store.state

  // 统一添加 token
  if (user) {
    // config.headers 获取操作请求头对象
    // Authorization 是后端要求的字段名称
    // 数据值后端要求提供：Bearer token数据
    //    注意：Bearer 后面有个空格
    // 老师，为啥？后端要求的
    config.headers.Authorization = `Bearer ${user.token}`
  }
  return config
}, function (error) {
  // Do something with request error
  return Promise.reject(error)
})点击复制复制失败复制成功
```

## [处理 Token 过期（在功能优化中进行讲解）](https://lxst9l.coding-pages.com/#/03-Token%E5%A4%84%E7%90%86?id=%e5%a4%84%e7%90%86-token-%e8%bf%87%e6%9c%9f%ef%bc%88%e5%9c%a8%e5%8a%9f%e8%83%bd%e4%bc%98%e5%8c%96%e4%b8%ad%e8%bf%9b%e8%a1%8c%e8%ae%b2%e8%a7%a3%ef%bc%89)

- 为什么 token 过期时间这么短？
  - 为了安全
- 过期了怎么办？
  - 通过登录页面重新登录获取 token
  - 使用 refresh_token 重新获取新的 token





在请求的响应拦截器中统一处理 token 过期：

```js
/**
 * 封装 axios 请求模块
 */
import axios from "axios";
import jsonBig from "json-bigint";
import store from "@/store";
import router from "@/router";

// axios.create 方法：复制一个 axios
const request = axios.create({
  baseURL: "http://ttapi.research.itcast.cn/" // 基础路径
});

/**
 * 配置处理后端返回数据中超出 js 安全整数范围问题
 */
request.defaults.transformResponse = [
  function(data) {
    try {
      return jsonBig.parse(data);
    } catch (err) {
      return {};
    }
  }
];

// 请求拦截器
request.interceptors.request.use(
  function(config) {
    const user = store.state.user;
    if (user) {
      config.headers.Authorization = `Bearer ${user.token}`;
    }
    // Do something before request is sent
    return config;
  },
  function(error) {
    // Do something with request error
    return Promise.reject(error);
  }
);

// 响应拦截器
request.interceptors.response.use(
  // 响应成功进入第1个函数
  // 该函数的参数是响应对象
  function(response) {
    // Any status code that lie within the range of 2xx cause this function to trigger
    // Do something with response data
    return response;
  },
  // 响应失败进入第2个函数，该函数的参数是错误对象
  async function(error) {
    // Any status codes that falls outside the range of 2xx cause this function to trigger
    // Do something with response error
    // 如果响应码是 401 ，则请求获取新的 token

    // 响应拦截器中的 error 就是那个响应的错误对象
    console.dir(error);
    if (error.response && error.response.status === 401) {
      // 校验是否有 refresh_token
      const user = store.state.user;

      if (!user || !user.refresh_token) {
        router.push("/login");

        // 代码不要往后执行了
        return;
      }

      // 如果有refresh_token，则请求获取新的 token
      try {
        const res = await axios({
          method: "PUT",
          url: "http://ttapi.research.itcast.cn/app/v1_0/authorizations",
          headers: {
            Authorization: `Bearer ${user.refresh_token}`
          }
        });

        // 如果获取成功，则把新的 token 更新到容器中
        console.log("刷新 token  成功", res);
        store.commit("setUser", {
          token: res.data.data.token, // 最新获取的可用 token
          refresh_token: user.refresh_token // 还是原来的 refresh_token
        });

        // 把之前失败的用户请求继续发出去
        // config 是一个对象，其中包含本次失败请求相关的那些配置信息，例如 url、method 都有
        // return 把 request 的请求结果继续返回给发请求的具体位置
        return request(error.config);
      } catch (err) {
        // 如果获取失败，直接跳转 登录页
        console.log("请求刷线 token 失败", err);
        router.push("/login");
      }
    }

    return Promise.reject(error);
  }
);

export default request;点击复制复制失败复制成功
```

## [总结](https://lxst9l.coding-pages.com/#/03-Token%E5%A4%84%E7%90%86?id=%e6%80%bb%e7%bb%93)

## [最终代码](https://lxst9l.coding-pages.com/#/03-Token%E5%A4%84%E7%90%86?id=%e6%9c%80%e7%bb%88%e4%bb%a3%e7%a0%81)

<https://github.com/lipengzhou/topline-m-89/tree/03-token>