## watch vs watchEffect

- watch 监听一个或多个响应式数据，并在数据发生变化后执行对应回调函数

```js
import { watch, ref } from "vue";

const x = ref(10);
const y = ref(100);

x.value = 20;

// 方式1：通过一个函数返回一个值
watch(
  () => x.value,
  (newVal, oldVal) => {
    console.log(newVal); //20
    console.log(oldVal); //10
  }
);

watch(x, (newVal, oldVal) => {
  console.log(newVal); //20
  console.log(oldVal); //10
});

watch([x, y], ([newX, oldX], [newY, oldY]) => {
  console.log("oldY:", oldY);
  console.log("newY:", newY);
  console.log("oldX:", oldX);
  console.log("newX:", newX);
});
```

- watchEffect, 会接受一个回调函数立即执行，自动追踪依赖，并在依赖发生变化时重新触发回调

```js
import { watchEffect } from "vue";

const x = ref(10);
const y = ref(100);

x.value += 10;
y.value += 1;

watchEffect(() => {
  console.log(x.value); //20
  console.log(y.value); //101
});
```

第三个参数可选项有：

- immediate: 是否立即出发监听 (watchEffect不支持)
- deep: 是否深度进行监听，3.5+版本还能指定监听的深度级别 (watchEffect不支持)
- flush: 设置刷新时机，
  - post，在侦听器回调中能访问被 Vue 更新之后的所属组件的 DOM
  - sync: 同步触发的侦听器，它会在 Vue 进行任何更新之前触发
- onTrack / onTrigger: 调试侦听器依赖
- once: 回调函数只会运行一次。侦听器将在回调函数首次运行后自动停止

### 对比

1. watch可以精准控制监听数据源
2. watch可以访问旧值
3. watch可以懒执行，也就是发生变化时才执行

### 使用场景

- 如果知道需要监听哪些数据，用watch
- 想让vue自动处理依赖，用watchEffect