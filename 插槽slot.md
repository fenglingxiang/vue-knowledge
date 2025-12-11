## slot

插槽相当于占位内容，允许父组件向子组件传递模板内容，使得组件内容可扩展。

### 插槽类型

1. 默认插槽

```vue
<!-- 子组件 child.vue -->
<template>
  <div>
    <div>标题</div>
    <slot>
      <div>默认内容</div>
    </slot>
  </div>
</template>

<!-- 父组件 -->
<template>
  <!-- 显示默认内容 -->
  <Child />

  <Child>
    <!-- 覆盖默认内容 -->
    <div>其他内容</div>
  </Child>
</template>
```

2. 具名插槽

通过 name 属性指定插槽名称

```vue
<!-- 子组件 child.vue -->
<template>
  <div>
    <div>标题</div>
    <slot name="content" />
    <slot name="footer" />
  </div>
</template>

<!-- 父组件 -->
<template>
  <Child>
    <div slot="content">内容</div>
    <div slot="footer">底部</div>
  </Child>
</template>
```

3. 作用域插槽

可以向父组件传递数据，父组件决定如何渲染数据

```vue
<!-- 子组件 child.vue -->
<template>
  <div>
    <div>标题</div>
    <div>
      <slot v-for="item in list" :key="item.id" :item="item">{{
        item.name
      }}</slot>
    </div>
  </div>
</template>

<!-- 父组件 -->
<template>
  <Child>
    <template #default="{ item }">
      <div>{{ item.name }}</div>
    </template>
  </Child>
</template>
```
