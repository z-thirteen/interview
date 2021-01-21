
### 1. 你怎么认识vuex的?

  [vuex 源码解读](https://juejin.cn/post/6906009895162609672)

### 2. vuex中的mutation和action的区别？

  ```js
    mutation用来处理同步状态的更改，action用来处理异步状态的更改。
  ```

### 3. 如何解决vuex持久化的问题? 

  ```js
    引入插件的方式解决, 本质是存在 sesstionStorage中。
  ```

###  4. nextTick实现原理?

  Vue.nextTick 用于在下次 DOM 更新循环结束之后执行延迟回调。nextTick 会根据执行环境分别尝试采用
  
  ```js
    Promise
    MutationObserver
    setImmediate
    setTimeout
  ```

  的方式来执行异步回调

  ```js
  let callbacks = [];
  let pending = false;
  function nextTick (cb) {
      callbacks.push(cb);
      if (!pending) {
          pending = true;
          setTimeout(flushCallbacks, 0);
      }
  }

  function flushCallbacks () {
      pending = false;
      const copies = callbacks.slice(0);
      callbacks.length = 0;
      for (let i = 0; i < copies.length; i++) {
          copies[i]();
      }
  }
  ```
  主体实现逻辑是把所有的回调都放到一个队列中。判断当前DOM环执行完成之后,拿出队列中的回调统一执行。

### 5. vue 中事件绑定的原理? 

  在实例化vue的时候会把 methods 作为参数传入, 并把对应的监听函数挂载在vue的实例上，编译模版的时候给对应的节点添加上对应的事件。

### 6. v-if和v-show的区别? 

  v-if 会根据条件判断节点的增加或者删除, v-show 通过css display控制节点的显示/隐藏。
  v-show  不能用于 template 模版上。 


### 7. 组件为何采用异步渲染? 

  因为vue是组件级更新视图，每一次update都要渲染整个组件，为了提高性能，采用了队列的形式，把所有的Watcher都放到一个队列中，然后统一调用nextTick 方法进行更新渲染（有且只调用一次）。

### 8. vue组件的生命周期。

  ```bash
    beforeCreate  在实例初始化之后，数据/事件/watcher 被配置之前调用。
    created       在实例创建完成后被立即调用，完成 数据/事件/watcher的配置。
    beforeMount   在挂载开始之前被调用。
    mounted       实例被挂载后调用。如果你希望等到整个视图都渲染完毕，可以在
                  mounted 内部使用 vm.$nextTick。
    beforeUpdate  数据更新时调用。
    updated       DOM 重新渲染之后会调用该钩子。
    activated     被 keep-alive 缓存的组件激活时调用。
    deactivated   被 keep-alive 缓存的组件停用时调用。
    beforeDestroy 实例销毁之前调用。在这一步，实例仍然完全可用。
    destroyed     实例销毁后调用。
    errorCaptured 当捕获一个来自子孙组件的错误时被调用。
  ```
  
### 9. Ajax 请求放在哪个生命周期中？
  mounted中。 

  ```bash
    Computed本质是一个具备缓存的watcher，依赖的属性发生变化就会更新视图。 适用于计算比较消耗性能的计算场景。当表达式过于复杂时，在模板中放入过多逻辑会让模板难以维护，可以将复杂的逻辑放入计算属性中处理
  ```

### 10. 何时需要使用 beforeDestroy ?

    页面卸载前清除当前页面中的 定时器/ 事件。

### 11. 谈谈你对 keep-alive 的了解？
    
    <keep-alive> 是vue的一个内置组件, 用来实现组件的缓存，它自身不会渲染DOM元素,
    组件内部的属性包括

    abstract: 用于在建立父子组件链的时候过滤到 keep-alive组件。
    props
      include: patternTypes  被匹配到的组件会被缓存
      exclude: patternTypes  被匹配到的组件不会被缓存
      max: [String, Number]  控制缓存的个数
    created 
      钩子里定义了 this.cache 和 this.keys 用于缓存组件
    render
      处理渲染逻辑
    组件是在patch过程中被真正渲染的。

### 12. vue 父子组件生命周期调用顺序?

    * 加载渲染过程

      父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted

    * 子组件更新过程
      父beforeUpdate->子beforeUpdate->子updated->父updated

    * 父组件更新过程
      父beforeUpdate->父updated

    * 销毁过程
      父beforeDestroy->子beforeDestroy->子destroyed->父destroyed

### 13. 为什么 v-if和v-for 不能连用?
    主要出于性能考虑 vue 给v-for的优先级比v-if的更高。

### 14. v-model的原理？

    v-model本质就是一个语法糖, 它通常用于表单元素中, 在vue的编译阶段会把元素分为节点类型元素和文本类型元素。在节点编译的时候获取到节点上的指令和节点的value值，对节点内容进行填充。

### 15. vue 组件是如何通信的？

    1. props/$emit, 
    2. .sync/update, 
    3. provide/inject, 
    4. $parent/$children; 
    5. refs,
    6. $attrs/$listeners,
    7. EventBus,
    8. Vuex

### 16. 什么是作用域插槽 ? 
    父组件中通过 slot-scope 获取子组件中绑定的属性值。

### 17. vue 模版编译原理? 

    compile 编译主要可以分成 parse optimize 与 generate 三个阶段。
    parse:
      会用正则等方式将 template 模板中进行字符串解析, 得到指令、class、style等数据，创建 AST。
    optimize: 
      给AST中的节点增加static属性，用于标记是否是静态节点, 如果是静态节点的话在patch的过程中会被忽略，从而达到优化的目的。
    generate:
      generate 会将 AST 转化成 真实的DOM。

### 18. new Vue做了啥/MVVM 原理?

    可以从两个层面来看：
      1. new Function 的过程:
         首先创建一个空对象, 把function的protorype指向空对象_prop_,用call的方式执行function最后返回这个对象。

      2. new Vue 的过程:

        这其中有5个比较重要的概念:

        ```js
          1. vue 类
          2. Observer 类
          3. Compiler 类
          4. Dep 类
          5. Watcher类
        ```

        整体的实现思路:

         1. 初始化Vue对象  将 el 和 data 挂载到vue实例上
         2. 实例化 Observer 通过 Object.defineProperty 对data中的数据进行劫持,在get方法中添加订阅，set方法中当目标值发生改变的时候触发Dep中的notify
         3. 实例化 Compiler 进行模版编译, 编译的同时给节点添加数据监听，当数据改变的时候触发相应试图的更新。
            Compiler 编译主要可以分成 parse optimize 与 generate 三个阶段。

### 19. 简述 vue 中的 diff原理。  

    diff 算法主要用于组件更新阶段对比新树和老树的节点的差异。 diff  采用的是先序深度优先的策略对比两颗树的差异。 key 主要意义是为了以最小的代价来更新dom 就是最小化性能对资源池（老树）的操作。diff算法的时间复杂度是 O(n)。

    1. 虚拟dom的创建过程 先序深度优先。
    2. diff 对比的过程只会对同级进行相应的比较。
    3. 打补丁的过程是 从下到上 倒着来的。
    4. 虚拟dom的意义是 我们把对dom 的操作 都交由他来处理 避免了一些不必要的dom 回流和重绘 , 也就是我们在对dom 操作时读写分离的重要性 以及一些 性能优化的注意点。

### 20. 用vNode来描述一个DOM结构?

    ```js
      {
          tag: 'span',
          data: {
              /* 指令集合数组 */
              directives: [
                  {
                      /* v-show指令 */
                      rawName: 'v-show',
                      expression: 'isShow',
                      name: 'show',
                      value: true
                  }
              ],
              /* 静态class */
              staticClass: 'demo'
          },
          text: undefined,
          children: [
              /* 子节点是一个文本VNode节点 */
              {
                  tag: undefined,
                  data: undefined,
                  text: 'This is a span.',
                  children: undefined
              }
          ]
      }
    ```
### 23. Vue 中 computed 和 watch 有什么区别?

    缓存: computed 支持缓存, watch不支持。

    使用场景:
    
      如果一个数据依赖其他数据的情况下适合用 computed 
      需要在数据变化时做一些事情的情况下适合用 watch 




  
    



    

    



    



      









  

