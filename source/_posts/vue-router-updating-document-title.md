---
title: 'VueRouter动态更新页面title'
date: 2017-07-27 11:24:41
categories: vue
tags: ['vue','vue-router']
---

## 写在前面

最近在折腾 `vue` 的时候，关于 `vue-router`，发现当切换不同页面的时候页面 `title` 是不会更新的。随后去找官方的文档，发现没有直接的方法来做这件事件，倒是有个[路由元信息](https://router.vuejs.org/zh-cn/advanced/meta.html) `meta`，通过这个方法可以事先定义好路由的一些信息比如文档中的例子：

```javascript
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          // a meta field
          meta: { requiresAuth: true }
        }
      ]
    }
  ]
})
```

这样的话也可以定义各页面的 `title: '登录'`，如何取到 `meta` 中的数据，下面是文档中的例子：

>那么如何访问这个 meta 字段呢？
>首先，我们称呼 routes 配置中的每个路由对象为 路由记录。路由记录可以是嵌套的，因此，当一个路由匹配成功后，他可能匹配多个路由记录
>例如，根据上面的路由配置，/foo/bar 这个 URL 将会匹配父路由记录以及子路由记录。
>一个路由匹配到的所有路由记录会暴露为 $route 对象（还有在导航钩子中的 route 对象）的 $route.matched 数组。因此，我们需要遍历 $route.matched 来检查路由记录中的 meta 字段。
>下面例子展示在全局导航钩子中检查 meta 字段：

```javascript
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})
```

## 过程

`meta` 中的数据取到后，该如何更新到 `title`， `document.title = ...`？

通过搜索发现有人觉得把事先定义好的 `title` 保存在 `vuex` 中，在页面切换的时候去获取 `store` 中的 `title`。但是这个方法有点 “大材小用的感觉”，不喜欢。

然后找到 github vue-router Issues [https://github.com/vuejs/vue-router/issues/112](https://github.com/vuejs/vue-router/issues/112) 中的讨论。


## 方法

[@roninliu](https://github.com/roninliu)给到的方法：

```javascript
//in router config
{
    path: "/login",
    name: "login",
    component: LoginView,
    meta: {
        title: 'Login System'
    }

}

//in init router file
router.afterEach(route => {
    document.title = route.meta.title;
})
```

## 一点点思考

最近一直在熟悉vue的开发，给我的感觉也是简单易上手，虽然自己的javaScript基础相对弱，还好在遇到问题的时候通过搜索能够解决难题，这就是互联网共享的价值吧。

同时也一直在看一些前端开发相关的文章，还是发现各种工具框架层出不穷，感觉过不了个把月可能当下你用的工具就过时了，不如 `npm`，现在的 `yarn` ......

作为前端开发，有时也觉得各位大神究竟在折腾个啥呢。

最后，基础是一切之根基。在折腾各种框架工具的同时还是要时刻提升自己的基础知识。

>路漫漫其修远兮