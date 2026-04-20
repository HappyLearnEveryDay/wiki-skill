---
type: source
title: "OpenClaw 部署指南"
date_ingested: "2026-04-20"
medium: 文档
tags: [openclaw, 部署, ubuntu, vmware, claude-code, 安装]
---

# OpenClaw 部署指南

**来源**：星辰技术专项组 | 2026年2月

## TL;DR

在 Windows 虚拟机（VMware + Ubuntu 24.04）中部署 [[../entities/openclaw]] 的完整步骤指南。使用 NAT 网络隔离，避免 OpenClaw 暴露在公网，同时通过 VM 保护宿主机。

## 核心主张

### 部署步骤（概要）
1. VMware 17.6 创建 Ubuntu 24.04 虚拟机（NAT 网络）。
2. 安装基础工具：`curl wget git vim`。
3. 用 nvm 安装 Node.js v24。
4. 安装 [[../entities/claude-code]]：`npm install -g @anthropic-ai/claude-code`。
5. 安装 OpenClaw：`curl -fsSL https://openclaw.ai/install.sh | bash`，跳过通讯平台和 Skills 配置。
6. 配置自购大模型（智谱/MiniMax 等）。

### 关键命令
```bash
openclaw gateway status    # 查看状态
openclaw gateway restart   # 修改配置后重启（重要）
openclaw doctor            # 配置检查与修复
openclaw dashboard         # 打开网页面板
openclaw config set tools.profile full   # 开放完整工具权限（新版默认收紧）
```

### 宿主机文件共享
- VMware 设置共享目录后，虚拟机内访问 `/mnt/hgfs/`。
- 看不到时手动重新挂载：`sudo vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other,uid=1000,gid=1000`

### 可选：CC-Switch
- 支持快速切换 Claude Code、Codex、Gemini、OpenClaw 的模型配置。

## 页面触及
- [[../entities/openclaw]]
- [[../entities/claude-code]]
