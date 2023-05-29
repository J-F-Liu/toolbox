# [Nu Shell](https://www.nushell.sh/)

Nushell, or Nu for short, is a new shell that takes a modern, structured approach to your command line.
Nu pipelines use structured data so you can safely select, filter, and sort the same way every time.
Nushell is designed to use a single compile step for all the source you send it, and this is separate from evaluation.

## Install

```
scoop install nu
```

Install dataframe functionality and plugins

```
cargo install nu --features=dataframe
cargo install nu_plugin_<plugin name>
```

VSCode extension: vscode-nushell-lang
VSCode config:

```
    "terminal.integrated.profiles.windows": {
        "PowerShell": {
            "source": "PowerShell",
            "icon": "terminal-powershell"
        },
        "NuShell": {
            "path": "${env:USERPROFILE}\\scoop\\shims\\nu.exe",
        },
```

## Configuration

Show configuration file path:

```
echo $nu.env-path
echo $nu.config-path
C:\Users\cs\AppData\Roaming\nushell\config.nu
```

## Environment

see the current environment variables

```
env | select name value
$env.Path
```

set environment variables for the duration of a Nushell session

```
let-env FOO = 'BAR'
let-env PATH = ($env.PATH | append '/some/path')
```

set an environment variable once per command

```
FOO=BAR $env.FOO
with-env { FOO: BAR } { $env.FOO }
```

set permanent environment variables

```
# In config.nu
let-env FOO = 'BAR'
```

## Aliases

Show aliases

```
$nu.scope.aliases
```

Create aliases

```
alias ll = ls -l
alias lt = (ls | sort-by modified -r | sort-by type)
```

To make your alias persistent the commands must be added to the root level your config.nu file.
