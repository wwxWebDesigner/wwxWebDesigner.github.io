# React
> [官网](https://react.dev/)

## React 简介

React 是由 Meta（原 Facebook）开源的用于构建用户界面的 JavaScript 库。  
它通过**组件**组织界面，用**声明式**的方式描述「在某个状态下界面应该长什么样」，由 React 负责把界面高效更新到 DOM（或原生视图等目标环境）。

与「模板 + 指令」一类框架不同，React 通常配合 **JSX** 在 JavaScript 中书写 UI 结构，并与 **Hooks** 等 API 管理状态与副作用，适合构建交互复杂、需要长期维护的前端应用。

## 核心特点

- **组件化**：把页面拆成独立、可复用的组件，便于协作与测试。
- **声明式 UI**：根据 state/props 推导界面，减少手写 DOM 操作。
- **虚拟 DOM 与协调（Reconciliation）**：在内存中比较变更，批量、有针对性地更新真实 DOM，减轻性能负担。
- **单向数据流**：数据自上而下传递，配合状态提升等模式，逻辑路径更清晰。
- **生态丰富**：路由（如 React Router）、状态管理（如 Redux、Zustand）、服务端渲染（如 Next.js）等可按需选用。

## 适用场景

- 中大型单页应用（SPA）与复杂交互界面
- 需要组件库与设计系统统一迭代的团队项目
- 希望与 TypeScript、各类构建工具深度集成的工程化项目

## 基础示例

下面是一个使用函数组件与 `useState` 的最小示例（需在支持 JSX 的构建环境中运行，例如 Vite、Create React App 等）：

```jsx
import { useState } from 'react'

function App() {
  const [title, setTitle] = useState('Hello React')
  const [showTip, setShowTip] = useState(true)

  return (
    <div>
      <h2>{title}</h2>
      <input
        value={title}
        onChange={(e) => setTitle(e.target.value)}
      />
      {showTip && <p>你正在学习 React</p>}
    </div>
  )
}

export default App
```

上面示例演示了：插值展示、受控输入（双向绑定的一种 React 写法）以及条件渲染。
