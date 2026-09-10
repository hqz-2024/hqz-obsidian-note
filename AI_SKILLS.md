---
title: AI_SKILLS（skills 集合）
tags:
  - ai-agent
  - skill
category: AI Agent / 开发工具
---

# AI_SKILLS（skills 集合）

AI skills 集合（含 chinese-law-skills 民法典检索等）
## GitHub

[仓库链接](https://github.com/hqz-2024/AI_SKILLS)

## 相关

[[项目总览]]

## 项目 README

﻿# AI Skills 汇总

### 安装方法

将 `skills/` 下的文件夹复制到项目的 `.augment/skills/` 目录即可，Augment 自动加载。

```powershell
Copy-Item -Path ".\skills\*" -Destination ".\.augment\skills\" -Recurse -Force
```

---

### Skills 列表

#### 🔧 嵌入式开发类

| Skill | 触发条件 | 说明 |
|---|---|---|
| `esp-idf-skills` | ESP32 / IDF 相关开发任务 | ESP-IDF 5.5.4 官方 API 规范，强制 component+main 架构 |
| `lvgl-skills` | LVGL UI 开发任务 | LVGL v9 模板式开发规范，800×480 RGB565 默认参数 |
| `k230-development` | K230 / CanMV 开发任务 | K230 SDK 架构、MPP API、AI 推理、跨核通信等完整知识库 |
| `pb03f` | PB03F 蓝牙模块开发 | PHY62XX BLE SoC SDK 3.1.5，涵盖驱动 API、BLE 栈、OSAL |

#### 🤖 工作流 / 开发流程类（superpowers）

| Skill | 触发条件 | 说明 |
|---|---|---|
| `using-superpowers` | 每次对话开始时手动调用 | 建立 skills 使用规范，确保合适时机调用正确 skill |
| `brainstorming` | 创建功能、构建组件、添加任何新功能之前 | 在写代码前探索需求与设计，必须先获得用户确认 |
| `writing-plans` | 有需求/规格需要拆解为多步任务时 | 将需求转化为可执行的实施计划 |
| `executing-plans` | 有实施计划需要执行时 | 带检查点地执行计划，分步推进 |
| `systematic-debugging` | 遇到 bug、测试失败、异常行为时 | 先找根因再修复，禁止症状修复 |
| `test-driven-development` | 实现任何功能或 bugfix 之前 | 先写测试再写实现代码 |
| `verification-before-completion` | 声称工作完成、提交或创建 PR 之前 | 必须运行验证命令并确认输出，不能凭感觉说完成 |
| `requesting-code-review` | 完成任务、实现主要功能、合并前 | 规范化 code review 请求流程 |
| `receiving-code-review` | 收到 code review 反馈时 | 技术严谨地处理 review 意见，不盲目接受 |
| `subagent-driven-development` | 当前 session 中执行含独立任务的实施计划时 | 通过 subagent 并行执行独立任务 |
| `dispatching-parallel-agents` | 面对 2 个以上可并行的独立任务时 | 决策何时及如何派发并行 agent |
| `finishing-a-development-branch` | 实现完成、测试通过、准备集成时 | 引导完成 merge / PR / cleanup 的决策 |
| `using-git-worktrees` | 开始需要隔离的功能开发时 | 通过 git worktree 创建隔离工作区 |
| `writing-skills` | 创建或编辑新 skill 时 | skill 编写规范与验证方法 |

#### 🗜️ Caveman 压缩类

| Skill | 触发条件 | 说明 |
|---|---|---|
| `caveman` | 说"caveman mode" / "少用token" / "be brief" | 穴居人风格压缩输出，节省约 75% token |
| `caveman-commit` | 说"write a commit" / "commit message" | 生成压缩风格的 Conventional Commits 提交信息 |
| `caveman-review` | 说"review this PR" / "code review" | 单行压缩格式的 code review 评论 |
| `caveman-compress` | 说"/caveman-compress 文件路径" | 将 md 文件压缩为穴居人风格，保留原文备份 |
| `caveman-help` | 说"/caveman-help" / "what caveman commands" | 显示所有 caveman 命令速查卡 |
| `caveman-stats` | 说"/caveman-stats" | 显示 token 使用统计（需 Claude Code Hook，Augment 中不可用） |
| `cavecrew` | 说"delegate to subagent" / "use cavecrew" | 决策何时派发穴居人风格 subagent |


#### 📋 任务规划类

| Skill | 触发条件 | 说明 |
|---|---|---|
| `planning-with-files-zh` | 说"任务规划" / "制定计划" / "分解任务" / "多步骤规划" | Manus 风格文件规划，创建 task_plan.md、findings.md、progress.md 三个文件跟踪进度 |


#### �️ 工具类

| Skill | 触发条件 | 说明 |
|---|---|---|
| `agent-skill-creator` | 说"create agent" / "create skill" / "automate workflow" | 从工作流描述创建跨平台 agent skill |
| `chinese-law-skills` | 中国法律 / 民法典相关咨询 | 中华人民共和国民法典全文，7 编 1260 条 |
| `mineru` | MinerU PDF 解析相关任务 | MinerU 文档解析工具，PDF 转 Markdown |

---

### open-design-skills 安装方法

```
skills/          →  .augment/skills/
craft/           →  .augment/rules/
```

`design-systems/` 目录下是网站配色风格案例，需要时将对应 md 文件复制到本地项目中引用。
