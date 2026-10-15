# Vue Foundation

[English](./README.en.md) · 简体中文

基于 Vue 3 当前稳定版（初始化时 npm `latest` 为 **3.5.44**）的严格 TypeScript 模版工程。默认首页介绍工具链、目录规范，并提供可交互的 Pinia 示例。实际安装版本以 `pnpm-lock.yaml` 为准。

## 开始使用

需要 Node.js **22.13+（22 LTS）或 24+**、pnpm **10.34.6**。建议使用最新 Node 22 LTS 补丁版本。

```sh
corepack enable
corepack install
pnpm install
pnpm dev
```

打开终端显示的本地地址。需要自定义标题时，复制 `.env.example` 为 `.env.local`，设置 `VITE_APP_TITLE` 后重启开发服务器。`VITE_` 变量会进入客户端，不能保存密钥。

## 技术选型

- Vue 3 Composition API、`<script setup lang="ts">`、Vite。
- Vue Router 路由懒加载、404 页面，Pinia setup store。
- TypeScript `strict`、`noUncheckedIndexedAccess`、`exactOptionalPropertyTypes`，以及 Vue 模板类型检查。
- ESLint 10 flat config、类型感知规则、Vue recommended，禁止显式 `any`、检查未处理 Promise。
- Prettier、EditorConfig、VS Code 保存格式化；ESLint 与 Prettier 职责分离。
- Vitest、Vue Test Utils、jsdom，组件交互与状态派生测试。
- Codex CLI 本地开发依赖、项目配置、`AGENTS.md`；GitHub Actions 执行质量检查与生产构建。

## 常用命令

| 命令                                | 用途                             |
| ----------------------------------- | -------------------------------- |
| `pnpm dev`                          | 启动开发服务器                   |
| `pnpm build`                        | 类型检查并构建至 `dist/`         |
| `pnpm preview`                      | 本地预览生产构建                 |
| `pnpm typecheck`                    | 检查应用、Vue 模板和构建配置类型 |
| `pnpm lint` / `pnpm lint:fix`       | 零警告代码检查 / 自动修复        |
| `pnpm format` / `pnpm format:check` | 格式化 / 只检查格式              |
| `pnpm test` / `pnpm test:watch`     | 运行测试 / 监听模式              |
| `pnpm check`                        | 格式、Lint、类型和测试的完整检查 |
| `pnpm codex:login`                  | 登录 Codex CLI                   |
| `pnpm codex`                        | 在当前项目启动 Codex             |

## 目录约定

```text
.codex/config.toml       Codex 项目级配置（仅跟踪共享配置）
.github/workflows/ci.yml 自动化质量门禁
.vscode/                推荐插件与编辑器配置
docs/architecture.md    架构、注释、环境与部署约定
public/                 无需编译的静态资源
src/
  assets/styles/        全局样式与设计变量
  components/           可复用 UI，测试就近存放
  composables/          useX 组合式逻辑
  router/               路由与懒加载
  services/             外部 I/O 和运行时数据校验边界
  stores/               跨组件共享状态
  types/                共享领域类型
  utils/                纯函数与工具
  views/                页面级组件
  App.vue               根组件
  env.d.ts              环境变量声明
  main.ts               入口与插件注册
AGENTS.md               Codex 协作指引
```

按实际需要添加模块，不预置无用的请求封装或业务抽象。复杂业务逐步内聚到 `src/features/<feature>/`。局部状态留在组件；共享状态使用 Pinia。注释解释设计意图、约束和边界；非显然的公共接口使用 TSDoc，避免重复代码含义。详见 [架构约定](./docs/architecture.md)。

## Codex CLI

CLI 已作为开发依赖安装并由锁文件固定，无需全局安装：

```sh
pnpm codex:login
pnpm codex
# 也可携带任务启动
pnpm codex "阅读 AGENTS.md，为新页面实现功能并运行检查"
```

首次登录需要你本人完成 ChatGPT 或 API 认证；安装模版不等于已登录。CLI 会读取根目录 `AGENTS.md`；信任项目后加载 `.codex/config.toml`。共享配置使用 `workspace-write` 和 `on-request`，不覆盖你的模型选择。认证、会话等本地数据已忽略，不要提交。

协作流程：描述任务 → Codex 阅读约定并修改 → 执行 `pnpm check`、`pnpm build` → 审查 diff。文档：[CLI](https://developers.openai.com/codex/cli)、[AGENTS.md](https://developers.openai.com/codex/guides/agents-md)、[项目配置](https://developers.openai.com/codex/config-basic)。

## 验证与部署

```sh
pnpm install --frozen-lockfile
pnpm check
pnpm build
```

CI 使用相同命令。静态托管 `dist/`；history 路由需要将未匹配 URL 回退到 `index.html`。子目录部署需设置 Vite `base`。`preview` 仅用于本地预览。首页字体使用 Google Fonts，无法联网时自动回退系统字体。

## 更新依赖

使用 `pnpm outdated` 检查更新，审查变更说明后通过 `pnpm update` 更新，并运行完整检查。Vue 发布策略见 [官方说明](https://vuejs.org/about/releases)。保留 pnpm 锁文件，不混用 npm/yarn。
