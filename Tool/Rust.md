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
rustup update nightly
rustup self update
rustup target list
rustup target add x86_64-unknown-linux-musl
```

- cargo用法
```
cargo new <project> --bin
cargo check
cargo run
cargo build --release
cargo build --release --target x86_64-unknown-linux-musl
cargo rustc --release -- -C target-cpu=skylake
RUSTFLAGS="-Ctarget-cpu=native" cargo rustc --release
cargo update
cargo doc --open
cargo test
cargo test -- --nocapture
cargo bench
CARGO_INCREMENTAL=1 cargo build
```

- Cross Compile
```
pacman -S mingw-w64
rustup target add x86_64-pc-windows-gnu
PKG_CONFIG_ALLOW_CROSS=1 cargo build --release --target x86_64-pc-windows-gnu
PKG_CONFIG_ALLOW_CROSS=1 cargo rustc --release --target x86_64-pc-windows-gnu  -- -C link-args=-mwindows
```

- Development tools
```
rustup show
rustup doc
rustup default nightly
rustup update nightly
rustup component add rls-preview
rustup component add rust-analysis
rustup component add rust-src
rustup component add rust-docs
cargo install rustfmt-nightly
cargo install clippy
cargo install cargo-edit
cargo install cargo-outdated
cargo install --git https://github.com/murarth/rusti

rustup update nightly # update RLS and its dependencies
```

- Tools developed using Rust
```
cargo install loc
cargo install ripgrep
cargo install tokei
cargo install xsv
cargo install --git https://github.com/sharkdp/fd
cargo install --git https://github.com/ogham/exa
cargo install simple-http-server
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

### concatenate strings
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
### format numbers
```
format_spec := [[fill]align][sign]['#'][0][width]['.' precision][type]
fill := character
align := '<' | '^' | '>'
sign := '+' | '-'
width := count
precision := count | '*'
type := identifier | ''
count := parameter | integer
parameter := argument '$'

mapping of types to traits is:
nothing ⇒ Display
? ⇒ Debug
o ⇒ Octal
x ⇒ LowerHex
X ⇒ UpperHex
p ⇒ Pointer
b ⇒ Binary
e ⇒ LowerExp
E ⇒ UpperExp
`{:.*}` get precision from argument list, e.g. `format!("{:.*}", 2, 1.234567) // => "1.23"`
# - This flag indicates alternate forms:
#? - pretty-print the Debug formatting
#x - precedes the argument with a 0x
#X - precedes the argument with a 0x
#b - precedes the argument with a 0b
#o - precedes the argument with a 0o

println!("{:b} of {:x}", 10, 10); // binary and hexical number
format!("{:04}", 42);             // => "0042" with leading zeros

// You can right-align text with a specified width. This will output
// "     1". 5 white spaces and a "1".
println!("{number:>width$}", number=1, width=6);

// You can pad numbers with extra zeroes. This will output "000001".
println!("{number:>0width$}", number=1, width=6);
```
### read user input from stdin
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
### function/closure as input parameter
Closures are more complicated than functions: it's basically a regular function pointer + the closure environment.
A named function's name can be used wherever you'd use a closure.
```
fn twice_ptr(x: i32, f: fn(i32) -> i32) -> i32 {
    f(x) + f(x)
}
fn twice_dyn(x: i32, f: &Fn(i32) -> i32) -> i32 {
    f(x) + f(x)
}
fn twice_stc<F:Fn(i32) -> i32>(x: i32, f: F) -> i32 {
    f(x) + f(x)
}

fn square(x: i32) -> i32 { x * x }

fn main() {
  println!("{:?}",twice_ptr(5, square));
  println!("{:?}",twice_stc(5, |x| x * x));
}
```

### Pass closures as callbacks.
```
struct Processor<CB> where CB: FnMut() {
    callback: CB,
}

impl<CB> Processor<CB> where CB: FnMut() {
    fn set_callback(&mut self, c: CB) {
        self.callback = c;
    }

    fn process_events(&mut self) {
        (self.callback)();
    }
}

fn main() {
    let s = "world!".to_string();
    let callback = || println!("hello {}", s);
    let mut p = Processor { callback: callback };
    p.process_events();
}
```
- Fn are closures that only read data, and may be safely called multiple times, possibly from multiple threads. Both above closures are Fn.
- FnMut are closures that modify data, e.g. by writing to a captured mut variable. They may also be called multiple times, but not in parallel. (Calling a FnMut closure from multiple threads would lead to a data race, so it can only be done with the protection of a mutex.) The closure object must be declared mutable by the caller.
- FnOnce are closures that consume the data they capture, e.g. by moving it to a function that owns them. As the name implies, these may be called only once, and the caller must own them.

There's 2 different types of move when discussing closures:
- moving into the closure which is what the move keyword is doing to let the closure own the data
- moving out of a captured variable, which can only be done in an FnOnce/FnBox

The above code requires each Processor instance to be parameterized with a concrete callback type, which means that a single Processor can only deal with one callback type.
If Processor stores Box<FnMut()>, it no longer needs to be generic, but the set_callback method is now generic.
```
struct Processor {
    callback: Box<FnMut()>,
}

impl Processor {
    fn set_callback<CB: 'static + FnMut()>(&mut self, c: CB) {
        self.callback = Box::new(c);
    }

    fn process_events(&mut self) {
        (self.callback)();
    }
}

fn simple_callback() {
    println!("hello");
}

fn main() {
    let mut p = Processor { callback: Box::new(simple_callback) };
    p.process_events();
    let s = "world!".to_string();
    let callback2 = move || println!("hello {}", s);
    p.set_callback(callback2);
    p.process_events();
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

Can not return local String as a slice, need to return a value of an owned type.
```
fn return_str<'a>() -> &'a str {
    let mut string = "".to_string();

    for i in 0..10 {
        string.push_str("ACTG");
    }

    &string[..]
}
// should be written as
fn return_str() -> String {
    let mut string = String::new();

    for _ in 0..10 {
        string.push_str("ACTG");
    }

    string
}
```

```
fn main() {
  let s = String::from("foo");
  let x = Some(5);
  // let r = x.map_or((s, 0), |v| (s, v)); // will get "value captured here after move" error
  let r = match x {
      None => (s, 0),
      Some(v) => (s, v)
  };
  println!("{:?}", r);
}
```
### Lifetime
There are four places where a lifetime can appear in a type:
- `&'a Type`: the lifetime of an immutable reference;
- `&'a mut Type`: the lifetime of a mutable reference;
- `Type<'a>`: a generic lifetime parameter on a type;
- `Trait + 'a`: a trait object’s lifetime, as with generic bounds.

Box<Trait> is normally equivalent to Box<Trait + 'static> and &'a Trait to &'a (Trait + 'a).

### Sizedness
Sized is a special compiler built-in trait that is automatically implemented or not based on the sizedness of a type.
Types for which the size is not known are called dynamically sized types (DSTs), and there’s two classes of examples in current Rust1: [T] and Trait.
Unsized values must always appear behind a pointer at runtime, like &[T] or Box<Trait>, putting an unsized type behind a pointer effectively makes it sized.
```
fn foo<T>() {} // can only be used with sized T
fn bar<T: ?Sized>() {} // can be used with both sized and unsized T
```

In Rust traits are types, but they are “unsized”, which roughly means that they are only allowed to show up behind a pointer like Box (which points onto the heap) or & (which can point anywhere).
A type like `&Trait` or `Box<Trait>` is called a “trait object”, and includes a pointer to an instance of a type T implementing `Trait`, and a vtable: a pointer to T’s implementation of each method in the trait.

### TypeID
```
use std::any::{Any, TypeId};

fn type_of<T: ?Sized + Any>(_s: &T) -> TypeId {
    TypeId::of::<T>()
}

fn type_id<E: 'static + ?Sized>() -> TypeId {
    TypeId::of::<E>()
}

fn main() {
    println!("{:?}", type_of(b"c") == type_id::<[u8;1]>());
}
```
### Deref Box
```
use std::any;
trait Foo {
    fn bar(&mut self) -> &mut any::Any;
}

impl<T: Foo> Foo for Box<T> {
    //fn bar(&mut self) -> &mut any::Any { (**self).bar() }

    fn bar<'a>(&'a mut self) -> &'a mut any::Any {
        |q: &'a mut Box<T>| -> &'a mut T { &mut **q }(self).bar()
    }
}
```

## Articles
- [On integer types in Rust](https://medium.com/@marcinbaraniecki/on-integer-types-in-rust-b3dc1b0a23d3)
- [str vs String](http://www.ameyalokare.com/rust/2017/10/12/rust-str-vs-String.html)
- [Setting up a Rust Development Environment](http://asquera.de/blog/2017-03-03/setting-up-a-rust-devenv/)
- [Rust: first impressions](http://xion.io/post/code/rust-first-impressions.html)
- [Rust and CSV parsing](http://blog.burntsushi.net/csv/)
- [& vs. ref in Rust patterns](http://xion.io/post/code/rust-patterns-ref.html)
- [Optional arguments in Rust 1.12](http://xion.io/post/code/rust-optional-args.html)
- [Communicating Intent](https://github.com/jaheba/stuff/blob/master/communicating_intent.md)
- [Non-lexical lifetimes: introduction](http://smallcultfollowing.com/babysteps/blog/2016/04/27/non-lexical-lifetimes-introduction/)
- [An Overview of Macros in Rust](http://words.steveklabnik.com/an-overview-of-macros-in-rust)
- [Convenient and idiomatic conversions in Rust](https://ricardomartins.cc/2016/08/03/convenient_and_idiomatic_conversions_in_rust)
- [Interior mutability in Rust: what, why, how?](https://ricardomartins.cc/2016/06/08/interior-mutability)
- [Finding Closure in Rust](http://huonw.github.io/blog/2015/05/finding-closure-in-rust/)
- [Good Practices for Writing Rust Libraries](https://pascalhertleif.de/artikel/good-practices-for-writing-rust-libraries/)
- [Six Easy Ways to Make Your Crate Awesome](http://www.integer32.com/2016/12/27/how-to-make-your-crate-awesome.html)
- [Writing Documentation in Rust](https://facility9.com/2016/05/writing-documentation-in-rust/)
- [Implementing Finite Automata](https://apanatshka.github.io/compsci/2016/10/03/implementing-finite-automata-part-1/)
- [Rusty Dynamic Loading](https://damienradtke.com/post/rusty-dynamic-loading/)
- [Abstracting over mutability in Rust](https://lab.whitequark.org/notes/2016-12-13/abstracting-over-mutability-in-rust/)
- [Rust futures at a glance](https://daiheitan.github.io/blog/2016/12/07/Rust-futures-at-a-glance/)
- [Getting Started with Tokio](https://lukesteensen.com/2016/12/getting-started-with-tokio/)
- [Why doesn't Rust have properties?](https://www.reddit.com/r/rust/comments/2uvfic/why_doesnt_rust_have_properties/)
- [Rust Tidbits: Box Is Special](https://manishearth.github.io/blog/2017/01/10/rust-tidbits-box-is-special/)
- [The Borrow Checker](https://github.com/rust-lang/rust/blob/master/src/librustc_borrowck/borrowck/README.md)
- [Strategies for Returning References in Rust](https://bryce.fisher-fleig.org/blog/strategies-for-returning-references-in-rust/)
- [Polymorphism in Rust: Enum vs Trait + Struct](http://keepcalmandlearnrust.com/2017/03/polymorphism-in-rust-enum-vs-trait-struct/)
- [Iteration patterns for Result & Option](http://xion.io/post/code/rust-iter-patterns.html)
- [Exploring lock-free Rust](https://morestina.net/blog/742/exploring-lock-free-rust-1-locks)

## crates
- [Native Windows GUI for rust](https://github.com/gabdube/native-windows-gui)
- [slog-rs - Structured, composable logging for Rust](https://github.com/dpc/slog-rs)
- [Put your Rust app's data in the right place on every platform](https://github.com/AndyBarron/app-dirs-rs)
- [An ergonomic HTTP Client for Rust](https://github.com/seanmonstar/reqwest)
- [Task Scheduler](https://github.com/dbeck/acto-rs)
- [Rust compiler plugin for reading and using a TOML configuration file at compile-time](https://github.com/Luthaf/confy)
- [Consistent error handling for Rust](https://github.com/brson/error-chain)
- [The mathematical constant tau](https://github.com/FranklinChen/rust-tau/)
- [Sorted key-value map](https://github.com/toffaletti/flat_map)
- [Insertion order preserved key-value map](https://github.com/contain-rs/linked-hash-map)
- [A lock-free, eventually consistent, concurrent multi-value map](https://github.com/jonhoo/rust-evmap)
- [Multiple values for the same key](https://github.com/havarnov/multimap)
- [safe and convenient store for one value of each type](https://github.com/chris-morgan/anymap)
- [A map containing varying types of values with compact storage](https://github.com/murarth/polymap)
- [Construct JSON objects in Rust from JSON-like literals](https://github.com/tomjakubowski/json_macros)
- [A small macro for defining lazy evaluated static variables in Rust](https://github.com/rust-lang-nursery/lazy-static.rs)
- [Container / collection literal macros for HashMap, HashSet, BTreeMap, BTreeSet](https://github.com/bluss/maplit)
- [Unicode Grapheme Cluster and Word boundaries](https://github.com/unicode-rs/unicode-segmentation)
- [A pull parser for CommonMark](https://github.com/google/pulldown-cmark)

## Notes

* Goal: memory safety and data-race-free concurrency.
* Strategy: establishes a clear lifetime for each value.
* Method: ownership, borrow checker, lifetime parameters, static type system.

If a program has been written so that no possible execution can exhibit undefined behavior, we say that program is well defined.
If a language’s type system ensures that every program is well defined, we say that language is type safe.

Rust is both type safe and a systems programming language.
Rust does provide for unsafe code, the great majority of programs do not require unsafe code, and Rust programmers generally avoid it, since it must be reviewed with special care.

Three key promises Rust makes about every program that passes its compile-time checks:
• No null pointer dereferences. Your program will not crash because you tried to dereference a null pointer.
• No dangling pointers. Every value will live as long as it must. Your program will never use a heap-allocated value after it has been freed.
• No buffer overruns. Your program will never access elements beyond the end or before the start of an array.

The problem with null pointers is that it’s easy to forget to check for them. Rus simply doesn’t provide a way to introduce null pointers.
When we need an optional argument, return value, or so on of some pointer type P, we use Option<P>.

The Box type is Rust’s simplest form of heap allocation: a Box<T> is an owning pointer to a heap-allocated value of type T.
When the box is dropped, the value in the heap it refers to is freed along with it.

When a Rust program indexes an array a with an expression like a[i], the program first checks that i falls within the array’s size n.
Sometimes the compiler recognizes that this check can be safely omitted, but when it can’t, Rust generates code to check the array’s index at runtime.
If the index is out of bounds, the thread panics.

Rust programs never try to access a heap-allocated value after it has been freed. What is unusual is that Rust does so without resorting to garbage collection or reference counting.
Rust has three rules that specify when each value is freed, and ensure all pointers to it are gone by that point. Rust enforces these rules entirely at compile time.
• Rule 1: Every value has a single owner at any given time. You can move a value from one owner to another, but when a value’s owner goes away, the value is freed along with it.
• Rule 2: You can borrow a reference to a value, so long as the reference doesn’t outlive the value (or equivalently, its owner). Borrowed references are temporary pointers; they allow you to operate on values you don’t own.
• Rule 3: You can only modify a value when you have exclusive access to it.

In Rust, for most types (including any type that owns resources like a heap-allocated buffer), assignment moves the value: the destination takes ownership, and the source is no longer considered initialized.
Passing arguments to a function and returning values from a function are handled like assignment: they also move such types, rather than copying.

• Some types can be copied bit-for-bit, without the need for any special treatment; Rust permits such types to implement the special trait Copy.
  Assignment copies Copy types, rather than moving them: the source of the assignment retains its value.
• All other types are moved by assignment, never implicitly copied.

Rust permits Copy implementations only for types that qualify: all the values your type comprises must be Copy themselves, and your type must not require custom behavior when it is dropped.
The Rust standard library includes a related trait, Clone, for types that can be copied explicitly. Its definition is simple:
```
pub trait Copy: Clone { }

pub trait Clone {
    fn clone(&self) -> Self;
    fn clone_from(&mut self, source: &Self) { ... }
}
```
Clone is a supertrait of Copy, so everything which is Copy must also implement Clone.
In other words, if something implements Copy, it is also cloneable.
Cloning a vector entails cloning each of its elements in turn, so Vec<T> implements Clone whenever T does so; the other container types behave similarly.

When we take a reference to a value, Rust looks at how we use it (Is it passed to function? Stored in a local variable? Saved in a data structure?) and assesses how long it could live.
Since these checks are all done at compile time, Rust can’t always be exact, and must sometimes err on the side of caution.
Then, it compares the lifetime of the value with the lifetime of the reference: if the reference could possibly outlive the value it points to, Rust complains, and refuses to compile the program.
Rust’s borrow checking has improved over time, and will continue to do so.

While a value is borrowed, it musn’t be moved to a new owner. **A variable must not go out of scope while it’s borrowed**.

References always have lifetimes associated with them; Rust simply lets us omit them when the situation is unambigous.

Rust’s ownership, moves, and borrows end up being a reasonably pleasant way to express resource management.


How Rust innovates the greatest is by statically tracking ownership and lifetimes of all variables and their references. The ownership system enables Rust to automatically deallocate and run destructors on all values immediately when they go out of scope, and prevents values from being accessed after they are destroyed. It is what makes Rust memory safe and thread safe, and why it doesn't need a GC to accomplish that.

Rust, through ownership, effectively enforces the higher-level single responsibility principle that sometimes is a struggle to be consistent about in other languages. That alone is amazing.

It does make hard things easy, but only after you've fully embraced Rust.

If you want to learn a language you don't know already, one very educational way to do it is to rewrite some piece of software you already understand.

A crate is a unit of compilation and linking, as well as versioning, distribution and runtime loading.
A crate contains a tree of nested module scopes.
The top level of this tree is a module that is anonymous and any item within a crate has a canonical module path denoting its location within the crate's module tree.

A Rust source file describes a module, the name and location of which — in the module tree of the current crate — are defined from outside the source file: either by an explicit mod item in a referencing source file, or by the name of the crate itself.
Every source file is a module, but not every module needs its own source file: module definitions can be nested within one file.

Each source file contains a sequence of zero or more item definitions, and may optionally begin with any number of attributes that apply to the containing module, most of which influence the behavior of the compiler.
The anonymous crate module can have additional attributes that apply to the crate as a whole.

A crate that contains a main function can be compiled to an executable.
If a main function is present, its return type must be () ("unit") and it must take no arguments.

A module is a container for zero or more items.
A module without a body is loaded from an external file.
When a nested submodule is loaded from an external file, it is loaded from a subdirectory path that mirrors the module hierarchy.
The directories and files used for loading external file modules can be influenced with the path attribute.

Items are organized within a crate by a nested set of modules.
Every crate has a single "outermost" anonymous module; all further items within the crate have paths within the module tree of the crate.

There are several kinds of item:
* extern crate declarations
* use declarations
* modules
* functions
* type definitions
* structs
* enumerations
* constant items
* static items
* traits
* implementations

All items except modules, constants and statics may be parameterized by type.
The type parameters of an item are considered "part of the name", not part of the type of the item.

`mod foo;` is basically a way of saying “look for foo.rs or foo/mod.rs and make a module named foo with its contents”. It’s the same as `mod foo { ... }` except the contents are in a different file.

`use blash` create an item named `blah` “within the module”, which is basically something like a symbolic link, paths are relative to the root module.
`pub use` says “make this symlink, but let others see it too”.
`use self::foo` means “find me `foo` within the current module”.

Usually a use declaration is used to shorten the path required to refer to a module item.
These declarations may appear in modules and blocks, usually at the top.
```
use p::q::r as x;
use a::b::{c,d,e,f};
use a::b::{self, c, d}; // use a::b; use a::b::{c,d};
use a::b::*;
```
The paths contained in use items are relative to the crate root.
It is also possible to use self and super at the beginning of a use item to refer to the current and direct parent modules respectively.

A function item defines a sequence of statements and a final expression, along with a name and a set of parameters. Other than a name, all these are optional.
A generic function allows one or more parameterized types to appear in its signature.

Trait bounds can be specified for type parameters to allow methods with that trait to be called on values of that type. This is specified using the where syntax:
```
use std::fmt::Debug;

fn foo<T>(x: &[T]) where T: Debug {
    // details elided
}

foo(&[1, 2]);
```
When a generic function is referenced, its type is instantiated based on the context of the reference.
The type parameters can also be explicitly supplied in a trailing path component after the function name.
This might be necessary if there is not sufficient context to determine the type parameters. For example, `mem::size_of::<u32>() == 4`.

A type alias defines a new name for an existing type.
```
type Point = (u8, u8);
let p: Point = (41, 68);
```

A unit-like struct is a struct without any fields, defined by leaving off the list of fields entirely. Such a struct implicitly defines a constant of its type with the same name.
A tuple struct is a nominal tuple type, also defined with the keyword struct.

tuple 类型没有自己的名字，每个成员也没有自己的名字；
tuple struct 类型有自己的名字，但是每个成员没有自己的名字；
struct 类型有自己的名字，而且每个成员也有自己的名字。

An enumeration is a simultaneous definition of a nominal enumerated type as well as a set of constructors, that can be used to create or pattern-match values of the corresponding enumerated type.
Enumeration constructors can have either named or unnamed fields:
```
enum Animal {
    Rat,
    Dog (String, f64),
    Cat { name: String, weight: f64 },
}

let mut a: Animal = Animal::Dog("Cocoa".to_string(), 37.2);
a = Animal::Cat { name: "Spotty".to_string(), weight: 2.7 };
```
Each enum value has a discriminant which is an integer associated to it if none of the variants have data attached.

A constant item is a named constant value which is not associated with a specific memory location in the program.
Constants are essentially inlined wherever they are used, meaning that they are copied directly into the relevant context when used.
References to the same constant are not necessarily guaranteed to refer to the same memory address.

A static item is similar to a constant, except that it represents a precise memory location in the program.
A static is never "inlined" at the usage site, and all references to it refer to the same memory location.
Static items have the static lifetime, which outlives all other lifetimes in a Rust program.
Static items may be placed in read-only memory if they do not contain any interior mutability.

If a static item is declared with the mut keyword, then it is allowed to be modified by the program.
An unsafe block is required when either reading or writing a mutable static variable.
Mutable statics have the same restrictions as normal statics, except that the type of the value is not required to ascribe to Sync.

A trait describes an abstract interface that types can implement. This interface consists of associated items, which come in three varieties:
* functions
* constants
* types

All traits define an implicit type parameter Self that refers to "the type that is implementing this interface". Traits may also contain additional type parameters.
Traits can include default implementations of methods, as in:
```
trait Foo {
    fn bar(&self);
    fn baz(&self) { println!("We called baz."); }
}
```
Trait methods may be static, which means that they lack a self argument.
```
trait Num {
    fn from_i32(n: i32) -> Self;
}
impl Num for f64 {
    fn from_i32(n: i32) -> f64 { n as f64 }
}
let x: f64 = Num::from_i32(42);
```

Type parameters can be specified for a trait to make it generic.
```
trait Seq<T> {
    fn len(&self) -> u32;
    fn elt_at(&self, n: u32) -> T;
    fn iter<F>(&self, F) where F: Fn(T);
}
```
It is also possible to define associated types for a trait.
```
trait Container {
    type E;
    fn empty() -> Self;
    fn insert(&mut self, Self::E);
}
```

Traits may inherit from other traits. Multiple supertraits are separated by +, `trait Circle : Shape + PartialEq { }`.
Traits also define a trait object with the same name as the trait. Values of this type are created by coercing from a pointer of some specific type to a pointer of trait type.

An implementation is an item that implements a trait for a specific type. Implementations are defined with the keyword `impl`.
All methods declared as part of the trait must be implemented, with matching types and type parameter counts.
```
struct Circle {
    radius: f64,
    center: Point,
}

impl Copy for Circle {}

impl Clone for Circle {
    fn clone(&self) -> Circle { *self }
}
```
It is possible to define an implementation without referring to a trait. The methods in such an implementation can only be used as direct calls on the values of the type that the implementation targets.
Such implementations are limited to nominal types (enums, structs, trait objects), and the implementation must appear in the same crate as the self type:
```
struct Point {x: i32, y: i32}

impl Point {
    fn log(&self) {
        println!("Point is at ({}, {})", self.x, self.y);
    }
}

let my_point = Point {x: 10, y:11};
my_point.log();
```
An implementation can take type parameters, which can be different from the type parameters taken by the trait it implements.
```
impl<T> Seq<T> for Vec<T> {
    /* ... */
}
impl Seq<bool> for u32 {
    /* Treat the integer as a sequence of bits */
}
```

External blocks form the basis for Rust's foreign function interface. Declarations in an external block describe symbols in external, non-Rust libraries.

By default, everything in Rust is private, with one exception. Enum variants in a pub enum are also public by default.
When an item is declared as pub, it can be thought of as being accessible to the outside world.
If an item is public, then it can be used externally through any of its public ancestors.
If an item is private, it may be accessed by the current module and its descendants.

Rust allows publicly re-exporting items through a pub use directive. It essentially allows public access into the re-exported item.
```
// Any external crate referencing implementation::api::f would receive a privacy violation, while the path api::f would be allowed.
pub use self::implementation::api;

mod implementation {
    pub mod api {
        pub fn f() {}
    }
}
```

Any item declaration may have an attribute applied to it.
An attribute is a general, free-form metadatum that is interpreted according to name, convention, and language and compiler version. Attributes may appear as any of:
* A single identifier, the attribute name
* An identifier followed by the equals sign '=' and a literal, providing a key/value pair
* An identifier followed by a parenthesized list of sub-attribute arguments

Attributes with a bang ("!") after the hash ("#") apply to the item that the attribute is declared within.
Attributes that do not have a bang after the hash apply to the item that follows the attribute.

An example of attributes:
```
// General metadata applied to the enclosing module or crate.
#![crate_type = "lib"]

// A function marked as a unit test
#[test]
fn test_foo() {
    /* ... */
}

// A conditionally-compiled module
#[cfg(target_os="linux")]
mod bar {
    /* ... */
}

// A lint attribute used to suppress a warning/error
#[allow(non_camel_case_types)]
type int8_t = i8;
```
The derive attribute allows certain traits to be automatically implemented for data structures.
```
#[derive(PartialEq, Clone)]
struct Foo<T> {
    a: i32,
    b: T,
}
```
The generated impl for PartialEq is equivalent to
```
impl<T: PartialEq> PartialEq for Foo<T> {
    fn eq(&self, other: &Foo<T>) -> bool {
        self.a == other.a && self.b == other.b
    }

    fn ne(&self, other: &Foo<T>) -> bool {
        self.a != other.a || self.b != other.b
    }
}
```

Rust is primarily an expression language. This means that most forms of value-producing or effect-causing evaluation are directed by the uniform syntax category of expressions.
A statement is a component of a block.
A block is a component of an outer expression or function.
A declaration statement is one that introduces one or more names into the enclosing statement block. The declared names may denote new variables or new items.
An expression statement is one that evaluates an expression and ignores its result.

A let statement introduces a new set of variables, given by a pattern. The pattern may be followed by a type annotation, and/or an initializer expression.
When no type annotation is given, the compiler will infer the type, or signal an error if insufficient type information is available for definite inference.

An expression evaluates to a value, and may has effects during evaluation.
The structure of expressions dictates the structure of execution.
Blocks are just another kind of expression, so blocks, statements, expressions, and blocks again can recursively nest inside each other to an arbitrary depth.

Expressions are divided into two main categories: lvalues and rvalues.
Likewise within each expression, sub-expressions may occur in lvalue context or rvalue context.
The evaluation of an expression depends both on its own category and the context it occurs within.

An lvalue is an expression that represents a memory location.
These expressions are paths (which refer to local variables, function and method arguments, or static variables), dereferences (`*expr`), indexing expressions (`expr[expr]`), and field references (`expr.f`).
All other expressions are rvalues.

The left operand of an assignment or compound-assignment expression is an lvalue context, as is the single operand of a unary borrow.
The discriminant or subject of a match expression may be an lvalue context, if ref bindings are made, but is otherwise an rvalue context.
All other expression contexts are rvalue contexts.

When an lvalue is evaluated in an lvalue context, it denotes a memory location;
when evaluated in an rvalue context, it denotes the value held in that memory location.

When an rvalue is used in an lvalue context, a temporary un-named lvalue is created and used instead.
The lifetime of temporary values is typically the innermost enclosing statement; the tail expression of a block is considered part of the statement that encloses the block.
When a temporary rvalue is being created that is assigned into a let declaration, however, the temporary is created with the lifetime of the enclosing block instead.

When a local variable is used as an rvalue, the variable will be copied if its type implements Copy. All others are moved.

A literal expression consists of one of the literal forms.
A path used as an expression context denotes either a local variable or an item.
Tuple expressions are written by enclosing zero or more comma-separated expressions in parentheses.
An array expression is written by enclosing zero or more comma-separated expressions of uniform type in square brackets.
A struct expression forms a new value of the named struct type. Note that for a given unit-like struct type, this will always be the same value.
A block expression is similar to a module in terms of the declarations that are possible. Each block conceptually introduces a new namespace scope.

A block will execute each statement sequentially, and then execute the expression (if given). If the block ends in a statement, its value is ():
```
let x: () = { println!("Hello."); };
```
If it ends in an expression, its value and type are that of the expression:
```
let x: i32 = { println!("Hello."); 5 };
assert_eq!(5, x);
```

A field expression denotes a field of a struct, consists of an expression followed by a single dot and an identifier.
A method call consists of an expression followed by a single dot, an identifier, and a parenthesized expression-list.
Method calls are resolved to methods on specific traits, either statically dispatching to a method if the exact self-type of the left-hand-side is known,
or dynamically dispatching if the left-hand-side expression is an indirect trait object.

Array-typed expressions can be indexed by writing a square-bracket-enclosed expression (the index) after them.
Indices are zero-based, and may be of any integral type.
Vector access is bounds-checked at compile-time for constant arrays being accessed with a constant index value.
Otherwise a check will be performed at run-time that will put the thread in a panicked state if it fails.

The .. operator will construct an object of one of the std::ops::Range variants.
```
1..2;   // std::ops::Range
3..;    // std::ops::RangeFrom
..4;    // std::ops::RangeTo
..;     // std::ops::RangeFull
```
Similarly, the ... operator will construct an object of one of the std::ops::RangeInclusive variants.

## MIR
MIR stands for mid-level IR, because the MIR comes between the existing HIR (“high-level IR”, roughly an abstract syntax tree) and LLVM (the “low-level” IR).

At the highest level MIR describes the execution of a single function.
It consists of a series of declarations regarding the stack storage that will be required and then a set of basic blocks.
```
MIR = fn({TYPE}) -> TYPE {
    {let [mut] B: TYPE;}  // user-declared bindings and their types
    {let TEMP: TYPE;}     // compiler-introduced temporary
    {BASIC_BLOCK}         // control-flow graph
};
```
The storage declarations are broken into two categories:
* User-declared bindings have a 1-to-1 relationship with the variables specified in the program.
* Temporaries are introduced by the compiler in various cases.

Each basic block has an id and consists of a sequence of statements and a terminator.
```
BASIC_BLOCK = BB: {STATEMENT} TERMINATOR
```
A STATEMENT can have one of three forms:
```
STATEMENT = LVALUE "=" RVALUE                 // assign rvalue into lvalue
          | SetDiscriminant(Lvalue, Variant)  // Write the discriminant for a variant to the enum Lvalue
          | StorageLive(Lvalue)   // Start a live range for the storage of the local
          | StorageDead(Lvalue)   // End the current live range for the storage of the local
```
The TERMINATOR for a basic block describes how it connects to subsequent blocks:
```
TERMINATOR = GOTO(BB)              // normal control-flow
           | IF(LVALUE, BB0, BB1)  // test LVALUE and branch to BB0 if true, else BB1
           | SWITCH(LVALUE, BB...) // load discriminant from LVALUE (which must be an enum),
                                   // and branch to BB... depending on which variant it is
           | CALL(LVALUE0 = LVALUE1(LVALUE2...), BB0, BB1)
                                   // call LVALUE1 with LVALUE2... as arguments. Write
                                   // result into LVALUE0. Branch to BB0 if it returns
                                   // normally, BB1 if it is unwinding.
           | Return                // return to caller normally
           | Drop                  // Drop the Lvalue
           | DropAndReplace        // Drop the Lvalue and assign the new value over it
           | Assert                // Jump to the target if the condition has the expected value,  otherwise panic with a message and a cleanup target.
           | Resume                // the landing pad is finished and unwinding should continue
           | Unreachable           // Indicates a terminator that can never be reached
```
An LVALUE represents a path to a memory location. This is the basic "unit" analyzed by the borrow checker.
```
LVALUE = B                   // reference to a user-declared binding
       | TEMP                // a temporary introduced by the compiler
       | ARG                 // a formal argument of the fn
       | STATIC              // a reference to a static or static mut
       | RETURN              // the return pointer of the fn
       | LVALUE.f            // project a field or tuple field, like x.f or x.0
       | *LVALUE             // dereference a pointer
       | LVALUE[LVALUE]      // index into an array (see disc. below about bounds checks)
       | (LVALUE as VARIANT) // downcast to a specific variant of an enum
```
An RVALUE represents a computation that yields a result. This result must be stored in memory somewhere to be accessible.
```
RVALUE = Use(LVALUE)                // just read an lvalue
       | [LVALUE; LVALUE]
       | &'REGION LVALUE
       | &'REGION mut LVALUE
       | LVALUE as TYPE
       | LVALUE <BINOP> LVALUE
       | <UNOP> LVALUE
       | Struct { f: LVALUE0, ... } // aggregates, see section below
       | (LVALUE...LVALUE)
       | [LVALUE...LVALUE]
       | CONSTANT
       | LEN(LVALUE)                // load length from a slice, see section below
       | BOX                        // malloc for builtin box, see section below
BINOP = + | - | * | / | ...         // excluding && and ||
UNOP = ! | -                        // note: no `*`, as that is part of LVALUE
```
We could also expand the scope of operands to include both lvalues and some simple rvalues like constants.
Constants are a subset of rvalues that can be evaluated at compilation time:
```
CONSTANT = INT
         | UINT
         | FLOAT
         | BOOL
         | BYTES
         | STATIC_STRING
         | ITEM<SUBSTS>                 // reference to an item or constant etc
         | <P0 as TRAIT<P1...Pn>>       // projection
         | CONSTANT(CONSTANT...)        //
         | CAST(CONSTANT, TY)           // foo as bar
         | Struct { (f: CONSTANT)... }  // aggregates...
         | (CONSTANT...)                //
         | [CONSTANT...]                //
```

## Nighly rust
what is suggested a generic closure should desugar to.
```
#![feature(unboxed_closures)]
#![feature(fn_traits)]

use std::ops::{FnOnce, FnMut, Fn};

struct Id;

impl<T> FnOnce<(T,)> for Id {
    type Output = T;
    extern "rust-call" fn call_once(self, (t,): (T,)) -> T { t }
}

impl<T> FnMut<(T,)> for Id {
    extern "rust-call" fn call_mut(&mut self, (t,): (T,)) -> T { t }
}

impl<T> Fn<(T,)> for Id {
    extern "rust-call" fn call(&self, (t,): (T,)) -> T { t }
}

fn main() {
    let id = Id;
    println!("{}", id(4));
    println!("{}", id(true));
    println!("{}", id("hello"));
}
```

Immutable data structures are inherently tree-like. It is a pain to work with graph-like structures and you have to give up some guarantees the languages give you.
