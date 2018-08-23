### VUE路由第一步～

[参考链接]: https://router.vuejs.org/zh-cn/essentials/getting-started.html

之前看vue-router，因为没有使用框架的经验，理解起来有点吃力，今日决心挥剑斩乱麻，搞定vue-router！



### 储备知识



我们需要做的是，就是将组件(components)映射到路由(routes)，然后告诉 vue-router 在哪里渲染它们。

+ <router-link>

  `<router-link>` 组件支持用户在具有路由功能的应用中（点击）导航。 通过 `to` 属性指定目标地址，默认渲染成带有正确链接的 `<a>` 标签，可以通过配置 `tag` 属性生成别的标签。另外，当目标路由成功激活时，链接元素自动设置一个表示激活的 CSS 类名。

  + to属性：表示目标路由的链接。当被点击后，内部会立刻把 `to` 的值传到 `router.push()`，所以这个值可以是一个字符串或者是描述目标位置的对象。
  + repalce属性：设置 `replace` 属性的话，当点击时，会调用 `router.replace()` 而不是 `router.push()`，于是导航后不会留下 history 记录。

+ <router-view>

  `<router-view>` 组件是一个 functional 组件，渲染路径匹配到的视图组件。`<router-view>` 渲染的组件还可以内嵌自己的 `<router-view>`，根据嵌套路径，渲染嵌套组件。

  + name属性：如果 `<router-view>`设置了名称，则会渲染对应的路由配置中 `components` 下的相应组件。

  #### 以下是一个小栗子～：

  ```javascript
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
  <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

  <div id="app">
      <h1>Hello App!</h1>
      <p>
          <!-- 使用 router-link 组件来导航. -->
          <!-- 通过传入 `to` 属性指定链接. -->
          <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
          <router-link to="/foo">Go to Foo</router-link>
          <router-link to="/bar">Go to Bar</router-link>
      </p>
      <!-- 路由出口 -->
      <!-- 路由匹配到的组件将渲染在这里 -->
      <router-view></router-view>
  </div>
  <script>
      // 0. 如果使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter)

      // 1. 定义（路由）组件。
      // 可以从其他文件 import 进来
      const Foo = { template: '<div>foo</div>' };
      const Bar = { template: '<div>bar</div>' };

      // 2. 定义路由
      // 每个路由应该映射一个组件。 其中"component" 可以是
      // 通过 Vue.extend() 创建的组件构造器，
      // 或者，只是一个组件配置对象。
      // 我们晚点再讨论嵌套路由。
      const routes = [
          { path: '/foo', component: Foo },
          { path: '/bar', component: Bar }
      ]

      // 3. 创建 router 实例，然后传 `routes` 配置
      // 你还可以传别的配置参数, 不过先这么简单着吧。
      const router = new VueRouter({
          routes // （缩写）相当于 routes: routes
      })

      // 4. 创建和挂载根实例。
      // 记得要通过 router 配置参数注入路由，
      // 从而让整个应用都有路由功能
      const app = new Vue({
          router
      }).$mount('#app');

      // 现在，应用已经启动了！
  </script>

  </body>
  </html>
  ```



### 动态路由匹配



如果我们需要某种把多个路由，映射到同个组件。例如，我们有一个 `User`组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染。那么，我们可以在`vue-router` 的路由路径中使用『动态路径参数』（dynamic segment）来达到这个效果：

```javascript
const User = {
  template: '<div>User</div>'
}

const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
```

> 现在呢，像 `/user/foo` 和 `/user/bar` 都将映射到相同的路由。

>  一个『路径参数』使用冒号 : 标记。当匹配到一个路由时，参数值会被设置到 this.$route.params，可以在每个组件内使用。于是，我们可以更新 User 的模板，输出当前用户的 ID。



### 嵌套路由



一个应用界面，通常由多层嵌套的组件组合而成；

举个栗子：在 `User` 组件的模板添加一个 `<router-view>`

```javascript
const User = {

  template: `

    <div class="user">

      <h2>User {{ $route.params.id }}</h2>

      <router-view></router-view>

    </div>
}
```

> 要在嵌套的出口中渲染组件，需要在 `VueRouter` 的参数中使用 `children` 配置：

```javascript
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User,
      children: [
        {
          // 当 /user/:id/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
          path: 'profile',
          component: UserProfile
        },
        {
          // 当 /user/:id/posts 匹配成功
          // UserPosts 会被渲染在 User 的 <router-view> 中
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})
```

> **以 / 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。**

基于上面的配置，当你访问 `/user/foo` 时，`User` 的出口是不会渲染任何东西，这是因为没有匹配到合适的子路由。如果你想要渲染点什么，可以提供一个 空的 子路由：

```javascript
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id', component: User,
      children: [
        // 当 /user/:id 匹配成功，
        // UserHome 会被渲染在 User 的 <router-view> 中
        { path: '', component: UserHome },

        // ...其他子路由
      ]
    }
  ]
})
```

栗子代码在后面github链接的children_router里～



### 使用编程式导航



#### `router.push(location, onComplete?, onAbort?)`

使用 `router.push` 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

当你点击 `<router-link>` 时，这个方法会在内部调用，所以说，点击 `<router-link :to="...">` 等同于调用 `router.push(...)`。

```javascript
const userId = 123
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 因为有path，这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```



### 命名路由



可以在创建 Router 实例的时候，在 `routes` 配置中给某个路由设置名称，以方便一些跳转：

```javascript
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```

```Java
router.push({ name: 'user', params: { userId: 123 }})
```



### 命名视图



有时候想同时（同级）展示多个视图，而不是嵌套展示，例如创建一个布局，有 `sidebar`（侧导航） 和 `main`（主内容） 两个视图，这个时候命名视图就派上用场了。你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。如果 `router-view` 没有设置名字，那么默认为 `default`。

```html
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```

完整代码在name_router文件里～



### 嵌套命名视图

*参考nchilds_router.html里的代码；*

- `Nav` 只是一个常规组件。
- `UserSettings` 是一个视图组件。
- `UserEmailsSubscriptions`、`UserProfile`、`UserProfilePreview` 是嵌套的视图组件



### 结语撒花

本文主要是对vue-router官网的小栗子进行了实践，具体代码请参考：

[github](https://github.com/dy21335/vue-router-sample/tree/master/day1)























