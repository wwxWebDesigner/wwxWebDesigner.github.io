# 📋 前端面试知识体系

> 用心整理，持续更新 | 涵盖 HTML、CSS、JavaScript、框架、网络、算法等核心考点

---

## 📚 目录

- [一、HTML](#一HTML)
- [二、CSS](#二CSS)
- [三、JS](#三JS)
- [四、框架与工程化](#四框架与工程化)
- [五、网络与浏览器](#五网络与浏览器)
- [六、算法与手写题](#六算法与手写题)

---

## 一、HTML和CSS


### 1.1 HTML 语义化

| 问题 | 答案要点 |
|------|----------|
| 什么是 HTML 语义化？ | 使用正确的标签表达正确的内容，如 `<header>`、`<nav>`、`<article>`、`<footer>` |
| 语义化的好处？ | SEO 优化、可读性高、无障碍访问支持好 |


## 二、CSS
### 2.2 CSS 盒模型

```css
/* 标准盒模型 */
box-sizing: content-box;  /* width = 内容宽度 */

/* IE 盒模型 */
box-sizing: border-box;   /* width = 内容 + padding + border */
```
## 三、JS
> 掌握 JavaScript 核心原理，是前端面试的重中之重。
### 2.1 this
> **this** 是 **js** 的一个关键字，指向的是执行上下文的对象
---

#### 2.1.1 this 绑定规则

> **this** 是函数执行时自动生成的内部参数，指向取决于**调用方式**，而非定义位置。

##### 📌 四种绑定规则

| 绑定类型 | 调用方式 | this 指向 | 示例 |
|----------|----------|-----------|------|
| 默认绑定 | 独立函数调用 | `window` / `undefined`（严格模式） | `foo()` |
| 隐式绑定 | 对象方法调用 | 调用该方法的对象 | `obj.foo()` |
| 显式绑定 | `call` / `apply` / `bind` | 指定的对象 | `foo.call(obj)` |
| new 绑定 | 构造函数调用 | 新创建的实例 | `new Foo()` |

##### 🎯 经典面试题
```javascript
// 题目1：混合场景
var name = 'window'
var obj = {
  name: 'obj',
  fn: function() {
    console.log(this.name)
  }
}

obj.fn()                    // 'obj'（隐式绑定）
var fn = obj.fn
fn()                        // 'window'（默认绑定）

// 题目2：箭头函数
var obj = {
  name: 'obj',
  fn: () => {
    console.log(this.name)
  }
}
obj.fn()  // 'window'（箭头函数 this 继承自外层作用域）
```

### 2.2 闭包

> **闭包** 是当一个函数被返回到外部，并且这个函数仍然能访问到外部函数的局部变量，这就是闭包。具体来说js是采用的词法作用域，就是一个函数被定义的时候就确定了它能访问到哪些变量。正常情况下，函数执行完，他的局部变量会被垃圾回收机制回收掉，但是内部的函数引用了他的变量，导致无法被回收，形成了闭包，所以也会导致内存泄漏。常见的应用场景有防抖节流。

### 2.3 原型与原型链

#### 2.3.1 原型

> **原型**：每个 JavaScript 对象在创建时，都会关联另一个对象，这个被关联的对象就是"原型"。原型相当于一个"公共模板"，所有共享该原型的对象都可以访问原型上的属性和方法。

##### 📌 通俗理解
> 原型就是对象的一个"公共祖先"，让多个对象可以共享同一个祖先身上的属性和方法，从而节省内存。

| 比喻 | 说明 |
|------|------|
| **工厂模具** | 原型就像生产零件的模具，所有用这个模具生产出来的零件都拥有相同的特征 |
| **家族基因** | 原型就像家族的基因，所有后代都继承了这些基因特征 |
| **公共仓库** | 原型就像共享仓库，多个对象可以从同一个仓库里取用工具 |

##### 📌 核心概念

```javascript
// 每个构造函数都有一个 prototype 属性
function Person(name) {
  this.name = name
}

// prototype 指向原型对象，存放共享的方法
Person.prototype.sayHi = function() {
  console.log(`Hi, I'm ${this.name}`)
}

// 实例通过 __proto__ 指向原型
const p1 = new Person('Tom')
const p2 = new Person('Jerry')

p1.sayHi()  // Hi, I'm Tom
p2.sayHi()  // Hi, I'm Jerry

// 关键：sayHi 方法只存在一份（在原型上），所有实例共享
p1.sayHi === p2.sayHi  // true
```
#### 2.3.1 原型链
>一句话总结，***原型链***是js实现继承的机制，每个对象都有一个内部链接__proto__指向其原型对象，当访问对象的属性或方法的时候，如果本身不存在，就会沿着这条链向上寻找，直到找到或到达null为止。

### 2.4 事件对象？

> **事件对象**：当事件触发时，浏览器自动传递给事件处理函数的参数，包含了事件相关的所有信息，比如触发位置、目标元素、按键信息等。

---

#### 📌 如何获取事件对象

```javascript
// 方式1：通过函数参数获取（最常用）
element.addEventListener('click', function(event) {
  console.log(event)  // 事件对象
})

// 方式2：内联事件（使用 event 关键字）
<button onclick="console.log(event)">点击</button>

// 方式3：箭头函数（仍然可以获取）
element.addEventListener('click', (e) => {
  console.log(e)
})
```
### 2.5 Promise？手写简易Promise。

#### 2.5.1 Promise
> ***Promise*** 是ES6 引入的异步编程解决方案，用来解决回调地狱问题。可以理解为一个"承诺"，代表一个异步操作的最终结果（成功或失败）

##### 📌 Promise 的三种状态

| 状态 | 说明 | 是否可变更 |
|------|------|-----------|
| `pending`（进行中） | 初始状态，既没有完成也没有拒绝 | 可变为 fulfilled/rejected |
| `fulfilled`（已成功） | 操作成功完成 | 不可再变 |
| `rejected`（已失败） | 操作失败 | 不可再变 |

##### 📌 Promise 的静态方法速查表
| 方法 | 作用 | 成功条件 | 失败条件 | 返回值 | 典型场景 |
|------|------|----------|----------|--------|----------|
| `Promise.all()` | 并行执行，等待全部完成 | 所有 Promise 都成功 | 任何一个失败 | 成功时返回结果数组 | 并发请求，缺一不可（如页面依赖多个接口） |
| `Promise.race()` | 竞速，返回最先完成的 | 最快的一个成功 | 最快的一个失败 | 最快的结果 | 超时控制、请求竞速 |
| `Promise.allSettled()` | 等待全部 settled | 所有都完成（无论成败） | 不会失败 | 所有结果状态数组 | 批量上报，不影响其他 |
| `Promise.any()` | 返回第一个成功的 | 任意一个成功 | 全部失败 | 第一个成功的结果 | 多个备用接口，取最快成功 |
| `Promise.resolve()` | 返回一个成功的 Promise | 立即成功 | 不会失败 | 成功的 Promise | 将值转为 Promise、微任务队列 |
| `Promise.reject()` | 返回一个失败的 Promise | 不会成功 | 立即失败 | 失败的 Promise | 快速返回错误状态 |

#### 2.5.2 手写简易Promise
##### 📌 Promise简易实现
``` javascript
class Promise {
  constructor(executor) {
    this.state = 'pending'
    this.value = undefined
    this.reason = undefined
    this.onResolvedCallbacks = []
    this.onRejectedCallbacks = []

    const resolve = (value) => {
      if (this.state === 'pending') {
        this.state = 'fulfilled'
        this.value = value
        this.onResolvedCallbacks.forEach(fn => fn())
      }
    }

    const reject = (reason) => {
      if (this.state === 'pending') {
        this.state = 'rejected'
        this.reason = reason
        this.onRejectedCallbacks.forEach(fn => fn())
      }
    }

    try {
      executor(resolve, reject)
    } catch (err) {
      reject(err)
    }
  }

  then(onFulfilled, onRejected) {
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : v => v
    onRejected = typeof onRejected === 'function' ? onRejected : e => { throw e }

    const promise2 = new Promise((resolve, reject) => {
      if (this.state === 'fulfilled') {
        setTimeout(() => {
          try {
            const x = onFulfilled(this.value)
            resolve(x)
          } catch (e) {
            reject(e)
          }
        })
      }

      if (this.state === 'rejected') {
        setTimeout(() => {
          try {
            const x = onRejected(this.reason)
            resolve(x)
          } catch (e) {
            reject(e)
          }
        })
      }

      if (this.state === 'pending') {
        this.onResolvedCallbacks.push(() => {
          setTimeout(() => {
            try {
              const x = onFulfilled(this.value)
              resolve(x)
            } catch (e) {
              reject(e)
            }
          })
        })
        this.onRejectedCallbacks.push(() => {
          setTimeout(() => {
            try {
              const x = onRejected(this.reason)
              resolve(x)
            } catch (e) {
              reject(e)
            }
          })
        })
      }
    })

    return promise2
  }

  catch(onRejected) {
    return this.then(null, onRejected)
  }

  static resolve(value) {
    return new Promise(resolve => resolve(value))
  }

  static reject(reason) {
    return new Promise((_, reject) => reject(reason))
  }

  static all(promises) {
    return new Promise((resolve, reject) => {
      const results = []
      let count = 0
      promises.forEach((p, i) => {
        Promise.resolve(p).then(res => {
          results[i] = res
          count++
          if (count === promises.length) resolve(results)
        }, reject)
      })
    })
  }

  static race(promises) {
    return new Promise((resolve, reject) => {
      promises.forEach(p => {
        Promise.resolve(p).then(resolve, reject)
      })
    })
  }
}
```
### 2.6 object.defineProperty 
> **object.defineProperty**是ES5提供的一个方法，用来**直接在一个对象上定义一个新属性**，或者**修改一个已有属性**，并且返回这个对象。它可以让我们精确的控制属性的行为，比如是否可枚举，是否可以修改，是否可以删除，以及设置getter/setter来实现数据劫持。

#### 2.6.1 核心的参数

```javascript
/**
 * obj-要定义属性目标对象
 * prop-要定义或者修改的属性名
 * descriptor-属性描述符对象
 * 
*/
Object.defineProperty(obj, prop, descriptor)

```
 - descriptor（属性描述符）的配置项
| 属性 | 含义 | 默认值 | 所属描述符类型 |
|------|------|--------|----------------|
| `value` | 属性的值 | `undefined` | 数据描述符 |
| `writable` | 是否可修改（赋值运算符重新赋值） | `false` | 数据描述符 |
| `enumerable` | 是否可枚举（`for...in` / `Object.keys` 是否能遍历到） | `false` | 两者共有 |
| `configurable` | 是否可删除属性 / 是否可修改描述符本身 | `false` | 两者共有 |
| `get` | 读取属性时调用，返回值作为属性值 | `undefined` | 访问器描述符 |
| `set` | 设置属性时调用，接收新值 | `undefined` | 访问器描述符 |
 - `注`：不能同时拥有value/writable和get/set
 - `注`：Object.defineProperty添加的属性xxx，writable、enumerable、configurable的默认值都是false，而通过obj.xxx = value添加的属性的这些配置项的值都是true

#### 2.6.2 应用场景
1. [数据劫持/观察者模式（vue2响应式原理）](#41-vue响应式)
2. 实现不可变/只读属性
``` javascript
Object.defineProperty(obj, 'PI', {
  value: 3.1415,
  writable: false
})
```
3. 隐藏内部私有属性
``` javascript
Object.defineProperty(obj, '_secret', {
  value: '密码',
  enumerable: false
})
```

#### 2.6.3 局限性
##### Object.defineProperty 的局限性（为何 Vue 3 改用 Proxy）

| 局限性 | 具体说明 | Vue 2 的解决方式 | Vue 3 (Proxy) 的优势 |
|--------|--------|--------|--------|
| **无法监听数组变化** | 直接通过索引修改数组项（`arr[0] = xxx`）以及修改 `length` 属性无法触发 setter | 重写了 7 个数组方法（`push`/`pop`/`shift`/`unshift`/`splice`/`sort`/`reverse`） | Proxy 可以拦截所有数组操作，包括索引赋值和 `length` 修改 |
| **必须递归遍历** | 深层嵌套对象需要递归遍历所有属性添加 getter/setter | 初始化时递归遍历，性能差且耗时 | Proxy 是懒代理，访问到深层属性时才进行代理，性能更好 |
| **无法监听新增属性** | `obj.newProp = 1` 不会触发 setter | 需要使用 `Vue.set(obj, 'newProp', 1)` | Proxy 的 `set` 可以拦截新增属性操作 |
| **无法监听删除属性** | `delete obj.prop` 无法被劫持 | 需要使用 `Vue.delete(obj, 'prop')` | Proxy 的 `deleteProperty` 可以拦截删除操作 |
| **只能劫持对象** | 无法代理 Map、Set、WeakMap 等数据结构 | 不支持 | Proxy 可以代理各种数据结构 |
| **无法一次性代理整个对象** | 必须逐个属性定义 | 遍历对象的所有 key 逐个调用 `defineProperty` | Proxy 直接代理整个对象，无需遍历 |
| **无法拦截所有操作** | 只能拦截 `get` 和 `set` 操作 | 不支持 | Proxy 支持 13 种拦截操作（`has`、`ownKeys`、`apply`、`construct` 等） |
> `注`：**Object.defineProperty**只能劫持已有属性的读取和赋值操作，无法监听数组索引赋值、新增/删除属性、length 变化等操作，且需要递归遍历深层对象导致性能问题。Vue 3 采用 Proxy 替代后，完美解决了这些局限性，实现了更完整的响应式系统

##### 代码示例对比

```javascript
// Object.defineProperty 无法监听新增属性
let obj = {}
let data = {}
Object.defineProperty(data, 'name', {
  get() { return obj.name },
  set(val) { 
    console.log('setter 触发')
    obj.name = val 
  }
})
data.name = '张三'  // ✅ 触发 setter
data.age = 18       // ❌ 无法触发 setter，Vue 无法响应
```

### 2.7 Proxy
> **Proxy** 是 ES6 引入的一个用于元编程的特性。它的核心作用是拦截并自定义对象的基本操作（如属性读取、赋值、枚举、函数调用等）
#### 2.7.1 核心参数
> 它接受两个参数：**target**（目标对象）和 **handler**（配置对象）。通过 handler 上定义的‘陷阱’方法，我可以拦截 target 上的操作。
 - 快速举例 
 ``` javascript 
    const obj = { name: 'Alice' };
    const proxy = new Proxy(obj, {
      get(target, key) {
        return key in target ? target[key] : 'Default';
      }
    });
    console.log(proxy.age); // 输出 'Default' (而不是 undefined)
 ```

#### 2.7.2 核心优势 vs Object.defineProperty（进阶点）

1. 对比列表
| 对比维度 | `Object.defineProperty` | `Proxy` |
| :--- | :--- | :--- |
| **监听能力** | 仅 `get` / `set` | **13 种操作**（`get`/`set`/`deleteProperty`/`ownKeys`/`apply`/`construct` 等） |
| **对象适应性** | 需**递归遍历**所有属性，无法检测**新增/删除**属性 | **懒拦截**，代理整个对象，动态属性自动支持 |
| **数组监听** | 需**改写数组原型**方法（`push`/`pop` 等），有性能损耗和边界 Bug | **直接拦截**，不改写原型，性能更好且无 Bug |
| **初始化性能** | **立即递归**所有属性，深层对象开销大 | **懒初始化**，仅当访问时才代理深层对象 |
| **返回值控制** | 无法改变操作的本质返回值（如 `delete` 返回固定值） | 可以**任意定制**返回值，甚至抛出错误 |
| **代码可维护性** | 需要维护 `getter`/`setter` 配置对象，代码冗余 | 集中式 `handler` 配置，逻辑清晰 |

2. 监听能力
- **Object.defineProperty** - 只能监听读取和赋值：

```javascript
// 只能拦截 obj.key 的读和写
Object.defineProperty(obj, 'key', {
  get() { ... },
  set() { ... }
});
```
- **Proxy** - 可拦截 13 种底层操作：

```javascript
new Proxy(target, {
  get, set, deleteProperty,          // 基础操作
  has, ownKeys, getOwnPropertyDescriptor, // 属性查询
  apply, construct,                  // 函数调用
  defineProperty, preventExtensions, // 对象扩展
  getPrototypeOf, setPrototypeOf,    // 原型操作
  isExtensible                       // 可扩展性
});
```



## 四、框架与工程化
### 4.1 vue响应式
#### 4.1.1 vue2
> 一句话就是，**vue2**的响应式核心原理是**数据劫持+发布订阅模式**，是通过**object.defineProperty**递归遍历对象属性，为每个属性创建**Dep**依赖收集器，在**getter**中收集依赖，在**setter**中触发更新。
1. 初始化阶段 (Observer)
- Vue 在 data 选项被传入后，会创建一个 Observer 实例。
- 它递归遍历 data 对象的所有属性，使用 Object.defineProperty 将它们转换为 getter/setter。
- 关键点：如果属性值还是对象，会继续递归处理（深度监听）。

2. 依赖收集阶段 (Dep 与 Watcher)
- Dep (依赖容器)：每个响应式属性都有一个 Dep 实例，就像一个“订阅器”，用来存放所有依赖该属性的 Watcher。
- Watcher (监听者)：谁用到了这个数据，谁就是一个 Watcher。比如模板编译时用到的数据、computed、watch、$watch 等。
- 收集时机：当渲染函数执行，读取 data 中的属性时，就会触发该属性的 getter。此时，全局会有一个“当前正在计算的Watcher”被记录，Dep 就会把这个 Watcher 添加到自己的列表中。简单说：“数据说：谁在用我，我就记下谁”。

3. 派发更新阶段
- 当响应式属性的值发生变化时，会触发该属性的 setter。
- setter 会通知自己的 Dep：“我的值变了，快通知订阅者们”。
- Dep 就会遍历自己收集的所有 Watcher，调用每个 Watcher 的 update() 方法。
- 最后 Watcher 触发重新渲染（或执行回调）。简单说：“数据说：我变了，快通知所有用过我的人重新渲染”

#### 4.1.2 vue3

> **vue3响应式**基于Proxy代理对象，实现了对象增删改属性的响应式、数组索引与length变更的响应式，并且抽离reactive、ref、computed、watch等独立API。
1. 核心原理
- 依赖收集与触发更新
``` text
组件渲染 → 读取数据 → getter 拦截 → track(收集依赖)
数据修改 → setter 拦截 → trigger(触发更新) → 组件重新渲染
``` 
2. Proxy vs defineProperty

| 对比维度 | Object.defineProperty (Vue 2) | Proxy (Vue 3) |
|---------|-------------------------------|---------------|
| **监听对象** | 需要遍历对象所有属性 | 代理整个对象 |
| **新增属性** | 不响应，需要 `$set` | ✅ 自动响应 |
| **删除属性** | 不响应，需要 `$delete` | ✅ 自动响应 |
| **数组监听** | 重写 7 个数组方法，索引/length 不响应 | ✅ 完全支持 |
| **初始化性能** | 需要递归遍历所有属性（深度劫持） | 懒代理，访问时才递归 |
| **嵌套对象** | 初始化时一次性深度劫持 | 访问到深层时才代理 |
| **兼容性** | IE9+ | 无法兼容 IE11 |
| **拦截能力** | 只有 getter/setter | 13 种拦截操作 |

3. reactive vs ref

|特性 |	reactive | ref|
|--------|-------------------|------------------|
|数据类型|	对象/数组|	任意（基本类型通过 .value）|
|实现原理|	Proxy|	对象包装，getter/setter + .value|
|模板中使用|	直接使用|	自动解包，无需 .value|
|传递函数时|	失去响应式（需 toRfs）|	保持响应式|


#### 4.1.3 vue2和vue3的区别
### 4.2 vue的diff算法
### 4.3 react与vue的区别

### 4.4 react常见hooks
### 4.5 react的fiber渲染
### 4.6 react的diff
### 4.7 react和vue的区别
### 4.8 vue源码解决了什么难题
### 4.9 react源码解决了什么难题

## 五、网络与浏览器
### 5.1 浏览器输入url
> 总的流程是：URL 解析与输入处理——检查缓存——DNS 域名解析——建立 TCP 连接（三次握手）——TLS/SSL 握手（仅 HTTPS）——发送 HTTP 请求——服务器处理与响应——浏览器接收与解析——连接关闭（四次挥手）——连接关闭（四次挥手）

> [见详细步骤](faceTest/afterInputUrl.md)

### 5.2 HTTP请求状态码
### 5.3 WebSocket

## 六、算法与手写题
### 6.1 实现flat
### 6.2 实现vue2响应式

``` javascript
// 极简 Dep
class Dep {
  constructor() {
    this.subs = []; // 存放 Watcher
  }
  depend() {
    if (Dep.target) {
      this.subs.push(Dep.target);
    }
  }
  notify() {
    this.subs.forEach(watcher => watcher.update());
  }
}
Dep.target = null;

// 极简 Observer
function observe(obj) {
  if (typeof obj !== 'object' || obj === null) return;
  Object.keys(obj).forEach(key => defineReactive(obj, key, obj[key]));
}

function defineReactive(obj, key, val) {
  observe(val); // 递归
  const dep = new Dep();
  Object.defineProperty(obj, key, {
    get() {
      dep.depend(); // 收集依赖
      return val;
    },
    set(newVal) {
      if (newVal === val) return;
      val = newVal;
      observe(val); // 新值也要变响应式
      dep.notify(); // 派发更新
    }
  });
}
```

### 6.3 实现vue3响应式

``` javascript
function reactive(obj) {
  return new Proxy(obj, {
    get(target, key, receiver) {
      console.log(`读取 ${key}`);
      // 懒代理：只有访问时才递归
      const result = Reflect.get(target, key, receiver);
      return typeof result === 'object' ? reactive(result) : result;
    },
    set(target, key, value, receiver) {
      console.log(`设置 ${key} = ${value}`);
      return Reflect.set(target, key, value, receiver);
    },
    deleteProperty(target, key) {
      console.log(`删除 ${key}`);
      return Reflect.deleteProperty(target, key);
    }
  });
}
```