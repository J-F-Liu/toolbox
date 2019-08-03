# [cURL](https://curl.haxx.se/)

A command line tool and library for transferring data with URLs.

下载文件，内容输出到屏幕

```
curl www.example.com
```

下载文件，内容输出到文件

```
curl -o example.html www.example.com
```

下载文件，内容输出到文件，使用原来的文件名，J 表示使用 Content-Disposition 给的文件名

```
curl -O www.example.com/file.html
curl -OJ www.example.com/file.html
```

自动访问重定向后的地址

```
curl -L www.example.com/file.html
```

匹配和下载多个地址的文件，[]指定范围，{}指定列表，可组合起来使用

```
curl -O -O http://example.com/{web,mail}-log[0-6].txt
```

下载文件，使用 SOCKS5 代理

```
curl --socks5 127.0.0.1:1080 -O www.example.com/file.html
```

输出响应消息的状态行、消息报头、空行和响应正文

```
curl -i http://httpbin.org/get
```

输出请求消息报头，响应消息的状态行、消息报头、空行和响应正文

```
curl -v http://httpbin.org/get
```

设置请求消息报头

```
curl -i --url http://httpbin.org/get --header "Apikey: ENTER_KEY_HERE"
```

使用 POST 方法

```
curl -i -X POST --url http://httpbin.org/post --data 'name=value'
```

发送 JSON 请求

```
curl -i -X POST \
  --url http://httpbin.org/post \
  --header "Content-Type: application/json" \
  --data '{"key1":"value1", "key2":"value2"}'
```

读取并发送文件内容

```
curl --data-binary @file.txt https://paste.rs/
```

提交表单(multipart formpost)

```
curl -F person=anonymous -F secret=@file.txt http://example.com/submit.cgi
```

记录和发送 cookie

```
curl -c cookies.txt https://paste.rs/
```

统计用时

```
curl -o /dev/null -s -w 'Establish Connection: %{time_connect}s\nTTFB: %{time_starttransfer}s\nTotal: %{time_total}s\n'  https://www.google.com
```
