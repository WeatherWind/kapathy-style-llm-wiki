# kapathy-style-llm-wiki

一个 Codex skill，用于自动化 Karpathy 风格 LLM Wiki 的 ingest 工作流。这种知识库采用 `raw/`、`wiki/` 和 `AGENTS.md` 三层结构。

## 安装

```bash
# 将 skill 安装到 Codex
sudo cp -r kapathy-style-llm-wiki ~/.agents/skills/kapathy-style-llm-wiki
```

## 使用方法

当新原始资料进入 `raw/` 目录时，该 skill 会指导 Codex 自动完成：

1. 读取 `raw/` 下所有未处理的 `.md` 文件
2. 在 `wiki/sources/` 下创建来源总结页
3. 提炼概念，在 `wiki/concepts/` 下创建或更新概念页
4. 添加双向交叉链接
5. 更新 `wiki/index.md`

只需说：**"请 ingest raw/ 中的新文件"** 或 **"处理新资料"**，Codex 就会遵循该工作流执行。

## 来源

受 [Andrej Karpathy 的 LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 启发。

## License

MIT

---

# kapathy-style-llm-wiki

A Codex skill that automates the ingest workflow for Karpathy-style LLM wikis — structured knowledge repos using `raw/`, `wiki/`, and `AGENTS.md`.

## Installation

```bash
sudo cp -r kapathy-style-llm-wiki ~/.agents/skills/kapathy-style-llm-wiki
```

## Usage

When new raw source files enter your `raw/` directory, the skill guides Codex to:

1. Read all unprocessed `.md` files in `raw/`
2. Create source summary pages in `wiki/sources/`
3. Extract and create/update concept pages in `wiki/concepts/`
4. Add bidirectional cross-links
5. Update `wiki/index.md`

Just say: *"Please ingest the new files in raw/"* and Codex will follow the workflow.

## Source

Inspired by [Andrej Karpathy's LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

## License

MIT
