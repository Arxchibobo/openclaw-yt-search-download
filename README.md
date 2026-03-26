# openclaw-yt-search-download

An [OpenClaw](https://github.com/Arxchibobo/openclaw) skill for searching, downloading, and extracting subtitles from YouTube videos using the YouTube Data API v3 and yt-dlp.

## Overview

`yt-search-download` gives Claude Code the ability to interact with YouTube directly from the terminal. It supports keyword search, channel browsing, video/audio downloads, metadata retrieval, and subtitle extraction — all driven by natural language triggers through the OpenClaw skill system.

## Features

- **Keyword search** — Search YouTube globally with result count, sort order, duration filters, and date range filters
- **Channel browsing** — List videos from any channel by handle, URL, or channel ID
- **Video download** — Download videos at selectable quality (best, 1080p, 720p, 480p) using yt-dlp
- **Audio extraction** — Download audio only as MP3
- **Subtitle download** — Fetch subtitles and convert to both SRT and plain TXT formats
- **Video info** — Retrieve detailed metadata for any video URL
- **Markdown table output** — Results rendered as Markdown tables with translation placeholders for titles
- **JSON export** — Machine-readable output with `--json`

## Requirements

| Dependency | Purpose |
|---|---|
| Python 3 | Runtime for `scripts/yt_search.py` |
| YouTube Data API v3 key | Search and metadata (set as `YOUTUBE_API_KEY` or `YT_BROWSE_API_KEY`) |
| [yt-dlp](https://github.com/yt-dlp/yt-dlp) | Video/audio/subtitle downloads |

### Get a YouTube API Key

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a project and enable the **YouTube Data API v3**
3. Generate an API key under **APIs & Services → Credentials**
4. Export it in your shell:

```bash
export YOUTUBE_API_KEY="your_api_key_here"
```

### Install yt-dlp

```bash
# macOS
brew install yt-dlp

# or via pip
pip install yt-dlp
```

## Installation

1. Clone this repository into your OpenClaw skills directory:

```bash
git clone https://github.com/Arxchibobo/openclaw-yt-search-download
```

2. Register the skill with OpenClaw by pointing it to the `SKILL.md` file in this repository.

3. Ensure your `YOUTUBE_API_KEY` environment variable is set.

## Usage

Once the skill is active in OpenClaw, Claude Code will invoke `scripts/yt_search.py` automatically based on natural language triggers. You can also run the script directly.

### Search videos

```bash
python scripts/yt_search.py search "machine learning tutorial" -n 10 -o date
```

| Option | Description |
|---|---|
| `-n N` | Number of results (default: 5) |
| `-o ORDER` | Sort by: `relevance`, `date`, `viewCount`, `rating` |
| `--min-duration` / `--max-duration` | Duration filters, e.g. `30m`, `1h`, `1h30m` |
| `--after` / `--before` | Date range filters (`YYYY-MM-DD`) |
| `--json` | Output as JSON |

### Browse a channel

```bash
python scripts/yt_search.py channel @mkbhd -n 20 --sort-by viewCount
```

### Download a video

```bash
python scripts/yt_search.py download "https://youtu.be/VIDEO_ID" --quality 1080p --out ~/Downloads
```

| Option | Description |
|---|---|
| `--quality` | `best`, `1080p`, `720p`, `480p` |
| `--audio` | Extract audio only (MP3) |
| `--out DIR` | Output directory |
| `--subs` | Download subtitles |

### Get video info

```bash
python scripts/yt_search.py info "https://youtu.be/VIDEO_ID"
```

### Extract subtitles

```bash
# Step 1: download subtitles
python scripts/yt_search.py download "https://youtu.be/VIDEO_ID" --subs

# Step 2: convert to plain text (yt-dlp generates .vtt; convert to SRT then TXT)
```

## Configuration

| Environment Variable | Description |
|---|---|
| `YOUTUBE_API_KEY` | YouTube Data API v3 key (primary) |
| `YT_BROWSE_API_KEY` | Alternative variable name for the API key |

## License

See [LICENSE](LICENSE) if present in this repository.
