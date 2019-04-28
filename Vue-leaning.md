### 函数式组件
----
无状态，没有响应式数据，没有实例，没有this上下文。
```
Vue.component('my-component',{
  functional:true,
  props:{//可选，省略props，所有组件上的特性都会自动隐式解析为prop。
  },
  render:function(createElement, context){}
})

//单文件组件
<template functionnal>
</template>
```
