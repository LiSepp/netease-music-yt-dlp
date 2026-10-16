# Troubleshooting

## Empty artist field or NA filenames

NetEase Cloud Music may return `creator`, `album_artist`, or `creators` instead of the standard `artist` field. Use `creator` with a fallback:

```
%(creator|Unknown)s - %(title)s [%(id)s].%(ext)s
```

Avoid `%(artist)s` for this extractor.

## Album should show the playlist name

Use:

```
--parse-metadata "%(playlist_title,album|Unknown)s:%(album)s"
```

This prefers `playlist_title` during playlist downloads, falls back to the original `album` for single tracks, then uses `Unknown`.

If a music manager splits a playlist album by performer, keep the per-track `Artist`, set one consistent `Album Artist`, and enable the compilation tag.

## Resume without downloading existing files again

Use a path with a playlist subfolder and track ID, together with `--no-overwrites`. Re-running that playlist skips a file already present at the same path. Keep the same output template across runs.

This is path-based: separate playlist folders intentionally allow the same track to exist in more than one playlist. `--no-overwrites` does not repair an existing short, damaged, or incorrectly tagged file. Move only the confirmed bad file to a backup folder before rerunning, and inspect the replacement before removing the backup.

## Very short preview files

A command can complete while restricted, unavailable, or region-limited tracks produce a short preview (often around 30–60 seconds). When authorized to access the track, use a valid browser cookie file stored locally, retry the affected track or playlist, then check duration. One tested preview grew from about 30 seconds to about 199 seconds with a logged-in cookie, and logs showed higher available audio tiers.

Keep cookie files out of Git. Move persistent previews out of the main library rather than deleting them immediately. A successful process exit alone does not prove that every track is complete.

## Proxy connection failures

A stale proxy environment setting can make yt-dlp connect to a dead local proxy (for example, a loopback port) and skip a large range of tracks. Check the process environment and proxy settings when many unrelated tracks fail together. After correcting the network route, rerun the same playlist; existing files are skipped by `--no-overwrites`.

Use direct connection settings only when that matches the user's network preference. Do not add a proxy option just to silence connection errors.

## No media links or region restrictions

Messages such as `No media links found` or geo-restriction errors usually mean the service did not provide a playable source for that track in the current session or region. With `--ignore-errors`, yt-dlp continues, so inspect the log and retry selectively later. These messages do not by themselves prove the config is broken.

## Unsupported music share pages

Some music services expose playlist and track metadata on a share page but do not expose a usable audio URL to yt-dlp. In that case yt-dlp may report `Unsupported URL`, and the generic extractor may fail too. Treat the service as unsupported by the current yt-dlp version; do not assume that a playlist page containing track names means the audio is downloadable.

## Long filenames and Windows path errors

Long artist lists can make a thumbnail or audio path fail on Windows. Retry only that track with a shorter output template while preserving its track ID:

```
-o "C:/path/to/music/PLAYLIST/Short Artist - Short Title [%(id)s].%(ext)s"
```

## Preserve Chinese filenames

Do not use `--restrict-filenames`; it removes non-ASCII characters. Use `--windows-filenames` and save the config as UTF-8.

Prefer forward slashes in config paths, for example `C:/path/to/music`.

## Case-sensitive post-processing edge case

A small number of uppercase `.MP3` source names have triggered a Windows post-processing/metadata error when yt-dlp changes the extension case. With `--ignore-errors`, the playlist continues but the affected ID may be missing or have incomplete tags. Check the expected track IDs after a run and retry those tracks individually if needed.