## 异步组件

异步组件是通过懒加载的方式加载的组件，当有需要时才会加载对应组件。常用于将一些不常用模块延迟加载，通过用户操作才加载，从而优化性能。

### 异步组件的使用

- 通过 defineAsyncComponent 方法定义异步组件

```vue
<template>
  <componentA />
  <componentB />
</template>

<script steup>
import { defineAsyncComponent } from "vue";

// 函数式
const componentA = defineAsyncComponent(() =>
  import("./components/componentA.vue")
);

// 选项式
const componentB = defineAsyncComponent({
  loader: () => import("./components/componentB.vue"), //加载函数
  delay: 200, //// 展示加载组件前的延迟时间，默认为 200ms
  timeout: 3000, //超时时间，单位 ms
  errorComponent: ErrorComponent, //加载失败时展示的组件
  loadingComponent: LoadingComponent, //加载时展示的组件
});
</script>
```

- 通过 import 动态引入

```vue
<template>
  <componentA />
</template>
<script steup>
const componentA = () => import("./components/componentA.vue");
</script>
```

### Suspense 内置组件

当 vue 渲染页面遇到异步组件时会先用一个占位符代替，当异步组件加载完毕后，再渲染异步组件。所以可以配合 Suspense 内置组件使用，Suspense 内置组件有两个插槽，一个是默认插槽，一个是 fallback 插槽，当异步组件加载完毕时，会渲染默认插槽，当异步组件加载中时，会渲染 fallback 插槽。

```vue
<template>
  <Suspense>
    <template #fallback>
      <div>loading...</div>
    </template>
    <componentA />
  </Suspense>
</template>
<script steup>
import { defineAsyncComponent } from "vue";

// 函数式
const componentA = defineAsyncComponent(() =>
  import("./components/componentA.vue")
);
</script>
```

### 异步组件的优缺点

1. 优点:

- 减少首屏加载时间
- 按需加载，避免资源浪费
- 通过占位组件可以及时反馈给用户，避免白屏，提升用户体验

2. 缺点:

- 异步组件的加载会导致额外的请求
- 增加复杂度，需要维护加载中、加载失败等状态
- 对 seo 不友好，服务端渲染时初始渲染中异步组件不可见，影响搜索引擎的检索
