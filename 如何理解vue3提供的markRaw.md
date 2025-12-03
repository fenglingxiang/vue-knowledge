## markRaw

markRaw() 方法用于标记一个对象，使其不会被转换为响应式对象。

```js
import { markRaw, ref } from "vue";

const markRawUser = markRaw({
  name: "jack",
  age: 18,
});

const state = ref({
  user: markRawUser,
  city: "BeiJing",
});
```

### 使用场景

1. 包含大量数据但不需要响应式时，使用 markRaw() 方法标记对象，使其不会被转换为响应式对象，从而提高性能。

2. 避免不必要的转换，特别是使用第三方库时，防止将第三方库实例转换为响应式对象。

3. 防止循环引用，避免 Vue 递归转换时的无限循环
