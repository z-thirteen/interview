1. react 生命周期?

  react生命周期主要分为: 挂载,更新,卸载 3个阶段。

  挂载: componentWillMount, render, componentDidMount
  更新: shouldComponentUpdate , componentWillUpdate, componentDidUpdate,componentWillReceiveProps。
  卸载: componentWillUnmount。

  16版本之后生命周期的变动

  移除:

  ```js
    componentWillMount,
    componentWillReceiveProps,
    componentWillUpdate
  ```

  新增:

  ```js
    static getDerivedStateFromProps
    getSnapshotBeforeUpdate
  ```


2. 什么情况下需要使用 shouldComponentUpdate?

    在React中，默认情况下，如果父组件数据发生了更新，那么所有子组件都会无条件更新,针对特定组件做性能优化的场景下需要使用 shouldComponentUpdate。 

3. PureComponent 和 memo? 

   PureComponent 和 memo 都是对组件的props进行一层浅比较。 类组件中用用 PureComponent, 函数组件中用memo。 

4. immutable.js?

   Immutablejs 主要用于生产不可变数据, 用于react 中主要用来避免一些不规范的state操作带来的问题，比如在setState前对state进行了更改然后再进行赋值操作，而在自组件的 SCU 中对比props。

5. React 组件如何通讯? 

   ```bash
    1. 父子组件通过 state 和 props 通讯
    2. 通过 contextAPi 通讯
    3. 通过 Redux 通讯
   ```

6. JSX本质是什么..... 

    ```
    语法糖
    ```

7. 解释React中的合成事件? 

    react 设计合成事件的目的主要有2个:

      1. 兼容浏览器监听写法 
      2. 避免大量节点绑定事件占用内存 
    
    具体方式: 

    ```bash
      将事件委托到document上, 进行统一的处理。合成事件中的 event不是原生的而是合成事件对象。
    ```

8. 触发多次setstate，那么render会执行几次。
   
  多次setState会合并为一次render, 因为setState并不会立即改变state的值，而是将其放到一个任务队列里，最终将多个state合并，一次性更新页面。

9. 当你点击每个按钮会发生什么?

  ```jsx
      import React from 'react';

      export default class TestComponent extends React.Component {

        constructor() {
          super();
          this.name = 'MyComponent';
          this.handleClick2 = this.handleClick1.bind(this);
        }

        handleClick1() {
          alert(this.name);
        }

        handleClick3 = () => alert(this.name);

        render() {
          return (
            <div>
              // 页面渲染的时候弹出 MyComponent
              <button onClick={this.handleClick1()}>click 1</button>
              // 报错找不到 this
              <button onClick={this.handleClick1}>click 2</button>
              // 弹出 MyComponent
              <button onClick={this.handleClick2}>click 3</button>
              // 弹出 MyComponent
              <button onClick={this.handleClick3}>click 4</button>
            </div>
          );
        }
      };
  ```


10. react 事件传参方式:

    1. 通过.bind() 传参
    2. 通过箭头函数传参

11. 无状态组件

    - 纯函数
    - 输入props, 输出JSX
    - 没有实例
    - 没有生命周期
    - 没有state
    
12. 受控组件和非受控组件

    - 受控组件和非受控组件主要是针对表单而言的, 通过state方式给表单设置值的是受控组件，通过ref方式操作表单元素的是非受控组件

13. portals使用场景:
    1. 父组件 overflow: hidden , 但是子组件又想展示;

14. 介绍 Render Props 以及主要使用场景:

    - 通过组件的props把state传到父组件中 
    - 路由拦截

15. setState是异步还是同步的? 
      - 在生命周期和react绑定的事件中是同步的。
      - 在 setTimeout setInterval和自定义的DOM事件中是异步的。

16. 什么是纯函数? 
    - 输出完全依靠输入

17. 列表渲染为何要用key? 
    - diff 算法中通过key 判断是否是相同节点。

18. react 异步组件? 
    - React.Suspense。使用异步组件，异步组件加载中时，显示fallback中的内容。

19. setState的流程:

  ```jsx
    shouldComponentUpdate
    componentWillUpdate
    render
    componentDidUpdate
  ``` 

20. 介绍react 中的fiber? 

    背景:

    react16 之前, 在进行组件渲染时到渲染完成整个过程是同步的, 如果需要渲染的组件比较庞大, js执行会占据主线程时间较长, 会导致页面出现卡顿的现象。为了解决这个问题,react团队经过两年的努力推出了fiber。核心是自己实现了浏览器的requestIdlCallback, 把更新的任务在不同的时间片中实现

21. setState 原理:  

    1. 把setState的参数传入加入 enqueueSetState 队列。
    2. 触发调和的过程。 进行 DOM diff 重新渲染页面。

22. redux 原理:

  概念:  redux 是一种架构模式, react-redux 是这种架构模式的实现。
  核心:  订阅发布模式。
  解决的问题是:  react状态共享和可控。

  createStore 函数接收reducer为参数, 在函数体内首先会初始化一个 listener 数组, 每次 dispatch action 的时候都把该函数放到listener数组中, 然后在subscribe 中通过forEach的方式执行每一项, 该函数会返回一个对象, 这个对象包括5个字段, 分别是 { dispatch getState subscribe replaceReducer }。

  provider 函数通过 contextApi 把创建好的 store 置于整个应用的最顶层。

  connect 函数是一个高阶组件, 所谓高阶组件, 就是返回值是函数的那么一个函数。
  接收 mapstateToprops 和 mapDispatchToProps 为参数, 用于建立顶层状态和底层组件之间的映射关系。

23. react 生命周期调用顺序?
    挂载: 
      constructor()  
      componentWillMount() 
      render() 
      componentDidMount()
    更新:
      componentWillReceiveProps()
      shouldComponentUpdata()
      componentWillUpdate()
      render()
      componentDidUpdata();
    卸载:
      componentWillUnmount();






    






    




















    














  

   
  











1. 介绍下react 的 fiber:

  背景:
    在页面元素很多, 且需要频繁刷新的情况下, react15会出现掉帧的情况。直接造成的影响就是用户感觉到页面的卡顿。针对这一问题, react团队在16.X版本中做出了相应的优化。fiber的主要目标就是实现节点的增量渲染具体原理是说将渲染工作分成多个块并将其分布到多个帧中去渲染。 





    
   
