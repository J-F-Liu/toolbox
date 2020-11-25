# Deno

Deno is a simple, modern and secure runtime for JavaScript and TypeScript that uses V8 and is built in Rust.

scoop install deno
brew install deno
cargo install deno
deno upgrade

deno completions bash > /usr/local/etc/bash_completion.d/deno.bash
source /usr/local/etc/bash_completion.d/deno.bash

Deno does not use npm, does not use package.json in its module resolution algorithm.
Uses "ES Modules" and does not support require(). Third party modules are imported via URLs.

Runs a basic Deno script without any file system or network permissions (sandbox mode):
```
deno run main.ts
```

Explicit flags are required to enable permissions:
```
deno run --allow-read --allow-net main.ts
```

To inspect the dependency tree of the script, use the info subcommand:
```
deno info main.ts
```

Format all JS/TS files in the current directory and subdirectories
```
deno fmt
```

Run script by providing the URL as the input filename:
```
deno run https://deno.land/std/examples/welcome.ts
deno run -A https://deno.land/std/http/file_server.ts
```

Install executable code
```
deno install --allow-net --allow-read https://deno.land/std@0.78.0/http/file_server.ts
/Users/deno/.deno/bin/file_server
```

Start interactive read-eval-print-loop
```
deno repl
```

### Permissions list
The following permissions are available:

* -A, --allow-all Allow all permissions. This disables all security.
* --allow-env Allow environment access for things like getting and setting of environment variables.
* --allow-hrtime Allow high-resolution time measurement. High-resolution time can be used in timing attacks and fingerprinting.
* --allow-net=<allow-net> Allow network access. You can specify an optional, comma-separated list of domains to provide an allow-list of allowed domains.
* --allow-plugin Allow loading plugins. Please note that --allow-plugin is an unstable feature.
* --allow-read=<allow-read> Allow file system read access. You can specify an optional, comma-separated list of directories or files to provide a allow-list of allowed file system access.
* --allow-write=<allow-write> Allow file system write access. You can specify an optional, comma-separated list of directories or files to provide a allow-list of allowed file system access.
* --allow-run Allow running subprocesses. Be aware that subprocesses are not run in a sandbox and therefore do not have the same security restrictions as the deno process. Therefore, use with caution.

### Debugging
To activate debugging capabilities run Deno with the --inspect or --inspect-brk flags.
Open chrome://inspect and click Inspect next to target.