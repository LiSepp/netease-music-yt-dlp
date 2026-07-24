# NetEase Music yt-dlp Downloader

使用 yt-dlp 下载网易云音乐歌单并整理本地音乐库。

## Features

- Download NetEase Cloud Music playlists
- Convert audio to MP3
- Embed cover artwork
- Write metadata for NAS music libraries
- Use the playlist name as the MP3 Album field
- Group files by playlist folder
- Filename format:

```
playlist name/creator - title [id].mp3
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

On Windows, yt-dlp can also read an explicit config file such as:

```
C:/Li_app/yt-dlp/yt-dlp.conf
```

Before use, replace `USERNAME` with your Windows user name.

The output directory in the sample config is:

```
C:/Users/USERNAME/Music/NewDownloads/PLAYLIST_NAME
```

## Metadata Notes

NetEase Cloud Music may leave the standard `artist` field empty for some songs. This config uses `creator` instead:

```
%(creator|Unknown)s - %(title)s [%(id)s].%(ext)s
```

For playlist downloads, the MP3 Album field is set to the playlist name:

```
--parse-metadata "%(playlist_title,album|Unknown)s:%(album)s"
```

This keeps albums grouped by NetEase playlist in music library software. If a track is downloaded outside a playlist, yt-dlp falls back to the original album name, then `Unknown`.

For Apple Music, iMazing, Apple Watch, and NAS imports, you may also want to rewrite existing MP3 tags as a compilation:

- `Album`: playlist name
- `Album Artist`: a fixed value such as `NetEase Playlist`
- `Compilation`: enabled

This prevents one playlist album from being split into many albums by different track artists.

## Usage Examples

```
yt-dlp "PLAYLIST_URL_1"
yt-dlp "PLAYLIST_URL_2"
yt-dlp "PLAYLIST_URL_3"
yt-dlp "PLAYLIST_URL_4"
```

Keep real playlist URLs, account paths, and cookie files local. Do not commit them to GitHub.

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

### Short preview files

Some restricted or unavailable tracks may download as very short files, even with a logged-in account. Scan downloaded MP3 durations, retry once with cookies if you have permission to access the content, and keep persistent preview files outside the main library.
