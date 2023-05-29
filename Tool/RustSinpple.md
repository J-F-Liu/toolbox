- FizzBuzz

```rust
// v1
for i in 1..101 {
    if i % 15 == 0 {
        println!("FizzBuzz");
    } else if i % 5 == 0 {
        println!("Buzz");
    } else if i % 3 == 0 {
        println!("Fizz");
    } else {
        println!("{}", i);
    }
}

// v1.1
for i in 1..101 {
    match (i % 3, i % 5) {
        (0, 0) => println!("FizzBuzz"),
        (_, 0) => println!("Buzz"),
        (0, _) => println!("Fizz"),
        (_, _) => println!("{}", i),
    }
}

// v2
for i in 1..101 {
    let x;
    let result = if i % 15 == 0 {
        "FizzBuzz"
    } else if i % 5 == 0 {
        "Buzz"
    } else if i % 3 == 0 {
        "Fizz"
    } else {
        x = i.to_string();
        &*x
    };
    println!("{}", result);
}

// v3
for i in 1..101 {
    let result: &dyn std::fmt::Display = if i % 15 == 0 {
        &"FizzBuzz"
    } else if i % 5 == 0 {
        &"Buzz"
    } else if i % 3 == 0 {
        &"Fizz"
    } else {
        &i
    };
    println!("{}", result);
}

// v4
use std::borrow::Cow;

fn fizz_buzz(i: i32) -> Cow<'static, str> {
    if i % 15 == 0 {
        "FizzBuzz".into()
    } else if i % 5 == 0 {
        "Buzz".into()
    } else if i % 3 == 0 {
        "Fizz".into()
    } else {
        i.to_string().into()
    }
}

for i in 1..101 {
    println!("{}", fizz_buzz(i));
}

// v5
use std::fmt;

enum Term {
    Fizz,
    Buzz,
    FizzBuzz,
    Number(i32),
}

impl fmt::Display for Term {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        match *self {
            Term::Fizz => f.write_str("Fizz"),
            Term::Buzz => f.write_str("Buzz"),
            Term::FizzBuzz => f.write_str("FizzBuzz"),
            Term::Number(num) => write!(f, "{}", num),
        }
    }
}

impl fmt::Debug for Term {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        fmt::Display::fmt(self, f)
    }
}

fn fizz_buzz(i: i32) -> Term {
    if i % 15 == 0 {
        Term::FizzBuzz
    } else if i % 5 == 0 {
        Term::Buzz
    } else if i % 3 == 0 {
        Term::Fizz
    } else {
        Term::Number(i)
    }
}

for i in 1..101 {
    println!("{}", fizz_buzz(i));
}
```

- Avoiding the Pyramid of Doom

```Rust
// Get field value of deeply nested optional structs
fn f(a: A){
    if let Some(b) = a.b {
        if let Some(c) = b.c {
            if let Some(d) = c.d {
                if let Some(f) = d.field {
                    println!("{}", f)
                }
                else {
                    // log the missing field
                }
            }
        }
    }
}


// chain and_then
if let Some(i) = a
    .b
    .and_then(|b| b.c)
    .and_then(|c| c.d)
    .and_then(|d| d.field)
{
    println!("I: {}", i);
}

// return from closure
if let Some(f) = (|| a.b?.c?.d?.field)() {
    println!("{}", f)
}

// destructing
fn f(a: A){
    if let Some(B { c: Some(C { d: Some(D { field: Some(f), ..}), ..}) , ..}) = a.b {
        println!("{}", f)
    } else {
        // log the missing field
    }
}
```

Rust team recommends only using Deref/DerefMut for smart pointers, to avoid confusion.
But derive Deref/DerefMut in newtype pattern is already widely used in the Rust ecosystem.

```rs
#[derive(Component, Deref, DerefMut)]
struct Items(Vec<Item>);

fn give_sword(mut query: Query<&mut Items>) {
    for mut items in query.iter_mut() {
        // No more .0!
        items.push(Item::new("Flaming Poisoning Raging Sword of Doom"));
    }
}
```

[Converting Integers to Floats Using Hyperfocus](https://blog.m-ou.se/floats/)

```rs
pub fn u128_to_f64(x: u128) -> f64 {
    x as f64
}

pub fn u128_to_f64(x: u128) -> f64 {
    if x == 0 {
        return 0.0;
    }
    let n = x.leading_zeros();
    let y = x << n;
    let exponent = (127 - n) + 1023;
    let mut mantissa = y << 1 >> 76;
    let dropped_bits = y << 53;
    if dropped_bits > 1 << 127 || (dropped_bits == 1 << 127 && mantissa & 1 == 1) {
        mantissa += 1;
    }
    f64::from_bits(((exponent as u64) << 52) + mantissa as u64)
}

pub fn u128_to_f64(x: u128) -> f64 {
    if x == 0 {
        return 0.0;
    }
    let n = x.leading_zeros();
    let y = x << n;
    let exponent = (127 - n) + 1022;
    let mut mantissa = y >> 75;
    let dropped_bits = y << 53;
    if dropped_bits > 1 << 127 || (dropped_bits == 1 << 127 && mantissa & 1 == 1) {
        mantissa += 1;
    }
    f64::from_bits(((exponent as u64) << 52) + mantissa as u64)
}

pub fn u128_to_f64(x: u128) -> f64 {
    if x == 0 {
        return 0.0;
    }
    let n = x.leading_zeros();
    let y = x << n;
    let exponent = (127 - n) + 1022;
    let mut mantissa = (y >> 75) as u64;
    let dropped_bits = (y >> 11 | y & 0xFFFFFFFF) as u64;
    mantissa += (dropped_bits - ((dropped_bits >> 63) & !mantissa)) >> 63;
    f64::from_bits(((exponent as u64) << 52) + mantissa)
}

pub fn u128_to_f64(x: u128) -> f64 {
    let n = x.leading_zeros();
    let y = x.wrapping_shl(n);
    let a = (y >> 75) as u64;
    let b = (y >> 11 | y & 0xFFFFFFFF) as u64;
    let m = a + ((b - (b >> 63 & !a)) >> 63);
    let e = if x == 0 { 0 } else { 1149 - n as u64 };
    f64::from_bits((e << 52) + m)
}
```

- Pseudorandom number generator

```rs
// Pseudorandom number generator from the "Xorshift RNGs" paper by George Marsaglia.
//
// https://github.com/rust-lang/rust/blob/1.55.0/library/core/src/slice/sort.rs#L559-L573
pub fn random_numbers(seed: u32) -> impl Iterator<Item = u32> {
    let mut random = seed;
    std::iter::repeat_with(move || {
        random ^= random << 13;
        random ^= random >> 17;
        random ^= random << 5;
        random
    })
}

pub fn random_seed() -> u64 {
  std::hash::Hasher::finish(&std::hash::BuildHasher::build_hasher(
    &std::collections::hash_map::RandomState::new(),
  ))
}
```

- Direction enum

```rust
#[derive(PartialEq, Copy, Clone)]
enum Direction {
    Left,
    Up,
    Right,
    Down,
}

impl Direction {
    fn opposite(self) -> Self {
        match self {
            Self::Left => Self::Right,
            Self::Right => Self::Left,
            Self::Up => Self::Down,
            Self::Down => Self::Up,
        }
    }
}
```

- use generators/coroutines to transform any recursive function into an iterative function

```rs
fn trampoline<Arg, Res, Gen>(
    f: impl Fn(Arg) -> Gen
) -> impl Fn(Arg) -> Res
where
    Res: Default,
    Gen: Generator<Res, Yield = Arg, Return = Res> + Unpin,
{
    move |arg: Arg| {
        let mut stack = Vec::new();
        let mut current = f(arg);
        let mut res = Res::default();

        loop {
            match Pin::new(&mut current).resume(res) {
                GeneratorState::Yielded(arg) => {
                    stack.push(current);
                    current = f(arg);
                    res = Res::default();
                }
                GeneratorState::Complete(real_res) => {
                    match stack.pop() {
                        None => return real_res,
                        Some(top) => {
                            current = top;
                            res = real_res;
                        }
                    }
                }
            }
        }
    }
}

fn triangular(n: u64) -> u64 {
    if n == 0 {
        0
    } else {
        n + triangular(n - 1)
    }
}

fn triangular_safe(n: u64) -> u64 {
    trampoline(|n| move |_| {
        if n == 0 {
            0
        } else {
            n + yield (n - 1)
        }
    })(n)
}
```

- binary search

```rust
///
/// Perform a binary search on a sorted list
///
pub fn binary_search(k: i32, items: &[i32]) -> Option<usize> {
    if items.is_empty() {
        return None;
    }

    let mut low: usize = 0;
    let mut high: usize = items.len() - 1;

    while low <= high {
        let middle = (high + low) / 2;
        if let Some(current) = items.get(middle) {
            if *current == k {
                return Some(middle);
            }
            if *current > k {
                if middle == 0 {
                    return None;
                }
                high = middle - 1
            }
            if *current < k {
                low = middle + 1
            }
        }
    }
    None
}


use std::cmp::Ord;

pub fn binary_search<T:Ord>(k: &T, items: &[T]) -> Option<usize> {
    if items.is_empty() {
        return None;
    }

    let mut low: usize = 0;
    let mut high: usize = items.len() - 1;

    while low <= high {
        let middle = (high + low) / 2;
        if let Some(current) = items.get(middle) {
            if current == k {
                return Some(middle);
            }
            if current > k {
                if middle == 0 {
                    return None;
                }
                high = middle - 1
            }
            if current < k {
                low = middle + 1
            }
        }
    }
    None
}
```

Improved version:

```rust
use std::cmp::Ordering;

fn binary_search(k: i32, items: &[i32]) -> Option<usize> {
    let mut low: usize = 0;
    let mut high: usize = items.len();

    while low < high {
        let middle = (high + low) / 2;
        match items[middle].cmp(&k) {
            Ordering::Equal => return Some(middle),
            Ordering::Greater => high = middle,
            Ordering::Less => low = middle + 1
        }
    }
    None
}
```

- Triangulation by Ear Clipping

```rust
fn triangulate(&self, edges: &[CurveSegment<Box<dyn Curve>>]) -> TriangleMesh {
    let mut vertices = Vec::new();
    for edge in edges {
        vertices.extend(edge.get_points());
    }

    let points: Vec<Point2> = vertices.iter().map(|v| self.project(*v)).collect();

    // todo: eliminate colinear or duplicate points

    let n = points.len();
    let mut is_dropped = vec![false; n];
    // let mut poly_edges = Vec::with_capacity(vertices.len());
    // for i in 0..vertices.len() {
    //     poly_edges.push(vertices[i + 1] - vertices[i]);
    // }

    let mut is_convex = vec![true; n];
    for i in 0..n {
        match compute_convexity(
            points[(n + i - 1).rem_euclid(n)],
            points[i],
            points[(i + 1).rem_euclid(n)],
        ) {
            Convexity::Concave => is_convex[i] = false,
            Convexity::Colinear => {
                is_convex[i] = false;
                is_dropped[i] = true;
            }
            _ => {}
        }
    }
    dbg!(&is_convex);

    // let is_ear = |prev, i, next| {
    //     let reflex_points =
    //         is_convex
    //             .iter()
    //             .enumerate()
    //             .filter_map(|(i, convex)| if *convex { None } else { Some(i) });
    //     for j in reflex_points {
    //         if j != prev && j != next {
    //             if is_inside_triangle(points[prev], points[i], points[next], points[j]) {
    //                 return false;
    //             }
    //         }
    //     }
    //     return true;
    // };

    let mut triangles = Vec::with_capacity((n - 2) * 3);
    let mut m = n + 1;

    loop {
        let remained_points = is_dropped
            .iter()
            .enumerate()
            .filter_map(|(i, dropped)| if *dropped { None } else { Some(i) })
            .collect::<Vec<_>>();
        if remained_points.len() >= m {
            dbg!(remained_points
                .iter()
                .map(|i| format!("({:.2},{:.2})", points[*i].x, points[*i].y))
                .collect::<Vec<_>>());
            break;
        }
        m = remained_points.len();
        if m < 3 {
            dbg!(m);
            break;
        } else if m == 3 {
            triangles.push(remained_points[0] as u32);
            triangles.push(remained_points[1] as u32);
            triangles.push(remained_points[2] as u32);
            break;
        }
        if let Some(mut i) = remained_points.iter().position(|v| is_convex[*v]) {
            // dbg!(i, m);
            while i < m {
                let curr = remained_points[i];
                let prev = remained_points[(m + i - 1).rem_euclid(m)];
                let next = remained_points[(i + 1).rem_euclid(m)];
                if is_convex[curr] && is_ear(&points, &is_convex, prev, curr, next) {
                    // dbg!(i, curr);
                    triangles.push(prev as u32);
                    triangles.push(curr as u32);
                    triangles.push(next as u32);
                    is_dropped[curr] = true;
                    if !is_convex[prev] {
                        let prev_prev = remained_points[(m + i - 2).rem_euclid(m)];

                        match compute_convexity(points[prev_prev], points[prev], points[next]) {
                            Convexity::Convex => is_convex[prev] = true,
                            Convexity::Colinear => is_dropped[prev] = true,
                            _ => {}
                        }
                    }
                    if !is_convex[next] {
                        let next_next = remained_points[(i + 2).rem_euclid(m)];

                        match compute_convexity(points[prev], points[next], points[next_next]) {
                            Convexity::Convex => is_convex[next] = true,
                            Convexity::Colinear => is_dropped[next] = true,
                            _ => {}
                        }
                    }
                    break;
                }
                i += 1;
            }
        }
    }

    TriangleMesh {
        vertices,
        triangles,
    }
}

enum Convexity {
    Convex,
    Colinear,
    Concave,
}

// check if b is a convex vertex
fn compute_convexity(a: Point2, b: Point2, c: Point2) -> Convexity {
    let product = (b - a).perp_dot(c - b);
    if product.near2(0.0) {
        Convexity::Colinear
    } else if product > 0.0 {
        Convexity::Convex
    } else {
        Convexity::Concave
    }
}

fn is_inside_triangle(a: Point2, b: Point2, c: Point2, p: Point2) -> bool {
    (a - p).perp_dot(b - p) >= 0.0
        && (b - p).perp_dot(c - p) >= 0.0
        && (c - p).perp_dot(a - p) >= 0.0
}

fn is_ear(points: &[Point2], is_convex: &[bool], prev: usize, i: usize, next: usize) -> bool {
    let reflex_points =
        is_convex
            .iter()
            .enumerate()
            .filter_map(|(i, convex)| if *convex { None } else { Some(i) });
    for j in reflex_points {
        if j != prev && j != next {
            if is_inside_triangle(points[prev], points[i], points[next], points[j]) {
                return false;
            }
        }
    }
    return true;
}
```

A pin ownership model

```rust
pub struct Pin {
    port: char,
    index: u8,
    mode: Mode,
}

pub struct Pin<MODE, PORT, PIN> {/* */}

pub struct Pin<MODE, const PORT: char, const INDEX: u8> {
    _marker: PhantomData<MODE>,
}

pub mod typestate {
   pub struct NotConfigured;
   pub struct Input;
   pub struct Output;
}
use typestate::*;

impl<const PORT: char, const INDEX: u8> OutputPin for Pin<Output, PORT, INDEX> {
    fn set_low(&mut self) { unimplemented!() }
    fn set_high(&mut self) { unimplemented!() }
}

impl<const PORT: char, const INDEX: u8> TogglePin for Pin<Output, PORT, INDEX> {
    fn toggle(&mut self) { unimplemented!() }
}

impl<const PORT: char, const INDEX: u8> InputPin for Pin<Input, PORT, INDEX> {
    fn is_high(&self) -> bool { unimplemented!() }
    fn is_low(&self) -> bool { !self.is_high() }
}

impl<MODE, const PORT: char, const INDEX: u8> Pin<MODE, PORT, INDEX> {
    // Private constructor to ensure there only exists one of each pin.
    fn new() -> Self { Self { _marker: Default::default() } }

    pub fn as_input(self) -> Pin<Input, PORT, INDEX> { unimplemented!() }
    pub fn as_output(self) -> Pin<Output, PORT, INDEX> { unimplemented!() }
    pub fn as_disabled(self) -> Pin<NotConfigured, PORT, INDEX> { unimplemented!() }
}
```

- Giving typestates their own namespace——even though it's immediately pulled in——just to have a bit of hygiene when looking at this GPIO module from the outside world.
- Since the only field in the pin is zero-sized, we arrive at the first positive consequence of using typestates: our pin representations don't take any space in the stack (or the heap, if you're fancy enough to afford one, look at you with all that memory to spare!). Any decisions made around these types will ultimately be compiled down to direct writes and reads to the adequate registers, and all trace of the struct will vanish from the final binary.
- A common first concern when dealing with unbounded typestates is that it's very easy to conjure concrete types that don't make sense. Yes, you can name a Pin<Potato, 'W', 42>; the compiler will look at you funny but won't judge. Thankfully there's no way you can ever actually construct one, thanks to the private constructor.
- We've made the `new` function private so we have full control over when it is called. Notice how the user can't simply construct their own pins because of the private PhantomData field, which is doing a great job spooking type thieves away——another of its valuable roles in zero-sized types.

```rust
impl Gpio {
    pub fn claim<const PORT: char, const INDEX: u8>(&mut self)
       -> Option<Pin<NotConfigured, PORT, INDEX>>
    {
        if !self.already_claimed(PORT, INDEX) {
            self.mark_as_claimed(PORT, INDEX);
            Some(Pin::new())
        } else {
            None
        }
    }
}
```

- Set endianness at construction

```rust
use std::io::Result;
use omnom::{ReadBytes, ReadExt};

/// Used to decide whether read_be or read_le is called
#[derive(Clone, Copy)]
pub enum ByteOrder {
  LittleEndian,
  BigEndian,
}

/// Structure that holds our read stream and also the mode (byte order) of reading
pub struct ByteStream<R: ReadExt> {
  order: ByteOrder,
  reader: R,
}

impl <R: ReadExt> ByteStream<R> {

  /// Construct a new ByteStream with a specific byte order (LE or BE)
  pub fn new(reader: R, order: ByteOrder) -> Self {
    Self {
      order,
      reader
    }
  }

  /// Read from the stream with the specified order, overriding the ByteStream order
  pub fn read_with_order<B: ReadBytes>(&mut self, order: ByteOrder) -> Result<B> {
    match order {
      ByteOrder::LittleEndian => self.reader.read_le(),
      ByteOrder::BigEndian => self.reader.read_be()
    }
  }

  /// Read function matches on the current read mode and reads using it (either LittleEndian or BigEndian).
  /// Uses methods from omnom library ReadExt to take from the reader
  pub fn read<B: ReadBytes>(&mut self) -> Result<B> {
    self.read_with_order(self.order)
  }
}


let mut stream = ByteStream(stream, ByteOrder::LittleEndian);
let x: i32 = stream.read()?;
let y: i32 = stream.read()?;
let depth: u8 = stream.read()?;
```

Expressions, initial style

```rs
enum Expr {
    Lit(i32),                   // integer literal
    Neg(Box<Expr>),             // negation
    Add(Box<Expr>, Box<Expr>),  // addition
}
impl Expr {
    fn lit(i: i32) -> Expr {
        Expr::Lit(i)
    }

    fn neg(r: Expr) -> Expr {
        Expr::Neg(Box::new(r))
    }

    fn add(r1: Expr, r2: Expr) -> Expr {
        Expr::Add(Box::new(r1), Box::new(r2))
    }
}
impl Expr {
    fn eval(&self) -> i32 {
        match self {
            Expr::Lit(i) => *i,
            Expr::Neg(r) => -r.eval(),
            Expr::Add(r1, r2) => r1.eval() + r2.eval(),
        }
    }
}
impl Expr {
    fn view(&self) -> String {
        match self {
            Expr::Lit(i) => i.to_string(),
            Expr::Neg(r) => format!("(-{})", r.view()),
            Expr::Add(r1, r2) => format!("({} + {})", r1.view(), r2.view()),
        }
    }
}
fn main() {
    let expr = Expr::add(
        Expr::lit(8),
        Expr::neg(Expr::add(Expr::lit(1), Expr::lit(2))),
    );
    println!("{}={}", expr.view(), expr.eval());
}
```

Expressions, final style
https://getcode.substack.com/p/efficient-extensible-expressive-typed

```rs
trait ExprSym {
    type Repr;

    fn lit(i: i32) -> Self::Repr;
    fn neg(r: Self::Repr) -> Self::Repr;
    fn add(r1: Self::Repr, r2: Self::Repr) -> Self::Repr;
}

struct Eval;
impl ExprSym for Eval {
    type Repr = i32;

    fn lit(i: i32) -> Self::Repr {
        i
    }

    fn neg(r: Self::Repr) -> Self::Repr {
        -r
    }

    fn add(r1: Self::Repr, r2: Self::Repr) -> Self::Repr {
        r1 + r2
    }
}

struct View;
impl ExprSym for View {
    type Repr = String;

    fn lit(i: i32) -> Self::Repr {
        i.to_string()
    }

    fn neg(r: Self::Repr) -> Self::Repr {
        format!("(-{})", r)
    }

    fn add(r1: Self::Repr, r2: Self::Repr) -> Self::Repr {
        format!("({} + {})", r1, r2)
    }
}

fn main() {
    fn expr<E: ExprSym>() -> E::Repr {
        E::add(E::lit(8), E::neg(E::add(E::lit(1), E::lit(2))))
    }
    println!("{}={}", expr::<View>(), expr::<Eval>());
}

// Adding multiplication can be done without changing existing code, and in a type-safe way.
trait MulExprSym: ExprSym {
    fn mul(r1: Self::Repr, r2: Self::Repr) -> Self::Repr;
}

impl MulExprSym for Eval {
    fn mul(r1: Self::Repr, r2: Self::Repr) -> Self::Repr {
        r1 * r2
    }
}

// Do type checking at compile time using the host language Rust
type Fun<A, B> = Box<dyn Fn(A) -> B>;

trait ExprSym {
    type Repr<T>;

    fn int(i: i32) -> Self::Repr<i32>;
    fn add(a: &Self::Repr<i32>, b: &Self::Repr<i32>) -> Self::Repr<i32>;
    fn lam<A, B, F: Fn(Self::Repr<A>) -> Self::Repr<B>>(f: F) -> Self::Repr<Fun<A, B>>
    where
        for<'a> F: 'a;
    fn app<F: Fn(A) -> B, A, B>(f: Self::Repr<F>, arg: Self::Repr<A>) -> Self::Repr<B>;
}

struct Eval;
impl ExprSym for Eval {
    type Repr<T> = T;

    fn int(i: i32) -> Self::Repr<i32> {
        i
    }

    fn add(a: &Self::Repr<i32>, b: &Self::Repr<i32>) -> Self::Repr<i32> {
        a + b
    }

    fn lam<A, B, F: Fn(Self::Repr<A>) -> Self::Repr<B>>(f: F) -> Self::Repr<Box<dyn Fn(A) -> B>>
    where
        for<'a> F: 'a,
    {
        Box::new(f)
    }

    fn app<F: Fn(A) -> B, A, B>(f: Self::Repr<F>, arg: Self::Repr<A>) -> Self::Repr<B> {
        f(arg)
    }
}
```
