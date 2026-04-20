---
type: entity
category: 工具
tags: [ai-coding, ide, vscode, 编辑器]
first_seen: "2026-02"
sources:
  - sources/2026-02-cursor-guide
  - sources/2026-02-token-optimization
  - sources/2026-02-activity-viz-tool
---

# Cursor

基于 VS Code 的 AI 编程编辑器，订阅制软件，由外国团队开发。

## 基本信息

- **订阅**：Free（有限次）/ Pro（无限 Tab 补全，高级模型有调用次数限制）
- **底层**：基于 VS Code，支持导入 VS Code 配置文件

## 重要配置

### 隐私模式（重要）
打开公司内部项目时**必须**将 Privacy Mode 设置为 `Legacy`，防止代码上传至外部服务器。
（来源：[[../sources/2026-02-cursor-guide]]）

### 文件索引优化
在 `.cursorignore` 中排除 `*.beam`、`*.meta`、`zone/merge/dets/` 等文件，加快索引速度和质量。

### User Rules
在 `Cursor Settings → Rules & Memories → User Rules` 中配置通用规则，让 AI 回答更符合团队习惯。

## 对话模式

- **Agent**：主动分析并跨文件修改，需 accept
- **Plan**：先规划再执行
- **Ask**：问答模式

## 常用快捷键

- `Ctrl+I`：呼出对话界面
- `Ctrl+K`：内联编辑（在代码块范围内局部修改）
- `Ctrl+L`：打开对话（含选中代码）

## 推荐插件

- 中文语言包、GitLens、Better Comments Next
- Erlang: ELP（比 Erlang LS 体验更佳）
- Color Highlight、vscode-icons

## 实用案例

- AI 代码审核：`@folder/ 请扫描...`
- 由图生成前端代码（截图 + 提示）
- Debug 模式：加打印 → 跑测 → 分析日志 → 修复
- 分析函数调用链（30 分钟问题缩短至 5-10 分钟）

（来源：[[../sources/2026-02-cursor-guide]]）
