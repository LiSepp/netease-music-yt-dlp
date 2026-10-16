# 网易云音乐歌单下载：零基础指南

用 yt-dlp 把自己有权访问的网易云音乐歌单保存到电脑。本指南按顺序一步一步来；完成一项，再做下一项。

## 开始前准备

电脑需要安装 **yt-dlp** 和 **FFmpeg**，并准备一个用于保存音乐的文件夹。

### 先准备登录 Cookie（重点）

> **先登录网易云音乐网页版，再导出 Cookie。**
>
> Cookie 相当于网页登录凭证。只保存在自己的电脑上，绝不要发给别人、贴到聊天里或上传到 GitHub。

1. 在 Chrome 打开[网易云音乐网页版](https://music.163.com/)并登录。
2. 在当前已登录的网易云页面，打开 Chrome 网上应用店安装 [Get cookies.txt LOCALLY](https://chromewebstore.google.com/detail/get-cookiestxt-locally/cclelndahbckbenkjhflpdbgdldlbecc)。
3. 安装后点 Chrome 右上角的“拼图”图标，找到扩展 **Get cookies.txt LOCALLY**，点旁边的“图钉”固定到工具栏。之后点工具栏上的扩展图标。
4. 在扩展小窗口确认 **Export Format** 选择 **Netscape**，然后点 **Export As**。
5. 把下载的文件保存或移动到 yt-dlp 配置文件夹之外的私人位置，例如 `C:\yt-dlp-private\netease-cookies.txt`。如果扩展保存为 `cookies.txt`，可改名为 `netease-cookies.txt`。

> 如果扩展窗口没有网易云 Cookie，先回到网易云网页确认仍然登录，再保持网易云页面为当前标签页，重新点扩展。无需把 Cookie 内容发给 Agent。

## 让 yt-dlp 使用 Cookie

在本机的 yt-dlp 配置文件中加入这一行，把示例路径换成你实际保存的位置：

```
--cookies "C:/yt-dlp-private/netease-cookies.txt"
```

路径使用正斜杠比较省心。Cookie 文件和配置文件都留在本机，不要放进 GitHub 仓库。

### 让 Agent 继续下载

把下面这段发给 Codex 或其他本机 Agent，并替换方括号里的内容：

```text
我已经登录网易云网页版，并用 Get cookies.txt LOCALLY 导出了 Netscape 格式 Cookie。
Cookie 文件在：[本机 Cookie 文件完整路径]
请先检查本机 yt-dlp 配置能否读取它，再用一首我有权访问的歌曲做小规模验证。
验证通过后下载这个歌单：[歌单链接]
音乐保存到：[本机音乐目录]
不要打印、复制或上传 Cookie 内容。已有文件不要重复下载；只有确认是试听或损坏的文件才先备份，再重下。
```

Agent 只需要本机 Cookie 文件的路径来配置 yt-dlp，不需要你把 Cookie 内容复制给它或贴到对话里。

## 下载一个歌单

将歌单 ID 放到标准链接格式中：

```
https://music.163.com/#/playlist?id=PLAYLIST_ID
```

然后运行：

```
yt-dlp "https://music.163.com/#/playlist?id=PLAYLIST_ID"
```

如果你不熟悉命令行，可以把歌单链接、本机 Cookie 文件路径和保存目录发给 Agent，并让它先验证一首歌，再开始整张歌单。

## 文件名、标签和重复下载

示例配置会把歌曲放进歌单名称目录，文件名格式为：

```
歌手 - 歌名 [歌曲ID].mp3
```

网易云有时没有标准的 `artist` 字段，因此模板使用 `creator`。MP3 的“专辑”字段可写歌单名称，便于音乐库按歌单归类。

配置中的 `--no-overwrites` 会跳过同一路径下已有的文件，适合中断后继续下载。要修复试听或损坏文件，先把那一首移到备份文件夹，再重跑歌单。相同歌曲出现在不同歌单文件夹时会各保存一份，这是为了保留各歌单的标签。

## 出问题时先看这里

- 只有部分歌曲失败：看日志里是否有“无媒体链接”或地区限制；其他歌曲通常会继续下载。
- 下载出来只有几十秒：可能拿到的是试听片段；先检查 Cookie，再备份短文件并单独重试。
- 一大批歌曲同时失败：检查网络或失效的代理设置。
- 中文文件名变成下划线：确认没有使用 `--restrict-filenames`，应使用 `--windows-filenames`。
- 重新运行没替换旧文件：这是 `--no-overwrites` 的预期行为；先备份要替换的那首。

更多排查步骤见[故障排查指南](docs/troubleshooting.md)。

## Cookie 安全提醒

本仓库只说明操作方法，不包含任何人的 Cookie、真实歌单链接或个人电脑路径。Cookie 过期后，在浏览器重新登录并重新导出即可。