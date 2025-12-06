## vue 路由导航守卫

路由守卫是用来控制路由跳转的一些行为，比如在跳转前，跳转中，跳转后执行特定的逻辑。路由守卫分为全局守卫，路由独享守卫，组件内守卫。

1. 全局守卫，影响所有路由
   - 全局前置守卫 beforEach
   - 全局解析守卫 beforeResolve
   - 全局后置钩子 afterEach

```js
// 全局前置守卫
router.beforeEach((to, from, next) => {
  // 特定逻辑

  next();
});

// 全局解析守卫
router.beforeResolve((to, from, next) => {
  // 特定逻辑

  next();
});

// 全局解析守卫
router.afterEach((to, from) => {
  // 特定逻辑
});
```

2. 路由独享守卫, 只影响特定路由

```js
const routers = [
  {
    path: "/login",
    component: Login,
    beforeEnter: (to, from, next) => {
      // 特定逻辑

      next();
    },
  },
];
```

3. 组件内守卫，只影响当前组件
   - 进入组件前，beforeRouteEnter
   - 路由更新，beforeRouteUpdate
   - 离开组件前，beforeRouteLeave

```js
beforeRouteEnter(to, from, next) {
  // 此时vue实例还未创建

  next(() => {
    // vue实例已创建
  })
}

beforeRouteUpdate(to, from, next) {

}

beforeRouteLeave(to, from, next) {
  if(flag) {
    next()
  }else {
    next(false) //阻止跳转
  }
}
```

### 执行顺序

从全局到局部

全局 beforeEach -> 组件 beforeRouterLeave -> 路由 beforeEnter -> 组件 beforeRouterEnter -> 全局 beforeResolve -> 全局 afterEach

### 使用场景

1. beforeEach，常用来进行登陆 token 验证，权限验证等
2. afterEach，常用来修改页面 title，实现页面埋点统计
3. beforeRouterEnter，常进行预加载、预请求，提升体验
4. beforeRouterLeave， 常用来进行数据保存的询问拦截
