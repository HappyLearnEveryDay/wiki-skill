---
type: source
title: "用 OpenClaw 搭建 AI 工作小队"
date_ingested: "2026-04-20"
medium: 文章
tags: [openclaw, multi-agent, subagent, 工作流]
---

# 用 OpenClaw 搭建 AI 工作小队

**来源**：星辰技术专项组 | 2026年2月

## TL;DR

介绍 [[../entities/openclaw]] 的两种 Multi-Agent 模式：真正的 MultiAgents（多个完全独立的 Agent 常驻）和主 Agent + SubAgents（主控派发、子 Agent 临时执行），对比优缺点并给出实用建议。

## 核心主张

### MultiAgents vs 主 Agent + SubAgents

| 模式 | 特点 | 实现难度 |
|------|------|----------|
| MultiAgents | 每个 Agent 完全独立：独立工作区、认证、会话、人设、记忆、Skills | 配置复杂，需绑定路由 |
| 主 Agent + SubAgents | 主控接需求、子 Agent 临时执行特定任务、完成后关闭 | 直接用自然语言指挥即可 |

### SubAgent 实践建议
- 直接用自然语言创建 SubAgent（"创建一个名叫技术大佬的 subagent，专门处理技术问题"）。
- SubAgent 的数据信息独立存放，不能存进主 Agent 的 memory（否则浪费 Token）。
- 主 Agent 只负责分配，不亲自干活；派发后立即汇报交给谁处理。
- 没有最优方案，按需选择或组合使用。

### 真正 MultiAgent 的关键步骤
1. 设置多 agent：修改 `agents.list`
2. 设置多聊天通道：修改 `channels.accounts`
3. 绑定 channel 和 agent：修改 `bindings`

**注意**：企微不支持多 agent bindings 路由（无法实现 MultiAgent）；飞书可以。

## 页面触及
- [[../entities/openclaw]]
- [[../concepts/multi-agent-architecture]]
- [[../concepts/subagent]]
