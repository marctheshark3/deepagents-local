# deepagents-local

LangChain [Deep Agents](https://github.com/langchain-ai/deepagents) CLI wired to
the local Ollama server on this machine (Spark, port 11434).

## Install

```bash
uv tool install --python 3.12 'deepagents-cli[ollama]'
```

Binaries land in `~/.local/bin/deepagents` and `~/.local/bin/deepagents-cli`.

## Activate the config

The CLI reads `~/.deepagents/config.toml`. To use the config in this repo:

```bash
cp config.toml ~/.deepagents/config.toml
# or, to keep this repo as the source of truth:
ln -sf "$PWD/config.toml" ~/.deepagents/config.toml
```

## Run

```bash
# Interactive session using the default model from config.toml
deepagents

# One-shot, piped
deepagents -M "ollama:qwen2.5:7b" -n "summarize this repo" -q

# Override model on the fly
deepagents -M "ollama:nemotron-cascade-2:30b"

# Full autonomy (auto-approve tool calls)
deepagents -y
```

Inside the TUI, `/model` switches model, `/help` lists commands.

## Models available via Ollama

Listed in `config.toml`. Confirm what's actually pulled with:

```bash
curl -s http://localhost:11434/api/tags | jq '.models[].name'
```

## Optional extras

- **Ripgrep** — faster `grep` tool: `sudo apt-get install ripgrep`
- **Web search** — set `TAVILY_API_KEY` in your shell to enable the search tool
- **Other providers** — re-install with more extras, e.g.
  `uv tool install --reinstall --python 3.12 'deepagents-cli[ollama,nvidia]'`

## Notes

- Ollama must be reachable at `http://localhost:11434` (see `~/PORT_REGISTRY.md`).
- Tool-calling is enabled in the profile; models without solid tool-call support
  (smaller Qwens, Gemma) may misbehave on multi-step agent tasks. Nemotron
  and `qwen2.5:7b`+ are the safer defaults.
