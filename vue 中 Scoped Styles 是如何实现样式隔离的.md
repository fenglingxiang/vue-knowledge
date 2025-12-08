## vue 中 Scoped Styles 是如何实现样式隔离的

Vue 的 scoped styles 隔离通过 PostCss 在编译时给每个组件的元素添加一个 data-v-hash 的唯一属性，并将 css 选择器重写成[data-v-hash] .class-name 的形式, 从而实现样式隔离。

### 编译过程

1. 首先，通过文件路径跟内容生成一个 data-v 的唯一属性值

```js
const hash = require("hash-sum");
const filePath = "src/components/HelloWorld.vue";
const fileContent = fs.readFileSync(filePath, "utf-8");
const id = hash(`${filePath}\n${fileContent}`);
```

2. 然后处理模板，添加 data-v 属性

```html
<!-- 编译前 -->
<div class="container"></div>

<!-- 编译后 -->
<div class="container" data-v-789dd9dd></div>
```

3. 处理样式，重写 css 选择器为[data-v-hash] .class-name 的形式

```css
/* 编译前 */
.container {
  background: blue;
}

/* 编译后 */
.container[data-v-789dd9dd] {
  background: blue;
}
```
