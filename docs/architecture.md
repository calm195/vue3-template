# 架构与编码约定 / Architecture

以职责组织基础目录。应用变大后，可将特定业务的组件、组合式函数、服务和类型内聚到 `src/features/<feature>/`；不要提前创建空业务层。

Organize the foundation by responsibility. As the application grows, colocate business-specific code under `src/features/<feature>/` instead of spreading a feature across global folders.

- 页面协调数据与布局，组件通过类型化 props / emits 通信。
- 组合式函数封装响应式逻辑；纯计算放入 utils。
- Pinia 保存跨页面共享状态；局部界面状态使用 ref / reactive。
- 服务层接收外部数据为 unknown，并在边界执行运行时校验。TypeScript 类型不能验证网络响应。
- 注释说明原因、约束、边界条件；复杂公共函数使用 TSDoc 说明参数、返回值与副作用。避免逐行复述代码。
- 使用语义化 HTML、可访问名称、键盘焦点样式与 reduced-motion 支持。

Views coordinate data and layout. Components communicate with typed props/emits. Composables encapsulate reactive behavior; utils contain pure computation. Keep local state local and reserve Pinia for shared state. Validate external data at runtime. Comments explain intent, constraints and edge cases; document nontrivial public APIs with TSDoc. Prefer semantic HTML and accessible keyboard interactions.

## 部署 / Deployment

`pnpm build` outputs `dist/`. Deploy it to a static host and rewrite unmatched URLs to `index.html` for history routing. For subdirectory hosting, set Vite `base`; router history already uses `import.meta.env.BASE_URL`. `pnpm preview` is a local build preview, not a production server.

## 环境变量 / Environment

Copy `.env.example` to `.env.local` when needed. Every `VITE_` variable is visible to browsers; keep credentials on a backend. The title is read through a typed `ImportMetaEnv` declaration.
