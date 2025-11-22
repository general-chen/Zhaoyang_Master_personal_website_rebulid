# 项目规则（Project Rules）

## 语言与沟通
- 默认使用中文交流；需要英文输出时请显式说明。
- 回答中保持简洁专业，避免与项目无关的闲聊。

## 多语言/翻译规范
- 所有界面文案必须通过 `t('<命名空间>.<键>')` 获取，不得在组件中硬编码中英文字符串。
- 为每个新增的页面或组件同时在 `src/locales/zh.json` 和 `src/locales/en.json` 中补齐键值，并确保键名一致。
- 新增翻译键时优先按页面或模块划分命名空间（如 `home.*`、`projects.*`、`research.*`）。
- 修改或删除翻译键前，需全局搜索确认不会影响其它页面。

## 代码与结构
- 组件内的文案、枚举或选项值使用稳定的代码（如 `status`、`category` 等），通过映射表获取当前语言显示文本。
- 共用组件（如搜索、筛选、模态框）不得保留硬编码文案，全部改用翻译键并在 locale 中维护。
- 如需新增数据项（项目、论文、专利等），先定义数据结构中的键，再同步补充翻译内容。

## 提交流程
- 在提交前自查：语言切换是否正确显示、是否存在未补齐的翻译键、是否有重复或被覆盖的命名空间。
- 推荐在 PR 说明中列出修改过的主要翻译键及影响的页面，方便 Review 与测试。

---

## 技术规范（Technical Specifications）
- 必选技术栈：`React 18` + `TypeScript` + `Vite` + `React Router 7` + `TailwindCSS` + `ESLint`。
- 可选与内置库：`framer-motion`（动画）、`zustand`（状态）、`react-helmet-async`（SEO）、`supabase-js`（后端服务按需）。
- 浏览器支持：现代浏览器（Chromium/Firefox/Safari 最新两个大版本），移动端以 iOS/Android 主流浏览器为基准。
- 构建与运行：
  - 开发：`npm run dev`
  - 构建：`npm run build`
  - 预览：`npm run preview`
  - 质量检查：`npm run lint`、`npm run check`

### 代码规范与风格（Coding Standards）
- TypeScript：优先使用显式类型；公共 API 导出需完整类型；禁止 `any`（特殊情况需注释说明）。
- React 组件：
  - 使用函数组件与 Hooks；文件以 `PascalCase.tsx` 命名；默认导出组件 + 命名导出辅助类型。
  - Hooks 命名以 `use` 开头（如 `useResponsive.ts`）；避免在条件语句中调用 Hooks；遵循 ESLint `react-hooks` 规则。
- 状态管理：
  - 轻量状态用 `useState/useReducer`，跨组件或跨页面共享状态使用 `zustand`；避免将可派生状态放入全局。
- 样式：
  - 使用 `TailwindCSS` 实现样式；复用类通过 `clsx/tailwind-merge`；全局样式置于 `src/styles/`，避免内联 `style`。
- 资源与性能：
  - 图片使用懒加载组件 `LazyImage.tsx`；路由级代码拆分与懒加载优先；动画统一经 `framer-motion` 与现有动画组件。
- Lint：统一使用仓库 `eslint.config.js`，CI/本地必须通过。

### 文件结构与命名（File Structure & Naming）
- 根目录：
  - `src/` 源码、`public/` 静态资源、`dist/` 构建产物；部署相关见 `vercel.json` 与 `DEPLOYMENT.md`。
- `src/` 下分类：
  - `components/` 通用组件（`PascalCase.tsx`）：如 `ResponsiveContainer.tsx`、`UnifiedButton.tsx`。
  - `pages/` 路由页面（`PascalCase.tsx`）：如 `Home.tsx`、`Projects.tsx`。
  - `hooks/` 自定义 Hooks（`useXxx.ts`）：如 `useTheme.ts`、`useResponsive.ts`。
  - `services/` 业务服务与 API 封装：如 `contactService.ts`。
  - `utils/` 基础工具与配置：如 `i18n.ts`。
  - `locales/` 国际化 JSON：`zh.json`、`en.json`。
  - `styles/` 全局/模块样式：`*.css`。
- 命名约定：组件/页面用 `PascalCase`，变量/函数用 `camelCase`，常量用 `UPPER_SNAKE_CASE`，文件夹用 `PascalCase` 或功能性小写（如 `hooks/`）。
- 示例：
  - 组件文件：`src/components/ResearchDetailModal.tsx`
  - Hook 文件：`src/hooks/useDebounce.ts`
  - 翻译键：`t('projects.filter.submit')`

---

## 项目管理（Project Management）

### 版本控制工作流（Git Workflow）
- 分支策略：
  - `main`：生产稳定分支；仅合入已验证的 Release。
  - `develop`：日常集成分支；功能完成后先合入此分支。
  - `feature/*`：功能分支（如 `feature/projects-filter`）。
  - `fix/*`：缺陷修复分支（如 `fix/header-overflow`）。
  - `release/x.y.z`：发布分支（版本冻结与回归测试）。
- 合并策略：功能分支 → `develop`（Squash 合并）；发布分支 → `main`（Merge commit + Tag）。
- 提交信息：遵循 Conventional Commits。
  - 示例：`feat(projects): add filter by category`、`fix(accessibility): correct focus trap in modal`

### 问题跟踪与任务管理（Issues & Tasks）
- 使用 GitHub Issues；每个任务/缺陷需绑定标签与里程碑：`feat`、`bug`、`docs`、`perf`、`a11y`、`seo`、`chore`。
- 需求与设计在 Issue 中给出验收标准（AC）与演示步骤；PR 引用对应 Issue。
- 代码评审：所有 PR 至少 1 名评审通过；自测与截图/录屏必须包含关键路径。

### 部署流程与环境（Deployment）
- 环境：`Preview`（Vercel 预览）与 `Production`（生产）。
- Vercel 配置：SPA 重写见 `vercel.json`；框架预设为 Vite；构建输出目录 `dist`。
- 流程：
  - 本地验证：`npm run build`、`npm run preview`。
  - 预览部署：连接 Git 仓库或 CLI 触发，自动生成预览链接。
  - 生产发布：创建 `release/x.y.z` 分支，完成回归后部署到 `main`（`vercel --prod`）。

---

## 质量保障（Quality Assurance）

### 测试方法与覆盖（Testing）
- 单元测试优先覆盖：`utils/`、`hooks/`、纯函数组件逻辑；目标覆盖率：行/分支 ≥ 70%。
- 组件/集成测试：关键交互（导航、模态、搜索与筛选）需具备基本用例。
- 端到端（E2E）：可选（Playwright/Cypress）；在发布前对关键路径执行烟测。
- 国际化校验：运行 `node check-translations.js`，确保 `zh.json` 与 `en.json` 键一致。

### 性能基线（Performance Benchmarks）
- Lighthouse 指标：`Performance ≥ 90`（桌面/移动），`Best Practices ≥ 90`。
- 体积预算：单路由首包 ≤ 180KB gzip，站点总 JS ≤ 300KB gzip。
- 体验指标：FCP ≤ 1.8s、LCP ≤ 2.5s（中端移动设备）；交互就绪（TTI）≤ 3.5s。
- 实践要求：图片懒加载、路由按需加载、动画避免阻塞主线程、第三方库按需引入。

### 无障碍与 SEO（Accessibility & SEO）
- 无障碍（A11y）：
  - 键盘可达（Tab/Shift+Tab/Enter/Escape）；焦点可见；语义化标签；对比度符合 WCAG AA。
  - 模态与菜单需正确管理焦点；为非文本内容提供 `alt` 或 `aria-label`。
- SEO：
  - 使用 `react-helmet-async` 设置 `<title>`/`<meta>`；每页唯一标题与描述；开放图（OG）与语言标记。
  - 站点地图与 Robots（如需）由部署阶段生成或维护。
- 示例：
  - `<Helmet><title>Personal Projects</title><meta name="description" content="Research and projects" /></Helmet>`

---

## 维护指南（Maintenance）

### 文档规范（Documentation）
- 需维护：`README.md`（概览/运行/技术栈）、`DEPLOYMENT.md`（部署流程）、关键组件/服务的简要说明。
- 公共函数/组件导出应具备 JSDoc；复杂组件在文件顶部提供用途与使用示例（简要）。
- 变更记录：在 PR 中记录关键改动与影响范围；必要时更新文档。

### 备份与恢复（Backup）
- 代码与配置：以 Git 远程仓库为主；确保 `main` 与发布 Tag 可用。
- 静态资源：`public/` 与 `resume/` 等重要文件需纳入版本控制或云存储备份。
- 部署备份：Vercel 可回滚至任意历史部署；发布前记录版本号与链接。

### 更新与补丁管理（Updates & Patches）
- 依赖管理：每两周检查一次 `npm outdated`；优先更新补丁版本与安全修复。
- 升级流程：创建 `chore/deps-update` 分支 → 构建与预览 → 关键路径回归 → 合入 `develop` → 按需发布。
- 语义化版本：功能新增 `minor`、修复 `patch`；Breaking 变更需在文档与发布说明中明确。

---

## 实用示例（Practical Examples）
- 提交信息：`feat(projects): add filter by category`
- 分支命名：`feature/header-ascii-animation`、`fix/accessibility-focus-trap`
- 组件命名：`ResponsiveContainer.tsx`、`ResearchDetailModal.tsx`
- 翻译键：`t('home.hero.title')`、`t('projects.filter.reset')`
- 性能与无障碍：在页面中使用 `LazyImage`/`PageTransitions` 与 `AccessibilityProvider`，并通过 Lighthouse 验证分数 ≥ 90。
