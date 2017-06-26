# 传统模式开发
vue可以有很多实例，但一般不会声明多个,vue中尽量少使用箭头函数，因为容易搞乱this指向
## vue指令：带有 `v-` 前缀的特殊属性，指令属性的值预期是单一 `JavaScript` 表达式（除了 `v-for`，之后再讨论）。指令的职责就是当其表达式的值改变时相应地将某些行为应用到 `DOM` 上。
[vue指令链接](https://cn.vuejs.org/v2/api/#指令)
## 简单的应用
```js
<style>
  [v-cloak] {  //中括号，属性选择器，带有v-cloak属性的样式
    display: none;
  }
</style>
<body>
<div id='app'>
  <h1 v-cloak>{{ msg }}</h1>    //{{msg}}相当于react的this.state.msg，dom里写变量是双大括号：{{`我的消息是:${msg}`}},语法和react一样，写双括号，因为没有执行到js，浏览器就把内容渲染出去了，所以加v-cloak属性配合v-cloak样式来使用，让内容加载时隐藏，加载到js时再显示
  <p v-text='num+12'></p>   //结果900，标签内插入一个text
  <p v-html='html'></p>   //解析标签，插入一个html
  <h1 v-once>{{`我的消息是:${msg}`}}</h1> //v-once 只渲染一次，下面定义了一秒后更改msg内容，上面没加v-once所以被修改，本条不发生变化
  <p v-show="show">我是由v-show控制的</p>  //基于css切换，通过添加display属性来决定显示隐藏，如果控制的代码少，则用v-show，控制的多用v-if
  <template v-if="type==='A'"><button>a</button><button>a</button></template>  //相当于if,表达式,结果为真的时候渲染,template是包装标签，最终渲染时不会被渲染
  <button v-else-if="type==='B'">B</button>  //紧挨着v-if出现，相当于else if
  <button v-else>没有AB</button>   //紧挨着v-if出现，相当于else
</div>
<script src="./vue.js"></script>
<script>
  var vm = new Vue({   //创建一个vue实例
    el:'#app',   //实例作用范围在哪里
    data: {    //相当于react的state
      msg: 'hello vue',
      num: 888,
      html:'<span>hello vue</span>',
      type:'A',
      show:false
    }
  })
  vm.$mount('#app')  //$mount相当于el  等同于el:'#app'
  setTimeout(()=>vm.msg="hello react",1000)  //一秒后更改msg的值
  console.log(vm.msg)  //vm.msg 拿到msg的值
</script>
</body>

```

### 列表渲染
```js
<body>
  <div id="app">
    <ul>
      <li v-for='item in todos' v-if='item.fini'>   //遍历todos，也支持传一个索引： (item, index) ,可以和v-if搭配使用，比v-if优先级高,v-if分别运行于每个v-for循环中
        {{item.id}}---{{item.text}}  //这里的id和text是下面todos的属性名
      </li>
    </ul>
    <div v-for='(value, key, index) in obj'>   //可以遍历对象，第一个参数(value)是属性值，第二个参数(key)是属性名,第三个参数(index)是索引值
      <p>{{ index }}.{{ key }} : {{ value }}</p>
    </div>
  </div>
<script src="./vue.js"></script>
<script>
  new Vue({
    el:'#app',
    data:{
      todos:[
        {id:1,text:'do what',fini:false},
        {id:2,text:'do what2',fini:true},
        {id:3,text:'do what3',fini:false},
        {id:4,text:'do what4',fini:true}
      ],
      obj:{
        name:'tian',
        age:23
      }
    }
  })
</script>
</body>
```
### class于style绑定
```js
<style>
  .aaa{background: #ccc;}
  .active{color: yellow;}
</style>
<body>
<div id="app">
  <div v-bind:id='id' v-bind:class='"aaa"' :style='{color:"red",fontSize:`${font}px`}'>hello vue</div>  //v-bind绑定属性，冒号后面写绑定的属性名,等号后面写值,可以直接去掉v-bind，缩写成 :xxx
  <div v-bind:class='{active: isActive, hasError}'>hello vue</div>  //可以写对象，根据isActive判断是否添加active，可以用es6的语法
  <div v-bind:class='[class1,class2]'>hello vue</div>  //可以写成数组，class1,2在下方data中定义
</div>
<script src="./vue.js"></script>
<script>
  var vm = new Vue({
    el:'#app',
    data:{
      id:'hello',
      isActive:true,
      hasError:true,
      class1:'Class1',
      class2:'Class2',
      font:20
    }
  })
  setTimeout(()=>vm.id='world',500)
</script>
</body>
```
### 表单控件绑定 v-model
```js
<body>
  <div id="app">
    <p>{{msg}}</p>
    <input name='msg' v-model='msg'>   //双向绑定，将input的value值与msg动态的绑定，相当于react的受控组件
    <input type="checkbox" v-model='checked'><label>{{checked}}</label>
    <br/>
    <input type="checkbox" value="Jack" v-model="checkedNames">   //多选按钮，将选中的按钮的value值保存到checkedNames数组里
    <label>Jack</label>
    <input type="checkbox" value="John" v-model="checkedNames">
    <label>John</label>
    <input type="checkbox" value="Mike" v-model="checkedNames">
    <label>Mike</label>
    <br>
    <span>Checked names: {{ checkedNames }}</span>
  </div>
  <script src="./vue.js"></script>
  <script>
    new Vue({
      el:'#app',
      data:{
        msg:'hello world',
        checked:false,
        checkedNames: []
      }
    })
  </script>
</body>
```
### 计算属性
```js
<body>
  <div id="app">
    <table :style='{width:"100%"}'>
      <thead>
        <tr>
          <td>书名</td>
          <td>价格</td>
          <td>数量</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for='item in books'>
          <td>{{item.name}}</td>
          <td>{{item.price}}</td>
          <td><input type="number" v-model.number='item.num'></td>
        </tr>
      </tbody>
    </table>
    <h1>总价为:{{totalMoney}}</h1>   //计算属性
    <h1>总价为:{{total()}}</h1>  //事件调用
  </div>
  <script src="./vue.js"></script>
  <script>
    new Vue({
      el:'#app',
      data:{
        books: [
          {name:'nodejs',price:5,num:3},
          {name:'js',price:5,num:6},
          {name:'react',price:50,num:100},
          {name:'vue',price:5,num:33},
        ]
      },
      computed: {  //computed  固定的，不可变
        totalMoney: function(){   //es6语法totalMoney(){}
          let total=0;
          this.books.forEach(item => total+=item.price*item.num )  //计算总价格，this指向当前的实例
          return total
        }
      },
      methods:{   //事件处理器，事件都要写在methods下
        total(){
          let total=0;
          this.books.forEach(item => total+=item.price*item.num )
          return total
        }
      }
    })
  </script>
</body>
```

### 事件处理器
```js
<body>
  <div id="app">
    <h1 v-show='show'>hello vue</h1>
    <button v-on:click='change("hello",$event)'>切换</button>   //绑定点击事件，点击执行change方法,传入参数，如果要获得事件对象，需要传入$event
    <p>{{num}}</p>
    <button @click='num+=1'>+1</button>   //@v-on:的缩写
  </div>
  <script src="./vue.js"></script>
  <script>
    new Vue({
      el:'#app',
      data:{
        show:true,
        num:0
      },
      methods:{   //写的是一个个方法
        change(text, event){   
          this.show = !this.show   //更改show的值，直接修改就行，不像react一样this.setState({})
        }
      },
      watch: {    //监听data的数据
        num:function(val, oldval){  //当num发生变化，就会执行，接收两个参数，第一个是新的值，第二个是旧的值
          console.log(val,oldval);
        }
      }
    })
  </script>
</body>
```

### 生命周期
```js
<body>
  <div id="app">
    {{num}}
    <button @click='num+=1'>+1</button>
  </div>
  <script src="./vue.js"></script>
  <script>
    var vm = new Vue({
      el:'#app',
      data:{
        num:0
      },
      beforeCreate(){  //在实例初始化之后，数据观测(data observer) 和 event/watcher 事件配置之前被调用。
        console.log('beforeCreate');
      },
      created(){  //实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。
        console.log('created');
      },
      beforeMount(){  //在挂载开始之前被调用：相关的 render 函数首次被调用。
        console.log('beforeMount');
      },
      mounted(){ //el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。
        console.log('mounted');
      },
      beforeUpdate(){   //数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。 你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。
        console.log('beforeUpdate');
      },
      updated(){  //由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。
        console.log('updated');
      },
      beforeDestroy(){  //实例销毁之前调用。在这一步，实例仍然完全可用。
        console.log('beforeDestroy');
      },
      destroyed(){  //Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用。
        console.log('destroyed');
      }
    })
  </script>
</body>
```

### 组件
```js
<body>
  <div id="app">
    // <child></child>  //挂载到页面，下面还有另外的挂载方法介绍，挂载的时候，如果有大写，直接写成小写，如果是驼峰命名，就用-连接
  </div>
  <script src="./vue.js"></script>
  <script>
  var Son = {
    template:'<div>i am son</div>'   //template 模板
  }
  var Child = {
    data(){   //数据写成一个方法
      return {
        name:'tian'
      }
    },
    components:{  //多层嵌套
      mySon:Son
    },
    template:'<div><my-son></my-son>i an child {{name}}</div>'  //挂载mySon组件
  }
  new Vue({
    el:'#app',
    data:{
      num:0
    },
    // template:'<Child/>',   //也可以不在app中挂载，写在template中也可以，是另一种挂载方法
    components:{
      Child:Child //可以直接写成Child
    },
    render: x => x(Child)  //用render函数来挂载，还是一种挂载方法
  })
  </script>
</body>
```

## 命令行
```
# 全局安装 vue-cli
$ npm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
$ vue init webpack my-project
# 安装依赖
$ cd my-project
$ npm install
$ npm run dev
```

### atom 的vue语法高亮
language-vue
