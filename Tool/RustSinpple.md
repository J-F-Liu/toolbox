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
