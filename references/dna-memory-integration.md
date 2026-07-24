# DNA Memory 融合增强

> 来源: [DNA Memory](https://github.com/AIPMAndy/dna-memory) — 跨客户端统一记忆层
> 融合目标: 认知分型、记忆强化学习、语义搜索、跨客户端记忆统一

## 认知分型（8种记忆类型）

| 类型 | 用途 | 写入条件 |
|------|------|---------|
| preference | 稳定偏好与协作习惯 | 多次出现的用户偏好 |
| fact | 已验证事实 | 有来源支撑的客观信息 |
| insight | 从事实抽象的认识 | 多事实归纳的规律 |
| decision | 决策及其理由 | 有明确理由的选择 |
| project_state | 项目当前状态 | 可验证的进度快照 |
| open_loop | 待闭环事项 | 明确的待办/待确认 |
| workflow | 可复用流程 | 经过验证的步骤序列 |
| error_lesson | 经过验证的失败经验 | 有复现步骤的错误 |

## 记忆强化学习

```
recall → task执行 → outcome判断
  → useful: 权重 +0.1
  → misleading: 权重 -0.05
  → 未使用: 不反馈
```

权重范围: 0.1 ~ 1.0。低权重记忆在召回时降序排列。

## 语义搜索策略

- 提取1-4个区分性关键词
- 按关键词分别召回，按memory_id去重
- 最多注入5条记忆，约2000 tokens
- 当前文件/进程/远程状态/测试优先于过期记忆

## 跨客户端统一层

Markdown/Obsidian为长期真源，SQLite为可重建索引。
多客户端（Codex/Claude Code/Hermes）通过同一MCP服务读写。

## 有界采集原则

只存结论，不存transcript：
- session ID + 路径 + 哈希 + 偏移 + 计数
- 有界来源指针，不是完整会话
- 安全、短小、可追溯的结论才写入
