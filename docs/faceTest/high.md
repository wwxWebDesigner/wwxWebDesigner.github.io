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



## 四、框架与工程化
### 4.1 vue响应式
#### 4.1.1 vue2
#### 4.1.2 vue3
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