## Vuex 和 Pinia 的区别

1. 核心架构不同

- Vuex 通过 createStore 创建 Store 实例，通过 state 初始化数据，通过 mutations 同步修改数据，通过 actions 处理异步逻辑，通过 getters 格式化数据，通过 modules 拆分模块。采用全局单例模式，通过一个 store 管理全局状态。
- Pinia 通过 defineStore 创建 Store 实例，采用分离模式每个组件的 store 都是独立的，对组合式 API 友好。

Vuex

```js
// index.js
import { createStore } from "vuex";
import cart from "./modules/cart";
import user from "./modules/user";
import getters from "./getters";

const store = createStore({
  getters,
  modules: {
    cart,
    user
  },
});

//cart.js
export default {
  namespaced: true,
  state: {
    count: 1,
  },

  mutations: {
    INCREMENT(state) {
      state.count++;
    },
  },

  actions: {
    INCREMENT_ASYNC({ commit }) {
      setTimeout(() => {
        commit("INCREMENT");
      }, 1000);
    },
  },
}

// user.js
export default {
  namespaced: true,
  state: {
    name: "张三",
    age: 18,
  },

  mutations: {
    UPDATE_NAME(state, name) {
      state.name = name;
    },
  },

  actions: {
    updateNameAsync({ commit }, name) {
      setTimeout(() => {
        commit("UPDATE_NAME", name);
      }, 1000);
    },
  },
};

// getters.js
export default {
  doubleCount: state => state.cart.count * 2,
  info: state => `我是${state.user.name},今年${state.user.age}`
}

// cart.vue
import { useStore } from "vuex";
import { computed } from "vue";

const store = useStore();
store.commit("cart/INCREMENT");
store.dispatch("cart/INCREMENT_ASYNC");
const doubleCount = computed(() => store.getters.doubleCount);
```

Pinia

```js
// cart.js
import { defineStore } from "pinia";
import {ref, computed} from "vue"

export const useCart = defineStore("cart", {
  const count = ref(1)
  const doubleCount = computed(() => count.value * 2)

  return {
    count,
    doubleCount
  }
})

// user.js
import { defineStore } from "pinia";
import {ref, computed} from "vue"
export const useUser = defineStore("user", {
  const name = ref("张三")
  const age = ref(18)
  const info = computed(() => `我是${name.value},今年${age.value}`)

  return {
    name,
    age,
    info
  }
})

//cart.vue
import { useCart } from "./cart";

const { count, doubleCount } = useCart();

count.value++;
```

2. Vuex 支持 vue2 和 vue3, Pinia 支持 vue2.7+和 vue3
3. Pinia 原生支持 TypeScript，Vuex 需要手动定义类型

Vuex

```ts
import { createStore } from "vuex";

// 手动定义类型
interface State {
  count: number;
}

const store = createStore<State>({
  state: {
    count: 1,
  },

  mutations: {
    INCREMENT(state) {
      state.count++;
    },
  },
});
```

Pinia

```ts
import { defineStore } from "pinia";
import {ref, computed} from "vue"
export const useMain = defineStore("main", {
  const count = ref(1) //自动推导类型 number
  const updateCount = () => count.value++
})
```
4. Pinia体积更小

### 使用场景

1. 全局状态管理，当多个组件共享数据时
2. 管理用户登陆状态、用户信息、权限等
3. 管理全局应用配置，比如主题色、国际化、菜单栏展开收起等
