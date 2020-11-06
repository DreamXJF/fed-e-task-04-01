# fed-e-task-04-01
React第一节
1. 请简述 React 16 版本中初始渲染的流程
答：初始渲染分为两个过程：
1、创建真实DOM 结点
2、将DOM结点插入到DOM树中
创建真实DOM结点的过程称为挂载，以ReactReconciler.mountComponent作为入口进行递归挂载，不断调用内部实例的mountComponent方法，最后得到一棵DOM子树。不断调用内部实例的mountComponent方法，最后得到一棵DOM子树。
通过挂载后，再将DOM子树插入到容器中，初次渲染完成，Hello World显示在屏幕上。


2. 为什么 React 16 版本中 render 阶段放弃了使用递归
答：Reconciler 层是调度任务的核心，旧版本的调度方式（递归）中，当我们调用setState更新页面的时候，React 会遍历应用的所有节点，diff计算出差异，然后再更新 UI。整个过程是一气呵成，不能被打断的。如果页面元素很多，整个过程占用的时机就可能超过 16 毫秒，就容易出现卡顿掉帧的现象。


3. 请简述 React 16 版本中 commit 阶段的三个子阶段分别做了什么事情
答：React 16.3版本中去掉了以下的三个生命周期：

 1.componentWillMount

 2.componentWillReceiveProps

 3.componentWillUpdate

新增了两个生命周期方法：

static getDerivedStateFromProps

getSnapshotBeforeUpdate

static getDerivedStateFromProps
  触发时间：在组件构建之后(虚拟dom之后，实际dom挂载之前) ，以及每次获取新的props之后。

  每次接收新的props之后都会返回一个对象作为新的state，返回null则说明不需要更新state.

  与componentDidUpdate一起使用，可以覆盖componentWillReceiveProps的所有用法

getSnapshotBeforeUpdate
   触发时间: update发生的时候，在render之后，在组件dom渲染之前。

   返回一个值，作为componentDidUpdate的第三个参数。

  配合componentDidUpdate, 可以覆盖componentWillUpdate的所有用法。


4. 请简述 workInProgress Fiber 树存在的意义是什么
workInProgress Fiber就是重新实现的堆栈帧，本质上Fiber也可以理解为是一个虚拟的堆栈帧，将可中断的任务拆分成多个子任务，通过按照优先级来自由调度子任务，分段更新，从而将之前的同步渲染改为异步渲染。
react内部有自己的优先级判断逻辑，比如动画，用户交互等任务优先级就明显要高。
Fiber的目标
1、将耗时长可中断的任务拆分成多个子任务
2、对正在做的工作调整优先级，可以重做，复用上次的结果

Fiber特性
1、增量渲染，把一个渲染任务拆分成多个子任务，平均到多个渲染帧中执行，每次只做一小段，做完后就把时间控制权上交给主线程
2、在渲染更新时，能够暂停，复用任务
3、不同类型的任务具有不同的优先级
4、并发方面的其他能力
5、错误边界