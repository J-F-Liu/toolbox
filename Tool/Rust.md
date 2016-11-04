# Rust

- 安装
```
pacman -S rust cargo
rustc -V
cargo -V
```

- [rustup](https://rustup.rs/)
```
pacman -S rustup
rustup install nightly
rustup default nightly
rustup update
```

- cargo用法
```
cargo new <project> --bin
cargo build --release
cargo run
cargo update
cargo doc
cargo test
cargo bench
```


- 样例程序
```
fn main() {
    // A simple integer calculator:
    // `+` or `-` means add or subtract by 1
    // `*` or `/` means multiply or divide by 2

    let program = "+ + * - /";
    let mut accumulator = 0;

    for token in program.chars() {
        match token {
            '+' => accumulator += 1,
            '-' => accumulator -= 1,
            '*' => accumulator *= 2,
            '/' => accumulator /= 2,
            _ => { /* ignore everything else */ }
        }
    }

    println!("The program \"{}\" calculates the value {}",
              program, accumulator);
}
```

- concatenate strings
```
// String + &str -> String
"hello ".to_owned() + "world" + "!";
format!("{} {}!", "hello", "world");
concat!("2+2=", 4);

let mut text = String::new();
text.push_str("Hello world");
text.push_str("!");
println!("{}", text)
```
- read user input from stdin
```
use std::io;
use std::io::prelude::*;

fn main() {
    let mut user_input = String::new();
    io::stdin().read_line(&mut user_input)
        .expect("Failed to read line!");

    println!("Hello, World.\n{}", user_input);
}

fn main() {
    let stdin = io::stdin();
    for line in stdin.lock().lines() {
        println!("{}", line.unwrap());
    }
}
```
```
use std::collections::HashMap;

fn frequency(s: &str) -> HashMap<char, i32> {
    let mut freqs = HashMap::new();
    for ch in s.chars() {
        let counter = freqs.entry(ch).or_insert(0);
        *counter += 1;  
    }
    freqs
}

struct Node {
    freq: i32,
    ch: Option<char>,
    left: Option<Box<Node>>,
    right: Option<Box<Node>>,
}
let mut p = Node {
                freq:10, ch: Some('A'),
                left:None, right: None,
            };
```

## Articles
- [Optional arguments in Rust 1.12](http://xion.io/post/code/rust-optional-args.html)
- [Implementing Finite Automata](https://apanatshka.github.io/compsci/2016/10/03/implementing-finite-automata-part-1/)
- [Rusty Dynamic Loading](https://damienradtke.com/post/rusty-dynamic-loading/)

## crates
- [Native Windows GUI for rust](https://github.com/gabdube/native-windows-gui)
- [slog-rs - Structured, composable logging for Rust](https://github.com/dpc/slog-rs)
- [Put your Rust app's data in the right place on every platform](https://github.com/AndyBarron/app-dirs-rs)
