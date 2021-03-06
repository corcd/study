## react 概念解析

### 状态提升

react中的状态提升指的是当前兄弟组件中公用的数据提取到父组件里面，包含redux

### react的生命周期

1. 初始化阶段：

    - getDefinedProps
    - getInitialState
    - componentWillMount
    - render
    - componentDidMount

2. 更新

    > 旧版本

    - componentWillReceiveProps
    - shouldComponentUpdate
    - componentWillUpdate
    - render
    - componentDidUpdate

    > 新版本

    - getDerivedStateFromProps
    - shouldComponentUpdate
    - getSnapshotBeforeUpdate
    - render
    - componentDidUpdate

3. 卸载

    - componentWillUnmount

### react获取数据方式

使用 `componentDidMount` 生命周期来请求接口获取数据

#### 为什么使用componentDieMount来获取

1. 在react初次渲染的过程中，其他生命周期中调用接口获取数据无法保证渲染，因为请求是异步的
2. `componentDidMount` 生命周期中，界面上已经渲染完毕，请求只会进行重新渲染
3. `componentWillMount` 生命周期中setState不会更新数据

### 什么是redux

redux是react的状态管理库

#### 如何使用redux

redux单向数据流，通过action->reducer->store->ReactComponent的流程将数据写入store并更新视图

其中

action是拥有type以及otherData属性的，他表示一个修改的内容，将action传入dispatch，redux会默认将我们action中对应的type所对应的reducer的内容进行相关联

reducer是一个纯函数，输入为action的type以及otherData数据，并返回新的state数据

修改完成之后会自动反馈到视图上，并更新数据数据

### 什么是mobx

mobx是一个状态管理库，内部使用`proxy`进行数据劫持，通过数据的监听获取来管理全局数据

### mobx跟redux有什么区别

mobx：

优势：

- 用法简单易上手
- 使用上更加简洁化
- 使用监听的方式能够通过.运算符直接修改对应的数据
- 获取数据不必跟redux一样通过`connect`去获取数据

缺点：

- 违反react的规范，state是可修改的
- 过于简洁，大型项目下，书写起来维护困难

redux：

优势：

- 符合react开发规范
- 便于操作追踪
- 拥有各种中间件能够提供更多的便利
- 易于维护

缺点：

- 上手难度比较大
- 书写比较复杂

### 错误边界

`getDerivedStateFromError` 或 `componentDidCatch` 实现其中任意一个方法，那么这个组件就会成为错误边界组件，当将其他组件放在他的子组件里面的时候，一旦其他组件发生错误，则会回退UI

### Portals

`Portals` 是一个能够直接将子组件构建在父组件之外的方法

`React.creaetePortals`

### 什么是context

全局上下文，是react的一个全局状态，能够通过context一路传递下来，redux也是基于context来实现的

16.8 提供了 `useContext` 可以直接使用context

> 使用context的焦虑

1. 直接使用context容易导致全局的数据崩塌（分不清数据来源）
2. 破坏组件复用性（最好不在组件中使用，通过状态提升）

### refs转发

父组件向子组件传入refs，子组件获取到ref并挂载到对应的组件上，就可以直接父组件里面获取到子组件内部的组件的refs

``` javascript

const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// 你可以直接获取 DOM button 的 ref：
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;

```

### 高阶组件

针对组件进行二次封装，封装公共业务逻辑，较少业务代码实现

用法：

1. 创建一个组件
2. children传入组件
3. 添加公共属性，公共样式，公共内容
4. 返回一个新组件（可以是原先组件并不变化）

### 集成第三方库（带dom操作）

创建一个新组件，内部引用第三方库，在render或者return中写入对应需要生成的组件，在`componentDidMount`生命周期中添加对应的库操作

可通过props传入需要的属性，然后在 `componentDidUpdate` 或 `getDerivedStateFromProps` 进行修改属性

### react的性能优化

暂无

### Profiler

react中检测组件多久渲染一次或者渲染一次需要的代价

``` javascript

<App>
    <Profiler id="Navigation" onRender={callback}>
      <Navigation {...props} />
    </Profiler>
    <Main {...props} />
</App>

```

### setState

setState是异步的，所以同时写好几个setState是不执行的，但是也是可以同步去执行setState

``` javascript

setState(() => ({
    a: '123'
}))

setTimeout(() => {
    setState({
        a: '123'
    });
})

```

以上两种方法都能够让setState同步的执行，说到这个，那就要看下为什么会出现这种情况

setState的运行机制，是存在一个事务的概念



### useState

### useEffect

