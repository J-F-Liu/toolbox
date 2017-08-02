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

下载文件，内容输出到文件，使用原来的文件名
```
curl -O www.example.com/example.html
```

下载文件，使用SOCKS5代理
```
curl --socks5 127.0.0.1:1080 -O www.example.com/example.html
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

使用POST方法
```
curl -i -X POST --url http://httpbin.org/post --data 'name=value'
```

发送JSON请求
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
