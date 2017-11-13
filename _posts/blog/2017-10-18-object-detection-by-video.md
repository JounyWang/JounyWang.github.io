---
layout:     post
title:      使用Tensorflow实现视频内容实时检测
category: blog
description: 1984
---
为了验证视频识别的效果，我就随便在Youtube找了一段电影的预告片，链接为：https://www.youtube.com/watch?v=wujuUmzKmvw

整个预告片有两分多钟，我只截取了2:00后的十几秒视频，这就足够了。

点击下方可观看原版

<iframe width="560" height="315" src="https://www.youtube.com/embed/wujuUmzKmvw?rel=0&amp;start=121" frameborder="0" allowfullscreen></iframe>

我们将其下载到本地。


我们来看最终检测效果：

<video width="480" height="320" controls>
<source src="https://github.com/JounyWang/JounyWang.github.io/raw/master/_posts/blog/video/TheBourneLegacy_output_water.mp4">
</video>