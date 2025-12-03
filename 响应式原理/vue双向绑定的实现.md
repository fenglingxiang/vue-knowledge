## 双向绑定

1. 简易的双向绑定

```html
<input type="text" id="input" />
<button id="btn">修改value</button>
<div id="value"></div>
```

```js
const data = {
  value: "",
};

const inputDom = document.querySelector("#input");
const valueDom = document.querySelector("#value");
const btnDom = document.querySelector("#btn");

Object.defineProperty(data, "value", {
  get() {
    return this._value;
  },

  set(val) {
    this._value = val;
    inputDom.value = val;
    valueDom.innerHTML = val;
  },
});

inputDom.addEventListener("input", e => {
  data.value = e.target.value;
});

btnDom.addEventListener("click", e => {
  data.value = Math.ceil(Math.random() * 100);
});
```

2. vue2 的双向绑定

- 遍历全部属性，基于 Object.defineProperty 给每个属性加上 getter，setter 方法，使其变成响应式数据。
- 通过Dep类管理Watcher，收集依赖，触发更新。
- 通过Watcher 订阅数据变化。
- 重写数组方法，使其变成响应式数据。

```js
function observer(data) {
  if (typeof data !== "object" || data === null) return;
  Object.keys(data).forEach(key => {
    defineReactive(data, key, data[key]);
  });
}

function defineReactive(obj, key, val) {
  const dep = new Dep(); //每个属性一个依赖收集器
  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get() {
      if (Dep.target) {
        dep.addSubs(Dep.target); //收集依赖
      }
      return val;
    },
    set(newVal) {
      if (val === newVal) return;
      this.observer(newVal); //递归响应式处理
      val = newVal;
      dep.notify(); //通知更新
    },
  });
}

// 依赖收集器
class Dep {
  constructor() {
    this.subs = []; //订阅者列表
  }

  addSubs(sub) {
    this.subs.push(sub);
  }

  notify() {
    this.subs.forEach(sub => sub.update());
  }
}

// 观察者（订阅者）
class Watcher {
  constructor(vm, exp, cb) {
    this.vm = vm;
    this.exp = exp;
    this.cb = cb;

    Dep.target = this; //将自身添加到订阅中
    this.value = vm.$data[exp]; //触发getter，收集依赖
    Dep.target = null;
  }

  update() {
    const newVal = this.vm.$data[this.exp];
    if (newVal !== this.value) {
      this.value = newVal;
      this.cb(newVal);
    }
  }
}
```

3. vue3 的双向绑定

- Proxy 替代了 defineProperty，解决了 vue2 的根本限制，按需代理，初始化性能更好
- 惰性响应，只有访问的属性才会被追踪
- 原生支持Map、Set

```js
function reactive(target) {
  return new Proxy(target, {
    get(target, key, receiver) {
      track(target, key); //收集依赖
      const res = Reflect.get(target, key, receiver);
      if (typeof target === "object" && target !== null) {
        return reactive(res);
      }
      return res;
    },
    set(target, key, value, receiver) {
      const oldValue = target[key];
      const res = Reflect.set(target, key, value, receiver);
      if (res && oldValue !== value) {
        trigger(target, key);
      }
      return res;
    },
    deleteProperty(target, key) {
      const hadKey = key in target;
      const res = Reflect.deleteProperty(target, key);
      if (res && hadKey) {
        trigger(target, key);
      }
      return res;
    },
  });
}

// 依赖收集和触发
const targetMap = new WeakMap();
let activeEffect = null;

function track(target, key) {
  if (!activeEffect) return;

  let depsMap = targetMap.get(target);
  if (!depsMap) {
    depsMap = new Map();
    targetMap.set(target, depsMap);
  }

  let dep = depsMap.get(key);
  if (!dep) {
    dep = new Set();
    depsMap.set(key, dep);
  }
  dep.add(activeEffect);
}

function trigger(target, key) {
  const depsMap = targetMap.get(target);
  if (!depsMap) return;
  const dep = depsMap.get(key);
  if (!dep) return;
  dep.forEach(effect => effect());
}
```
