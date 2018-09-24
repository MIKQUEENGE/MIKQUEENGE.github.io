---
title: Vuex入门
toc: true
date: 2018-09-23 22:30:52
categories:
- Web
tags:
- Vue
- Vuex
---

## 安装

比较常用的两种：

### 直接下载或CDN引用

从<https://unpkg.com/vuex>下载后利用script标签在vue后引入：

```html
<script src="/path/to/vue.js"></script>
<script src="/path/to/vuex.js"></script>
```

或

```html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vuex@3.0.1/dist/vuex.js"></script>
```

### 使用npm

在项目目录下运行：

```powershell
npm install vuex --save
```

在模块化的打包系统中利用这种方法时，必须显式地利用`Vue.use()`来安装Vuex（而script标签引入后是自动安装的）：

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

## 介绍

功能：把组件的共享状态抽取出来，用一个全局单例模式管理。

核心：store（仓库），它包含了应用中的大部分state（状态，驱动应用的数据源）。

这种全局单例模式管理和单纯的全局变量的区别：

- **Vuex 的状态存储是响应式的**。若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
- **不能直接改变store中的state**。改变 store 中的state的唯一途径就是显式地**commit (提交) mutation（变化）**。这样我们可以方便地跟踪每一个状态的变化。

一个栗子：

```js
// 如果在模块化构建系统中，请确保在开头调用了 Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})

// 触发状态变更
store.commit('increment')

console.log(store.state.count) // -> 1
```

**再次强调，使用提交 mutation ，而不是直接改变 `store.state.count`，**

**是因为我们想要更明确地追踪到状态的变化。**

当然，使用 Vuex 并**不意味着**需要将**所有的**状态放入 Vuex。

虽然将所有的状态放到 Vuex 会使状态变化更显式和易调试，但也会使代码变得冗长和不直观。

如果有些状态严格属于单个组件，最好还是作为组件的局部状态。

你应该根据你的应用开发需要进行权衡和确定。

## 核心概念

### State

每个应用只包含一个 store 实例，它包含了所有需要vuex管理的状态。

#### 利用计算属性读取state

从 store 实例中读取状态最简单的方法就是在[计算属性](https://cn.vuejs.org/guide/computed.html)中返回某个状态：

```js
// 创建一个 Counter 组件
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return store.state.count
    }
  }
}
```

但这种模式导致组件依赖全局状态单例。

#### 注册 `store` 选项

为了解决上述模式导致的组件依赖全局状态单例的问题，

我们可以通过在**根实例**中注册 `store` 选项——

这样 store 实例会注入到根组件下的所有子组件中，

且子组件能通过 `this.$store` 访问到：（需调用 `Vue.use(Vuex)`）

```js
// 根组件
const app = new Vue({
  el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```

```js
// 子组件
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      // 通过 this.$store 访问store
      return this.$store.state.count
    }
  }
}
```

#### `mapState`辅助函数

当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。

为了解决这个问题，我们可以使用 `mapState` 辅助函数帮助我们生成计算属性，让你少按几次键：

```js
// 在单独构建的版本中辅助函数为 Vuex.mapState
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // 箭头函数可使代码更简练
    // 将 `this.count` 映射为 `this.$store.state.count
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
```

当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 `mapState` 传一个字符串数组。

```js
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'
])
```

#### 对象展开操作符

ES6引入的新语法，由名字就可以看出来这个操作符的含义：把对象展开，

来个栗子更容易理解：

```js
var a = {'a':1, 'b':2, 'c':3};
{...a,'d':4} // {a: 1, b: 2, c: 3, d: 4}

var b = [1,2,3];
[...b,4] // [1, 2, 3, 4]
```

有了这个操作符，我们就可以把`mapState`函数和局部计算属性混合使用了：

```js
computed: {
  // 局部计算属性
  localComputed () { /* ... */ },
  // 使用对象展开运算符将此对象展开混入到外部对象中
  ...mapState({
    // ...
  })
}
```

### Getter

Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。

就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

Getter 会暴露为 `store.getters` 对象，你可以以属性的形式访问这些值；getter 在通过属性访问时是作为 Vue 的响应式系统的一部分缓存其中的。

Getter 接受 state 作为其第一个参数：

```js
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})

store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]
```

Getter 也可以接受其他 getter 作为第二个参数：

```js
getters: {
  // ...
  doneTodosCount: (state, getters) => {
    return getters.doneTodos.length
  }
}

store.getters.doneTodosCount // -> 1
```

你也可以通过让 getter 返回一个函数，来实现给 getter 传参。在你对 store 里的数组**进行查询**时非常有用。

```js
getters: {
  // ...
  // getter 在通过方法访问时，每次都会去进行调用，而不会缓存结果。
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}

store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
```

我们可以很容易地在任何组件中使用getter：

```js
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
```

我们也可以使用`mapGetters` 辅助函数将 store 中的 getter 映射到局部计算属性：

```js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
      doneCount: 'doneTodosCount',
      // ...
    ])
  }
}
```

### Mutation

**更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。**

Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**。

这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    // 这里的事件类型为 'increment' 
    increment (state) {
      // 变更状态
      state.count++
    }
  }
})
```

但我们不能直接调用一个 mutation 回调函数，就像前面说的，我们只能提交 mutation。

就像是事件注册：“当触发一个类型为 `increment` 的 mutation 时，调用此函数。”

要唤醒一个 mutation handler，你需要以相应的 type 调用 **store.commit** 方法（即提交mutation）：

```js
store.commit('increment')
```

#### 提交载荷Payload

我们还可以向 `store.commit` 传入额外的参数，即 mutation 的 **载荷（payload）**：

```js
// ...
mutations: {
  increment (state, n) {
    state.count += n
  }
}
store.commit('increment', 10)
```

在大多数情况下，载荷应该是一个对象，这样可以包含多个字段，并且记录的 mutation 会更易读：

```js
// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
// ...

store.commit('increment', {
  amount: 10
})
```

我们可以使用对象风格的提交方式，将一个直接包含 `type` 属性的对象作为载荷传给 mutations ：

```js
store.commit({
  type: 'increment',
  amount: 10
})
```

并且handler 无需改变：

```js
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```

#### Mutation 需遵守 Vue 的响应规则

因为 Vuex 的 store 中的状态是响应式的，那么当我们使用Mutation变更状态时，监视状态的 Vue 组件也会自动更新。

因此使用 Vuex 中的 mutation 也需要像使用Vue 一样遵守一些注意事项：

1. 提前在 store 中初始化好所有所需属性。

2. 当需要在对象上添加新属性时，你应该

   - 使用 `Vue.set(obj, 'newProp', 123)`, 或者

   - 以新对象替换老对象。例如利用ES6的对象展开运算符：

     ```js
     state.obj = { ...state.obj, newProp: 123 }
     ```

#### Mutation 必须是同步函数

 **mutation 必须是同步函数**。为什么？请参考下面的例子：

```js
mutations: {
  someMutation (state) {
    api.callAsyncMethod(() => {
      state.count++
    })
  }
}
```

假设我们正在 debug 一个 app 并且观察 devtool 中的 mutation 日志：

每一条 mutation 被记录，devtools 都需要捕捉到前一状态和后一状态的快照。

然而，在上面的例子中 mutation 中的**异步函数**中的回调让这不可能完成：

当 mutation 触发的时候，回调函数还没有被调用，devtools 不知道什么时候回调函数实际上被调用——

实质上任何在回调函数中进行的状态的改变都是不可追踪的。

#### 在组件中提交 Mutation

我们可以在组件中使用 `this.$store.commit('xxx')` 提交 mutation，

或者使用 `mapMutations` 辅助函数将组件中的 methods 映射为 `store.commit` 调用（需要在根节点注入 `store`）：

```js
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

      // `mapMutations` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    })
  }
}
```

#### 使用常量替代 Mutation 事件类型

使用常量替代 mutation 事件类型在各种 Flux 实现中是很常见的模式。这样可以使 linter 之类的工具发挥作用，同时把这些常量放在单独的文件中可以让你的代码合作者对整个 app 包含的 mutation 一目了然：

```js
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'
// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})
```

用不用常量取决于实际情况——在需要多人协作的大型项目中，这会很有帮助。你果然如果不想用，也完全可以不用。

### Action

Action 类似于 mutation，不同在于：

- Action 是提交 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。

```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      // 变更状态
      state.count++
    }
  },
  actions: {
    // 接受一个与 store 实例具有相同方法和属性的 context 对象
    increment (context) {
      // 提交mutation
      context.commit('increment')
    }
  }
})
```

实践中，我们可以使用 ES2015 的 **参数解构** 来简化代码（特别是我们需要调用 `commit` 很多次的时候）：

```js
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

#### 进行异步操作

因为action是提交mutation而不是直接变更状态，因此我们就可以在action内部执行异步操作了：

```js
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```

#### 分发Action

Action 通过 `store.dispatch` 方法触发：

```js
store.dispatch('increment')
```

Actions 支持Mutation同样的载荷方式和对象方式进行分发：

```js
// 以载荷形式分发
store.dispatch('incrementAsync', {
  amount: 10
})

// 以对象形式分发
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```

也可以在组件中使用 `this.$store.dispatch('xxx')` 分发 action，或者使用 `mapActions` 辅助函数将组件的 methods 映射为 `store.dispatch` 调用（需要先在根节点注入 `store`）：

```js
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

      // `mapActions` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    })
  }
}
```

#### 组合 Action

Action 通常是异步的，那么如何知道 action 什么时候结束呢？更重要的是，我们如何才能组合多个 action，以处理更加复杂的异步流程？

##### 使用promise

`store.dispatch` 可以处理被触发的 action 的处理函数返回的 Promise，

然后返回这个 Promise：

```js
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  }
}
```

现在我们就可以：

```js
store.dispatch('actionA').then(() => {
  // ...
})
```

在另外一个 action 中也可以：

```js
actions: {
  // ...
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {
      commit('someOtherMutation')
    })
  }
}
```

##### 使用async / await

如果我们可以利用 **async / await**，我们还可以如下组合 action：

```js
// 假设 gotData() 和 gotOtherData() 返回的是 Promise

actions: {
  async actionA ({ commit }) {
    commit('gotData', await getData())
  },
  async actionB ({ dispatch, commit }) {
    await dispatch('actionA') // 等待 actionA 完成
    commit('gotOtherData', await getOtherData())
  }
}
```

> 一个 `store.dispatch` 在不同模块中可以触发多个 action 函数。在这种情况下，只有当所有触发函数完成后，返回的 Promise 才会执行。

### Module

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割：

```js
const moduleA = {
  state: { ... },
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: { ... },
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```

#### 模块的局部状态

对于模块内部的 mutation 和 getter，接收的第一个参数是**模块的局部状态对象**。

```js
const moduleA = {
  state: { count: 0 },
  mutations: {
    increment (state) {
      // 这里的 `state` 对象是模块的局部状态
      state.count++
    }
  },

  getters: {
    doubleCount (state) {
      return state.count * 2
    }
  }
}
```

同样，对于模块内部的 action，局部状态通过 `context.state` 暴露出来，根节点状态则为 `context.rootState`：

```js
const moduleA = {
  // ...
  actions: {
    incrementIfOddOnRootSum ({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit('increment')
      }
    }
  }
}
```

对于模块内部的 getter，根节点状态也会作为第三个参数暴露出来：

```js
const moduleA = {
  // ...
  getters: {
    sumWithRootCount (state, getters, rootState) {
      return state.count + rootState.count
    }
  }
}
```

## 项目结构

Vuex 并不限制我们的代码结构。但是，它规定了一些需要遵守的规则：

1. 应用层级的状态应该集中到单个 store 对象中。
2. 提交 **mutation** 是更改状态的唯一方法，并且这个过程是同步的。
3. 异步逻辑都应该封装到 **action** 里面。

如果 store 文件太大，只需将 action、mutation 和 getter 分割到单独的文件。

对于大型应用，我们会希望把 Vuex 相关代码分割到模块中。下面是项目结构示例：

```bash
├── index.html
├── main.js
├── api
│   └── ... # 抽取出API请求
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── actions.js        # 根级别的 action
    ├── mutations.js      # 根级别的 mutation
    └── modules
        ├── cart.js       # 购物车模块
        └── products.js   # 产品模块
```





---

有时候看不进去[文档](https://vuex.vuejs.org/zh/)，一边总结一边看就能看进去了 :)