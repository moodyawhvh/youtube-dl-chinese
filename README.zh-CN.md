<div align="center">

# youtube-dl 中文文档

**[中文版] youtube-dl — 从 YouTube 等视频网站下载视频的命令行工具**

[![原项目](https://img.shields.io/badge/原项目-ytdl--org--youtube--dl-blue?style=flat-square&logo=github)](https://github.com/ytdl-org/youtube-dl)
[![微信联系](https://img.shields.io/badge/微信-uaycar-brightgreen?style=flat-square&logo=wechat)](#)

</div>

---

> 本文档是 [ytdl-org/youtube-dl](https://github.com/ytdl-org/youtube-dl) 官方 README 的中文翻译版,仅翻译说明文字,不包含任何源代码。命令、链接与项目名保留英文原样。

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

## 简介

**youtube-dl** 是一个从 YouTube.com 及更多视频网站下载视频的命令行程序。它需要 Python 解释器(2.6、2.7 或 3.2+),不限定平台,在 Unix、Windows、macOS 上都能正常工作。项目以公有领域(Public Domain)形式发布,意味着你可以随意修改、再分发或以任何方式使用它。

基本用法:

```
youtube-dl [OPTIONS] URL [URL...]
```

## 安装

**所有 UNIX 用户(Linux、macOS 等)立即安装:**

```bash
sudo curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
sudo chmod a+rx /usr/local/bin/youtube-dl
```

没有 curl 的话,用较新版本的 wget 也可以:

```bash
sudo wget https://yt-dl.org/downloads/latest/youtube-dl -O /usr/local/bin/youtube-dl
sudo chmod a+rx /usr/local/bin/youtube-dl
```

**Windows 用户:** 可以直接[下载 .exe 文件](https://yt-dl.org/latest/youtube-dl.exe),放到 PATH 中的任意目录,**但不要**放在 `C:\Windows\System32` 下。

**使用 pip 安装:**

```bash
sudo -H pip install --upgrade youtube-dl
```

如果已经安装过,这条命令会把它升级到最新版。

**macOS 用户:** 可以用 [Homebrew](https://brew.sh/) 安装:

```bash
brew install youtube-dl
```

或者用 [MacPorts](https://www.macports.org/):

```bash
sudo port install youtube-dl
```

更多安装方式(包括 PGP 签名)请看 [youtube-dl 下载页](https://ytdl-org.github.io/youtube-dl/download.html)。

## 命令行选项

youtube-dl 的选项非常多,这里按类别汉化说明并保留代表性条目,完整列表请以 `youtube-dl --help` 与原版 README 为准。

### 通用选项

- `-h, --help`:打印帮助信息并退出
- `--version`:打印程序版本并退出
- `-U, --update`:升级到最新版本(注意权限,必要时加 sudo)
- `-i, --ignore-errors`:遇到下载错误继续,比如跳过播放列表里不可用的视频
- `--abort-on-error`:一旦出错就中止后续下载
- `--list-extractors`:列出所有支持的提取器
- `--flat-playlist`:不解析播放列表中的视频,仅列出
- `--no-color`:输出中不使用颜色代码
- `--config-location PATH`:指定配置文件位置

### 网络选项

- `--proxy URL`:使用指定的 HTTP/HTTPS/SOCKS 代理,例如 `socks5://127.0.0.1:1080/`;传空字符串表示直连
- `--socket-timeout SECONDS`:放弃前的等待时间(秒)
- `--source-address IP`:客户端绑定的 IP 地址
- `-4, --force-ipv4` / `-6, --force-ipv6`:强制走 IPv4 / IPv6

### 地理限制选项

- `--geo-bypass`:通过伪造 X-Forwarded-For 头绕过地域限制
- `--no-geo-bypass`:不绕过地域限制
- `--geo-bypass-country CODE`:用显式指定的两位国家代码绕过地域限制

### 视频选择选项

- `--playlist-start NUMBER` / `--playlist-end NUMBER`:播放列表起止序号
- `--playlist-items ITEM_SPEC`:指定下载播放列表中的哪些项,如 `--playlist-items 1,2,5,8` 或 `--playlist-items 1-3,7,10-13`
- `--match-title REGEX` / `--reject-title REGEX`:按标题正则匹配下载 / 排除
- `--min-filesize SIZE` / `--max-filesize SIZE`:过滤文件大小,如 `50k`、`44.6m`
- `--date DATE` / `--datebefore DATE` / `--dateafter DATE`:按上传日期过滤
- `--match-filter FILTER`:通用过滤表达式,如 `"like_count > 100 & dislike_count <? 50 & description"`
- `--no-playlist`:URL 同时指向视频和播放列表时只下载视频
- `--yes-playlist`:URL 同时指向视频和播放列表时下载整个播放列表
- `--download-archive FILE`:只下载归档文件中没有记录的视频,并自动记录已下载 ID

### 下载选项

- `-r, --limit-rate RATE`:限速,如 `50K`、`4.2M`
- `-R, --retries RETRIES`:重试次数(默认 10),也可设为 `infinite`
- `--fragment-retries RETRIES`:分片重试次数(DASH、HLS、ISM)
- `--skip-unavailable-fragments`:跳过不可用的分片
- `--keep-fragments`:下载完成后保留分片文件
- `--buffer-size SIZE`:下载缓冲区大小(默认 1024)
- `--http-chunk-size SIZE`:分块下载的块大小,可能有助于绕过服务器的带宽限流(实验性)
- `--playlist-reverse` / `--playlist-random`:倒序 / 随机下载播放列表
- `--external-downloader COMMAND`:使用外部下载器,支持 aria2c、avconv、axel、curl、ffmpeg、httpie、wget
- `-a, --batch-file FILE`:从文件批量读取 URL(每行一个,`-` 表示 stdin)
- `-w, --no-overwrites`:不覆盖已有文件
- `-c, --continue`:强制续传半成品文件(默认会尽量续传)
- `--write-description` / `--write-info-json`:把视频描述 / 元数据写到文件
- `--cookies FILE`:读写 Cookie 文件
- `--rm-cache-dir`:删除所有缓存文件

### 字幕选项

- `--write-sub`:下载字幕文件
- `--write-auto-sub`:下载自动生成的字幕(仅 YouTube)
- `--all-subs`:下载所有可用字幕
- `--list-subs`:列出视频的所有可用字幕
- `--sub-format FORMAT`:字幕格式偏好,如 `"srt"` 或 `"ass/srt/best"`
- `--sub-lang LANGS`:要下载的字幕语言,逗号分隔

### 认证选项

- `-u, --username USERNAME` / `-p, --password PASSWORD`:账号密码登录(省略密码时会交互式询问)
- `-2, --twofactor TWOFACTOR`:两步验证码
- `-n, --netrc`:使用 `.netrc` 认证数据
- `--video-password PASSWORD`:视频密码(vimeo、youku)

### 后处理选项

- `-x, --extract-audio`:把视频转换为纯音频文件(需要 ffmpeg/avconv 与 ffprobe/avprobe)
- `--audio-format FORMAT`:音频格式:`best`、`aac`、`flac`、`mp3`、`m4a`、`opus`、`vorbis`、`wav`(默认 best,仅在配合 `-x` 时生效)
- `--audio-quality QUALITY`:音频质量,VBR 取 0(最好)到 9(最差),或指定比特率如 `128K`(默认 5)
- `--recode-video FORMAT`:必要时把视频转码为其他格式(支持 mp4|flv|ogg|webm|mkv|avi)
- `-k, --keep-video`:后处理后保留原视频文件(默认删除)
- `--embed-subs`:把字幕嵌入视频(仅 mp4、webm、mkv)
- `--embed-thumbnail`:把封面嵌入音频
- `--add-metadata`:向文件写入元数据
- `--metadata-from-title FORMAT`:从视频标题解析歌曲名 / 艺术家等元数据

## 输出模板

用 `-o` 可以自定义输出文件名模板,模板中可以用视频的各类元数据字段,常见的有:

- `%(title)s`:视频标题
- `%(id)s`:视频 ID
- `%(uploader)s`:上传者
- `%(upload_date)s`:上传日期(YYYYMMDD)
- `%(duration)s`:时长(秒)
- `%(view_count)s`:播放量
- `%(ext)s`:扩展名
- `%(autonumber)s`:自动编号

示例:

```bash
# 按 "标题-ID.ext" 命名
youtube-dl -o '%(title)s-%(id)s.%(ext)s' URL

# 按上传者分目录保存
youtube-dl -o '%(uploader)s/%(title)s.%(ext)s' URL
```

完整字段列表请参考原版 README 的 OUTPUT TEMPLATE 一节。

## 格式选择

用 `-f, --format FORMAT` 指定要下载的格式,支持按格式编号、扩展名、分辨率等自由组合,几个常用示例:

- `-f 22`:下载格式编号 22
- `-f bestvideo+bestaudio --merge-output-format mp4`:下载最佳视频流和最佳音频流并合并为 mp4(需要 ffmpeg)
- `-f 'bestvideo[height<=720]+bestaudio'`:限制最高 720p
- `-f 'best[ext=mp4]'`:只要 mp4 扩展名的最佳格式
- `-F, --list-formats`:先列出视频的所有可用格式再决定

## 视频选择示例

```bash
# 只下载播放列表的第 5 到第 7 个视频
youtube-dl --playlist-items 5-7 PLAYLIST_URL

# 只下载 2015 年 1 月 1 日之后上传、且小于 100MB 的视频
youtube-dl --dateafter 20150101 --max-filesize 100m URL

# 只下载标题包含 "concert" 的视频
youtube-dl --match-title "concert" PLAYLIST_URL
```

## 常见问题(节选)

- **下载报错 / 网站改版?** 先用 `-U` 升级到最新版;多数"无法提取视频"的问题都是版本过旧导致的。
- **可以同时下载多个视频吗?** youtube-dl 本身串行下载,可以配合 aria2c 等外部下载器(`--external-downloader`)提升速度。
- **如何批量管理已下载内容?** 使用 `--download-archive` 记录已下载 ID,配合定时任务可实现增量同步。

## 开发者说明

- 想参与开发,直接 clone 本 git 仓库,基于 master 分支开发并提交 Pull Request。
- 新增站点支持需要编写对应的提取器(extractor),参考仓库内现有的提取器写法。
- 上报 Bug 前请务必用最新版本复现,并附上 `-v` 的完整日志输出(注意隐去账号、Cookie 等敏感信息)。

## 嵌入 youtube-dl

youtube-dl 提供公开的 Python API,可以在自己的程序中导入 `youtube_dl` 模块调用,并支持通过 `YoutubeDL` 的参数字典控制行为、通过 `progress_hooks` 回调获取下载进度。具体接口请参考原版 README 的 EMBEDDING YOUTUBE-DL 一节与源代码。

## 版权

youtube-dl 发布于公有领域(Public Domain),不受版权保护,你可以出于任何目的修改、再分发或使用它。原项目由 [ytdl-org](https://github.com/ytdl-org) 维护,感谢所有贡献者。

---

> 本文档为 [ytdl-org/youtube-dl](https://github.com/ytdl-org/youtube-dl) 官方 README 的中文翻译版本,所有代码版权归原项目作者所有,遵循其原始许可证(公有领域)。
> 翻译内容仅供学习参考,如有歧义请以英文原版为准。

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

**如果觉得有用,请给原项目点个 Star!** ⭐
