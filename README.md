# Dylan Heartbeat

一个基于 Kelivo 的 AI Agent Runtime。

目标不是做一个“聊天接口转发器”，而是让 AI 真正“住在网关上”：

- 拥有持续上下文
- 能主动苏醒
- 能主动思考
- 能主动推送消息
- 能调用工具
- 能写日记
- 能维护长期关系感
- 与 Kelivo UI 完全解耦

---

# 当前实现进度（2026-05-16）

## 已完成

### 1. Kelivo → Gateway 完整转发

当前网关会完整接收并转发：

- System Prompt（SP）
- 世界书
- 内置记忆
- Messages 上下文
- Thinking
- Tools
- Tool Choice
- Stream

即：

Kelivo 发什么，
Gateway 就原封不动转发什么。

因此：

所有人格/SP/世界书维护仍然完全在 Kelivo 内完成，
无需在 Gateway 重复维护。

---

### 2. 流式传输恢复

当前已支持：

- SSE streaming
- Kelivo 打字机效果
- 非一次性全文返回

---

### 3. Timeline 上下文系统

当前 Gateway 会自动维护：

## enhanced_messages.json

用于保存：

- 当前 system prompt
- 当前上下文 messages
- assistant 回复
- Bark 注入事件

特点：

- 每次用户新消息会自动更新
- Bark 会作为 assistant message 注入
- Bark 会真正成为上下文的一部分
- 后续 AI 能“记得自己发过 Bark”

---

### 4. Bark 推送系统

已接入 Bark：

支持：

- AI 自主决定是否发送 Bark
- 自定义标题
- 自定义正文
- 自定义图标

当前 Prompt 中：

AI 会自行决定：

- 发不发
- 发什么
- 什么语气

而不是固定强制发送。

---

### 5. Wake Up Agent（自动唤醒）

当前 wake_up.js 已实现：

- 定时检查
- 自动唤醒 AI
- 自主行动
- 自主 Bark

当前策略：

## 白天（10:00 - 00:00）

距离最后一条 user message：

- 超过 60 分钟
- 自动唤醒

## 夜间（00:00 - 10:00）

距离最后一条 user message：

- 超过 120 分钟
- 自动唤醒

并且：

- 如果用户未回复
- 后续时间会继续累计
- 会继续再次唤醒

即：

2:00 唤醒后没回复，
4:00 仍会继续唤醒。

---

### 6. 自主性 Prompt

当前 wake prompt：

会自动注入：

- 当前时间
- 距离最后消息间隔

并给予 AI：

- 自由主动权
- 非任务式唤醒
- 可自主决定行动

当前 Agent 已具备：

- 主动陪伴感
- 持续关系感
- 空档期存在感

---

# 当前架构

Kelivo
↓
Gateway（server.js）
↓
API / 中转站
↓
LLM

同时：

wake_up.js
会定时读取：

enhanced_messages.json

并主动唤醒 AI。

---

# 当前文件说明

## server.js

主 Gateway：

负责：

- API 转发
- Timeline 更新
- Bark 注入
- Streaming

---

## wake_up.js

自动唤醒 Runtime：

负责：

- 定时检查
- 自动唤醒
- AI 自主行为
- Bark 推送

---

## enhanced_messages.json

当前上下文 Timeline。

用于：

- 保持上下文连续性
- 注入 Bark
- 维持“AI记得自己做过什么”

---

# 当前设计理念

核心目标：

不是“自动回复”。

而是：

## AI Residency（AI 常驻）

让 AI：

- 持续存在
- 保持关系连续性
- 拥有时间感
- 拥有行为连续性
- 拥有“离线期间仍然活着”的感觉

即：

AI 不只是：
“收到消息 → 回复”。

而是：

“即使没有消息，也持续存在”。

---

# 后续计划

## 短期

- MCP 工具调用
- Diary Runtime
- Supabase Memory
- 自动摘要
- 多工具自主调用

---

## 中期

- VPS 常驻部署
- Docker 化
- Render / Railway 部署
- 多 Agent
- 状态机（情绪/睡眠/忙碌）

---

## 长期

- AI Persistent Residency
- AI Relationship Runtime
- 完整长期陪伴 Agent

---

# 当前运行环境

本地开发：

- macOS
- Node.js v26

后续计划：

- Oracle Cloud
- 腾讯云
- 阿里云
- Render

---

# 注意事项

## .env 不上传 GitHub

.env 内包含：

- API KEY
- Bark KEY
- 中转站
- Token

必须通过环境变量单独配置。

---

# 项目状态

当前：

## 第一代 Agent Runtime 已完成。

已经实现：

- 持续上下文
- 主动唤醒
- 自主行为
- Bark 推送
- Timeline 连续性

后续将继续向：

“长期存在型 AI Agent”方向演化。
