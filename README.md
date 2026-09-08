<div align="center">

# youtube-dl 中文翻译版

**[中文版] youtube-dl — 从 YouTube 等视频网站下载视频的命令行工具**

[![原项目](https://img.shields.io/badge/原项目-ytdl-org--youtube-dl-blue?style=flat-square&logo=github)](https://github.com/ytdl-org/youtube-dl)
[![中文文档](https://img.shields.io/badge/中文文档-README.zh--CN.md-orange?style=flat-square)](README.zh-CN.md)
[![GitHub Stars](https://img.shields.io/github/stars/ytdl-org/youtube-dl?style=flat-square&label=原项目Stars)](https://github.com/ytdl-org/youtube-dl/stargazers)
[![微信联系](https://img.shields.io/badge/微信-uaycar-brightgreen?style=flat-square&logo=wechat)](#)

</div>

---

> 这是 [ytdl-org/youtube-dl](https://github.com/ytdl-org/youtube-dl) 的中文翻译版本。
> 完整源代码请访问原项目:https://github.com/ytdl-org/youtube-dl

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

## 📖 项目简介

youtube-dl 是一个从 YouTube.com 及众多其他视频网站下载视频的命令行小程序。它基于 Python 解释器运行,不挑平台,Linux、Windows、macOS 通吃。项目以公有领域(Public Domain)形式发布,你可以随意修改、再分发、按自己的方式使用。它支持成百上千个视频站点,提供格式选择、播放列表批量下载、字幕下载、元数据写入、后处理转码等一整套能力,是命令行视频下载领域的经典工具。

## ✨ 主要特性

- 支持从 YouTube 及大量其他视频平台下载视频,持续更新提取器
- 跨平台:Linux / Windows / macOS 均可运行,只需 Python 2.6/2.7 或 3.2+
- 丰富的格式选择语法:按编码、分辨率、扩展名自由组合,如 `bestvideo+bestaudio`
- 支持播放列表与批量文件下载,可按序号、日期、标题、播放量等条件筛选视频
- 灵活的输出文件名模板,可用标题、上传者、日期等元数据命名文件
- 支持字幕/自动字幕下载,并可嵌入视频文件
- 支持代理、限速、断点续传、重试、分片下载等网络与下载控制
- 内置后处理:提取音频、转码、嵌入封面、写入元数据(依赖 ffmpeg/avconv)
- 支持账号登录、两步验证、Cookie、`.netrc` 等认证方式
- 可通过 JSON 输出与 Python API 嵌入到其他程序中

## 📁 文件说明

| 文件 | 说明 |
|:-----|:-----|
| README.md | 本文件(中文简介) |
| README.zh-CN.md | 详细中文文档(完整汉化) |

## 🚀 快速开始

**1. Linux / macOS 安装(curl 或 wget):**

```bash
sudo curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
sudo chmod a+rx /usr/local/bin/youtube-dl
```

**2. Windows 安装:** 从官网下载 `.exe` 文件,放到 PATH 中的任意目录(注意不要放进 `C:\Windows\System32`)。

**3. 使用 pip 安装 / 升级:**

```bash
sudo -H pip install --upgrade youtube-dl
```

**4. macOS 用 Homebrew 安装:**

```bash
brew install youtube-dl
```

**5. 最基本的下载:**

```bash
youtube-dl https://www.youtube.com/watch?v=VIDEO_ID
```

**6. 下载最高画质(视频+音频自动合并,需要 ffmpeg):**

```bash
youtube-dl -f 'bestvideo+bestaudio' URL
```

**7. 批量下载播放列表中第 1 到第 3 个视频:**

```bash
youtube-dl --playlist-items 1-3 PLAYLIST_URL
```

**8. 仅提取音频为 mp3:**

```bash
youtube-dl -x --audio-format mp3 URL
```

完整源代码与最新版本请访问原项目:https://github.com/ytdl-org/youtube-dl

## 📞 联系方式

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

本项目为 [ytdl-org/youtube-dl](https://github.com/ytdl-org/youtube-dl) 的中文翻译版本,所有代码版权归原项目作者所有,遵循其原始许可证。

**如果觉得有用,请给原项目点个 Star!** ⭐
