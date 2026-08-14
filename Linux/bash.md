.bash_profile 登录 shell 时加载（首次打开 Bash）
```
test -f ~/.profile && . ~/.profile
test -f ~/.bashrc && . ~/.bashrc
```

.bashrc 每次打开交互式终端 都加载（包括新 tab、子 shell）
```
alias br='cargo build --release'

ts() {
    if [ -f "src/lib.rs" ]; then
        cargo test --lib --release -- --nocapture --test-threads=1 "$@"
    else
        cargo test --release -- --nocapture --test-threads=1 "$@"
    fi
}
```