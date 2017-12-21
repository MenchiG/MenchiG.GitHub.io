---
layout: post
key: 20171221
modify_date: 2017-12-21
tags: [Huginn , RSS, 知乎日报]
title: 利用 Huginn 和知乎日报 API 制作全文 RSS
---

Huginn 是北欧神话里，主神奥丁的一只乌鸦，每天乌鸦都会为奥丁获取全世界的新闻。 这也是基于 ruby 的 [Huginn](https://github.com/huginn/huginn) 所希望替用户做到的，在这个碎片化的时代替爬取和筛选任何互联网上信息。

<!--more-->

自从 Google Reader 关闭之后，RSS 已逐渐式微，但这不妨碍它成为一种高效获取信息的手段，RSS 没有推荐和评论等任何干扰性元素（吾之蜜糖彼之砒霜），信息来源掌握在用户自己手里。配合 [Tiny Tiny RSS](https://tt-rss.org/) ，所有文章通过 Reeder 阅读而不必在多个 XX 头条，XX News 和 XX 号之间切换以及忍受他们的启动广告。 RSS 已经成为了我获取每日信息的主要途径。

![信息聚合flow](/assets/images/2017-12-21/flow.jpg){: .center-image} 

- RSS 信息源
  - 新闻、媒体、博客
  - 微信公众号
  - 即刻
  - Flipboard
  - 希望稍后阅读的文章

大多数主流媒体仍然提供 RSS 摘要作为引流途径，好在可以通过 [Mercury](https://mercury.postlight.com/web-parser/) 来获取文章全文（虽然开发者从不回复邮件），以省去每次跳转到网站的麻烦。

### WebsiteAgent 1 - 获取日报文章链接
通过 [izzyleung 的逆向分析](https://github.com/izzyleung/ZhihuDailyPurify/wiki/%E7%9F%A5%E4%B9%8E%E6%97%A5%E6%8A%A5-API-%E5%88%86%E6%9E%90)所得出的知乎日报 API [https://news-at.zhihu.com/api/4/stories/latest](https://news-at.zhihu.com/api/4/stories/latest) ，用 Huginn 创建一个 `WebsiteAgent` 来接收 API 返回的大爱 JSON 格式。

{% highlight JSON %}
{
  "date": "20171221",
  "stories": [{
    "images": ["https:\/\/pic1.zhimg.com\/v2-a3a6dc8cceadecc2a7857a4fde489070.jpg"],
    "type": 0,
    "id": 9661975,
    "ga_prefix": "122119",
    "title": "每天都一样，我没有可以发朋友圈的好照片"
  }, {
    "images": ["https:\/\/pic3.zhimg.com\/v2-55a299f36ca56acc4f45977404b73596.jpg"],
    "type": 0,
    "id": 9661999,
    "ga_prefix": "122119",
    "title": "简历不会做，求职信不会写？先学习三个经济模型"
  }]
}
{% endhighlight %}

##### agent 参数 [^1]

![Huginn WebsiteAgent 1](/assets/images/2017-12-21/huginn_agent_1.png){: .center-image} 

- 在 `url` 中填入 API 地址
- 在 `type` 中填入返回格式 `JSON`
- 在 `mode` 中填入 `on_change` ，即只存储与上次事件不同的抓取结果
- 在 `extract` 中填入我们要抓取的 JSON 键名 （ Huginn 支持 regex 正则式和 [Liquid](https://shopify.github.io/liquid/) ），这里抓取的是 `stories` 键下的每一个对象的 `id` 和 `title` ，**并将事件存储到 `id` 和 `title` 这两个变量中备用**。


##### 输出事件

id|title
9661975|每天都一样，我没有可以发朋友圈的好照片
9661999|简历不会做，求职信不会写？先学习三个经济模型


### WebsiteAgent 2 - 获取日报文章全文
基于上一步的输出事件，ID 代表文章的唯一识别符，创建第二个 `WebsiteAgent` 进一步通过 API [https://news-at.zhihu.com/api/4/news/9661975](https://news-at.zhihu.com/api/4/news/9661975) 可以获取 ID 为 9661975的全文 HTML。

{% highlight JSON %}
{
 "body": "<div class=\"main-wrap content-wrap\">\n<div class=\"headline\">\n\n<div class=\"img-place-holder\"><\/div>\n\n\n\n<\/div>\n\n<div class=\"content-inner\">\n\n\n\n<div class=\"question\">\n<h2 class=\"question-title\">怎么在非常平凡的环境里拍出好照片？| 再说陌生感<\/h2>\n<div class=\"answer\">\n\n<div class=\"meta\">\n<img class=\"avatar\" src=\"http:\/\/pic3.zhimg.com\/v2-be72a3471a5b9a246313cca944dd7d62_is.jpg\">\n<span class=\"author\">原画册韩松，<\/span><span class=\"bio\">IPPA,MPA,MIRA手机摄影获奖，公众号：原画册<\/span>\n<\/div>\n\n<div class=\"content\">\n<p>文 \/ KKM<\/p>\r\n<p>摄 \/ 韩松<\/p>\r\n<p>看到这个问题我第一个想到的依然是三个字——<strong>“陌生感”<\/strong>。<\/p>\r\n<p>即一个在我们眼中很路人的地方，通过摄影师构图和后期，呈现出<strong>“这是什么？”<\/strong>的第一观感，和琢磨琢磨之后<strong>会心一笑<\/strong>的结果。这种迷人的，被延长的审美时间，构成了读片乐趣。这种乐趣我理解为是一种“好”照片。<\/p>\r\n<p>-<\/p>\r\n<p>陌生感大法是就<strong>形式<\/strong>上而言的，核心在于让照片看起来和我们平时肉眼所见的有所不同，就手法而言还可以细分为“局部大法”“对焦大法”“角度大法”“曝光大法”“结构大法”“讲故事大法”“组照大法”等。<\/p>\r\n<p>提供几张尬图来论述。手机拍摄。<\/p>\r\n<p>-<\/p>\r\n<p>首先看这组最近我最近发布在微博的照片，其中用到了很多对局部和场景片段的捕捉：<\/p>\r\n<figure><img class=\"content-image\" src=\"https:\/\/pic1.zhimg.com\/v2-4ed10ecd2b4b2e5bacce5d58b24da7e5_b.jpg\" alt=\"\"><\/figure><p><strong>1\/“局部大法”<\/strong><\/p>\r\n<p>其实是一种讨巧的做法。因为它以最简单的方式规避了人直接看到的东西，将人从平凡而无聊的视觉体验中抽离出来。<\/p>\r\n<figure><img class=\"content-image\" src=\"https:\/\/pic4.zhimg.com\/v2-f16f1d659c9c6bc2a23e746672f48d33_b.jpg\" alt=\"\"><figcaption>韩松 \/ 摄 iPhone7p 潮湿的地面<\/figcaption><\/figure><p>而这张照片的“真实场景”如下，可以看到没有陌生化的处理，显得平庸无趣：<\/p>\r\n<figure><img class=\"content-image\" src=\"https:\/\/pic4.zhimg.com\/v2-6f1ba5abc19e16fbb53a64374a52f369_b.jpg\" alt=\"\"><figcaption>“真实场景”对比<\/figcaption><\/figure><figure><img class=\"content-image\" src=\"https:\/\/pic2.zhimg.com\/v2-02a9303d61de079c1fc94d626a1c7376_b.jpg\" alt=\"\"><figcaption>韩松 \/ 摄 iPhone7p 城市局部<\/figcaption><\/figure><figure><img class=\"content-image\" src=\"https:\/\/pic4.zhimg.com\/v2-c05945f75dfa503044d64f03ad634c33_b.jpg\" alt=\"\"><figcaption>韩松 \/ 摄 iPhone7p 油画般的水面<\/figcaption><\/figure><figure><img class=\"content-image\" src=\"https:\/\/pic4.zhimg.com\/v2-905d3ad258bcc6e3e5b1f72f73733f84_b.jpg\" alt=\"\"><figcaption>“真实场景”对比<\/figcaption><\/figure><p><strong>2\/“角度大法”<\/strong><\/p>\r\n<p>我反复用了这个栗子：京都火车站站前广场，普通的角度只能带来平凡的照片：<\/p>\r\n<figure><img class=\"content-image\" src=\"https:\/\/pic3.zhimg.com\/v2-6bbd41cb62ac7636f3c53982642c235a_b.jpg\" alt=\"\"><\/figure><p>在上图所示的角度拍摄，可以获得这种效果：<\/p>\r\n<figure><img class=\"content-image\" src=\"https:\/\/pic4.zhimg.com\/v2-4d03f0063dd705e601f201cf38c89e2a_b.jpg\" alt=\"\"><figcaption>谭凌飞／摄 iPhone6s。京都火车站。MPA 手机摄影大赛建筑类荣誉奖<\/figcaption><\/figure><p>当然，局部、角度二法一脉相承，靠的都是观察力和归纳能力。要说一些操作上的技巧，可以从对焦和曝光说起：<\/p>\r\n<p><strong>3\/“对焦大法”<\/strong><\/p>\r\n<p>自然是运用对焦的一点雕虫小技，来制造肉眼难以达到的视觉体验。这种感觉像是<strong>用一句诗表达了一句大白话<\/strong>，意思还是那个意思，表达却完全不同。下面两张照片的拍摄很简单，用手机对着近处某个东西对焦，并按住屏幕锁定焦点，然后对着远处，便会有失焦的效果。<\/p>\r\n<figure><img class=\"content-image\" src=\"https:\/\/pic1.zhimg.com\/v2-c7ad99ef483c2d938b660211b121d69d_b.jpg\" alt=\"\"><figcaption>韩松 \/ 摄 iPhone7p 城市局部<\/figcaption><\/figure><figure><img class=\"content-image\" src=\"https:\/\/pic4.zhimg.com\/v2-b7b96b98613b7d1ade925b4e12a4ed63_b.jpg\" alt=\"\"><figcaption>韩松 \/ 摄 iPhone7p 河流<\/figcaption><\/figure><p>而这张我也准备放出没有经过“陌生化”处理的场景，一条普通的河流，实在乏善可陈：<\/p>\r\n<figure><img class=\"content-image\" src=\"https:\/\/pic4.zhimg.com\/v2-ddc3b84577ed98efcb098ac8108148ce_b.jpg\" alt=\"\"><figcaption>“真实场景”对比<\/figcaption><\/figure><p>最近对虚焦的拍摄比较着迷，再举几个例子：<\/p>\r\n<figure><img class=\"content-image\" src=\"https:\/\/pic4.zhimg.com\/v2-04463008b0c6fa8aafdfe994daade978_b.jpg\" alt=\"\"><figcaption>韩松 \/ 摄 iPhone7p 北京，回家的路上<\/figcaption><\/figure><p>这张照片还可以通过这个视频感受：<\/p>\r\n<p><strong>4\/“曝光大法”<\/strong><\/p>\r\n<p>其本质是制造了人眼感受不到的“错误曝光效果”，下面这组《灯》拍摄于莫斯科地铁，通过<strong>降低曝光<\/strong>的调节营造了一组简单、节制、具有抽象美的图像。<\/p>\r\n<figure><img class=\"content-image\" src=\"https:\/\/pic4.zhimg.com\/v2-73dde9149680f79b5a9abe993dff90c0_b.jpg\" alt=\"\"><figcaption> 韩松 \/ 摄 iPhone 7p，《灯》。2017 年平遥国际摄影大展中，这组照片在“自然生长”联展中展出<\/figcaption><\/figure><figure><img class=\"content-image\" src=\"https:\/\/pic1.zhimg.com\/v2-1e8e1bb8872a40dd9380951cbc0c851b_b.jpg\" alt=\"\"><figcaption> 韩松 \/ 摄 iPhone 7p，《灯》。2017 年平遥国际摄影大展中，这组照片在“自然生长”联展中展出<\/figcaption><\/figure><figure><img class=\"content-image\" src=\"https:\/\/pic1.zhimg.com\/v2-15e310fd71a23f3e8e7a07568e8a8af1_b.jpg\" alt=\"\"><figcaption> 韩松 \/ 摄 iPhone 7p，《灯》。2017 年平遥国际摄影大展中，这组照片在“自然生长”联展中展出<\/figcaption><\/figure><p>很多朋友觉得莫斯科地铁并不属于平凡的环境，实际上，华丽的莫斯科地铁站最不起眼的就是这些通道里的日光灯了。这个案例还让我感到，很多惯常意义上的美好和华丽，其实蒙蔽了真正的平凡之美。同样的手法，自己在某宜家也进行了演示。<\/p>\r\n<figure><img class=\"content-image\" src=\"https:\/\/pic4.zhimg.com\/v2-bb144c732ad3f39838018a3acea38e1e_b.jpg\" alt=\"\"><figcaption> 韩松 \/ 摄 iPhone 7p，天鹅<\/figcaption><\/figure><p><strong>5\/“组照大法”<\/strong><\/p>\r\n<p>找到日常\\旅行中一些线索，组合起来，用有力的主题来改变平凡场景的意义。这时候，我们不再只关注照片“好看与否”，而是通过照片传达“我看到了什么”。<\/p>\r\n<figure><img class=\"content-image\" src=\"https:\/\/pic5.zhimg.com\/v2-1c7c1367c2e5e176f5d51ca2612dd3b1_b.jpg\" alt=\"\"><figcaption> 韩松 \/ 摄 iPhone 7p，云层<\/figcaption><\/figure><figure><img class=\"content-image\" src=\"https:\/\/pic3.zhimg.com\/v2-045ddd9efa37f1b02c7b53a5b78b96a9_b.jpg\" alt=\"\"><figcaption> 韩松 \/ 摄 iPhone 7p，没有蓝天白云的海边<\/figcaption><\/figure><figure><img class=\"content-image\" src=\"https:\/\/pic2.zhimg.com\/v2-839678667afdcc5cd7816b1a7f657a83_b.jpg\" alt=\"\"><figcaption> 韩松 \/ 摄 iPhone 7p，half and half<\/figcaption><\/figure><p>当然，组照的存在并不是为了掩饰平凡，单看其中很多照片，可以感受到另一种手法：<\/p>\r\n<p><strong>6\/“结构大法”<\/strong><\/p>\r\n<p>日常真实场景是柔软而混沌的，通过构图强化结构感和几何感，能让一些“路人”的场景变得不一般。看这几个例子：<\/p>\r\n<figure><img class=\"content-image\" src=\"https:\/\/pic4.zhimg.com\/v2-2fac137677fc008131444a3421394c1b_b.jpg\" alt=\"\"><figcaption> 韩松 \/ 摄 iPhone 7p，有意制造了对分的明暗结构<\/figcaption><\/figure><figure><img class=\"content-image\" src=\"https:\/\/pic1.zhimg.com\/v2-213375fcc8d9eb58a2a6b35a53ea7bea_b.jpg\" alt=\"\"><figcaption> 韩松 \/ 摄 iPhone 7p，有意捕捉了倒三角的结构意向<\/figcaption><\/figure><figure><img class=\"content-image\" src=\"https:\/\/pic1.zhimg.com\/v2-d4717332497d394368e2d98afebb11c8_b.jpg\" alt=\"\"><figcaption> 韩松 \/ 摄 iPhone 7p，空间<\/figcaption><\/figure><figure><img class=\"content-image\" src=\"https:\/\/pic2.zhimg.com\/v2-9e7d5bb663816887324eba16b46c8449_b.jpg\" alt=\"\"><figcaption> 韩松 \/ 摄 iPhone 7p，建筑和玻璃上的灯<\/figcaption><\/figure><p>-<\/p>\r\n<p>不得不说，<strong>陌生感<\/strong>的思想 \/ 手法对自己拍照有着很深的影响，也是自己反复提到的。<\/p>\r\n<p><a class=\"internal\" href=\"https:\/\/zhuanlan.zhihu.com\/p\/24671113\">手摄观念提升：视觉上的陌生感<\/a><\/p>\r\n<p><a class=\"internal\" href=\"https:\/\/zhuanlan.zhihu.com\/p\/31885152\">手摄意象集 | 我从初冬的日本提取颜色，得到了这组色卡。<\/a><\/p>\r\n<p>以上是我浅显的理解，以及继续追求的基石。<\/p>\r\n<p><em>声明：本文所有文字及照片为原画册工作室原创，版权归韩松、谭凌飞、张薇所有，仅供知乎专栏、原画册微信公众平台发布，若需转载请后台联系。违者必究。<\/em><\/p>\r\n<hr><p>欢迎关微信注公众号：原画册<\/p>\n\n<div class=\"view-more\"><a href=\"http:\/\/zhuanlan.zhihu.com\/p\/32200493\">查看知乎讨论<\/a><\/div>\n\n<\/div>\n<\/div>\n<\/div>\n\n\n<\/div>\n<\/div><script type=“text\/javascript”>window.daily=true<\/script>",
 "image_source": "原画册韩松 \/ 知乎",
 "title": "每天都一样，我没有可以发朋友圈的好照片",
 "image": "https:\/\/pic4.zhimg.com\/v2-ef4853ad22c5a5aa1d315ee2bb647c43.jpg",
 "share_url": "http:\/\/daily.zhihu.com\/story\/9661975",
 "js": [],
 "ga_prefix": "122119",
 "images": ["https:\/\/pic1.zhimg.com\/v2-a3a6dc8cceadecc2a7857a4fde489070.jpg"],
 "type": 0,
 "id": 9661975,
 "css": ["http:\/\/news-at.zhihu.com\/css\/news_qa.auto.css?v=4b3e3"]
}
{% endhighlight %}

##### agent 参数 [^1]

![Huginn WebsiteAgent 2](/assets/images/2017-12-21/huginn_agent_2.png){: .center-image} 

- 在 `url` 中填入 `{% raw %}https://news-at.zhihu.com/api/3/news/{{id}}{% endraw %}` ，这里 `{% raw %}{{id}}{% endraw %}` 即为上一步我们存储的变量 `id` ，也就是`9661975`。
- 在 `type` 中填入返回格式 `JSON`
- 在 `mode` 中填入 `merge` ，**即把上一步存储的变量 `id` 和 `title` 合并到这一次的输出事件中**
- 在 `extract` 中填入我们要抓取的 JSON 键名 ，这次抓取的是 `body` 和 `path` 键，**并将内容存储到 `body` 和 `link` 这两个变量中备用**。

##### 输出事件

id|title|body|link
9661975|每天都一样，我没有可以发朋友圈的好照片|{% raw %}<div class=\"main-wrap content-wrap\">\n<div class=\"headline\">\n\n<div class=\"img-place-holder\"><\/div>\n\n\n\n<\/div>\n\n<div class=\"content-inner\">\n\n\n\n<div class=\"question\">\n<h2 class=\"question-title\">怎么在非常平凡的环境里拍出好照片？......{% endraw %}|http://daily.zhihu.com/story/9661975
9661999|简历不会做，求职信不会写？先学习三个经济模型|同上， 一串 HTML 代码|http://daily.zhihu.com/story/9661999


### DataOutputAgent - 输出 RSS
前两步已经抓取了所有需要的内容，这一步就可以通过 `DataOutputAgent` 输出 RSS 源。

![Huginn DataOutputAgent](/assets/images/2017-12-21/huginn_agent_3.png){: .center-image}

##### agent 参数 [^1]
- 在 `template` 中填入任意想要的 `title` 和 `description` 作为 RSS 源的标题和描述。**在 `item` 中， `title` 和 `description` 则是作为单独一条内容的标题和正文， `link` 则是该内容的链接**。在前两步已经分别将对应的内容存储在了变量中。

##### 输出事件
Huginn 会输出两个 RSS 源地址，分别为 XML 和 JSON 格式。

[https://bot.wangqiru.com/users/1/web_requests/92/知乎日报.xml](https://bot.wangqiru.com/users/1/web_requests/92/%E7%9F%A5%E4%B9%8E%E6%97%A5%E6%8A%A5.xml)

[https://bot.wangqiru.com/users/1/web_requests/92/知乎日报.json](https://bot.wangqiru.com/users/1/web_requests/92/%E7%9F%A5%E4%B9%8E%E6%97%A5%E6%8A%A5.json)

### 所有 agent 的串联及更新频率

在 agent 选项中，有一项为 `Sources` ，将第二个 agent 的 `sources` 指定为第一个，第三个 agent 的 `sources` 指定为第二个，如下图所示：

![Link agents](/assets/images/2017-12-21/huginn_agent_link.png){: .center-image}

`Schedule` 代表这个 agent 自动运行的频率，将第一个 `WebsiteAgent` 设置为每天早上7点自动运行（或更高频率如每5小时）。 第二个 `WebsiteAgent` 设置为 `never` ，但勾选 `Propagate immediately` ，代表**每当新事件输入即自动运行**。


### 大功告成

Huginn 会输出两个 RSS 源地址，分别为 XML 和 JSON 格式。
- [https://bot.wangqiru.com/users/1/web_requests/92/知乎日报.xml](https://bot.wangqiru.com/users/1/web_requests/92/%E7%9F%A5%E4%B9%8E%E6%97%A5%E6%8A%A5.xml)

- [https://bot.wangqiru.com/users/1/web_requests/92/知乎日报.json](https://bot.wangqiru.com/users/1/web_requests/92/%E7%9F%A5%E4%B9%8E%E6%97%A5%E6%8A%A5.json)

在 Reeder iOS 中的效果和知乎日报的对比：

![Feed rendered in Reeder](/assets/images/2017-12-21/reeder_1.jpg)|![Feed rendered in Reeder](/assets/images/2017-12-21/reeder_2.jpg)
![Zhihu Daily](/assets/images/2017-12-21/zhihu_daily_1.png)|![Zhihu Daily](/assets/images/2017-12-21/zhihu_daily_2.png)

可以看出 RSS 形式的文章**省略了封面和尾部**的 "客官，我又来送二维码了"（顺便吐槽下知乎的 logo 图片什么的和 js 资源居然还是用 http 传输的，浏览器都不让加载），仍然保留了查看原文的超链接（在 Reeder 中点击文章标题也可以跳转到原文），**正文显示效果则是一样的**。

附上三个 agent 的配置文件
- [https://bot.wangqiru.com/scenarios/4/export.json](https://bot.wangqiru.com/scenarios/4/export.json)

##### 注
[^1]: 其他非重要的对应选项请参照 [Huginn 官方 wiki](https://github.com/huginn/huginn/wiki)  。