---
name: summarize
description: Use the steipete/summarize CLI to summarize URLs, local files, stdin, YouTube links, podcasts, and media with LLM models. Trigger when users ask to install or run summarize, configure model/provider API keys, tune output flags (length/language/json/extract/slides), set defaults in ~/.summarize/config.json, or troubleshoot summarize CLI errors.
---

# Summarize CLI

Use `summarize` to extract and summarize web pages, documents, and media from terminal workflows.

## Workflow

1. Identify the input type and desired output format.
2. Verify installation with `summarize --version`.
3. Configure provider credentials for the selected model.
4. Run a minimal command first, then add flags incrementally.
5. Use `--verbose` or `--json` for diagnostics when a run fails.

## Quick Patterns

- Summarize a URL: `summarize "https://example.com"`
- Summarize a local file: `summarize "/path/to/file.pdf"`
- Summarize piped input: `cat notes.md | summarize -`
- Force model/provider: `summarize "https://example.com" --model openai/gpt-5-mini`
- Control length: `summarize "https://example.com" --length long`
- Return diagnostics JSON: `summarize "https://example.com" --json`
- Extract content without summarizing: `summarize "https://example.com" --extract --format md`

## Configuration Rules

- Prefer explicit `--model` on task-critical runs.
- Use `~/.summarize/config.json` for stable defaults.
- Remember precedence for model selection:
  1. `--model`
  2. `SUMMARIZE_MODEL`
  3. `~/.summarize/config.json`
  4. built-in default (`auto`)

## Troubleshooting Flow

1. Re-run with `--verbose`.
2. Confirm API key matches the chosen provider/model.
3. Switch to a known-good model (for example `openai/gpt-5-mini` or `google/gemini-3-flash-preview`).
4. For media or YouTube failures, verify external dependencies (`yt-dlp`, `ffmpeg`, optionally `tesseract`).
5. Use `--json` to inspect extraction and metric fields.

## References

Read [references/cli.md](references/cli.md) for install commands, provider keys, advanced flags, and error-specific guidance.
