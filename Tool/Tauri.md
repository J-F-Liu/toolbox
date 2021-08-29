# Tauri

https://tauri.studio/en/

[Get started making desktop apps using Rust and React](https://kent.medium.com/get-started-making-desktop-apps-using-rust-and-react-78a7e07433ce)

yarn create react-app project-folder

cargo install tauri-cli --version ^1.0.0-beta
cd project-folder
cargo tauri init

```
❯ cargo tauri --help
cargo-tauri 1.0.0-beta.5
Tauri CLI

USAGE:
    cargo tauri [SUBCOMMAND]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    build    Tauri build.
    dev      Tauri dev.
    help     Prints this message or the help of the given subcommand(s)
    info     Shows information about Tauri dependencies
    init     Initializes a Tauri project
    sign     Tauri updates signer.

❯ cargo tauri build -h
cargo-tauri-build 1.0.0-beta.5
Tauri build.

USAGE:
    cargo tauri build [FLAGS] [OPTIONS]

FLAGS:
    -d, --debug      Builds with the debug flag
    -h, --help       Prints help information
    -v, --verbose    Enables verbose logging
    -V, --version    Prints version information

OPTIONS:
    -b, --bundle <bundle>...        list of bundles to package
    -c, --config <config>           config JSON to merge with tauri.conf.json
    -f, --features <features>...    list of cargo features to activate
    -r, --runner <runner>           binary to use to build the application
    -t, --target <target>...        target triple to build against
```
