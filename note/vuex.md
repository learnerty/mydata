### npm安装vuex
`npm install vuex --save`

### 简单介绍
每一个 Vuex 应用的核心就是 store（仓库）。"store" 基本上就是一个容器，它包含着你的应用中大部分的状态(state)。Vuex 和单纯的全局对象有以下两点不同：

1. Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
2. 你不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地提交(commit) mutations。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

#### 新建一个store.js文件
```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)   //注册vuex

let state = {   //状态树
  count:0
}
let mutations = {  //下面写方法，用来更改store的数据
  increment (state) {
    state.count++
  }
}

const store = new Vuex.Store({
  state,
  mutations
})

export default store

```
### 在main.js文件中注册store
```js
import Vue from 'vue'
import App from './App.vue'
import store from './store.js'
new Vue({
  el:'#app',
  store,   //注册store，注册之后在组件内可以得到$store
  components:{
    App   //注册app组件
  },
  render: x => x(App)  //挂载app组件
})

```
### 可以在组件中如此调用
```js
<template>
  <div>
    <h1>hello vuex</h1>
    {{$store.state.count}}
  </div>
</template>

<script>
export default {
  mounted(){
    this.$store.commit('increment')  //commit提交  increment是在mutations定义的方法
  }
}
</script>

<style>
</style>
```

### mapState 辅助函数
当一个组件需要获取多个状态时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 mapState 辅助函数帮助我们生成计算属性，让你少按几次键
```js
computed:mapState({
  testCount:state => state.count,  //获得count的值
  count: 'count',    //同上，两种写法
  countPlusLocalState (state) {   //store的数据和组件内部数据计算
    return state.count + this.localCount
  }
})

如果只需要获得store的值：
computed:mapState(['count'])

但一般写成：
computed:{
  totalMoney(){
    return this.localCount*100
  },
  ...mapState({
    count: state => state.count
  })
}
因为前两种组件内部的计算属性无法使用,故一般使用第展开运算符
```

### getters
可以对store中的数据处理
```js
store中定义
let state = {
  //...
  todos:[
    {title:'first',done:true},
    {title:'last',done:false}
  ]
}
let getters = {
  doneTodos: function (state) {
    return state.todos.filter(item=>item.done)
  }
}
const store = new Vuex.Store({
  //...
  getters
})
组件中使用
<ul>
  <li v-for='item in $store.getters.doneTodos'>
    {{item.title}}
  </li>
</ul>
```

### mapGetters 辅助函数
mapGetters 辅助函数仅仅是将 store 中的 getters 映射到局部计算属性：
```js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
  // 使用对象展开运算符将 getters 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```

### 提交载荷（Payload）
可以向 store.commit 传入额外的参数
```js
<button @click='add(10)'>+10</button>
<button @click='add(-10)'>-10</button>

methods:{
  add(num){
    this.$store.commit('increment',num)
  }
}

let mutations = {
  increment (state,num) {
    state.count+=num
  }
}
```
### 对象风格的提交方式
提交 mutation 的另一种方式是直接使用包含 type 属性的对象
```js
methods:{
  add(num){
    this.$store.commit({type: 'increment',num:num})
  }
}

let mutations = {
  increment (state,payload) {
    state.count+=payload.num
  }
}
```
### mutation 必须是同步函数

### 在组件中提交 Mutations
可以在组件中使用 this.$store.commit('xxx') 提交 mutation，或者使用 mapMutations 辅助函数将组件中的 methods 映射为 store.commit 调用（需要在根节点注入 store）。
```js
import { mapMutations } from 'vuex'
export default {
  // ...
  methods: {
    ...mapMutations([
      'increment' // 映射 this.increment() 为 this.$store.commit('increment')
    ]),
    ...mapMutations({
      add: 'increment' // 映射 this.add() 为 this.$store.commit('increment')
    })
  }
}
```
### Actions
Action 提交的是 mutation，而不是直接变更状态。  
Action 可以包含任意异步操作。
```js
let actions = {
  increment (context, payload){   //context表示store状态树，如果只需要commit方法，可以传入{commit}，需要接收的参数从第二个参数依次排列
    setTimeout(function(){
      context.commit('increment',payload)
    },5000)
  }
}
const store = new Vuex.Store({
  //...
  actions
})
export default {
  mounted(){
    this.$store.dispatch('increment',100)
  }
}
```

其他更详细的数据，请参考[vuex官方网站](https://vuex.vuejs.org/zh-cn/)
