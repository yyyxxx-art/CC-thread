---
name: cc-load
description: 加载「CC的探讨」存档 — 列表/恢复/自动加载配置，内建上下文防膨胀、幽灵跳过、归档提醒
---

# /cc-load — CC对话恢复

> **定位**：跨会话的思维接续器——让新会话在 3 秒内回到上次中断的思维位置，而不是花 10 分钟重新解释背景。
>
> **核心原则**：启动时优先加载**压缩上下文**（≤500字），不全量加载历史摘要。User 有"跳过"按钮。

---

## 一、触发方式

| 方式 | 命令/口语 | 功能 |
|------|-----------|------|
| 列出全部 | `/cc-load`、「有哪些存档」「工作流列表」「列出存档」 | 展示所有 active 工作流 |
| 列出全部（含休眠） | `/cc-load --all`、「所有存档包括旧的」 | 展示所有工作流（含 dormant/archived） |
| 恢复指定 | `/cc-load {id/关键词}`、「继续上次的」「加载存档」「恢复上下文」「上次聊到哪了」 | 加载匹配的工作流上下文 |
| 设自动加载 | `/cc-load auto {id}`、「设为启动自动加载」 | 添加到 .auto-load.json |
| 取消自动加载 | `/cc-load unauto {id}`、「取消自动加载」 | 从 .auto-load.json 移除 |

**模糊匹配**：`/cc-load ai` → 匹配 `ai-utilization-framework`（按 id、topic、tags 模糊搜索）

---

## 二、执行逻辑

### 2.1 无参数 → 列出工作流

读取 `.index.json`，按 `status` 分层展示：

**🔧 修正 #5（工作流泛滥）—— 默认只列 active：**

```
📁 CC的探讨 — 工作流清单 (Active)

  🔵 L4觉醒-AI协作等级探讨                        [auto-load]
     ID: ai-utilization-framework
     最后更新: 2026-06-22  |  存档次数: 1
     📌 用户是L4级Agent-Native开发者，正在构建跨会话知识管理...

  ⚪ 小程序回收预约项目
     ID: miniapp-recycling
     最后更新: 2026-07-15  |  存档次数: 8
     📌 已完成微信审核整改，正在优化用户行为分析模块...

---
📊 统计: 2 active | 0 dormant | 0 archived
💡 /cc-load --all 查看全部 (含休眠工作流)
💡 /cc-load {id} 恢复指定工作流
💡 /cc-load auto {id} 设为启动自动加载
```

### 2.2 带参数 → 恢复工作流

**步骤 1：匹配**

按 id / 关键词模糊匹配 `.index.json` 中的 `topic` 和 `tags`。返回最佳匹配。

**步骤 2：检查压缩上下文**

**🔧 修正 #2（上下文膨胀）—— 优先加载压缩版：**

```
如果 sessionCount ≥ 5 且 condensed = true:
  → 读取 context-condensed.md (≤500字) ← 轻量快速
如果 sessionCount < 5:
  → 读取 context 字段 + 最新 1 次对话摘要 ← 尚未需要压缩
```

**步骤 3：展示恢复结果 + 幽灵上下文保护**

**🔧 修正 #3（幽灵上下文）—— 展示跳过按钮：**

```
🔄 已恢复工作流：「{topic}」

📌 当前状态: {context-condensed 或 context 字段}

📜 最近会话:
   • 2026-07-15: 优化了用户行为分析模块，修复了数据导出bug
   • 2026-07-08: 完成微信审核整改，调整了隐私弹窗逻辑

📄 核心文件:
   • 对话摘要-20260715.md
   • api-optimization-notes.md
   • context-condensed.md (压缩上下文)

---
🤔 这是你要继续的话题吗？
   回复 yes/对 → 开始基于此上下文对话
   回复 skip/跳过 → 忽略此上下文，开启全新对话
   回复 load {其他id} → 换一个工作流
```

**User 回复 skip/跳过 → 清除注入的上下文，以空白状态开始对话。不追问。**

### 2.3 auto 子命令 → 管理自动加载

```
/cc-load auto {id}:
  1. 验证工作流存在且 status ≠ archived
  2. 添加到 .auto-load.json
  3. 如果 autoLoad 数量 ≥ maxAutoLoadWorkflows (3) → 警告：「自动加载数量已达上限(3)，超过会影响启动速度。建议先取消某个旧工作流的自动加载后再添加。」
  4. 展示更新后的 autoLoad 列表

/cc-load unauto {id}:
  1. 从 .auto-load.json 移除
  2. 展示更新后的列表
```

---

## 三、启动自动加载机制

**每次新会话启动时，AI 必须自动执行：**

```
1. 检查 Desktop/CC的探讨/.auto-load.json 是否存在
   → 不存在 → 跳过（User 可能尚未设置，不做任何提示）
   → 存在 → 继续

2. 读取 autoLoad 数组
   → 空数组 → 跳过

3. 对每条工作流（上限 maxAutoLoadWorkflows=3）：
   a. 读取 .index.json 中的 context+status+sessions
   b. 如果 sessionCount ≥ 5 → 读取 context-condensed.md (≤500字)
   c. 如果 sessionCount < 5 → 读取 context + 最新1次对话摘要

4. 🔧 修正 #3（幽灵上下文）—— 不自动展开，先询问：
   「👋 欢迎回来！
    检测到你在继续以下话题：
    • 🔵 「L4觉醒-AI协作等级探讨」— 上次在重构CC的探讨体系
    需要恢复上下文吗？
    （回复 yes 恢复 / skip 跳过 / load {id} 换话题）」

5. User 回复后才加载完整上下文
```

**🔧 修正 #6（永不删除）—— 启动时顺带检查僵尸工作流：**

如果存在 `dormant` 超过 180 天的工作流，启动后在上下文恢复完成后顺带一句：
```
💡 小提示: 有 3 条工作流已超过 6 个月未更新（如「xxx」），要归档清理吗？
（回复 yes 查看 / 忽略则跳过）
```
**不主动提醒超过 1 次/会话。User 忽略后本会话内不再提。**

---

## 四、状态生命周期

```
active ──(90天无更新)──→ dormant ──(180天无更新)──→ archived
  ↑                          │                          │
  └──(有新存档)──────────────┘                          │
                                                        │
  用户手动 /cc-load archive {id} ───────────────────────┘
  用户手动 /cc-load activate {id} ← 可从 archived 恢复
```

| 状态 | 含义 | `/cc-load` 默认显示 | `/cc-load --all` |
|------|------|---------------------|-------------------|
| `active` | 90天内活跃 | ✅ 显示 | ✅ 显示 |
| `dormant` | 90-180天未更新 | ❌ 隐藏 | ✅ 显示 (标注💤) |
| `archived` | >180天或手动归档 | ❌ 隐藏 | ✅ 显示 (标注📦) |

---

## 五、文件夹读取权限

Claude Code 始终拥有以下目录的完整读写权限（User 永久授权，2026-06-22 生效，跨平台）：
- `~/Desktop/CC的探讨/`（Windows: `%USERPROFILE%\Desktop\CC的探讨\`）— 所有子文件夹和配置文件
- 当前 Claude Code 项目的 memory 目录（`~/.claude/projects/*/memory/`）— 记忆系统
- `~/.claude/skills/` — 技能定义

如遇权限弹窗，提醒 User：「根据 2026-06-22 建立的永久授权，请允许此操作。」
