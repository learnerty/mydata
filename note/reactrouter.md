## 路由
首先要在项目内装一个包  
npm install react-router-dom --save

function App(props){}默认传入一个props  
```js
function App({title,age}){
  return (
    <div>
      {title}{age}
    </div>
  )
}
<App title='tian' age={23}/>
```

`BrowserRouter as Router  把Ccc作为App来使用,Router代替BrowserRouter`  
`BrowserRouter 需要后台服务器支撑，需要服务器配合使用，HashRouter不需要服务器支撑，在页面路径前多了/#，表示在本页面跳转，两个都是找的一个html`   
import {BrowserRouter as Router, Route, Link} from 'react-router-dom';  
Router组件本身是一个容器，所有组件都放在<Router></Router>里,只能有一个children，不能有多个节点，Route写单条的路由规则:<Route path='地址' component={组件名} />   exact严格匹配
```js
<Router>
  <div>
  <Link to='/about'>about</Link>
  <Route exact path='/' component={Home} />
  <Route path='/about' component={About} />
  </div>
</Router>
```
可以嵌入一个变量，冒号表示变量,冒号后写变量名来写变量  path='/:变量名'  可以用{props.match.params.变量名}来拿到变量的值

#### 请求重定向
<Redirect from='/old' to='/new'/>  将old页面请求跳转到new页面

<Switch></Switch> 不加的话是所有符合的都会挂载上，加上后匹配规则变为，从上往下找，成功匹配到一条就不往下找了
```js
<Switch>
  <Route exact path='/' component={Home} />
  ...
</Switch>
```

不写path任何页面都能匹配到，用来写404页面，放在最底下
```js
<Switch>
  ...
  <Route component={Not} />
  最好用：
  <Route path='/404' component={NotFound} />
  <Redirect from='*' to='/404'/>
</Switch>
```

<NavLink></NavLink> 用作导航，在当前选中页有默认的class名：active，可以用activeClassName=""来更改默认的名称，也可以activeStyle={{...: '...'}}来写行内样式

`history方法`
```js
this.props.history.push('/work')  //跳转到
this.props.history.goBack();  //返回上一级
this.props.history.go(-2);  //往回倒两步
this.props.history.goForward();  //向前一步
```

挂载到root的组件默认会被注入props，但二级组件没有被注入props，需要
```js
import { withRouter } from 'react-router';
export default withRouter(Home)
```
