> 🌐 本文档由 [ytdl-org/youtube-dl](https://github.com/ytdl-org/youtube-dl) 翻译,英文原版见原项目。
>
> 📝 注:原文件超过 10000 字符,本文仅翻译核心章节;完整细节请参阅英文原版 [CONTRIBUTING.md](https://github.com/ytdl-org/youtube-dl/blob/master/CONTRIBUTING.md)。

**提交 issue 时请附带 `-v` 参数运行 youtube-dl 的完整输出**,即在你的**命令行**中**加上** `-v` 标志,复制**全部**输出,用 \`\`\` 包裹后贴到 issue 正文里。输出应类似:

```
$ youtube-dl -v <your command line>
[debug] System config: []
[debug] User config: []
[debug] Command-line args: [u'-v', u'https://www.youtube.com/watch?v=BaW_jenozKcj']
[debug] Encodings: locale cp1251, fs mbcs, out cp866, pref cp1251
[debug] youtube-dl version 2015.12.06
[debug] Git HEAD: 135392e
[debug] Python version 2.6.6 - Windows-2003Server-5.2.3790-SP2
[debug] exe versions: ffmpeg N-75573-g1d0487f, ffprobe N-75573-g1d0487f, rtmpdump 2.4
[debug] Proxy map: {}
...
```

**不要贴详细日志的截图,只接受纯文本。**

输出(包括开头几行)包含重要的调试信息。缺少完整输出的 issue 往往无法复现,因此很难被解决。

提交前请把你的 issue 再读一遍,避开以下常见错误(可以把这当成检查清单):

### 描述本身是否足够清楚?

请详细说明你要请求的功能或想修复的 Bug,确保能一眼看出:

- 问题是什么
- 可能如何修复
- 你设想的解决方案长什么样

对 Bug 报告而言,这意味着报告必须包含加 `-v` 参数后的*完整*输出。**站点支持请求必须包含示例 URL**,例如 `https://www.youtube.com/watch?v=BaW_jenozKc`;视频服务的主页(如 `https://www.youtube.com/`)*不是*示例 URL。

### 用的是最新版本吗?

报告任何 issue 前先运行 `youtube-dl -U` 确认已是最新。收到的报告中约 20% 的问题其实早已修复。功能请求同理。

### 问题是否已被报告过?

先在 [GitHub Issues](https://github.com/ytdl-org/youtube-dl/search?type=Issues) 中搜索。若已存在,可以补充"我在版本 2015.01.01 也遇到此问题,补充信息如下:……"之类的内容,新的回复常常能推动旧 issue 快速处理。

### 现有选项为什么不够用?

请求新功能前,先看一眼[支持的选项列表](https://github.com/ytdl-org/youtube-dl/blob/master/README.md#options)——很多功能请求要的东西其实已经存在!请说明现有类似选项*为什么*解决不了你的问题。

### Bug 报告的上下文足够吗?

每个不涉及"新增站点支持"的功能请求都应包含使用场景,说明缺失的功能在什么情况下有用。避免把大问题拆成"一步简单、一步不可能"的两步请求。

### 是否只涉及一个问题?

不要把一堆问题塞进同一个 ticket:修了其中一个的人无法关闭整个 issue。站点支持请求一次只应针对一个站点(同一域名、同一后端技术),Bug 报告也不要和功能请求混在一起。

### 会有人需要这个功能吗?

只提交你(或你能亲自联系到的朋友)确实需要的功能,不要因为"听起来不错"就提。

### 你的问题确实与 youtube-dl 有关吗?

有些报告其实来自其他应用或报告者自己的程序。如果你在用 youtube-dl 的图形界面(UI),请把 Bug 报告给该 UI 的维护者;如果你确信问题出在 youtube-dl 本身,欢迎报告。

# 开发者说明

大多数用户无需自行构建,可直接[下载构建版本](https://ytdl-org.github.io/youtube-dl/download.html)。

开发者运行 youtube-dl 也无需构建,直接执行:

    python -m youtube_dl

运行测试,用你喜欢的测试运行器或直接执行测试文件:

    python -m unittest discover
    python test/test_download.py
    nosetests

自行构建需要:python、make(仅支持 GNU make)、pandoc、zip、nosetests。

### 为新站点添加支持

首先**务必确认**该站点**不专门从事[版权侵权](README.md#can-you-add-support-for-this-anime-video-site-or-site-which-shows-current-movies-for-free)**。youtube-dl **不支持**此类站点,相关 PR **将被拒绝**。

确认站点合法分发内容后,按以下步骤(假设你的服务叫 `yourextractor`):

1. [Fork 本仓库](https://github.com/ytdl-org/youtube-dl/fork)
2. 检出源码:`git clone git@github.com:YOUR_GITHUB_USERNAME/youtube-dl.git`
3. 新建分支:`cd youtube-dl && git checkout -b yourextractor`
4. 以如下模板为起点,保存到 `youtube_dl/extractor/yourextractor.py`:

    ```python
    # coding: utf-8
    from __future__ import unicode_literals

    from .common import InfoExtractor


    class YourExtractorIE(InfoExtractor):
        _VALID_URL = r'https?://(?:www\.)?yourextractor\.com/watch/(?P<id>[0-9]+)'
        _TEST = {
            'url': 'https://yourextractor.com/watch/42',
            'md5': 'TODO: md5 sum of the first 10241 bytes of the video file (use --test)',
            'info_dict': {
                'id': '42',
                'ext': 'mp4',
                'title': 'Video title goes here',
                'thumbnail': r're:^https?://.*\.jpg$',
                # TODO more properties, either as:
                # * A value
                # * MD5 checksum; start the string with md5:
                # * A regular expression; start the string with re:
                # * Any Python type (for example int or float)
            }
        }

        def _real_extract(self, url):
            video_id = self._match_id(url)
            webpage = self._download_webpage(url, video_id)

            # TODO more code goes here, for example ...
            title = self._html_search_regex(r'<h1>(.+?)</h1>', webpage, 'title')

            return {
                'id': video_id,
                'title': title,
                'description': self._og_search_description(webpage),
                'uploader': self._search_regex(r'<div[^>]+id="uploader"[^>]*>([^<]+)<', webpage, 'uploader', fatal=False),
                # TODO more properties (see youtube_dl/extractor/common.py)
            }
    ```

5. 在 [`youtube_dl/extractor/extractors.py`](https://github.com/ytdl-org/youtube-dl/blob/master/youtube_dl/extractor/extractors.py) 中添加 import(类名以 `IE` 结尾即可被发现)
6. 运行 `python test/test_download.py TestDownload.test_YourExtractor`,起初*会失败*,反复修改直到通过。多个测试时把 `_TEST` 改成 `_TESTS` 并用字典列表;含 `only_matching` 的测试不计入命名
7. 参考 [`youtube_dl/extractor/common.py`](https://github.com/ytdl-org/youtube-dl/blob/master/youtube_dl/extractor/common.py) 的辅助方法及[信息字典的详细说明](https://github.com/ytdl-org/youtube-dl/blob/7f41a598b3fba1bcab2817de64a08941200aa3c8/youtube_dl/extractor/common.py#L94-L303),尽量多写测试和代码
8. 确保代码符合 [youtube-dl 编码规范](#youtube-dl-编码规范)并通过 [flake8](https://flake8.pycqa.org/en/latest/index.html#quickstart) 检查:`flake8 youtube_dl/extractor/yourextractor.py`
9. 确保代码在 youtube-dl 声称支持的所有 [Python](https://www.python.org/) 版本(2.6、2.7、3.2+)下都能工作
10. 测试通过后,add + commit + push:

        $ git add youtube_dl/extractor/extractors.py
        $ git add youtube_dl/extractor/yourextractor.py
        $ git commit -m '[yourextractor] Add new extractor'
        $ git push origin yourextractor

11. 最后[创建 pull request](https://help.github.com/articles/creating-a-pull-request),等待审核合并

无论如何,非常感谢你的贡献!

## youtube-dl 编码规范

本节给出编写地道、健壮、面向未来的提取器代码的指南。

提取器天然脆弱:它依赖你无法控制的第三方媒体站点的页面布局,而布局会变。你的任务不仅是正确提取媒体链接和元数据,还要最小化对源布局的依赖,甚至预判未来的变化。这样提取器才不会因小的布局改动而失效,让旧版本 youtube-dl 继续工作——很多发行版的包更新并不及时,甚至可能永远收不到更新。

### 必填与可选元字段

youtube-dl 依赖提取器提供的[信息字典](https://github.com/ytdl-org/youtube-dl/blob/7f41a598b3fba1bcab2817de64a08941200aa3c8/youtube_dl/extractor/common.py#L94-L303)(*info dict*)。成功提取只要求以下元字段:

 - `id`(媒体标识)
 - `title`(媒体标题)
 - `url`(媒体下载地址)或 `formats`

严格说只有最后一项在技术上必填,但按惯例 `id` 和 `title` 也视为必填;任一必填字段提取失败,即视为提取器彻底损坏。

其余[任何字段](https://github.com/ytdl-org/youtube-dl/blob/7f41a598b3fba1bcab2817de64a08941200aa3c8/youtube_dl/extractor/common.py#L188-L303)都是**可选**的:提取过程对这些字段的来源缺失必须**容错**(即使当前总能取到),并保持**面向未来**,以免连累必填字段的提取。

#### 示例

可选字段要用容错写法。`description` 是可选字段,应写成:

```python
description = meta.get('summary')  # correct
```

而不是:

```python
description = meta['summary']  # incorrect
```

后者在 `summary` 消失时会抛 `KeyError` 中断提取;前者只是让 `description` 为 `None` 后继续(`None` 等价于数据缺失)。

同理,用 `_search_regex`、`_html_search_regex` 等方法提取可选数据时传 `fatal=False`,失败时仅告警并继续;也可传 `default=<回退值>`,失败时静默继续。

### 提供回退方案

提取元数据尽量从多个来源获取,例如 `title` 在多处出现时至少尝试其中几处,某处失效仍可工作:

```python
title = meta.get('title') or self._og_search_title(webpage)
```

### 正则表达式

#### 不用的组不要捕获

捕获组必须表示"该结果在代码中被使用",否则用非捕获组 `(?:...)`:

```python
r'(?:id|ID)=(?P<id>\d+)'   # correct
r'(id|ID)=(?P<id>\d+)'     # incorrect
```

#### 正则要宽松灵活

跳过容易变化的无关部分,引号兼容单双引号等。匹配 `title` 时:

```python
title = self._search_regex(
    r'<span[^>]+class=(["\'])title\1[^>]*>(?P<title>[^<]+)',
    webpage, 'title', group='title')
```

而不要把 `style` 属性的值原样写死进正则。

### 长行策略

代码行软性限制 80 字符,以不损害可读性为前提。**永远**不要为了凑行宽把 URL 等常被复制的长字符串拆成多行。

### 内联值

提取变量以消除重复、提升复杂表达式可读性是可以的,但不要把只用一次的变量挪到文件另一头,破坏代码的线性阅读流。

### 合并回退

多个回退值应合并为单个表达式:

```python
description = self._html_search_meta(
    ['og:description', 'description', 'twitter:description'],
    webpage, 'description', default=None)
```

支持模式列表的方法:`_search_regex`、`_html_search_regex`、`_og_search_property`、`_html_search_meta`。

### 尾随括号

尾随括号始终放在最后一个参数之后,不要单独成行。

### 使用便捷转换与解析函数

所有提取出的数值都用 [`youtube_dl/utils.py`](https://github.com/ytdl-org/youtube-dl/blob/master/youtube_dl/utils.py) 的安全函数包裹:`int_or_none`、`float_or_none`;字符串转数字也用它们。URL 用 `url_or_none`,JSON 元数据用 `try_get`,统一 `upload_date`/`YYYYMMDD` 用 `unified_strdate`,统一 `timestamp` 用 `unified_timestamp`,文件大小用 `parse_filesize`,计数字段用 `parse_count`,分辨率用 `parse_resolution`,时长用 `parse_duration`,年龄限制用 `parse_age_limit`。更多便捷函数请自行探索 [`youtube_dl/utils.py`](https://github.com/ytdl-org/youtube-dl/blob/master/youtube_dl/utils.py)。

#### 更多示例

##### 从已解析 JSON 中安全提取可选 description

```python
description = try_get(response, lambda x: x['result']['video'][0]['summary'], compat_str)
```

##### 安全提取更多可选元数据

```python
video = try_get(response, lambda x: x['result']['video'][0], dict) or {}
description = video.get('summary')
duration = float_or_none(video.get('durationMs'), scale=1000)
view_count = int_or_none(video.get('views'))
```
