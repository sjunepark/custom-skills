# Summarize CLI Reference

## Install

Requires Node 22+.

```bash
npx -y @steipete/summarize "https://example.com"
npm i -g @steipete/summarize
brew install steipete/tap/summarize
```

Verify install:

```bash
summarize --version
summarize --help
```

## Core Command Forms

```bash
summarize "https://example.com"
summarize "/path/to/file.pdf"
echo "text" | summarize -
```

Media and feeds:

```bash
summarize "https://youtu.be/<id>" --youtube auto
summarize "https://feeds.npr.org/500005/podcast.xml"
summarize "/path/to/video.mp4"
```

## Model and Provider Selection

Use gateway-style ids: `<provider>/<model>`.

```bash
summarize "https://example.com" --model openai/gpt-5-mini
summarize "https://example.com" --model anthropic/claude-sonnet-4-5
summarize "https://example.com" --model google/gemini-3-flash-preview
summarize "https://example.com" --model auto
```

OpenRouter-specific:

```bash
summarize "https://example.com" --model openrouter/openai/gpt-5-mini
summarize refresh-free --set-default
summarize "https://example.com" --model free
```

## Environment Variables

Match keys to provider/model:

- `OPENAI_API_KEY`
- `ANTHROPIC_API_KEY`
- `GEMINI_API_KEY`
- `XAI_API_KEY`
- `NVIDIA_API_KEY`
- `OPENROUTER_API_KEY`
- `Z_AI_API_KEY` (or `ZAI_API_KEY`)

Optional service keys/tools:

- `FIRECRAWL_API_KEY`
- `APIFY_API_TOKEN`
- `FAL_KEY`
- `YT_DLP_PATH`
- `OPENAI_USE_CHAT_COMPLETIONS=1` (OpenAI-compatible toggle)

## High-Value Flags

```bash
--length short|medium|long|xl|xxl|<chars>
--language <lang>
--max-output-tokens <count>
--json
--verbose
--extract
--format md|text
--slides
--slides-ocr
--timeout 2m
--retries 1
```

Examples:

```bash
summarize "https://example.com" --length long --language en
summarize "https://example.com" --extract --format md
summarize "https://www.youtube.com/watch?v=<id>" --slides --slides-ocr
summarize "https://example.com" --json --verbose
```

## Config File

Location: `~/.summarize/config.json`

Minimal example:

```json
{
  "model": "openai/gpt-5-mini",
  "env": { "OPENAI_API_KEY": "sk-..." },
  "ui": { "theme": "ember" }
}
```

Model precedence:

1. `--model`
2. `SUMMARIZE_MODEL`
3. `~/.summarize/config.json`
4. default `auto`

## Troubleshooting

`Missing command`
- Install with npm/Homebrew and re-check `summarize --version`.

`Provider auth or model errors`
- Confirm correct API key env var for selected model provider.
- Retry with `--model auto` or another known-good model.

`Website extraction incomplete`
- Retry with `--extract --format md`.
- Configure `FIRECRAWL_API_KEY` and use `--firecrawl auto|always`.

`YouTube/media extraction issues`
- Ensure `yt-dlp` and `ffmpeg` are installed.
- Add `tesseract` for `--slides-ocr`.
- For transcript fallback, set `OPENAI_API_KEY` or `FAL_KEY`.

`Need machine-readable diagnostics`
- Use `--json`.
- Add `--verbose` for stderr-level details.
