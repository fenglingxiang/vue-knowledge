## computed 是如何实现缓存的

1. 首次访问执行 getter 并缓存结果
2. 依赖的响应式数据发生变化时，缓存标记失效
3. 再次访问时，如果缓存标记失效则重新计算，否则直接返回缓存结果

```js
const computed = getter => {
  let value;
  let dirty = true; //缓存标记

  // 创建响应式effect
  const runner = effect({
    lazy: true,
    scheduler: () => {
      dirty = true; //依赖响应式数据变化时，缓存标记失效
      trigger(obj, "value"); //触发依赖收集的effect重新执行
    },
  });

  const obj = {
    get value() {
      if (dirty) {
        value = runner(); //重新计算
        dirty = false; //标记缓存
      }

      track(obj, "value"); //依赖收集

      return value;
    },
  };

  return obj;
};
```
