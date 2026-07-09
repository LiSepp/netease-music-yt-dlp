# Troubleshooting

## artist field is empty

NetEase Cloud Music may return:

- creator
- album_artist
- creators

instead of the standard artist field.

Use:

```
%(creator)s - %(title)s
```

## Filename becomes NA.mp3

Cause:

The requested metadata field is missing.

Solution:

Use fallback:

```
%(creator|Unknown)s
```

## Do not use restrict-filenames

`--restrict-filenames` removes Chinese characters and is not suitable for a music library.

Use:

```
--windows-filenames
```

## Windows path issue

Recommended:

```
C:/Users/USERNAME/Music/NewDownloads
```

instead of backslashes inside configuration files.

## Config encoding

Save `yt-dlp.conf` as UTF-8.
