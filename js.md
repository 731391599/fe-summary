## 数据类型

- 类型
  - 基本类型 Number Boolean String Undefined Null Symbol Bigint
  - 复杂类型 Object Function Array
- 储存方式
  - 基本类型 储存在栈中
  - 复杂类型 值储存在堆中 对应的地址储存在栈中

## 数组方法

- push/unshift 尾部添加元素/头部添加元素 返回数组长度

- pop/shift 尾部删除元素/头部删除元素 返回被删除元素

- concat 合并多个数组 返回新数组

- slice(begin, end)  包含begin不包含end 返回新数组（浅拷贝）

- splice 删除/替换/新增

- indexOf/lastIndexOf 返回索引 未找到返回-1

- includes 找到返回true 否则false

- find 找到匹配的第一个元素 否则返回undifined

- findIndex

- reverse 倒序 改变元素组

- sort

- join 拼接并返回字符串

- some/every 有一个元素通过就返回true/所有都通过返回true

- forEach 返回undifined 不可被链式调用 无法跳出循环除非抛出异常

- filter/map 返回新的数组

- reduce/reduceRight callback(acc累加器/cur/当前值/idx当前索引/array当前数组) initialValue调用cb时第一个参数，没有则使用数组中第一个元素

  ```js
  // 使用reduce模拟map
  Array.prototype._map = function(cb,thisValue = null){
    const res = []
    const arr = this
    arr.reduce((acc,cur,idx,arr)=>{
      res.push(cb.call(thisValue,cur,idx,arr))
    },null)
    return res
  }
  ```

- flat(depth) 扁平化 depth默认值1

- flatMap

  ```js
  let arr1 = ["it's Sunny in", "", "California"];
  arr1.map(x => x.split(" "));
  // [["it's","Sunny","in"],[""],["California"]]
  arr1.flatMap(x => x.split(" "));
  // ["it's","Sunny","in", "", "California"]
  ```

- entries/keys/values/@@iterator]()

- copyWithin 浅拷贝数组的一部分到数组中另一个位置 不改变原数组长度

- fill 固定值填充一个数组

- toString/toLocaleString

## 字符串

- concat
- slice
- substr
- substring
- trim/trimLeft/trimRight
- repeat
- padStart/padEnd
- toLowerCase/toUpperCase
- charAt/indexOf/startWith/includes
- split
- match/search/replace
- ...

## 深拷贝/浅拷贝

- 浅拷贝

  - Object.assign

  - slice/concat

  - 拓展运算符

  - 遍历

    ```js
    function shallowClone(target) {
      const res = {}
      for(let key in target) {
        if(target.hasOwnProperty(key)) {
          res[key] = target[key]
        }
      }
      return res
    }
    ```

- 深拷贝

  - JSON.stringfy

  - 遍历递归

    ```js
    function getDataType(target) {
      return Object.prototype.toString.call(target).toLowerCase().slice(8,-1)
    }
    function getDataType(target) {
        return Object.prototype.toString.call(target).toLowerCase().slice(8, -1)
    }
    function deepClone(target) {
        const hash = new Map()
        return clone(target, hash)
    }
    function clone(target, hash) {
        const dataType = getDataType(target)
        let res
        if (hash.get(target)) {
            return hash.get(target)
        } else {
            hash.set(target, target)
        }
        switch (dataType) {
            case 'object':
                res = {}
                for (let key in target) {
                    if (target.hasOwnProperty(key)) {
                        res[key] = clone(target[key], hash)
                    }
                }
                break;
            case 'array':
                res = []
                for (let i = 0; i <= target.length; i++) {
                    res.push(clone(target[i], hash))
                }
                break;
            default:
                res = target
                break;
        }
        return target
    }
    ```

## 闭包

  - 定义
    - 忍者秘籍：闭包允许函数访问并操作函数外部的变量。
    -  红宝书：闭包是指有权访问另外一个函数作用域中的变量的函数。
    -  MDN ：闭包是指那些能够访问自由变量的函数。这里的自由变量是外部函数作用域中的变量。
  - 优点
    - 创建私有变量
    - 延长变量的生命周期
  - 缺点
    - 内存泄露

## 作用域与作用域连

- 作用域定义：当前的执行上下文。值和表达式在其中 "可见" 或可被访问到的上下文
- 作用域分类
  - 全局作用域
  - 函数/局部作用域
  - 块级作用域
    - 暂时性死区 {} let/const
- 词法作用域/静态作用域 创建时决定了确定了其作用域
- 作用域链 从当前作用域向上查找一直到全局作用域

## 原型与原型链

- 定义：JavaScript 常被描述为一种**基于原型的语言 (prototype-based language)**——每个对象拥有一个**原型对象**，对象以其原型为模板、从原型继承方法和属性。原型对象也可能拥有原型，并从中继承方法和属性，一层一层、以此类推。这种关系常被称为**原型链 (prototype chain)**

  ```js
  function Test(){}
  let t = new Test()
  
  // t
  // t是构造函数Test的一个实例
  // t.__proto__ 指向 Test.prototype
  // 而Test.prototype又是一个对象 是Object构造的一个实例
  // 因此Test.prototype.__proto__ 指向 Object.prototype
  // 最终 Object.prototype.__proto__ 指向null
  
  // Test
  // Test函数 是Function构造的一个 实例
  // Test.__proto__ 指向 Function.prototype
  // Function.prototype也是一个对象 指向Object.prototype
  // 最终 Object.prototype.__proto__ 指向null
  ```

## 继承

- 继承可以使得子类具有父类别的各种属性和方法，而不需要再次编写相同的代码

- 在子类别继承父类别的同时，可以重新定义某些属性，并重写某些方法，即覆盖父类别的原有属性和方法，使其获得与父类别不同的功能

- 实现继承的方式

  - 原型链继承 实例使用的是同一个原型对象互相影响

    ```js
    function Parent() {
      this.arr = [1,2,3]
    }
    function Child() {}
    Child.prototype = new Parent()
    let child1 = new Child()
    let child2 = new Child()
    child1.arr.push(4)
    console.log(child2.arr) // [1,2,3,4]
    ```

  - 构造函数继承 无法继承父类原型上的方法

    ```js
    function Parent() {
      this.arr = [1,2,3]
    }
    Parent.prototype.getName = function(){
      console.log('hello')
    }
    function Child() {
      Parent.call(this)
    }
    let child1 = new Child()
    child1.getName() //  TypeError:child1.getName is not a function
    ```

  - 组合继承 Parent执行了2次

    ```js
    function Parent() {
      this.arr = [1,2,3]
    }
    Parent.prototype.getName = function(){
      console.log('hello')
    }
    function Child() {
      Parent.call(this)
    }
    Child.prototype = new Parent()
    Child.prototype.constructor = Child;
    ```

  - 原型式继承 多个实例的引用类型属性指向相同的内存

    ```js
    const parent = {
      arr:[1,2,3],
      getName(){
        console.log('hello')
      }
    }
    // 或者使用 Object.create()
    function createObj(obj){
      function F(){}
      F.prototype = obj;
      return new F();
    }
    
    let child1 = createObj(parent)
    let child2 = createObj(parent)
    child1.getName() // hello
    child1.arr.push(4)
    console.log(child2.arr) // [1, 2, 3, 4]
    ```

  - 寄生式继承 同上

    ```js
    const parent = {
      arr:[1,2,3],
      getName(){
        console.log('hello')
      }
    }
    // 或者使用 Object.create()
    function createObj(obj){
      function F(){}
      F.prototype = obj;
      return new F();
    }
    function clone(original){
      let clone = createObj(original);
       clone.getTest = function() {
         console.log('test')
       }
      return clone
    }
    let child1 = clone(parent)
    let child2 = clone(parent)
    
    child1.getTest() // test
    child1.arr.push(4)
    console.log(child2.arr)  //  [1, 2, 3, 4]
    ```

  - 寄生组合式继承 extends的实现方式

    ```js
    function Parent() {
      this.arr = [1,2,3]
    }
    Parent.prototype.getName = function(){
      console.log('hello')
    }
    function Child() {
      Parent.call(this)
    }
    Child.prototype = Object.create(Parent.prototype);
    Child.prototype.constructor = Child;
    
    let child1 = new Child()
    let child2 = new Child()
    
    child1.getName() // hello
    child1.arr.push(4)
    console.log(child2.arr) // [1, 2, 3]
    ```

## this

- 运行时绑定 函数的调用方式决定了 `this` 的值 
- 关键字是函数运行时自动生成的一个内部对象，只能在函数内部使用，总指向调用它的对象
- 绑定规则
  - 默认绑定
  - 隐式绑定
  - 显式绑定
  - new绑定

## new原理

- 原理

  - 创建一个新对象 原型指向构造函数原型对象
  - 将构建函数的this指向新对象
  - 根据返回值判断

- 实现

  ```js
  function _new(Fn) {
    const fn = Object.create(Fn.prototype)
    const args = [...arguments].slice(1)
    const res = Fn.apply(fn,args)
    return typeof res === 'object' ? res : fn
  }
  ```

## call/apply/bind原理

- call

  ```js
  Function.prototype._call = function(ctx = window) {
  	const fn = this
    ctx.fn = this
    const args = [...arguments].slice(1)
    const res = ctx.fn(...args)
    delete ctx.fn
    return res
  }
  ```

- apply

  ```js
  Function.prototype._apply = function(ctx = window,args) {
    const fn = this
    ctx.fn = this
    const res = ctx.fn(...args)
    delete ctx.fn
    return res
  }
  ```

- bind

  ```js
  Function.prototype._bind = function(ctx) {
    const fn = this
    const args1 = [...arguments].slice(1)
    const fNOP = function () {};
    const bindFn = function (...args2) {
      return fn.apply(this instanceof fn? this :ctx,[...args1,...args2])
    }
   	fNOP.prototype = this.prototype;
    bindFn.prototype = new fNOP();
    return bindFn
  }
  ```

## 执行上下文与执行栈

- 执行上下文
  - 全局执行上下文
  - 函数执行上下文 函数被调用时会创建 每次调用将会创建一个新的执行上下文
  - eval函数执行上下文
- 周期
  - 创建阶段
    - this绑定
    - 词法环境创建
      - 全局环境
        - 没有外部环境的词法环境 其外部环境引用null
        - 有一个全局对象 this指向这个全局对象
      - 函数环境
        - 用户在环境中的变量储存在环境中，包含arguments对象等
        - 外部环境引用可以是全局环境或包含内部函数的外部函数环境
    - 变量环境创建
  - 执行阶段
    - 执行变量赋值 代码执行
  - 回收阶段
    - 执行上下文出栈 等待回收执行上下文
- 执行栈
  - 用于储存在代码执行期间创建的所有执行上下文
  - 执行流程
    - 当js引擎执行第一行代码时，会创建一个全局执行上下文，将它压入执行栈中。
    - 每当引擎碰到一个函数时，会创建一个函数执行上下文，将它压入执行栈中。
    - 引擎会执行位于执行栈顶的执行上下文，当函数执行结束后，对应的执行上下文就会弹出。

## var let const区别

- 变量提升
- 暂时性死区
- 块级作用域
- 重复声明
- 修改声明的变量

## 扩展运算符

## 解构

## ES6数组

- Array.from
- Array.of
- copyWithin/find/findIndex/fill/entries/keys/values/includes/flat/flatMap

## ES6对象

- 属性简写

- 属性遍历

  - for in 无法遍历symbol
  - Object.keys() 返回一个数组 不包含symbol
  - Object.getOwnPropertyNames() 返回一个数组 不包含symbol 包含不可枚举属性
  - Object.getOwnPropertySymbols() 返回一个数组 包含所有symbol
  - Reflect.ownKeys() 返回一个数组 包含symbol与不可枚举

- Object.is()

  ```js
  +0 === -0 //true
  NaN === NaN // false
  
  Object.is(+0, -0) // false
  Object.is(NaN, NaN) // true
  ```

- Object.assign() 浅拷贝 同名属性会替换

- Object.getOwnPropertyDescriptors() 返回自身属性的描述对象

- Object.setPrototypeOf()/Object.getPrototypeOf() 设置对象的原型/读取对象的原型

- Object.keys()/Object.values()/Object.entries()

- Object.fromEntries()

## ES6函数

- 函数参数默认值

- length 返回没有默认值参数个数 扩展运算符也不计入

- name 函数名 使用bind则会返回 bound 前缀

- 作用域

  ```js
  let x = 1;
  function f(y = x) { 
    // 等同于 let y = x  
    let x = 2; 
    console.log(y);
  }
  
  f() // 1
  ```

-  箭头函数

  - this 定义所在的对象
  - 不可以被new 
  - 没有arguments
  - 无法使用yield 无法作为Generator

## Set Map

- Set 集合
  - add/delete/has/clear
  - keys/values/entries/forEach
  - WeakSet区别
    - 没有遍历的api
    - 没有size
    - 成员只能是引用值
    - 垃圾回收
- Map 字典
  - size/set/get/has/delete/clear
  - keys/values/entries/forEach
  - WeakMap区别
    - 没有遍历的api
    - 没有clear方法
    - 只接受对象作为键名（除了null）
    - 垃圾回收

## Promise

## Generator

## Proxy

## Decorator

