
> RSS (RDF Site Summary or Really Simple Syndication)

中文称为 *简易内容聚合*，是一种 **消息来源（web feed）格式规范**，用以聚合 *多个* 网站更新的內容并且自动通知网站订阅者。

也就是说，你可以在多个支持 RSS 的网站上订阅你感兴趣的内容（知乎专栏、博客等），而在一个单独的 RSS 阅读器中获取并阅读它们。

## WHY RSS ?

~~文艺复兴~~

RSS 的对立面是算法推荐，比如百度、知乎、微博等平台，算法推荐根据你的历史数据判断你的喜好，从而自动为你推送内容，这些平台上充斥着广告，替你定义了你的用户画像，然后把你潜移默化中变成了大数据眼中的你。

RSS 是一种公开的协议，可自由更换平台与客户端。**获取信息的权力完全自治**，是一种更高效的阅读方式，相比算法推荐，拥有了可控性和安全感，隐私完全掌握在自己手里。[^2]

当然，RSS 的门槛也比算法推荐要高，毕竟需要自己挑选阅读内容。

## 简单几步在你的 Web 浏览器上开启 RSS 阅读之旅

1. 在浏览器上安装扩展（插件） *Tampermonkey（油猴）脚本管理器*（后续安装脚本推荐 [GreasyFork](https://greasyfork.org/zh-CN) 网站）
   - [适用于 Firefox 的下载地址](https://addons.mozilla.org/zh-CN/firefox/addon/tampermonkey/) 
   - [适用于 Google Chrome 的下载地址](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo/related)，国内访问需要科学上网
   - [适用于 Microsoft Edge 的下载地址](https://microsoftedge.microsoft.com/addons/detail/tampermonkey/iikmkjmpaadaobahmlepeloendndfphd)
2. 安装用于显示当前网站所有 RSS 的 [RSS+ 脚本](https://greasyfork.org/zh-CN/scripts/373252-rss-show-site-all-rss)，安装后你可以在支持 RSS 的网站右下角找到一个显示数字的小圆圈，点击会弹出 RSS 订阅源，复制后添加到下面的 RSS 阅读器中即可订阅此内容的 RSS
    💡 **NOTE**: 若链接失效，请直接到上面的 GreasyFork 网站搜索 RSS+  
3. 安装 RSS 阅读器，这里推荐浏览器插件 *Feedbro*

   - [适用于 Firefox 的下载地址](https://addons.mozilla.org/zh-CN/firefox/addon/feedbroreader/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search) 
   - [适用于 Google Chrome 的下载地址](https://chrome.google.com/webstore/detail/feedbro/mefgmmbdailogpfhfblcnnjfmnpnmdfa/related)，国内访问需要科学上网
   - [适用于 Microsoft Edge 的下载地址](https://microsoftedge.microsoft.com/addons/detail/feedbro/pdfbckdfhgaohcfdkcgpggcifmalimfd)

    安装后在浏览器右上角会显示此插件（或者点击扩展功能图标显示插件列表），点击对应插件可进行需要的操作，比如打开阅读器选择 Open Feed Reader

## 制作 RSS

> 网站不提供 RSS 怎么办？如何为我的个人博客提供 RSS？

使用工具自己制作 RSS 源，~~你甚至可以为抖音制作 RSS 源~~

- 新手推荐使用 [feed43](https://feed43.com/)，适用于 *静态页面* [^1]，可使用全文订阅，只需要源网址即可制作
- 需要抓动态页面、多页面内容，可以进阶到 [Huginn](https://github.com/huginn/huginn/blob/master/doc/manual/installation.md)
- 对微博、知乎、豆瓣、bilibili、Youtube 等主流网站进行 RSS 转化可以使用开源项目 [RSSHub](https://docs.rsshub.app/)[^3]

## 附录：主流浏览器扩展商店网址

若上述插件链接失效，可以直接在扩展商店中搜索对应插件

- [Firefox](https://addons.mozilla.org/zh-CN/firefox/)
- [Google Chrome](https://chrome.google.com/webstore/category/extensions)
- [Microsoft Edge](https://microsoftedge.microsoft.com/addons/Microsoft-Edge-Extensions-Home)

## 推荐阅读与参考文献

[^1]: [Static vs Dynamic Web Pages – What's the Difference?](https://www.freecodecamp.org/news/static-vs-dynamic-web-pages/)
[^2]: [RSS - 高效率的阅读方式](https://sspai.com/post/56198#!)
[^3]: [RSS 速成篇：RSSHub 捡现成的轮子](https://zhuanlan.zhihu.com/p/61562265)
