# Model

Model 负责处理业务逻辑，连接View层和Service层，为 Page 做数据、状态的读写、变换、暂存等。

# DVA

dva是一个基于redux和redux-saga的数据流方案，为了简化开发体验，还额外内置了react-router和fetch，所以也可以理解为一个轻量级的应用框架。简单来说，它是基于React-Router、Redux和Redux-saga封装的一个Model层解决方案。

## 核心概念

一个简单的model示例如下：

```
app.model({
  namespace: 'todoList',
  state: [],
  effects: {
    *query({ _ }, { put, call }) {
      const rsp = yield call(queryTodoListFromServer);
      const todoList = rsp.data;
      yield put({ type: 'save', payload: todoList });
    },
  },
  reducers: {
    save(state, { payload: todoList }) {
      return [...state, todoList];
    },
  },
});
```

- namespace：model的命名空间，只能用字符串。一个大型应用可能包含多个 model，通过namespace区分。
- State：一个对象，保存整个应用状态
- View：React 组件构成的视图层
- Action：一个对象，描述事件
- connect方法：一个函数，绑定 State 到 View
- dispatch方法：一个函数，发送 Action 到 State
- reducers：用于处理同步操作，可以修改 state，由 action 触发。reducer 是一个纯函数，它接受当前的 state 及一个 action 对象。action 对象里面可以包含数据体（payload）作为入参，需要返回一个新的 state。
- effects：用于处理异步操作（例如：与service层交互）和业务逻辑，也是由 action 触发。但是，它不可以修改 state，要通过触发 action 调用 reducer 实现对 state 的间接操作。

## State 和 View

State 是储存数据的地方，收到 Action 以后，会更新数据。

View 就是 React 组件构成的 UI 层，从 State 取数据后，渲染成 HTML 代码。只要 State 有变化，View 就会自动更新。

## Action

Action 是用来描述 UI 层事件的一个对象。

```
{
  type: 'add',
  payload: this.form.data
}
```

## connect 方法

connect 是一个函数，绑定 State 到 View。

```
import { connect } from 'dva';

function mapStateToProps(state) {
  return { todos: state.todos };
}
connect(mapStateToProps)(App);
```

connect 方法返回的也是一个 React 组件，通常称为容器组件。因为它是原始 UI 组件的容器，即在外面包了一层 State。

connect 方法传入的第一个参数是 mapStateToProps 函数，mapStateToProps 函数会返回一个对象，用于建立 State 到 Props 的映射关系。

## dispatch 方法

dispatch 是一个函数方法，用来将 Action 发送给 State。

```
dispatch({
  type: 'add',
  payload: this.form.data
})
```

dispatch 方法从哪里来？被 connect 的 Component 会自动在 props 中拥有 dispatch 方法。

## Reducer

Reducer是Action处理器，用来处理同步操作，可以看做是state的计算器。它的作用是根据Action，从上一个State算出当前State。

## Effect

Action处理器，处理异步动作，基于Redux-saga实现。Effect指的是副作用。根据函数式编程，计算以外的操作都属于Effect，典型的就是 I/O 操作、数据库读写。

Effect是一个Generator函数，内部使用yield关键字，标识每一步的操作（不管是异步或同步）。

**call 和 put**

dva提供多个effect函数内部的处理函数，比较常用的是call和put。

- call：执行异步函数
- put：发出一个Action，类似于dispatch。（调用另一个effect或reducer）
