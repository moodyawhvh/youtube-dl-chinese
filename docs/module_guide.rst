> 🌐 本文档由 [ytdl-org/youtube-dl](https://github.com/ytdl-org/youtube-dl) 翻译,英文原版见原项目。

使用 ``youtube_dl`` 模块
========================

使用 ``youtube_dl`` 模块时,先创建一个 :class:`YoutubeDL` 实例,并添加所有可用的提取器:

.. code-block:: python

    >>> from youtube_dl import YoutubeDL
    >>> ydl = YoutubeDL()
    >>> ydl.add_default_info_extractors()

提取视频信息
------------

使用 :meth:`YoutubeDL.extract_info` 方法获取视频信息,它会返回一个字典:

.. code-block:: python

    >>> info = ydl.extract_info('http://www.youtube.com/watch?v=BaW_jenozKc', download=False)
    [youtube] Setting language
    [youtube] BaW_jenozKc: Downloading webpage
    [youtube] BaW_jenozKc: Downloading video info webpage
    [youtube] BaW_jenozKc: Extracting video information
    >>> info['title']
    'youtube-dl test video "\'/\\ä↭𝕐'
    >>> info['height'], info['width']
    (720, 1280)

如果想下载或播放视频,可以获取它的 URL:

.. code-block:: python

    >>> info['url']
    'https://...'

提取播放列表信息
----------------

播放列表信息的提取方式类似,但返回的字典略有不同:

.. code-block:: python

    >>> playlist = ydl.extract_info('http://www.ted.com/playlists/13/open_source_open_world', download=False)
    [TED] open_source_open_world: Downloading playlist webpage
    ...
    >>> playlist['title']
    'Open-source, open world'



可以通过 ``entries`` 字段访问播放列表中的视频:

.. code-block:: python

    >>> for video in playlist['entries']:
    ...     print('Video #%d: %s' % (video['playlist_index'], video['title']))

    Video #1: How Arduino is open-sourcing imagination
    Video #2: The year open data went worldwide
    Video #3: Massive-scale online collaboration
    Video #4: The art of asking
    Video #5: How cognitive surplus will change the world
    Video #6: The birth of Wikipedia
    Video #7: Coding a better government
    Video #8: The era of open innovation
    Video #9: The currency of the new economy is trust
