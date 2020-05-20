# Web Assembly

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
