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

## Do not use artist for NetEase

NetEase Cloud Music extractor data is not always mapped to the standard `artist` field. Some tracks return the singer in `creator` and `album_artist`, while `artist` may be empty.

Use `creator` in output templates:

```
%(creator|Unknown)s - %(title)s [%(id)s].%(ext)s
```

Do not use:

```
%(artist)s
```

## Album should show playlist name

For NAS music libraries, it is often more useful for Album to show the NetEase playlist name instead of the original release album.

Use:

```
--parse-metadata "%(playlist_title,album|Unknown)s:%(album)s"
```

This means:

- use `playlist_title` during playlist downloads
- otherwise fall back to the original `album`
- use `Unknown` only when neither field is available

## Playlist album splits by artist

Some music managers split one album into separate albums when every track has a different Artist.

For Apple Music, iMazing, Apple Watch, or NAS imports, keep the track Artist as the singer, but rewrite existing MP3 files as a compilation:

- `Album`: playlist name
- `Album Artist`: one fixed value, for example `NetEase Playlist`
- `Compilation`: enabled

This keeps a playlist album grouped while preserving the per-song Artist field.

## Very short preview files

Restricted, unavailable, or region-limited tracks may download as very short MP3 files, such as 30 to 60 seconds, even when the command itself exits successfully.

Recommended workflow:

1. Export browser cookies locally when you are logged in and allowed to access the tracks.
2. Retry the short files once using the same direct connection.
3. Scan MP3 duration after retry.
4. Move persistent preview files out of the main library instead of deleting them immediately.

Do not commit cookie files, account paths, or private playlist URLs.

## Long filename errors

Some tracks have very long artist lists. Windows may reject the thumbnail or audio path with an invalid argument error.

Retry that single song with a shorter output template:

```
-o "C:/Users/USERNAME/Music/NewDownloads/PLAYLIST_NAME/Short Artist - Short Title [%(id)s].%(ext)s"
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
