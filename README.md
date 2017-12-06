#React 全栈教程

##教程环境 create-react-app

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
###实战Redux
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
###react 和 redux结合
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
### 处理异步请求
``` npm i redux-thunk --save```
在创建store时引入中间件
```
import  {applyMiddleware} from 'redux'
import { thunk } from 'redux-thunk'
const store = createStore(counter, applyMiddleware(thunk))//可以处理异步请求
```
在index.redux.js中添加异步reducer
``` erport function addAsync() {
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