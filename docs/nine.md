# [九、文章详情](https://lxst9l.coding-pages.com/#/09-%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85?id=%e4%b9%9d%e3%80%81%e6%96%87%e7%ab%a0%e8%af%a6%e6%83%85)





## [准备](https://lxst9l.coding-pages.com/#/09-%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85?id=%e5%87%86%e5%a4%87)

### [创建组件并配置路由](https://lxst9l.coding-pages.com/#/09-%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85?id=%e5%88%9b%e5%bb%ba%e7%bb%84%e4%bb%b6%e5%b9%b6%e9%85%8d%e7%bd%ae%e8%b7%af%e7%94%b1)

1、创建 `views/article/index.vue` 组件

```html
<template>
  <div class="article-container">文章详情</div>
</template>

<script>
export default {
  name: 'ArticlePage',
  components: {},
  props: {
    articleId: {
      type: String,
      required: true
    }
  },
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped lang="less"></style>
点击复制复制失败复制成功
```

2、然后将该页面配置到根级路由

```js
{
  path: '/article/:articleId',
  name: 'article',
  component: () => import('@/views/article'),
  // 将路由动态参数映射到组件的 props 中，更推荐这种做法
  props: true
}点击复制复制失败复制成功
```

> [官方文档：路由 props 传参](https://router.vuejs.org/zh/guide/essentials/passing-props.html)

最后访问 `/article/*` 测试。

### [布局](https://lxst9l.coding-pages.com/#/09-%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85?id=%e5%b8%83%e5%b1%80)

一、注册组件

- [Loading 加载](https://youzan.github.io/vant/#/zh-CN/loading)

二、导入素材到文章组件目录

- no-network.png
- [github-markdown.css](https://raw.githubusercontent.com/sindresorhus/github-markdown-css/gh-pages/github-markdown.css)

三、布局

```html
<template>
  <div class="article-container">
    <!-- 导航栏 -->
    <van-nav-bar
      title="文章详情"
      left-arrow
      fixed
      @click-left="$router.back()"
    ></van-nav-bar>
    <!-- /导航栏 -->

    <!-- 加载中 -->
    <van-loading
      class="loading"
      color="#1989fa"
      vertical
    >
      <slot>加载中...</slot>
    </van-loading>
    <!-- /加载中 -->

    <!-- 文章详情 -->
    <div class="detail">
      <h3 class="title">一个合格的中级前端工程师需要掌握的 28 个 JavaScript 技巧</h3>
      <div class="author-wrap">
        <div class="base-info">
          <van-image
            class="avatar"
            round
            fit="cover"
            src="https://img.yzcdn.cn/vant/cat.jpeg"
          />
          <div class="text">
            <p class="name">黑马头条号</p>
            <p class="time">4 小时前</p>
          </div>
        </div>
        <van-button class="follow-btn" type="info" size="small" round>+ 关注</van-button>
      </div>
      <div class="markdown-body">
        <p>作为战斗在业务一线的前端，要想少加班，就要想办法提高工作效率。这里提一个小点，我们在业务开发过程中，经常会重复用到日期格式化、url参数转对象、浏览器类型判断、节流函数等一类函数，这些工具类函数，基本上在每个项目都会用到，为避免不同项目多次复制粘贴的麻烦，我们可以统一封装，发布到npm，以提高开发效率。</p>
        <p>使用 Object.prototype.toString 配合闭包，通过传入不同的判断类型来返回不同的判断函数，一行代码，简洁优雅灵活（注意传入 type 参数时首字母大写）</p>
        <p>使用 Object.prototype.toString 配合闭包，通过传入不同的判断类型来返回不同的判断函数，一行代码，简洁优雅灵活（注意传入 type 参数时首字母大写）</p>
        <p>使用 Object.prototype.toString 配合闭包，通过传入不同的判断类型来返回不同的判断函数，一行代码，简洁优雅灵活（注意传入 type 参数时首字母大写）</p>
        <p>使用 Object.prototype.toString 配合闭包，通过传入不同的判断类型来返回不同的判断函数，一行代码，简洁优雅灵活（注意传入 type 参数时首字母大写）</p>
      </div>
    </div>
    <!-- /文章详情 -->

    <!-- 加载失败提示 -->
    <div class="error">
      <img src="./no-network.png" alt="no-network">
      <p class="text">亲，网络不给力哦~</p>
      <van-button
        class="btn"
        type="default"
        size="small"
      >点击重试</van-button>
    </div>
    <!-- /加载失败提示 -->

    <!-- 底部区域 -->
    <div class="footer">
      <van-button
        class="write-btn"
        type="default"
        round
        size="small"
      >写评论</van-button>
      <van-icon
        class="comment-icon"
        name="comment-o"
        info="9"
      />
      <van-icon
        color="orange"
        name="star"
      />
      <van-icon
        color="#e5645f"
        name="good-job"
      />
      <van-icon class="share-icon" name="share" />
    </div>
    <!-- /底部区域 -->
  </div>
</template>

<script>
export default {
  name: 'ArticlePage',
  components: {},
  props: {
    articleId: {
      type: String,
      required: true
    }
  },
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped lang="less">
@import "./github-markdown.css";

.article-container {
  padding: 46px 20px 50px;
  background: #fff;
  .loading {
    padding-top: 100px;
    text-align: center;
  }
  .detail {
    .title {
      margin: 0;
      padding-top: 24px;
      font-size: 20px;
      color: #3A3A3A;
    }
    .author-wrap {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 20px 0;
      height: 40px;
      .base-info {
        display: flex;
        align-items: center;
        .avatar {
          width: 40px;
          height: 40px;
          margin-right: 8px;
        }
        .text {
          line-height: 1.5;
          .name {
            margin: 0;
            font-size: 14px;
          }
          .time {
            margin: 0;
            font-size: 12px;
            color: #999;
          }
        }
      }
      .follow-btn {
        width: 85px;
      }
    }
  }
  .error {
    padding-top: 100px;
    text-align: center;
    .text {
      font-size: 15px;
    }
    .btn {
      width: 30%;
    }
  }
  .footer {
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;
    box-sizing: border-box;
    display: flex;
    justify-content: space-around;
    align-items: center;
    height: 44px;
    border-top: 1px solid #d8d8d8;
    background-color: #fff;
    .write-btn {
      width: 160px;
    }
    .van-icon {
      font-size: 20px;
    }
    .comment-icon {
      bottom: -2px;
    }
    .share-icon {
      bottom: -2px;
    }
  }
}
</style>点击复制复制失败复制成功
```

## [页面 loading 组件](https://lxst9l.coding-pages.com/#/09-%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85?id=%e9%a1%b5%e9%9d%a2-loading-%e7%bb%84%e4%bb%b6)

## [页面错误组件](https://lxst9l.coding-pages.com/#/09-%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85?id=%e9%a1%b5%e9%9d%a2%e9%94%99%e8%af%af%e7%bb%84%e4%bb%b6)

## [图片预览](https://lxst9l.coding-pages.com/#/09-%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85?id=%e5%9b%be%e7%89%87%e9%a2%84%e8%a7%88)

## [展示文章详情](https://lxst9l.coding-pages.com/#/09-%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85?id=%e5%b1%95%e7%a4%ba%e6%96%87%e7%ab%a0%e8%af%a6%e6%83%85)

这里我们主要实现两个主要功能：

- 获取展示文章详情
- 处理加载中 loading

一、请求并展示文章详情

1、在 `api/article.js` 中新增封装接口方法

```js
/**
 * 根据 id 获取指定文章
 */
export const getArticleById = articleId => {
  return request({
    method: 'GET',
    url: `/app/v1_0/articles/${articleId}`
  })
}点击复制复制失败复制成功
```

2、在组件中调用获取文章详情

```js
+ import { getArticleById } from '@/api/article'

export default {
  name: 'ArticlePage',
  components: {},
  props: {
    articleId: {
      type: String,
      required: true
    }
  },
  data () {
    return {
+      article: {} // 文章详情
    }
  },
  computed: {},
  watch: {},
  created () {
+    this.loadArticle()
  },
  mounted () {},
  methods: {
+++    async loadArticle () {
      try {
        const { data } = await getArticleById(this.articleId)
        this.article = data.data
      } catch (err) {
        console.log(err)
      }
    }
  }
}
点击复制复制失败复制成功
```

3、模板绑定

```html
<!-- 文章详情 -->
<div class="detail">
  <h3 class="title">{{ article.title }}</h3>
  <div class="author-wrap">
    <div class="base-info">
      <van-image
        class="avatar"
        round
        fit="cover"
        :src="article.aut_photo"
      />
      <div class="text">
        <p class="name">{{ article.aut_name }}</p>
        <p class="time">{{ article.pubdate }}</p>
      </div>
    </div>
    <van-button class="follow-btn" type="info" size="small" round>+ 关注</van-button>
  </div>
  <div class="markdown-body" v-html="article.content"></div>
</div>
<!-- /文章详情 -->

<!-- 底部区域 -->
<div class="footer">
  <van-button
    class="write-btn"
    type="default"
    round
    size="small"
  >写评论</van-button>
  <van-icon
    class="comment-icon"
    name="comment-o"
    info="9"
  />
  <van-icon
    color="orange"
    :name="article.is_collected ? 'star' : 'star-o'"
    @click="onCollect"
  />
  <van-icon
    color="#e5645f"
    name="good-job"
    :name="article.attitude === 1 ? 'good-job' : 'good-job-o'"
  />
  <van-icon class="share-icon" name="share" />
</div>
<!-- /底部区域 -->点击复制复制失败复制成功
```

二、处理加载中 loading

需求：

- 加载中，显示 loading
- 加载成功，显示文章详情
- 加载失败，显示错误提示

步骤：

- 添加 loading 数据用来控制 loading 展示
- 在请求获取数据处理函数中处理 loading
  - 请求开始，loading = true
  - 请求结束，loading = false
- 在模板中绑定处理

下面是具体实现过程。

1、在 data 中添加数据用来控制加载中的 loading 状态

```js
data () {
  return {
    ...
    loading: true
  }
}点击复制复制失败复制成功
```

2、在请求获取文章的处理函数中处理 loading 状态

```js
async loadArticle () {
  this.loading = true
  try {
    const { data } = await getArticleById(this.articleId)
    this.article = data.data
  } catch (err) {
  }
  this.loading = false
}点击复制复制失败复制成功
```

3、在模板中绑定条件渲染

```html
<!-- 加载中 -->
<van-loading
  v-if="loading"
  class="loading"
  color="#1989fa"
  vertical
>
  <slot>加载中...</slot>
</van-loading>
<!-- /加载中 -->

<!-- 文章详情 -->
<div v-else-if="article.title" class="detail">
  ...
</div>
<!-- /文章详情 -->

<!-- 加载失败提示 -->
<div v-else class="error">
  ...
</div>
<!-- /加载失败提示 -->点击复制复制失败复制成功
```

## [文章收藏](https://lxst9l.coding-pages.com/#/09-%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85?id=%e6%96%87%e7%ab%a0%e6%94%b6%e8%97%8f)

思路：

- 给收藏按钮注册点击事件
- 如果已经收藏了，则取消收藏
- 如果没有收藏，则添加收藏

下面是具体实现。

1、在 `api/article.js` 添加封装数据接口

```js
/**
 * 收藏文章
 */
export const addCollect = target => {
  return request({
    method: 'POST',
    url: '/app/v1_0/article/collections',
    data: {
      target
    }
  })
}

/**
 * 取消收藏文章
 */
export const deleteCollect = target => {
  return request({
    method: 'DELETE',
    url: `/app/v1_0/article/collections/${target}`
  })
}点击复制复制失败复制成功
```

2、给收藏按钮注册点击事件

3、处理函数

```js
async onCollect () {
  // 这里 loading 不仅仅是为了交互提示，更重要的是请求期间禁用背景点击功能，防止用户不断的操作界面发出请求
  this.$toast.loading({
    duration: 0, // 持续展示 toast
    message: '操作中...',
    forbidClick: true // 是否禁止背景点击
  })

  try {
    // 如果已收藏，则取消收藏
    if (this.article.is_collected) {
      await deleteCollect(this.articleId)
      // this.article.is_collected = false
      this.$toast.success('取消收藏')
    } else {
      // 添加收藏
      await addCollect(this.articleId)
      // this.article.is_collected = true
      this.$toast.success('收藏成功')
    }
    this.article.is_collected = !this.article.is_collected
  } catch (err) {
    console.log(err)
    this.$toast.fail('操作失败')
  }
}点击复制复制失败复制成功
```

## [文章点赞](https://lxst9l.coding-pages.com/#/09-%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85?id=%e6%96%87%e7%ab%a0%e7%82%b9%e8%b5%9e)

article 中的 `attitude` 表示用户对文章的态度

- `-1` 无态度
- `0` 不喜欢
- `1` 已点赞

思路：

- 给点赞按钮注册点击事件
- 如果已经点赞，则请求取消点赞
- 如果没有点赞，则请求点赞

1、添加封装数据接口

```js
/**
 * 点赞
 */
export const addLike = articleId => {
  return request({
    method: 'POST',
    url: '/app/v1_0/article/likings',
    data: {
      target: articleId
    }
  })
}

/**
 * 取消点赞
 */
export const deleteLike = articleId => {
  return request({
    method: 'DELETE',
    url: `/app/v1_0/article/likings/${articleId}`
  })
}点击复制复制失败复制成功
```

2、给点赞按钮注册点击事件

3、处理函数

```js
async onLike () {
  // 两个作用：1、交互提示 2、防止网络慢用户连续不断的点击按钮请求
  this.$toast.loading({
    duration: 0, // 持续展示 toast
    message: '操作中...',
    forbidClick: true // 是否禁止背景点击
  })

  try {
    // 如果已经点赞，则取消点赞
    if (this.article.attitude === 1) {
      await deleteLike(this.articleId)
      this.article.attitude = -1
      this.$toast.success('取消点赞')
    } else {
      // 否则添加点赞
      await addLike(this.articleId)
      this.article.attitude = 1
      this.$toast.success('点赞成功')
    }
  } catch (err) {
    console.log(err)
    this.$toast.fail('操作失败')
  }
}点击复制复制失败复制成功
```

## [关注用户](https://lxst9l.coding-pages.com/#/09-%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85?id=%e5%85%b3%e6%b3%a8%e7%94%a8%e6%88%b7)

### [用户不能关注自己](https://lxst9l.coding-pages.com/#/09-%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85?id=%e7%94%a8%e6%88%b7%e4%b8%8d%e8%83%bd%e5%85%b3%e6%b3%a8%e8%87%aa%e5%b7%b1)

什么情况下展示关注按钮？

- 如果用户没有登录
- 如果当前文章作者不是当前登录用户

```html
v-if="!$store.state.user || article.aut_id !== 当前登录用户.id"点击复制复制失败复制成功
```

下面是具体的实现。

一、获取当前登录用户的 ID

详见：[三、Token 处理—解析 Token](https://lxst9l.coding-pages.com/#/03-Token%E5%A4%84%E7%90%86?id=%e8%a7%a3%e6%9e%90-token)

二、条件绑定展示

```html
<van-button
  v-if="!$store.state.user || article.aut_id !== $store.state.user.id"
  class="follow-btn"
  :type="article.is_followed ? 'default' : 'info'"
  size="small"
  round
  :loading="isFollowLoading"
  @click="onFollow"
>{{ article.is_followed ? '已关注' : '+ 关注' }}</van-button>点击复制复制失败复制成功
```

最后测试。

### [关注|取消关注](https://lxst9l.coding-pages.com/#/09-%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85?id=%e5%85%b3%e6%b3%a8%e5%8f%96%e6%b6%88%e5%85%b3%e6%b3%a8)

思路：

- 给按钮注册点击事件
- 在事件处理函数中
  - 如果已关注，则取消关注
  - 如果没有关注，则添加关注

下面是具体实现。

1、在 `api/user.js` 中添加封装请求方法

```js
/**
 * 添加关注
 */
export const addFollow = userId => {
  return request({
    method: 'POST',
    url: '/app/v1_0/user/followings',
    data: {
      target: userId
    }
  })
}

/**
 * 取消关注
 */
export const deleteFollow = userId => {
  return request({
    method: 'DELETE',
    url: `/app/v1_0/user/followings/${userId}`
  })
}点击复制复制失败复制成功
```

2、给关注/取消关注按钮注册点击事件

3、在事件处理函数中

```js
import { addFollow, deleteFollow } from '@/api/user'点击复制复制失败复制成功
async onFollow () {
  // 开启按钮的 loading 状态
  this.isFollowLoading = true

  try {
    // 如果已关注，则取消关注
    const authorId = this.article.aut_id
    if (this.article.is_followed) {
      await deleteFollow(authorId)
    } else {
      // 否则添加关注
      await addFollow(authorId)
    }

    // 更新视图
    this.article.is_followed = !this.article.is_followed
  } catch (err) {
    console.log(err)
    this.$toast.fail('操作失败')
  }

  // 关闭按钮的 loading 状态
  this.isFollowLoading = false
}点击复制复制失败复制成功
```

最后测试。

## [日期格式化](https://lxst9l.coding-pages.com/#/09-%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85?id=%e6%97%a5%e6%9c%9f%e6%a0%bc%e5%bc%8f%e5%8c%96)

[moment](https://momentjs.com/) 是一个专门处理日期时间的一个纯 JavaScript 工具函数库，它不仅可以在浏览器中使用，也可以在 Node.js 中使用。

1、安装 moment

```sh
npm i moment点击复制复制失败复制成功
```

2、创建 `utils/datetime.js`

```js
/**
 * moment 日期处理封装
 */
import moment from 'moment'
import Vue from 'vue'

// 配置中文语言
moment.locale('zh-cn')

// 相对时间
Vue.filter('relativeTime', value => {
  // 参考文档：http://momentjs.cn/docs/#/manipulating/start-of/
  return moment(value).startOf('second').fromNow()
})

// 日期格式化
Vue.filter('datetime', (value, format = 'YYYY-MM-DD HH:mm:ss') => {
  return moment(value).format(format)
})
点击复制复制失败复制成功
```

3、在 `main.js` 中加载初始化

```js
import './utils/datetime'点击复制复制失败复制成功
```

之后在任何组件的模板中直接调用过滤器函数即可，例如。

```html
<p class="time">{{ article.pubdate | relativeTime }}</p>点击复制复制失败复制成功
```

 