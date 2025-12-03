## ref 跟 reactive 的区别

都是用于创建响应式数据，但是语法、用途上存在差异

1. 语法上的区别

```js
import { ref, reactive } from "vue";

// ref
const count = ref(10);
console.log(count.value); //10

// reactive
const person = reactive({
  name: "张三",
  age: 18,
});
console.log(person.age); //18
```

2. 对象解构的差异

- ref 解构出来的值是基本数据类型则失去响应性，是对象的话则具有响应性。这是因为 ref 包装对象时，如果遇到属性是对象的情况使用 reactive 进行深层代理，所有嵌套属性都是响应式的。
- 解构 reactive 则会失去响应性
- 如果需要保留响应性，可以借助toRef或toRefs实现

```js
import { ref, reactive } from "vue";

// ref
const person = ref({
  name: "张三",
  address: {
    city: "广州市",
  },
});
const { name, address } = person.value;
name = "李四"; //不触发更新，失去响应式
address.city = "深圳市"; //触发更新

// reactive
const form = reactive({
  inputValue: "",
  checked: 0,
});
let { checked } = form;
checked = 1; //不触发更新
```

3. 使用场景

- ref 全部数据类型都适用，但是常用于基本数据类型的响应式，或是遇到可能整个对象被重新赋值的情况
- reactive 只能用于对象的响应式，适合处理结构化的数据
