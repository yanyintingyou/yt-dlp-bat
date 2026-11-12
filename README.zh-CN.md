# yt-dlp-bat

简体中文 | [English](README.md)

一个 Windows 批处理文件，双击就能用 yt-dlp 下载视频。粘贴链接，文件存到 `source\`。

脚本首次运行会下载 `yt-dlp.exe`，自动创建 `source` 文件夹，然后循环等待输入链接，直到退出。

## 为什么用它

yt-dlp 负责下载，麻烦的是记住那一串参数。这个脚本固定了常用选项（最佳音视频、MP4 合并、16 个分片并发、失败重试），做成双击即用的 `.bat`，适合偶尔下载长视频。

## 快速开始

环境要求：

- Windows，系统自带 PowerShell（首次下载 `yt-dlp.exe` 时使用）
- ffmpeg 已加入 PATH（需要把分离的视频流和音频流合并为 MP4 时使用）

下载 [video_download.bat](video_download.bat)，双击运行。首次运行会把最新版 `yt-dlp.exe`（约 17 MB）下载到同一目录。

提示为中文：

```
请输入视频链接（直接回车或输入 q 退出）：
```

粘贴链接后回车。下载完成后会回到输入提示；直接回车，或输入 `q` / `exit` 退出。文件保存在 `source\`。

## 它做了什么

| 环节 | 行为 |
|---|---|
| 首次运行 | 缺少 `yt-dlp.exe` 时，从 GitHub 最新 release 下载 |
| 每次输入 | 用下面的固定参数调用 yt-dlp |
| 输出 | 写入 `source\<标题> [<id>].<扩展名>` |
| 循环 | 回到输入提示；空输入、`q`、`exit` 退出 |

## yt-dlp 参数

| 参数 | 当前值 | 作用 |
|---|---|---|
| `-f` | `bestvideo+bestaudio/best` | 最佳视频加最佳音频，失败时回退到最佳单文件 |
| `--merge-output-format` | `mp4` | 合并输出为 MP4 |
| `--concurrent-fragments` | `16` | 最多 16 个分片并发下载 |
| `--retries` | `10` | 请求失败重试次数 |
| `--fragment-retries` | `10` | 分片失败重试次数 |
| `-o` | `source\<标题> [<id>].<扩展名>` | 输出目录和文件名 |

要改画质、并发数或命名，编辑 `video_download.bat` 里对应的几行。

## 已知限制

- 仅支持 Windows，提示和状态信息为中文。
- 没有画质选择，yt-dlp 直接取可用的最佳流。
- 脚本只下载 `yt-dlp.exe`，ffmpeg 需要自己安装。
- `yt-dlp.exe` 只在缺失时下载。要更新版本，删掉它再运行。
- 不支持 Cookie、登录、代理和字幕选项。私密或年龄限制视频可能失败，需要自己补参数。
- 每次处理一个链接。脚本会循环，但不是队列或批量调度器。
- 只下载你有权保存的内容。脚本不绕过 DRM 或访问控制。

## 文件

| 路径 | 用途 |
|---|---|
| `video_download.bat` | 全部逻辑 |
| `source/` | 运行时创建，存放下载的视频（已加入 .gitignore） |
| `yt-dlp.exe` | 运行时下载（已加入 .gitignore） |

## License

MIT，见 [LICENSE](LICENSE)。作者：[yanyintingyou](https://github.com/yanyintingyou)。
