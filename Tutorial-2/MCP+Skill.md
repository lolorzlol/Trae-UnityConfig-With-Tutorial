# MCP 与 Skill 教程

本教程介绍 MCP（Model Context Protocol）和 Skill（技能）两个核心概念。

> 官方课程：https://anthropic.skilljar.com/introduction-to-agent-skills

---

# 一、MCP（Model Context Protocol）

## 1. 什么是 MCP？

**MCP** 是 AI 应用程序的"USB-C 接口"——统一标准，让 AI 能连接各种外部工具和数据源。

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   AI Client     │ ←→  │   MCP Server    │ ←→  │  External Data  │
│  (Claude/Trae)  │     │  (中间层服务)    │     │   (数据源/工具)  │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

**核心能力：**
| 组件 | 说明 | 示例 |
|------|------|------|
| **Tools** | 可执行的"动作" | `github.create_issue`、`db.query` |
| **Resources** | 可读取的"数据源" | PR 列表、告警详情、表数据 |
| **Prompts** | 预定义的"提示模板" | 事故复盘模板、发布检查模板 |

## 2. 配置 MCP

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"]
    }
  }
}
```

## 3. 常用 MCP Server

| Server | 功能 |
|--------|------|
| **filesystem** | 文件系统访问 |
| **github** | GitHub API 操作 |
| **brave-search** | 网页搜索 |
| **unity-mcp** | Unity 编辑器控制 |
| **postgres** | PostgreSQL 数据库 |

---

# 二、Skill（技能）

## 1. 核心理解：Skill 是文件夹

> ⚠️ 常见误解：Skill 只是 Markdown 文件

**Skill 是文件夹**，可以包含：
```
my-skill/
├── SKILL.md          # 技能说明（必需）
├── scripts/          # 可执行脚本
├── references/       # 参考资料、API 文档
├── examples/         # 代码片段
└── hooks/            # 动态钩子配置
```

**这意味着：** AI 不仅能"读" Skill，还能"用"里面的脚本、执行里面的代码。

## 2. 与普通提示词的区别

| 特性 | 普通提示词 | Skill |
|------|-----------|-------|
| 形态 | 文本 | 文件夹（文档+脚本+数据） |
| 复用性 | 一次性 | 跨项目复用 |
| 可发现性 | 需手动输入 | 自动发现加载 |
| 能力 | 只有知识 | 知识+工具+流程 |

## 4. Skill 编写最佳实践

### 4.1 不要陈述显而易见的事

Claude 已经很了解编程。专注于能让 Claude **跳出常规思维** 的信息。

**示例：** `frontend-design` Skill 专门避开 Inter 字体、紫色渐变等 AI 常见模式。

### 4.2 建立 Gotchas（易错点）部分

**这是 Skill 中信号最强的内容。** 记录 Claude 使用 Skill 时经常遇到的失败点。

```markdown
## Gotchas（易错点）

- Unity 编辑器脚本必须放在 `Assets/Editor/` 目录
- 调用 MCP 工具后需等待编译完成再执行下一步
- 截图验证时使用 `include_image=True` 才能返回图像
```

### 4.3 渐进式披露

大型内容放到 `references/` 目录，让 Claude 按需读取：

```
my-skill/
├── SKILL.md           # 核心流程（精简）
└── references/
    ├── api.md         # API 详细文档
    └── examples.md    # 完整示例
```

## 5. SKILL.md 结构

```markdown
---
name: skill-name
description: Use when [触发条件]
---

# Skill Name

## Overview
核心原则（1-2 句）

## When to Use
适用场景

## Gotchas
易错点和陷阱

## Core Pattern
具体方法和示例
```

**命名规范：**
- 只用字母、数字、连字符
- 动词开头：`creating-skills`
- description 只描述触发条件，不描述流程

## 6. 元技能

| 元技能 | 功能 |
|--------|------|
| **find-skills** | 发现和安装新 Skill |
| **skill-creator** | 创建、修改和优化 Skill |

---

# 三、MCP 与 Skill 协同

## 1. 类比理解

> **MCP 是五金店，Skill 是店员的专业知识。**
>
> 有了工具（MCP）还不够，还需要知道什么时候用什么工具、怎么用（Skill）。

## 2. 职责划分

| 特性 | MCP | Skill |
|------|-----|-------|
| **本质** | 工具和数据接口 | 知识和工作流程 |
| **解决什么** | AI 能"做"什么 | AI "会做"什么 |
| **类比** | USB 接口 | 使用手册 |

## 3. 协同示例

**场景：Unity 自动化开发**

```
MCP 提供"能力"：
- manage_gameobject → 创建/修改对象
- create_script → 编写脚本
- run_tests → 运行测试
- manage_camera → 截图验证

Skill 提供"流程"：
1. 先写测试（TDD Skill）
2. 再实现功能
3. 等待编译完成
4. 验证结果
```

---

# 四、参考资源

- MCP 官方：https://modelcontextprotocol.io/
- MCP Servers：https://github.com/modelcontextprotocol/servers
- Skill 官方文档：https://code.claude.com/docs/en/skills
- Agent Skill 课程：https://anthropic.skilljar.com/introduction-to-agent-skills
- 本项目 Skills：`Tutorial-1/Test/.trae/skills/`