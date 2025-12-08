## Vue 和 React 技术层面的区别

1. Vue 基于数据劫持实现响应式，React 基于状态驱动的重新渲染
   - Vue: 自动追踪依赖，细粒度更新
   - React: 状态变化触发组件函数重新执行
2. Vue 使用模板语法，React 使用 JSX 语法
3. Vue 生命周期分为创建、挂载、更新、销毁，React 分为初始化、更新、卸载
4. Vue 使用 vuex 或 pinia 进行状态管理，React 使用 redux 或 mobx 进行状态管理
