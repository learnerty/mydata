# redux-thunk

thunk 的意思是”模式转换“。

### Action Creator

Action Creator 不是 thunk 的功能，是 redux 自带功能，但是 thunk 发挥作用，就是在 action creator 里面。所以我们先不安装 redux-thunk ，先单纯用一下 Action Creator 。

官方给的定义是：

>functions that create actions

Action Creator 就是用来创建 Action 的函数。类似下面这样

```
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```

他是一个函数，同时他的返回值是一个 action 。

### 引入 redux-thunk

有了 redux-thunk ，redux 的工作模式就会发生一定的“转换”，具体来讲就是[官网](https://github.com/gaearon/redux-thunk)上说的


>Redux Thunk middleware allows you to write action creators that return a function instead of an action

翻译：Redux-thunk 中间件允许我们的 action creator 不直接返回 action ，而是去返回一个函数。


这样做的好处有两个：

- 可以直接在 Action Creators 里面，运行 dispatch
- 另外，更重要的就是为了实现异步操作

这个两点，稍后我们会具体演示。现在先来安装加载 Redux-Thunk 。

### 安装 Redux-Thunk

先来装包：

```
npm install --save Redux-Thunk
```

安装的版本是：

```
"redux-thunk": "^2.2.0"
```

然后修改代码，到 store.js 中，

```
import { createStore, applyMiddleware } from 'redux';
```

- applyMiddleware: 使用中间件


### 使用 dispatch

先来使用前面提过的第一个好处。这样 commentBox 组件里面，就不需要导入 store ，也不用 store.dispatch 了。


首先，有了 redux-thunk 之后，actions/commentActions.js 中，就可以使用下面的语法

```
export function addComment() {
  return dispatch => {
    dispatch({type: 'ADD_COMMENT'})
  }
}
```

上面的 action creator 中，不仅仅有 action ，而且直接 dispatch 了这个 action 。

但是，如果到 commentBox 组件中，直接导入 addComment 进行使用，那么 Action Creator 中的 dispatch 语句是不会执行的。

解决方法就是进行 connect ，把这个函数跟 redux 的 store 连接起来。具体做法是，

commentBox.js 中

```
export default connect(mapStateToProps, {addComment})(CommentBox);
```

然后，后面使用 `this.props.addComment()` 来呼叫执行 Action Creator 。

可以看到 reducer 中有输出了，可见 dispatch 确实工作了，action 被发出了。


### 异步操作

Action Creator 中能够使用 dispatch ，目的主要就是服务异步操作。

注：什么是异步操作（ async ）？实现异步操作的最主要的形式是？主要形式，就会回调函数（ call back ），更为流行的是使用 promise（axios） 。异步操作的很大的特点，就是非阻塞（ no-blocking ）。


没有 thunk 之前，一旦用户点按钮，action 立即被发出，reducer 立即执行，store 立即改变。这个是 redux 默认工作机制。但是这种机制在很多情况下是不能满足要求的，例如，用户点按钮之后，action 中要携带的数据需要从网络上读取的这种情况。有了 thunk 之后，即使要等很长时间也没有问题，因为 dispatch 操作可以**等待**网络请求结束之后再去执行。
