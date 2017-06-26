如果我们整个项目只用一个 reducer ，随着状态树变大，reducer 就会变得很复杂。那么如何进行 reducer 的拆分呢？这就涉及到 rootReducer/combineReducers 这些技巧了。


拆分后，要达成两个目标：

- 每个 reducer 负责一类数据，所以每个 reducer 都不会很大
- 每个 reducer 修改的 state 值，都**不是**整个状态树


### 创建 reducers/index.js 文件

```
let defaultState = {
  comments: ['xxx1', 'hello2'],
  likes: 0
}

function rootReducer(state = defaultState, action) {
  switch (action.type) {
    case 'ADD_COMMENT':
      return {...state, comments: [...state.comments, action.comment]};
    case 'INCREMENT_LIKE':
      return {...state, likes: state.likes + 1 }
    default:
      return state
  }
}

export default rootReducer
```
创建redux/store.js
```
import { createStore } from 'redux'
import rootReducer from './reducers'

let store = createStore(rootReducer)
export default store
```

### 拆出两个 reducer

每个 reducer 的 state 默认值都只是自己对应的那一部分数据。

```
import { combineReducers } from 'redux'

let comments = ['xxx1', 'hello2']

function commentReducer(state= comments, action) {
  switch (action.type) {
    case 'ADD_COMMENT':
      return [...state, action.comment]
    default:
      return state
  }
}

let likes = 0

function likeReducer(state = likes, action) {
  switch (action.type) {
    case 'INCREMENT_LIKE':
      return state + 1
    default:
      return state
  }
}

const rootReducer = combineReducers({
  comments: commentReducer,
  likes: likeReducer
})


export default rootReducer
```
注意： combine 的意思是“合并”

```
const rootReducer = combineReducers({
  comments: commentReducer,
  likes: likeReducer
})
```

上面的参数中，`comments` 是需要被更新的 state ，commentReducer 是负责更新它的 reducer ，同理 likes 和 likeReducer 也是这样的关系。最终的整个 App 的状态树，就是

```
{
  comments,
  likes
}
```

后续在各个组件的 mapStateToProps(state) 函数中的 state 就是上面的这个值。


### 参考资料

- [官方的 combineReducers()文档](http://cn.redux.js.org/docs/recipes/reducers/UsingCombineReducers.html)
