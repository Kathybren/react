# React 全栈教程

## 教程环境 create-react-app

```
cnpm i -g create-react-app
create-react-app react-family
cd react-family
npm start
```
如果需要配置脚手架
```
npm run eject
```
## react基础知识
## redux基础知识
### react 是什么
1.Redux专注于状态管理，和react解耦
 
2.单一状态，单向数据流

3.核心概念： store,state, action, reducer
### 实战Redux
```npm i redux --save```
1.创建store
```import { createStore } from 'redux'```
2.编写reducer
```
function counter(state=0,action) {
    switch(action.type) {
        case 
            return state+1
        case 
            return state+1
        default 
            return 10
    }
}
```
```
const store = createStore(counter)
const init = store.getState()
console.log(init) //10
```
控制台可以看见打印初始状态10
3.派发事件 传递action
```
store.dispatch({type:'加'})
console.log(store.getState())
store.dispatch({type:'减'})
console.log(store.getState())
```
可以看出通过dispatch改变state的值
4.通过subscribe监听变化
```
functin listener(){
    const current = store.getState()
}
store.subscribe(listener)
```
### react 和 redux结合
1.把store.dispatch方法传递给组件，内部可以调用修改状态
2.subscribe订阅 render函数，每次修改都重新渲染
新建index.redux.js
```
const ADD_GUN = '加'
const JIAN_GUN = '减'
export function counter(state=0,action) {
    switch(action.type) {
        case ADD_GUN:
            return state+1
        case JIAN_GUN:
            return state-1
        default:
            return 10
    }
}
export function add() {
    return { type: ADD_GUN}
}
export function jian() {
    return { type: JIAN_GUN}
}
```
在app.js里调用dispatch
```
import React, {Component} from 'react'
class App extends Component{
    render() {
        const store = this.props.store
        const num = store.getState()
        const add = this.props.add
        const jian = this.props.jian//纯组件，只依赖付组件传递
        return(
            <div>
            <h1>当前状态是{num}</h1>
            <button onClick={()=>store.dispatch(add())}>加</button>
            <button onClick={()=>store.dispatch(jian())}>减</button>
            </div>
        )
    }
}
```
### 处理异步请求
``` npm i redux-thunk --save```
在创建store时引入中间件
```
import  {applyMiddleware} from 'redux'
import { thunk } from 'redux-thunk'
const store = createStore(counter, applyMiddleware(thunk))//可以处理异步请求
```
在index.redux.js中添加异步reducer
``` 
erport function addAsync() {
    return dispatch=>{
        setTimeout(()=>{
            dispatch(add())
        })
    }
}
```
在app.js里调用
```
const addAsync = this.props.addAsync
<button onClick={()=>store.dispatch(addAsync())}>addAsync</button>
```
## redux管理插件
1.在index.js文件里
```
import { compose } from 'redux'//组合函数
 const reduxDevtools = window.devToolsExtension?window.devToolsExtension():() => {}
 const store = createStore(counter, compose(
     applyMiddleware(thunk),reduxDevtools
 ))
 ```
## 使用react-redux
`npm install react-redux --save`
忘记`subscribe`,记住`reducer`、`action`和`dispatch`
`react-redux`提供`Provider`和`connect`两个接口来链接
### 具体使用react-redux
`Provider`组件在应用最外层，传入`store`即可，只用一次
`Connect`复制从外部获取组件需要参数
在index.js里
```
import { Provider } from 'react-redux'
ReactDOM.render(
  <Provider store = {store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```
在app.js
```
import { connect} from 'react-redux'
import {add, jian, addAsync } from './index.redux'
App = connect()(App)//装饰器可以传4个参数

const mapStateToProps=(state)=>{
    return {num: state}
}
const actionCreators = { add, jian, addAsync }
App = connect(mapStateToProps, actionCreators)(App)
```
此时不需要dispatch
```
render() {
    const num = this.props.num
    const add = this.props.add
    const jian = this.props.jian
    const addAsync = this.props.addAsync
    return (
      <div>
       <div>当前状态{num}</div>
       <button onClick={add}>加</button>   
       <button onClick={jian}>减</button>   
       <button onClick={addAsync}>addAsync</button>   
      </div>
    );
```
### 使用装饰器优化connect代码
`npm run eject`弹出个性配置
```npm i babel-plugin-transform-decorators-legacy```// 支持装饰器插件
改进connect
```
const mapStatetoProps= (state) =>{
  return {num:state}
}
const actionCreaters = {add, jian, addAsync }
```
之前
`App = connect(mapStatetoProps,actionCreaters)(App)`
现在
`@connect(mapStatetoProps,actionCreaters)`// 传入需要的属性和方法
# react-router4基础知识
### 核心概念
动态路由、Route、Link、Switch
1.安装
`npm i react-router-dom --save`
入门组件
`BrowserRouter`包裹整个应用
`Route`路由对应渲染的组件，可嵌套
`link`跳转专用
在index.js
`import { BrowserRouter, Route, Link} from 'react-router-dom'`
```
 <Provider store = {store}>
  <BrowserRouter>
  <div>
    <ul>
      <li>
        <Link to="/">首页</Link>  
      </li>
      <li>
        <Link to="page1">page1</Link>  
      </li>
      <li>
        <Link to="page2">page2</Link>  
      </li>
    </ul>
    <Route path="/" exact component={App}></Route>
    <Route path="/page1" component={Page1}></Route>
    <Route path="/page2" component={Page2}></Route>
    </div>
    </BrowserRouter>
  </Provider>,
  ```