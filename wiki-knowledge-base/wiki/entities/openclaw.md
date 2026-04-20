---
type: entity
category: 工具
tags: [ai-agent, 通讯平台, multi-agent, 部署]
first_seen: "2026-02"
sources:
  - sources/2026-02-openclaw-deploy
  - sources/2026-02-openclaw-multi-agent
---

# OpenClaw

一款支持通过通讯平台（如飞书）接入 AI Agent 的工具，支持 Multi-Agent 架构。

## 基本信息

- **安装**：`curl -fsSL https://openclaw.ai/install.sh | bash`
- **部署推荐**：在 VMware Ubuntu 虚拟机中部署，通过 NAT 网络隔离，避免暴露在公网。
- **支持的模型**：可对接第三方大模型（智谱、MiniMax 等）

## 常用命令

```bash
openclaw gateway status          # 查看状态
openclaw gateway restart         # 修改配置后重启（重要）
openclaw doctor                  # 配置检查与修复
openclaw dashboard               # 打开网页面板
openclaw config set tools.profile full   # 开放完整工具权限
openclaw config                  # 重新配置
```

## Multi-Agent 架构

### 两种模式

**MultiAgents（真正多 Agent）**
- 每个 Agent 完全独立：独立工作区、认证、会话、人设、记忆、Skills。
- 通过 `bindings` 将不同入口消息路由到不同 Agent。
- 配置步骤：修改 `agents.list` → `channels.accounts` → `bindings`。
- 注意：企微不支持多 agent bindings 路由；飞书可以。

**主 Agent + SubAgents**
- 主控接需求、判断、分包；SubAgent 临时执行完成后关闭。
- 直接用自然语言创建 SubAgent。
- SubAgent 数据独立存放，不能存进主 Agent memory（否则浪费 Token）。

（来源：[[../sources/2026-02-openclaw-multi-agent]]，[[../sources/2026-02-openclaw-deploy]]）

## 部署步骤（概要）

1. VMware + Ubuntu 24.04（NAT 网络）
2. 安装 Node.js v24（via nvm）
3. 安装 [[claude-code]]（可选，用于问题解决）
4. 安装 OpenClaw，配置第三方模型
5. 宿主机文件共享：`/mnt/hgfs/`
