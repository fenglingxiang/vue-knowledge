## ref、toRef、toRefs 的区别

ref、toRef、toRefs 都能用于创建响应式数据

1. ref 用于创建独立的响应式引用

```js
const obj = {
  name: "张三",
  age: 18,
};

const name = ref(obj.name);
obj.name = "李四";

console.log(name.value); //张三 独立的响应式引用，不受源对象影响
```

2. toRef 用于创建与源对象的连接，当源对象改变时，toRef 创建的响应式引用也会改变

```js
const obj = {
  name: "张三",
  age: 18,
};

const name = toRef(obj, "name");
const age = toRef(obj, "age");
obj.name = "李四";
age.value = 20;

console.log(name.value); //李四 源对象变化，toRef会同步变化
console.log(obj.age); //20 toRef创建的响应式引用也会同步到源对象
```

**toRef 原理**

```js
function toRef(obj, key) {
  const val = obj[key];
  if (isRef(val)) return val;
  return new ObjectRefImpl(obj, key);
}

class ObjectRefImpl {
  constructor(obj, key) {
    this._object = obj;
    this._key = key;
    this._v_isRef = true;
  }

  get() {
    //读取源对象的属性值
    return this._object[this._key];
  }

  set(val) {
    //设置源对象的属性值
    this._object[this._key] = val;
  }
}
```

3. toRefs 批量解构响应式对象，实际就是批量进行 toRef 操作

```js
const obj = reactive({
  name: "张三",
  age: 18,
});

const { name, age } = toRefs(obj);
console.log(name.value); //张三
console.log(age.value); //18

//同步源对象
name.value = "李四";
age.value = 20;
console.log(obj); //{name: "李四", age: 20}
```

**toRefs 原理**

```js
function toRefs(obj) {
  if (!isProxy(obj)) {
    console.warn("toRefs只能用于响应式对象");
    return obj;
  }
  const res = Array.isArray(obj) ? new Array(obj.length) : {};
  for (const key in obj) {
    res[key] = toRef(obj, key);
  }
  return res;
}
```
