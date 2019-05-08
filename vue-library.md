Vue render 
----------

总共有三个参数。

- 第一个参数必须参数
  
  type:{String | Object | Function}
  
  {一个 html 片段|组件选项对象|一个返回值类型为String/Object的函数}

- 第二个参数 非必须

  type:{Object}

  {一个包含模板相关属性的数据对象}

  ```
  {
    'class': {
      // 和`v-bind:class`一样的API
    },
    style: {
      // 和`v-bind:style`一样的API
    },
    attrs: {
      // 正常的 html 特性
    },
    props: {
      // 组件 props
    },
    domProps: {
      // DOM 属性
    },
    on: {
      // 事件监听器基于on
      // 不再支持如 `v-on:keyup.enter` 修饰器，需手动匹配 keyCode
    },
    nativeOn: {
      // 仅对于组件，用于监听原生组件，而不是组件内部使用`vm.$emit`触发的事件
    },
    directives: [
      {
        // 自定义指令，注意事项：不能对绑定的旧值设值
        // vue 会为您持续追踪
      }
    ],
    scopedSlots: {
    },
    slot: '', // 如果组件是其他组件的子组件，需为 slot 指定名称
    // 其他特殊顶层属性
    key: '',
    ref: ''
  }
  ```

- 第三个参数 非必须

  type:{String|Array}
  
  {子节点（VNnodes),由createElement()生成，或者见得使用字符串来生成文本节点}



```
Vue.component('component',{
  render:functio(createElement){
    return createElement('div',{
      props:...,
      attrs:...,
      class:....,
      ...
    },[
      VNode
    ])
  }
})
```

Vuex
-----
### State
### Getter
- 可以提取多个组件需要使用的属性。就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。接受state作为参数。
 ```
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
通过 store.getters.doneTodos访问。// -> [{ id: 1, text: '...', done: true }]

 ```
- Getter 接受第二个参数 getter
 ```
 getters: {
  // ...
  doneTodosCount: (state, getters) => {
    return getters.doneTodos.length
  }
}

组件中使用
computed:{
  doneTodosCount(){
    return this.$store.getters.doneTodosCount;
  }
}
 ```
- 通过方法访问
```
getters:{
  getTodoById：state => id => state.todos.find(todo => todo.id === id)
}
```
#### MapGetters 辅助函数
```
computed:{
  ...MapGetters([
    'doneTodosCount,
    //...
  ])
  
}
```
另取一个名字
```
mapGetters({
  // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
  doneCount: 'doneTodosCount'
})

```

### Mutation （同步事物）
更改Vuex的store中的状态的唯一方法是提交mutaition，每个mutation都有一个字符串的事件类型（type)和一个回调函数（handler)。回调函数就是我们实际进行更改状态的地方，

```
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state, n) {
      // 变更状态
      state.count++
    }
  }
})
store.commit('increment', 10);

//或者
//store.commit({
// type: 'increment',
// amount: 10
//})

```
mutation都必须是同步函数。因为当 mutation 触发的时候，回调函数还没有被调用，devtools 不知道什么时候回调函数实际上被调用——实质上任何在回调函数中进行的状态的改变都是不可追踪的。

在组件中提交 Mutation
```
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

### Action
- Action 提交 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。

Action 函数接受一个与store实例具有相同的方法和属性的context对象。
```
//context:{
//  commit,
//  state,
// getters
//};


const store = new Vuex.Store({
...
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
  //或者使用解构
   actions: {
    increment ({commit}) {
      commit('increment')
    }
  }
...
})

//在组件中分发 Action
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
命名空间
---

```
const store = new Vuex.Store({
  modules: {
    account: {
      namespaced: true,

      // 模块内容（module assets）
      state: { ... }, // 模块内的状态已经是嵌套的了，使用 `namespaced` 属性不会对其产生影响
      getters: {
        isAdmin () { ... } // -> getters['account/isAdmin']
      },
      actions: {
        login () { ... } // -> dispatch('account/login')
      },
      mutations: {
        login () { ... } // -> commit('account/login')
      },

      // 嵌套模块
      modules: {
        // 继承父模块的命名空间
        myPage: {
          state: { ... },
          getters: {
            profile () { ... } // -> getters['account/profile']
          }
        },

        // 进一步嵌套命名空间
        posts: {
          namespaced: true,

          state: { ... },
          getters: {
            popular () { ... } // -> getters['account/posts/popular']
          }
        }
      }
    }
  }
})
```
Vue computed 原理
---
流程总结如下：

当组件初始化的时候，computed 和 data 会分别建立各自的响应系统，Observer遍历 data 中每个属性设置 get/set 数据拦截
初始化 computed 会调用 initComputed 函数
注册一个 watcher 实例，并在内实例化一个 Dep 消息订阅器用作后续收集依赖（比如渲染函数的 watcher 或者其他观察该计算属性变化的 watcher ）
调用计算属性时会触发其Object.defineProperty的get访问器函数
调用 watcher.depend() 方法向自身的消息订阅器 dep 的 subs 中添加其他属性的 watcher
调用 watcher 的 evaluate 方法（进而调用 watcher 的 get 方法）让自身成为其他 watcher 的消息订阅器的订阅者，首先将 watcher 赋给 Dep.target，然后执行 getter 求值函数，当访问求值函数里面的属性（比如来自 data、props 或其他 computed）时，会同样触发它们的 get 访问器函数从而将该计算属性的 watcher 添加到求值函数中属性的 watcher 的消息订阅器 dep 中，当这些操作完成，最后关闭 Dep.target 赋为 null 并返回求值函数结果。
当某个属性发生变化，触发 set 拦截函数，然后调用自身消息订阅器 dep 的 notify 方法，遍历当前 dep 中保存着所有订阅者 wathcer 的 subs 数组，并逐个调用 watcher 的  update 方法，完成响应更新。