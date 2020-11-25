# Web Assembly

WebAssembly has the potential to be the future of the web and also the future of containerized applications in general!
Solomon Hykes, the creator of Docker, has tweeted "If WASM + WASI existed in 2008, we wouldn't have needed to create Docker."

Wasmer is an open-source runtime for executing WebAssembly.

Install Wasmer and WAPM

```
curl https://get.wasmer.io -sSfL | sh
source /Users/Junfeng/.wasmer/wasmer.sh
wasmer self-update
```

Install and run a package

```
wapm install cowsay
wapm run cowsay hello wapm!

wapm install kherrick/pwgen
wapm run pwgen -s 12 1
```
