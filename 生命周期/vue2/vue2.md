# vue2 生命周期

# beforeCreate 组件实例创建之前，无法访问data，methods等属性
# created 组件实例初始化完成，可以访问data，methods等属性，但是无法访问dom（通常在这里执行初始化的操作、发送网络请求等）
# beforeMount 组件挂载之前，无法访问dom
# mounted 组件挂载完成，可以访问dom了
# beforeUpdate 组件更新之前
# updated 组件更新完成
# beforeDestroy 组件销毁之前，还是可以访问data，methods等属性
# destroyed 组件销毁完成
# activated 被 keep-alive 缓存的组件激活时调用。
# deactivated 被 keep-alive 缓存的组件停用时调用。

# 组件间生命周期执行顺序
# 父组件 beforeCreate -> 父组件 created -> 父组件 beforeMount -> 子组件 beforeCreate -> 子组件 created -> 子组件 beforeMount -> 子组件 mounted -> 父组件 mounted
# 父组件 beforeUpdate -> 子组件 beforeUpdate -> 子组件 updated -> 父组件 updated
# 父组件 beforeDestroy -> 子组件 beforeDestroy -> 子组件 destroyed -> 父组件 destroyed

![示例图](./vue2生命周期示例图.png)