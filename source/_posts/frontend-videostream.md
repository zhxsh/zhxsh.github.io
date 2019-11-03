---
title: 前端播放视频流
date: 2019-07-12 17:27
catagories: 前端
tags:
  - 视频流
  - rmtp
---

视频流有 rtmp、rtsp、 http 等格式，前端播放最方便的是 rtmp，rtmp 播放的方法如下

引入 video.js，5.x 版本(7.x 版本可能有问题)

```html
<script src="http://vjs.zencdn.net/5.5.3/video.js"></script>
<link href="http://vjs.zencdn.net/5.5.3/video-js.css" rel="stylesheet" />
```

使用 video 标签，添加样式**video-js**

```js
<video preload="auto" class="video-js videoWrap" id="myvideo">
  <source src="rtmp://58.200.131.2:1935/livetv/hunantv" type="rtmp/flv" />
</video>
```

默认播放

```js
videojs.options.autoplay = true;
videojs("myvideo");
```

修改标签大小需在 video 标签外面加样式
