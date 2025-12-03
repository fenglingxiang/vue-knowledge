## h 函数

h 函数返回的是虚拟节点，在 render 函数或 setup 函数返回的函数中使用。常用于创建高阶组件、函数式组件。动态控制组件渲染内容。

h 函数接收三个参数：

- 第一个参数：标签名、组件、异步组件或函数式组件
- 第二个参数：属性对象
- 第三个参数：子节点，可以是数组或字符串

### 基本使用

```js
import { h } from "vue";

const vnode1 = h("div", { id: "app" }, "Hello World"); //标签名、属性对象、子节点
const vnode2 = h("div", "Hello World"); //标签名、子节点
const vnode3 = h("div", { id: "app" }); //标签名、属性对象
const vnode4 = h("div", { id: "app" }, [
  h("span", "Hello"),
  h("span", "World"),
]); //标签名、属性对象、子节点数组
```

### 动态控制组件渲染内容

```js
import { h, defineComponent, ref } from 'vue'

const ComponentA = defineComponent({
  setup() {
    return () => h('div', 'Component A')
  },
})

const ComponentB = defineComponent({
  setup() {
    return () => h('div', 'Component B')
  },
})

export default defineComponent({
  setup() {
    const components = { ComponentA, ComponentB }
    const componentName = ref('ComponentA')
    return () => {
      const curComponent = components[componentName.value as keyof typeof components]
      return h('div', [
        h(
          'button',
          {
            onClick: () =>
              (componentName.value =
                componentName.value == 'ComponentA' ? 'ComponentB' : 'ComponentA'),
          },
          '切换组件',
        ),
        h(curComponent),
      ])
    }
  },
})
```
