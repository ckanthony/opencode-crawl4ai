---
name: crawl4ai-cli-installation
description: |
  Install the Crawl4AI CLI and handle setup errors.
---

# Crawl4AI CLI Installation

## Quick Install

```bash
pip install -U crawl4ai
# or
pipx install crawl4ai
```

## Verify Installation

Check if installed:

```bash
crwl --version
```

Expected output:

```
Crawl4AI v0.x.x
```

## Browser Setup

After installation, run the browser setup once:

```bash
crwl --setup
```

This installs the required browser drivers (Playwright) for JavaScript rendering.

## If you fail to install or run, use the following error handling instructions:

If ANY command returns an error (e.g., "command not found", "browser not found", "playwright"), use an ask user question tool if available:

**Question:** "How would you like to set up Crawl4AI?"

**Options:**

1. **Install via pip (Recommended)** - `pip install -U crawl4ai`
2. **Install via pipx** - `pipx install crawl4ai`
3. **Run with Docker** - Start the Crawl4AI server in Docker

### If user selects pip or pipx install:

Run the appropriate install command, then run `crwl --setup` for browser drivers. After setup, verify with `crwl --version`.

### If user selects Docker:

Run:

```bash
docker pull unclecode/crawl4ai:latest
docker run -d -p 11235:11235 --shm-size=1g --name crawl4ai-server unclecode/crawl4ai:latest
```

Then inform the user the server is running at `http://localhost:11235` and the dashboard is available. Use REST API calls (`curl` or Python `requests`) instead of the `crwl` CLI for all subsequent operations.

## Troubleshooting

### Command not found

If `crwl` command is not found after installation:

1. Make sure pip/pipx bin directory is in PATH
2. Try: `python -m crawl4ai --version`
3. Or reinstall: `pip install --force-reinstall -U crawl4ai`

### Browser setup fails

If `crwl --setup` fails:

```bash
# Install playwright separately
pip install playwright
python -m playwright install chromium
```

### Permission errors

If you get permission errors during installation:

```bash
# Option 1: Use --user flag
pip install --user -U crawl4ai

# Option 2: Use virtual environment (recommended)
python -m venv ~/.venv/crawl4ai
source ~/.venv/crawl4ai/bin/activate
pip install -U crawl4ai
crwl --setup
```

### LLM Configuration

If LLM features (`-q`, schema extraction) fail with authentication errors, create `~/.crawl4ai/global.yml`:

```yaml
llm:
  provider: openai
  api_key: sk-your-api-key
  model: gpt-4o
```

Or set environment variables:

```bash
export OPENAI_API_KEY=sk-your-api-key
```

Supported providers: `openai`, `anthropic`, `ollama`, and any LiteLLM-compatible provider.