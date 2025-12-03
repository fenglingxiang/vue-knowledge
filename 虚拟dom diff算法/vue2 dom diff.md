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
