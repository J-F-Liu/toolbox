# Web技术

## HTTP
- [互联网协议入门](http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html)
- [99%的人都理解错了HTTP中GET与POST的区别](https://zhuanlan.zhihu.com/p/22536382)
- [聊聊HTTPS和SSL/TLS协议](http://mp.weixin.qq.com/s?__biz=MjM5ODE0MTM1MA==&mid=204884896&idx=1&sn=039ecac06ffc7e57e3d38f6d54480492#rd)
- [webim如何保证消息的可靠投递](http://www.habadog.com/2015/04/29/webim-msg-send-ack/)

## HTML
- [HTML5定稿：手机App三年内将彻底消失？](http://mp.weixin.qq.com/s?__biz=MzAxODIwNzc4NQ==&mid=204304446&idx=1&sn=aec247fc38409da7b2c5cea0ace35f6d)

## CSS
- [CSS实现垂直居中的常用方法](http://www.cnblogs.com/yugege/p/5246652.html)
- [浅谈Web自适应](http://www.cnblogs.com/constantince/p/5708930.html)
- [伪元素的妙用–单标签之美](http://web.jobbole.com/86261/)
- [酷酷的CSS3三角形运用](http://www.cnblogs.com/keepfool/p/5616326.html)
- [用 CSS3 绘制你需要的几何图形](http://www.cnblogs.com/wdlhao/p/5751211.html)
- [CSS魔法堂：Box-Shadow没那么简单啦:)](http://web.jobbole.com/86168/)
- [Understanding Flexbox: Everything you need to know](https://medium.freecodecamp.org/understanding-flexbox-everything-you-need-to-know-b4013d4dc9af)

## JavaScript
- [总结ES6常用的新特性](http://luckykun.com/work/2016-05-10/es6-feature.html)
- [30行代码实现Javascript中的MVC](http://www.cnblogs.com/front-end-ralph/p/5190442.html)
- [JavaScript 节流函数 Throttle 详解](https://keelii.github.io/2016/06/11/javascript-throttle/)
- [用 ParallelJS 并行处理 JavaScript](http://web.jobbole.com/86557/)
- [JavaScript 资源大全中文版](https://github.com/jobbole/awesome-javascript-cn)
- [让 Javascript 优雅如诗](http://www.ycwalker.com/2016/09/19/elegant-javascript/)
- [JavaScript 模块化入门Ⅰ：理解模块](https://zhuanlan.zhihu.com/p/22890374)
- [跨域问题，解决之道](http://blog.720ui.com/2016/web_cross_domain/)
- [深入理解ES7的async/await](http://coolcao.com/2016/12/12/deeper-understanding-of-async-await/)

## Design
- [解读设计风格的变迁史](http://www.jianshu.com/p/3bb4f671094f)
- [色彩想象力－迷之渐变色](https://blog.maxleap.cn/archives/1201)

## Tools
- [HTTPie — a command line HTTP client](https://httpie.org/)
- [mitmproxy — interactive examination and modification of HTTP traffic](https://mitmproxy.org/)
- [The best HTTP Static File Server](https://github.com/codeskyblue/gohttpserver)
- [DevDocs: API Documentation Browser](http://devdocs.io/)
- [Text encoder decoder, format converter](https://toolkit.site/)

## Open Source
- [Art of README](https://github.com/noffle/art-of-readme/blob/master/README-zh.md)

# Web协议

Ocsp stapling
Ocsp 全称在线证书状态检查协议 (rfc6960)，用来向 CA 站点查询证书状态，比如是否撤销。通常情况下，浏览器使用 OCSP 协议发起查询请求，CA 返回证书状态内容，然后浏览器接受证书是否可信的状态。

这个过程非常消耗时间，因为 CA 站点有可能在国外，网络不稳定，RTT 也比较大。那有没有办法不直接向 CA 站点请求 OCSP 内容呢？ocsp stapling 就能实现这个功能。

详细介绍参考 RFC6066 第 8 节。简述原理就是浏览器发起 client hello 时会携带一个 certificate status request 的扩展，服务端看到这个扩展后将 OCSP 内容直接返回给浏览器，完成证书状态检查。

由于浏览器不需要直接向 CA 站点查询证书状态，这个功能对访问速度的提升非常明显。

Nginx 目前已经支持这个 ocsp stapling file，只需要配置 ocsp stapling file 的指令就能开启这个功能：

ssl_stapling on;ssl_stapling_file ocsp.staple;
