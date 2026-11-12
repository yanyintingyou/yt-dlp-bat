# yt-dlp-bat

[简体中文](README.zh-CN.md) | English

A single Windows batch file for downloading videos with yt-dlp. Double-click it, paste a link, get the video in `source\`.

The script downloads `yt-dlp.exe` on first run, creates a `source` folder, and keeps prompting for links until you quit.

## Why this exists

yt-dlp handles the download. Remembering the flags is the annoying part. This wrapper fixes the options most people need (best video and audio, MP4 merge, 16 parallel fragments, retries) and turns the job into a double-clickable `.bat` file for occasional long videos.

## Quick start

Requirements:

- Windows with PowerShell (included with Windows, used for the first-run download)
- ffmpeg on PATH if you want separate video and audio streams merged into MP4

Download [video_download.bat](video_download.bat) and double-click it. On first run it fetches the latest `yt-dlp.exe` (about 17 MB) into the same folder.

The prompt is in Chinese:

```
请输入视频链接（直接回车或输入 q 退出）：
```

Paste a URL and press Enter. When the download finishes, the prompt returns. Press Enter, or type `q` or `exit`, to quit. Files go to `source\`.

## What it does

| Stage | Behavior |
|---|---|
| First run | Downloads `yt-dlp.exe` from the latest GitHub release if the file is missing |
| Each URL | Runs yt-dlp with the fixed flags below |
| Output | Writes to `source\<title> [<id>].<ext>` |
| Loop | Returns to the prompt; empty input, `q`, or `exit` quits |

## yt-dlp flags

| Flag | Value | Effect |
|---|---|---|
| `-f` | `bestvideo+bestaudio/best` | Best video plus best audio; falls back to the best single file |
| `--merge-output-format` | `mp4` | Merges the streams into MP4 |
| `--concurrent-fragments` | `16` | Downloads up to 16 fragments in parallel |
| `--retries` | `10` | Retries failed requests |
| `--fragment-retries` | `10` | Retries failed fragments |
| `-o` | `source\<title> [<id>].<ext>` | Output folder and filename |

Edit these lines in `video_download.bat` to change quality, concurrency, or naming.

## Known limits

- Windows only. The prompt and status messages are in Chinese.
- No quality selector. yt-dlp takes the best available streams.
- The script downloads `yt-dlp.exe` only. Install ffmpeg separately for MP4 merging.
- `yt-dlp.exe` is fetched only when missing. Delete it to pull a newer release.
- No cookies, login, proxy, or subtitle options. Private or age-restricted videos may fail until you add those flags.
- One URL per prompt. It loops, but it is not a queue or batch scheduler.
- Download only content you have the right to keep. The script does not bypass DRM or access controls.

## Files

| Path | Purpose |
|---|---|
| `video_download.bat` | The whole tool |
| `source/` | Created at runtime, holds downloaded videos (gitignored) |
| `yt-dlp.exe` | Downloaded at runtime (gitignored) |

## License

MIT. See [LICENSE](LICENSE). Built by [yanyintingyou](https://github.com/yanyintingyou).
