---
name: kapathy-style-llm-wiki
description: Use when a new raw source file enters an llm-wiki-style knowledge repository's raw/ directory and needs structured ingestion into wiki/. Trigger when asked to process, ingest, or summarize a source file, or when the task involves reading raw/ content and creating/updating wiki/ pages with cross-links.
---

# Karpathy Style LLM Wiki

## Overview

一个结构化的工作流，用于将新的 raw 资料摄入到 raw/wiki/AGENTS.md 三层知识库中（Karpathy 推广的 "LLM Wiki" 模式）。将原始的 Markdown 笔记转化为由来源总结页和概念页组成的互联知识图。

## When to Use

- `raw/` 目录中出现了新的 `.md` 文件需要处理
- 被要求"读取这个文件并创建 wiki 页面"
- 被要求"总结"或"摄入"某篇内容
- 需要给已有知识库补充交叉链接的 wiki 页面

**Do NOT use** 在没有新 raw 输入的情况下编辑已有 wiki 页面，或在没有 raw/ 来源的情况下从头搭建 wiki。

## Workflow: The Ingest Sequence

每一步的输出是下一步的输入。

```
raw/  →  来源页  →  提炼概念  →  创建/更新概念页  →  交叉链接  →  更新索引
  1         2            3                 4              5           6
```

### Step 1 — 读取 raw 源文件

读取 `raw/YYYY-MM-DD-title.md`，识别：
- 标题、作者、URL、日期（从 frontmatter 或 heading 提取）
- 核心论点 / 主要观点
- 可能独立成页的 distinct 概念
- 与已有 wiki 页面的关联

**流式处理**：读取 `raw/` 下所有 `.md` 文件，逐一处理。如果某个文件已有对应的 `wiki/sources/` 来源页（通过文件名日期+标题匹配），则跳过。确保运行后 `raw/` 中没有未处理的文件。

### Step 2 — 创建来源总结页

路径：`wiki/sources/{slug}.md`

Slug 规则：从文件名中提取有意义的部分，转为小写、连字符连接。例如 `2026-06-09-karpathys-llm-wiki.md` → `karpathys-llm-wiki-2026-06-09`。

```markdown
# 原标题 — 来源总结

- **原始资料**: `raw/原始文件名.md`
- **作者**: ...
- **URL**: ...
- **日期**: ...

## 概述

2–3 句话概括该来源的内容。

## 关键观点

提炼自该来源的关键要点。

## 相关概念

- [[wiki/concepts/概念名|概念名]]
- [[wiki/index|知识库首页]]
```

### Step 3 — 提炼 distinct 概念

扫描来源中的概念，判断：
- **已有概念**：如果来源**提供了新角度或新信息**，则更新该概念页；如果只是**顺带提及**，则只在新创建的来源页和索引中引用，不修改原概念页。
- **尚无概念**：判断是否值得独立成页。

**判断标准**：如果一个概念会被多个来源引用，或很可能被多个来源引用，就值得独立成页。一次性细节应只保留在来源总结中。

### Step 4 — 创建或更新概念页

路径：`wiki/concepts/{slug}.md`

Slug 规则：中文概念名译为英文 slug（如"模型路由" → `model-routing` 或保持与页面标题一致的英文译名）。

```markdown
# 概念名称

## 定义

1–3 句清晰定义。

## （可选章节 — 按需使用）

- 核心特征 / 典型场景 / 实现方式
- 与仓库已有概念的关系

## 相关页面

- [[wiki/concepts/相关概念|相关概念]]
- [[wiki/sources/来源slug|来源：...]]
- [[wiki/index|知识库首页]]
```

### Step 5 — 双向交叉链接

创建/更新页面后，验证链接完整性：

- **来源页** → 链接到它涉及的所有概念
- **概念页** → 链接到相关的概念 + 引用它的来源
- **被更新的概念页** → 添加指向新来源的链接

### Step 6 — 更新索引

编辑 `wiki/index.md`：
- "来源"部分：按来源日期**字母序**排列（即时间倒序，因为日期在 slug 末尾）
- "概念"部分：按概念名**字母序**排列
- 每个条目格式：`- [[wiki/...|显示名]] — 一句话描述`

## Decision Guide: 新概念 vs 不建新概念

```
发现了新想法？
├─ 通用、可复用的概念？ → 创建 wiki/concepts/ 页
├─ 已有概念的扩展？ → 如果有新角度则更新已有页 + 交叉链接；仅顺带提及则不修改原概念页
├─ 一次性或上下文特定细节？ → 只保留在来源总结中
└─ 来源可能有误或过时？ → 询问用户如何处理
```

## Common Mistakes

- **为每个想法都创建概念页** — 只创建跨来源可复用的概念页。太多小页面会让 wiki 更难导航。
- **忘记更新索引** — 最后一步是最常被跳过的。始终更新 `wiki/index.md`。
- **链接断裂** — 重命名或移动页面后，验证所有 `[[wiki/...]]` 链接仍然可解析。
- **更新概念页时遗漏交叉链接** — 当你给已有概念页添加新来源时，也要添加指向该来源页的反向链接。
- **slug 不一致** — 始终遵循 Step 2 和 Step 4 中定义的 slug 规则。

## Real-World Impact

在实践中，这个工作流将临时的 raw 笔记转化可浏览、可交叉引用的知识图。经过 5+ 次 ingest 后，wiki 会自我强化：新来源会自动联系到已有页面，索引无需手动重组就能自然增长。

