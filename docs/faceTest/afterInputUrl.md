# 浏览器输入url的详细过程
## URL 解析与输入处理
1. **判断输入内容**
   - 搜索关键词 → 使用默认搜索引擎拼接搜索 URL
   - 域名/URL → 直接进行后续处理

2. **URL 结构解析**
   - https://www.example.com:443/path/to/page?name=value#section 为例
   - 协议 + 域名 + 端口 + 路径 + 参数 + 锚点

## 检查缓存

> 浏览器按以下顺序检查缓存：1. 浏览器缓存 → 2. 操作系统 Hosts 文件 → 3. 路由器缓存 → 4. DNS 缓存

- 命中且未过期 → 直接使用缓存资源
- 未命中 → 进入下一步

## DNS 域名解析（域名 → IP 地址）
>查询顺序：
浏览器DNS缓存 → 操作系统DNS缓存 → hosts文件 → 本地DNS服务器（递归查询）
→ 根DNS → 顶级域DNS → 权威DNS

- 最终获得目标服务器的 IP 地址
- 同时可能获得 CNAME 记录（别名）

## 建立 TCP 连接（三次握手）

>客户端 ──SYN──>服务器 
客户端 <─SYN+ACK─ 服务器 
客户端 ──ACK──> 服务器 
- 目的：建立可靠的双向通信通道
- 端口：HTTP 默认 80，HTTPS 默认 443
- 同时进行 **TCP 拥塞控制**（慢启动、拥塞避免等）

## TLS/SSL 握手（仅 HTTPS）
1. **Client Hello**：客户端发送支持的加密套件、随机数
2. **Server Hello**：服务端选择加密套件、发送数字证书
3. **证书验证**：客户端验证证书有效性（CA、域名、有效期等）
4. **密钥交换**：生成会话密钥（用于后续对称加密）
5. **加密通信开始**：切换为加密通道

## 发送 HTTP 请求

- **请求行**：`GET /path/to/page HTTP/1.1` 或 `HTTP/2` / `HTTP/3`
- **请求头**：Host、User-Agent、Accept-Encoding、Cookie 等
- **请求体**（POST 等方法时有）

## 服务器处理与响应

1. **服务器接收请求**（可能经过反向代理、负载均衡）
2. **处理请求**：
   - 静态资源 → 直接读取文件
   - 动态请求 → 执行后端代码（如 PHP、Java、Python 等）
3. **返回 HTTP 响应**：
   - 状态行：`HTTP/1.1 200 OK`
   - 响应头：Content-Type、Cache-Control、Set-Cookie 等
   - 响应体：HTML/CSS/JS/图片等

## 浏览器接收与解析
 1. 检查响应状态
- `2xx`：成功，继续处理
- `3xx`：重定向，重新发起请求
- `4xx/5xx`：显示错误页面

 2. 解析与渲染（关键性能阶段）
 HTML解析 → DOM树
↳ 遇到CSS → CSSOM树
↳ 遇到JS → 阻塞解析（除非有 async/defer）
DOM树 + CSSOM树 = 渲染树(Render Tree)
↓
布局(Layout) → 计算每个节点的大小和位置
↓
绘制(Paint) → 填充像素，绘制到图层
↓
合成(Composite) → GPU合成图层显示到屏幕
3. 资源加载行为
- `<img>`、`<video>` → 异步加载
- `<link>` 外部 CSS → 并行下载，不阻塞渲染（但阻塞 JS 执行）
- `<script>` → 默认阻塞 HTML 解析，需下载并执行

## 连接关闭（四次挥手）

> 客户端 ──FIN──> 服务器
客户端 <─ACK─── 服务器
客户端 <─FIN─── 服务器
客户端 ──ACK──> 服务器
- HTTP/1.1 默认开启 Keep-Alive，连接会保持一段时间复用
- HTTP/2 多路复用：多个请求共享一个 TCP 连接

## 后续交互

- 页面加载完成后继续执行异步任务（AJAX、WebSocket 等）
- 用户交互触发新的请求/响应循环

## 关键性能优化点总结

| 阶段 | 优化手段 |
|------|---------|
| DNS 解析 | DNS 预解析 `<link rel="dns-prefetch">` |
| TCP 连接 | 使用 HTTP/2（多路复用）、TLS 1.3（0-RTT） |
| 请求/响应 | 减少请求数（雪碧图、合并文件）、使用 CDN |
| 渲染 | 减少重排重绘、使用 transform/opacity 做动画 |
| 资源加载 | 关键 CSS 内联、异步加载 JS（async/defer） |
