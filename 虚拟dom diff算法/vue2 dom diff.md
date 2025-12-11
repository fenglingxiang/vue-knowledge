## vue2 dom diff

- 只比较同一层级的节点， 不跨层级移动节点
- 双端比较算法
- key 映射

### diff 过程

```text
初始状态：
旧节点：[a, b, c, d, e]
新节点: [c, d, e, a, b]

步骤一： 头头比较，尾尾比较, 旧头新尾比较，旧尾新头比较
a ≠ c, e ≠ b, a ≠ b, e ≠ c

步骤二：创建key映射表
{a: 0, b: 1, c: 2, d: 3, e: 4}

步骤三：查找c在旧节点中的位置，c在旧节点中的位置为2
移动c到a前面， 标记oldChildren[2]为undefined
旧节点：[a, b, undefined, d, e]

步骤四：继续比较
a ≠ d, e ≠ b, a ≠ b, e ≠ d

步骤五：查找d在旧节点中的位置，d在旧节点中的位置为3
移动d到a前面， 标记oldChildren[3]为undefined
旧节点：[a, b, undefined, undefined, e]

步骤六：继续比较
a ≠ e, e ≠ b, a ≠ b, e = e
移动e到a前面

步骤七：继续比较
a = a, b = b

结束比较
```

### 总结

vue 的虚拟 dom 是一个用来描述真实 dom 的 js 对象结构。是通过 vnode 类创建的，当数据变化时，vue 不会直接操作真实 dom，而是生成新的虚拟 don 树，然后通过 diff 算法对比新旧虚拟 dom 树，找出差异，然后根据差异最小化操作真实 dom，从而实现最小化更新。

diff 过程在 vue2 中采用的是双端比较算法，通过头头比较、尾尾比较、旧头新尾比较、旧尾新头比较，找出差异，然后通过 key 映射表，找出新节点在旧节点中的位置，然后移动节点。vue3 则是在侧基础上增加了最长递增子序列算法，在序列中的节点说明无需移动，从而减少了 dom 移动次数。

### vue 是如何实现 MVVM 的

vue 是通过数据劫持 + 发布订阅模式 + 虚拟 dom 实现 mvvm 的。Model 指的是数据层，View 指的是视图层，ViewModel 指的是 vue 的实例用于连接数据层和视图层，实现自动同步。vue2 中通过 Object.defineProperty()实现数据劫持，访问数据时触发 getter，收集依赖，数据变化时触发 setter，通知所有依赖更新。vue3 则使用了 Proxy 代理替换了 Object.defineProperty，为了解决 vue2 中的一些局限性，比如无法监听新增/删除属性，无法通过索引或 length 监听数组变化等。Proxy 代理拥有更多的拦截操作，除了 get，set 以外，还有 deleteProperty，has，onwKeys 等等，功能更加强大。

### 什么时候 diff 算法效率会降低

1. 没有使用 key 或是 key 值不唯一时
2. 组件层级过深并且频繁更新
3. 大量动态节点并且结构频繁变化
4. 频繁强制重新渲染整个组件
