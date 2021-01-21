### 1. Set 和 WeakSet 的区别？

  Set 对应数据结构中的集合, 成员唯一且无序 且提供对数据的增删改查方法。
  WeakSet 成员只能是弱应用类型的对象。一般用于存放DOM节点
  Map 对应数据结构中的字典, 本质上是键值对的集合。各种类型的值都可以当作键。
  WeakMap 只接受对象作为键名, 键名所指向的对象，不计入垃圾回收机制。

### 2. 手写实现 Promise.all: 

  ```js
    const PromiseAll = (promises)=> {
      if(!Array.isArray(promises)){
        throw new Error('params typeError!');
      } 
      const length = promises.length;
      const resArr = [];
      let count = 1;
      return new Promise((resolve, reject)=>{
        for(let i =0;i<length;i++){
          Promise.resolve(promises[i]).then((res)=>{
            count++;
            resArr.push(res);
            if(count===length){
              resolve(resArr);
            } 
          },(res)=>{
            reject(res);
          })
        }
      })
    }
  ```

### 3. 数组去重不用 Set

   * indexOf方法去重1
   * 对象属性去重

### 4. 使如下判断成立

   ```js
    if (a === 1 && a === 2 && a === 3) {
      console.log('object');
    };
   ```

   解:

   ```js

    let value = 0;

    Object.definedPrototype(window,'a',{
      get(){
        return value+=1;
      }
    });

    if(a===1&&a===2&&a===3){
      console.log('object');
    }
    
   ```

### 5. 求数组最大值

  ```js
   // way1 
   const arr = [1,2,3,4,5,6,7];
   const maxNumber = Math.max(...arr);
   console.log('maxNumber', maxNumber);
   // way2 
   const arr = [1,2,3,4,5,6,7];
   const maxNumber = Math.max.apply(null,arr);
   console.log('maxNumber', maxNumber);
  ```

### 6. js 实现new 操作符

 ```js

  function Persion(name){
    this.name = name;
  };

  Persion.prototype.getName = function(){
    return this.name;
  };

  function newFunc(){
    const obj = {};
    const Constructor = [].shift.call(arguments);
    obj.__proto__ = Constructor.prototype;
    const res = Constructor.apply(obj, arguments);
    return typeof res==="object"?res:obj;
  };
  
  const object = newFunc(Persion,'ceshi');
  console.log('object', object.getName());

```
### 7. js中判断类型的方式有哪些？

   1. instanceof  判断引用类型值

      instanceof 的类型检验方式:  判断对象的原型链上是否包含目标类型的prototype属性
      
      ```js
        源码实现:
        function myInstance(instanceofObj,targetType){
          const prototype = targetType.prototype;
          const instance = instanceofObj.__proto__;
          while(instance){
            if(instance===prototype){
              return true;
            }
            instance = instance.__proto__;
          }
          return false;
        }
      ```
      

   2. typeof      判断基本类型的值

   3. Object.prototype.toString.call  判断任何值
      
      原理: 因为Object类是所有子类的父亲, 所以任何类型的对象都可以用 Object.prototype.toString.call 的方式返回该对象的字符串表示。

### 8. Promise 之问(四) —— 实现Promise的 resolve、reject 和 finally；

### 9. js 实现数组展平: 
  let array = [1, 2, [3, 4], [5, [6, 7]]];
   ```js
      // 1. 用 reduce 函数;

      function flatten(array){
        return array.reduce((prev,current)=>{
          return prev.concat(Array.isArray(current)?flatten(current):current)
        },[])
      };

      console.log('flatten', flatten(array));

      // 2. 用 es6 自带的flat函数; 
      const flatArray = (array)=>{
        return array.flat(Infinity);
      }


      // 3. 用while 循环加扩展运算符;
      const flatFunc = (array) => {
        while(array.some(Array.isArray)){
          array = [].concat(...array);
        }
        return array;
      };

      // 4. eval的方式。
      const flatten = (arr) => {
        return eval(`[${arr}]`);
      }

   ```

### 10. js实现一个完整的深拷贝:

    1. JSON.parse(JSON.stringify()) 可以实现但是会存在两个问题:

      1> 特殊对象无法克隆, 比如 RegExp, Date ,Set , Map 
      2> 无法拷贝函数

### 11. js实现一个流程控制函数:
      实现一个LazyMan，可以按照以下方式调用:
      LazyMan(“Hank”)输出:
      Hi! This is Hank!

      LazyMan(“Hank”).sleep(10).eat(“dinner”)输出
      Hi! This is Hank!
      //等待10秒..
      Wake up after 10
      Eat dinner~

      LazyMan(“Hank”).eat(“dinner”).eat(“supper”)输出
      Hi This is Hank!
      Eat dinner~
      Eat supper~

      LazyMan(“Hank”).sleepFirst(5).eat(“supper”)输出
      //等待5秒
      Wake up after 5
      Hi This is Hank!
      Eat supper
      以此类推。
      复制代码
  ```js
    class Lazyman{
      constructor(name){
        this.tasks = [];
        const task =()=>{
          console.log(`Hi! This is ${name}`);
          this.next();
        } 
        this.tasks.push(task);
        setTimeout(()=>{
          this.next();
        })
      }
      next(){
        const task = this.tasks.shift();
        task&&task();
      }
      _sleepWrapper(timer,isFirst){
        const task = ()=>{
          setTimeout(()=>{
            console.log(`Wake up after ${timer}`);
            this.next();
          },timer*1000)
        }
        if(isFirst){
          this.tasks.unshift(task);
        }else{
          this.tasks.push(task);
        }
      }
      sleep(timer){
        this._sleepWrapper(timer,false);
        return this;
      }
      sleepFirst(timer){
        this._sleepWrapper(timer,true);
        return this;
      }
      eat(food){
        const task = ()=>{
          console.log(`Eat ${food}`);
          this.next();
        }
        this.tasks.push(task);
        return this;
      }
    };

    function lazyman(name) {
      return new Lazyman(name);
    };

    lazyman("Hank").sleep(10).eat("dinner");
```

### 题目五: 写一个函数 curry 完成下列输出（函数柯里化）:

```js

let curry = (fn) => {
  const resFn = (...allArgs) => allArgs.length>=fn.length
  ? fn(...allArgs)
  : (...args)=>resFn(...allArgs,...args);
  return resFn
};

const foo = curry((a,b,c,d)=>{console.log(a,b,c,d);});

foo(1)(2)(3)(4); // 1,2,3,4 

foo(1)(2)(3);    // 无输出

const f = foo(1)(2)(3);

f(5) //1,2,3,5

```
### 题目6  手写请求超时方法 
fetch 

```js
  function httpAjax({params,timer}) {
  const controller = new AbortController();
  const signal = controller.signal;
  const timeOutPromise = new Promise((resolve, reject) =>{
    setTimeout(() => {
      signal.abort();
      resolve('超时返回结果');
    },timer);
  });
  const promises = ()=>{
    fetch(params.url,{
      signal
    });
  };
  promises.race([timeOutPromise,promises]).then(res=>{
    console.log('res', res);
  }).catch(error=>{
    console.log('error', error);
  });
}
```
ajax 

```js
function httpAjax({params, intervalue}){
  return new Promise((resolve, reject) =>{
    const xhr = new XMLHttpRequest();
    xhr.open(params.url);
    xhr.send(params.data);
    xhr.onreadystatechange = ()=>{
        if(xhr.readyState===4 &&xhr.status===200){
          resolve(xhr.responseText);
        }
    };
    setTimeout(()=>{
      xhr.abort();
      reject(new Error("请求超时"));
    },intervalue)
  })
}
```