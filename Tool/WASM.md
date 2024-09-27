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

# WASI: The WebAssembly System Interface

WASI has fully rebased on the Wit IDL and the component model type system and semantics, making it modular, fully virtualizable, and accessible to a wide variety of source languages.

##  WebAssembly component-model
A Wasm component is a deployable unit of software.
A World is a virtual environment for a component.

An environment here is the surroundings that a Wasm component can run in. It contains imports and exports that a component's exports and imports can link to.
Wasm components are isolated, so this is the only way they can interact with the outside.

A World is a description of what an environment contains. A component can be built to run in a particular World, and then any environment that implements that World can run it.
The implementation of a World may be provided by native host code, by other Wasm components, or a mix of both.

Virtualization enables testing with mocked dependencies, adapting components written for one environment to run in another, polyfilling older versions of APIs for backwards compatibility, and more.

In the component model, high level type definitions are written in a language called WIT (Wasm Interface Type), and the way they translate into bits and bytes is called the Canonical ABI (Application Binary Interface). A Wasm component is thus a wrapper around a core module that specifies its imports and exports using such Interfaces.

- Logically, components are containers for modules - or other components - which express their interfaces and dependencies via WIT.
- Conceptually, components are self-describing units of code that interact only through interfaces instead of shared memory.
- Physically, a component is a specially-formatted WebAssembly file. Internally, the component could include multiple traditional ("core") WebAssembly modules, and sub-components, composed via their imports and exports.

The external interface of a component - its imports and exports - corresponds to a world. The component, however, internally defines how that world is implemented.

## WASM Runtime

https://www.jakobmeier.ch/wasm-road-0

The addressable Wasm memory is usually called linear memory. Everything else has no address.
Local variables in Wasm aren’t stored on a stack, nor are they addressable with pointers. Rather, they have a separate namespace and are accessed by an index of available local variables in a given context.
For the abstract stack, the Wasm spec gives no way to access it other than using operations that implicitly use values on the stack. In Wasm, all operations manipulate the stack.
Function code, likewise, is stored somewhere inaccessible. All the Wasm program gets to see is the table of available functions to call.