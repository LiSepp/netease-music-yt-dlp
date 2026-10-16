# NetEase Music yt-dlp Downloader

使用 yt-dlp 下载网易云音乐歌单并整理本地音乐库。

## Features

- Download NetEase Cloud Music playlists
- Convert audio to MP3 and embed cover artwork
- Write metadata for music library and NAS use
- Use the playlist name as the MP3 Album field
- Group files by playlist folder
- Keep filenames stable and identifiable by track ID:

```
playlist name/creator - title [id].mp3
```

## Requirements

- Windows
- yt-dlp
- FFmpeg

## Configuration

Copy `config/yt-dlp.conf` to your yt-dlp configuration directory. On Windows, yt-dlp can read an explicit configuration file from a path such as:

```
C:/path/to/yt-dlp/yt-dlp.conf
```

Replace the sample output path in the config with a local music-library path. Keep usernames, cookies, account paths, and personal playlist URLs out of this public repository.

## Metadata

NetEase's extractor may provide `creator` while leaving the standard `artist` field empty. Use `creator` in the output template:

```
%(creator|Unknown)s - %(title)s [%(id)s].%(ext)s
```

For playlist downloads, set the MP3 `Album` field to the playlist name:

```
--parse-metadata "%(playlist_title,album|Unknown)s:%(album)s"
```

This falls back to the original album for a single-track download, then to `Unknown`. For libraries that split albums by performer, set a consistent `Album Artist` and the compilation tag while retaining each track's real `Artist`.

## Usage

Pass a playlist URL to yt-dlp:

```
yt-dlp "https://music.163.com/#/playlist?id=PLAYLIST_ID"
```

The `#/playlist?id=...` form is recognized reliably. Mobile/share routes such as `#/my/m/music/playlist` may be treated as generic webpages; normalize them to the canonical playlist route using the same playlist ID.

## Safe repeat runs and duplicate handling

The sample config uses a playlist-name subfolder, a filename containing the track ID, and `--no-overwrites`. Re-running the same playlist skips files already present at the same path, so it is suitable for resuming or updating a library without downloading those files again. Keep the playlist folder and track ID in the path; a flat template can collide across playlists or cause one playlist's metadata to replace another's.

`--no-overwrites` also means an existing file will not be refreshed if its tags or audio are wrong. To replace a confirmed preview or damaged file, move that file out of the target folder first, then rerun the playlist. Do not delete it until the replacement is checked.

## Troubleshooting

See [docs/troubleshooting.md](docs/troubleshooting.md) for extractor, network, preview, filename, and metadata issues.