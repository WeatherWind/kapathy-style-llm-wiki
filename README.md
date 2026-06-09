# kapathy-style-llm-wiki

A Codex skill that automates the ingest workflow for Karpathy-style LLM wikis — structured knowledge repos using `raw/`, `wiki/`, and `AGENTS.md`.

## Installation

```bash
# Install the skill to Codex
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
