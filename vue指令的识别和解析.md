## vue 指令的识别和解析

vue 指令的识别和解析是将模板字符串转换成真实 dom 的一个过程

1. 首先，当 vue 接收到模板字符串后，会通过正则匹配指令特征解析成 AST 抽象语法树
2. 然后进行静态节点和静态指令的标记
3. 然后将 AST 转换成渲染函数字符串。不同的指令的处理不同，比如 v-if 会转换成三元表达式，v-for 会转换成\_l()渲染列表函数，v-slot 会进行作用域插槽处理，普通指令则存储在 AST 节点的 directives 数组中。
4. 然后执行渲染函数，创建虚拟 dom。
5. 当虚拟 dom 挂载成真实 dom 后，会执行指令的 bind、inserted 钩子函数
6. 当数据发生变化重写渲染时，会执行指令的 update、componentUpdated 钩子函数。
7. 当组件销毁时，会执行指令的 unbind 钩子函数。

```js
//优化点：
1. **组合式API**：指令可以更好地与 setup() 集成
2. **钩子改名**：更直观的生命周期对应
   bind → beforeMount
   inserted → mounted
   update → beforeUpdate
   componentUpdated → updated
   unbind → unmounted
3. **性能提升**：更好的 tree-shaking，编译时更多优化
4. **片段支持**：指令可以用于根元素为 Fragment 的组件

// 新特性：多 v-model
<CustomComponent v-model:first="first" v-model:last="last" />"
```
