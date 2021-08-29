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

Use wax

```
scoop install wasmer
wax cowsay hello
wax demimud
```

```demimud
- How to Move -   exit / direction (east or e for short)

- How to Look -   look / look <keyword> / look in <container>
                  examine <keyword>

- How to Open -   open <direction>

- How to Unlock - unlock <direction> / open <direction>

- How to Get -    get <keyword> / get <object> <container>
                  get all / get all.<keyword>

 - How to Give -        give <item> <keyword of player>
                        give <item> <name of player>
                        give all <player> / give all.<item> <player>

 - How to Talk -        say <message> / 'message
                        sayto <player> <message> / '>player message

 - How to use Socials - type socials for a list available

 - How to do Emotes -   emote <action> / ,action
                        type help emote for more info

 - How to Follow -      follow <player> / beckon <player>
                        follow self / nofollow
```
