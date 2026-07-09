# NetEase Music yt-dlp Downloader

使用 yt-dlp 下载网易云音乐歌单并整理本地音乐库。

## Features

- Download NetEase Cloud Music playlists
- Convert audio to MP3
- Embed cover artwork
- Write metadata
- Filename format:

```
creator - title [id].mp3
```

Example:

```
杨又歌 - 过客 [1857317023].mp3
```

## Requirements

- Windows
- yt-dlp
- FFmpeg

## Configuration

Copy `config/yt-dlp.conf` to your yt-dlp configuration directory.

Before use, replace `USERNAME` with your Windows user name.

## Troubleshooting

### artist is NA

NetEase extractor may provide `creator` instead of `artist`.

Use:

```
%(creator)s - %(title)s
```

### Wrong filenames

Do not use:

```
--restrict-filenames
```

It removes Chinese characters. Use:

```
--windows-filenames
```

### Config encoding error

Save `yt-dlp.conf` as UTF-8.
