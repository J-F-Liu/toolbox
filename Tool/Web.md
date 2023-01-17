# Web 技术

## HTTP

- [互联网协议入门](http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html)
- [99%的人都理解错了 HTTP 中 GET 与 POST 的区别](https://zhuanlan.zhihu.com/p/22536382)
- [聊聊 HTTPS 和 SSL/TLS 协议](http://mp.weixin.qq.com/s?__biz=MjM5ODE0MTM1MA==&mid=204884896&idx=1&sn=039ecac06ffc7e57e3d38f6d54480492#rd)
- [webim 如何保证消息的可靠投递](http://www.habadog.com/2015/04/29/webim-msg-send-ack/)

## HTML

- [HTML5 定稿：手机 App 三年内将彻底消失？](http://mp.weixin.qq.com/s?__biz=MzAxODIwNzc4NQ==&mid=204304446&idx=1&sn=aec247fc38409da7b2c5cea0ace35f6d)

## CSS

- [CSS: From Zero to Hero](https://dev.to/aspittel/css-from-zero-to-hero-3o16)
- [CSS 实现垂直居中的常用方法](http://www.cnblogs.com/yugege/p/5246652.html)
- [浅谈 Web 自适应](http://www.cnblogs.com/constantince/p/5708930.html)
- [伪元素的妙用–单标签之美](http://web.jobbole.com/86261/)
- [酷酷的 CSS3 三角形运用](http://www.cnblogs.com/keepfool/p/5616326.html)
- [用 CSS3 绘制你需要的几何图形](http://www.cnblogs.com/wdlhao/p/5751211.html)
- [CSS 魔法堂：Box-Shadow 没那么简单啦:)](http://web.jobbole.com/86168/)
- [Understanding Flexbox: Everything you need to know](https://medium.freecodecamp.org/understanding-flexbox-everything-you-need-to-know-b4013d4dc9af)
- [写给自己看的 display: flex 布局教程](https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/)
- [Understanding Layout Algorithms](https://www.joshwcomeau.com/css/understanding-layout-algorithms/)
- [An Interactive Guide to Flexbox](https://www.joshwcomeau.com/css/interactive-guide-to-flexbox/)
- [Tree views in css](https://iamkate.com/code/tree-views/)
- [Building a Magical 3D Button](https://www.joshwcomeau.com/animation/3d-button/)

```css
html {
  max-width: 70ch;
  padding: 3em 1em;
  margin: auto;
  line-height: 1.75;
  font-size: 1.25em;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  margin: 3em 0 1em;
}
p,
ul,
ol {
  margin-bottom: 2em;
  color: #1d1d1d;
  font-family: sans-serif;
}
```

## JavaScript

- [总结 ES6 常用的新特性](http://luckykun.com/work/2016-05-10/es6-feature.html)
- [30 行代码实现 Javascript 中的 MVC](http://www.cnblogs.com/front-end-ralph/p/5190442.html)
- [JavaScript 节流函数 Throttle 详解](https://keelii.github.io/2016/06/11/javascript-throttle/)
- [用 ParallelJS 并行处理 JavaScript](http://web.jobbole.com/86557/)
- [JavaScript 资源大全中文版](https://github.com/jobbole/awesome-javascript-cn)
- [让 Javascript 优雅如诗](http://www.ycwalker.com/2016/09/19/elegant-javascript/)
- [JavaScript 模块化入门 Ⅰ：理解模块](https://zhuanlan.zhihu.com/p/22890374)
- [跨域问题，解决之道](http://blog.720ui.com/2016/web_cross_domain/)
- [Make small focused modules](https://github.com/sindresorhus/ama/issues/10#issuecomment-117766328)
- [HACKING SEMICOLONS](https://slides.com/evanyou/semicolons)
- [The Evolution of Async JavaScript: From Callbacks, to Promises, to Async/Await](https://tylermcginnis.com/async-javascript-from-callbacks-to-promises-to-async-await/)
- [深入理解 ES7 的 async/await](http://coolcao.com/2016/12/12/deeper-understanding-of-async-await/)

## Svg

- [SVG 图像入门教程](http://www.ruanyifeng.com/blog/2018/08/svg.html)
- [Create diagrams using Graphviz](https://ncona.com/2020/06/create-diagrams-with-code-using-graphviz/)

## Font

- [TrueType 入门：基本概念](https://mp.weixin.qq.com/s/4ITS940TWItraV5NZGGDWw)

## Design

- [解读设计风格的变迁史](http://www.jianshu.com/p/3bb4f671094f)
- [色彩想象力－迷之渐变色](https://blog.maxleap.cn/archives/1201)

## Tools

- [HTTPie — a command line HTTP client](https://httpie.org/)
- [mitmproxy — interactive examination and modification of HTTP traffic](https://mitmproxy.org/)
- [The best HTTP Static File Server](https://github.com/codeskyblue/gohttpserver)
- [DevDocs: API Documentation Browser](http://devdocs.io/)
- [Text encoder decoder, format converter](https://toolkit.site/)
- [The Front-End Developer's Guide to the Terminal](https://www.joshwcomeau.com/javascript/terminal-for-js-devs/)

## Open Source

- [Art of README](https://github.com/noffle/art-of-readme/blob/master/README-zh.md)

# Web 协议

Ocsp stapling
Ocsp 全称在线证书状态检查协议 (rfc6960)，用来向 CA 站点查询证书状态，比如是否撤销。通常情况下，浏览器使用 OCSP 协议发起查询请求，CA 返回证书状态内容，然后浏览器接受证书是否可信的状态。

这个过程非常消耗时间，因为 CA 站点有可能在国外，网络不稳定，RTT 也比较大。那有没有办法不直接向 CA 站点请求 OCSP 内容呢？ocsp stapling 就能实现这个功能。

详细介绍参考 RFC6066 第 8 节。简述原理就是浏览器发起 client hello 时会携带一个 certificate status request 的扩展，服务端看到这个扩展后将 OCSP 内容直接返回给浏览器，完成证书状态检查。

由于浏览器不需要直接向 CA 站点查询证书状态，这个功能对访问速度的提升非常明显。

Nginx 目前已经支持这个 ocsp stapling file，只需要配置 ocsp stapling file 的指令就能开启这个功能：

ssl_stapling on;ssl_stapling_file ocsp.staple;

# PWA

https://love2dev.com/blog/15-minute-progressive-web-app-upgrade/
https://developer.mozilla.org/en-US/docs/Web/Manifest
https://github.com/lyzadanger/serviceworker-example
https://web.dev/pwa-checklist/
https://web.dev/customize-install/
https://googlechrome.github.io/samples/service-worker/custom-offline-page/index.html
https://medium.com/james-johnson/a-simple-progressive-web-app-tutorial-f9708e5f2605

Buttons are the “killer feature” of the web.

Every significant thing we do online, from ordering food to scheduling an appointment to playing a video, involves pressing a button. Buttons (and the forms they submit) make the web dynamic and interactive and powerful.
