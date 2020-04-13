![img](https://images2015.cnblogs.com/blog/593627/201604/593627-20160418100233882-504389266.png)

#### 什么是Action 

- **Action** 是把数据从应用传到 **Store** 的有效载荷
- 是 store 数据的**唯一**来源
- 通过`store.dispatch()`传递 **Action** 到 **Store**

```javascript
// type :这个是唯一标示  
const ADD_TODO = 'ADD_TODO' 

{
  type: ADD_TODO,
  text: 'Build my first Redux app' 
}
```



#### 什么是Reducer

- **Reducers** 指定了应用状态的变化如何响应 **Action**并发送到 **Store**的

#### 什么是Store

**Store** 就是把它们联系到一起的对象。Store 有以下职责：

- 维持应用的 state；
- 提供 [`getState()`](https://www.redux.org.cn/docs/api/Store.html#getState) 方法获取 state；
- 提供 [`dispatch(action)`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 方法更新 state；
- 通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 注册监听器;
- 通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 返回的函数注销监听器。