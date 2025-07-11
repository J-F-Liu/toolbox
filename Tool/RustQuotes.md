            +----------------+
            | blub blub blub |
            +----------------+
            /
    ≽(◕ ᴗ ◕)≼

# Who "owns" Rust ?

From: https://www.reddit.com/r/rust/comments/10z6qnu/comment/j826t4r/

1. The Rust foundation, which is a nonprofit general (delaware) corporation with bylaws, employees, a normal legal existence. It owns the trademarks and domain names, acts as a legal and administrative point of contact when one is needed, and has I think operational and funding responsibility for infrastructure (crates.io, CI, etc.) The foundation has members which are almost all corporate sponsors who donate money (and sometimes people) to further its mandate. There's a fairly broad set of companies involved here: Microsoft, Google, Amazon, Meta, Huawei, etc. etc.

2. The Rust project which is an informal (not a legal entity) collection of people (some employed by companies, some volunteers) who self-organize into teams (that mostly select their own future membership) who collaborate over the internet, each with their own area of responsibility. They have their own processes and governance system (which they're currently in the process of fairly significantly overhauling after the previous generation of processes .. encountered some problems). The project is in practice responsible for all technical decision-making, in terms of what goes into the language, how the tools and processes work, what content is in the docs, what features to change and when, etc. etc. It's where a decision like "we should put telemetry in the compiler" would be made, not the foundation.

The project does not take direction from the foundation, nor does the foundation take direction from the project. They exist in parallel and are mostly-separate entities with mostly-separate areas of responsibility, though there are several people who have roles in both. So far, as far as I know, there has not been any serious conflict between them, and it's not entirely clear to me what would happen if there was. But the foundation's mandate is to support the project, so it'd be a somewhat odd situation if they do find themselves in conflict.

See also: https://www.rust-lang.org/governance

# What people say about Rust

> Rust is the first programming language I've encountered in years that makes me excited. And not just normal excited: irrationally excited. Like the kind of excitement you have for something when you are naive about its limitations and don't know any better (like many blockchain/cryptocurrency advocates). I feel like the discovery of Rust is transporting me back to my younger self, before I discovered the ugly realities of how computers and software work, and is giving me hope that better tools, better ways of building software could actually exist. To channel my inner Marie Kondo: Rust sparks joy.
>
> When I started learning Rust in earnest in 2018, I thought this was a fluke. It is just the butterflies you get when you think you fall in love, I told myself. Give it time: your irrational excitement will fade. But after using Rust for ~2.5 years now, my positive feelings about it have only grown stronger.
> --[Rust is for Professionals](https://gregoryszorc.com/blog/2021/04/13/rust-is-for-professionals/)

> Perlism: you'll only learn the language once, you'll use it for the rest of your life.

> Rust is one of only a handful of programming languages that make coding a really enjoyable experience for me: an expressive type system, strong memory-safety guarantees without a GC, high performance, and an awesome community. Whenever I can come up with an excuse to use Rust, I do just that — and this is no different.
> -- [writing-aws-lambda-functions-in-rust](https://silentbyte.com/writing-aws-lambda-functions-in-rust)

> When I am writing C++, I'm thinking about undefined behavior and bugs the compiler won't catch. When I'm writing Rust, I'm thinking instead about how to optimize things or add features. There is a very real cognitive load difference and it makes me more productive.
> -- [Rewriting m4vgalib in Rust](http://cliffle.com/blog/m4vga-in-rust/)

> Rust is an intriguing language for me. It closely resembles C++ in many ways, hitting all the right notes when it comes to compilation and runtime model, type system and deterministic finalization, that I could not help but get a little excited about this fresh new take on language design. I have spent almost every waking moment over the last few months (that I’m not hanging out with my family) exploring, studying, and experimenting with Rust.
> -- [My Rust adventure begins](https://kennykerr.ca/2019/11/05/rust/)

> The beauty of programming language design is not building the most complex edifice like Scala or making the language unacceptably crippled like Go - but giving the programmer the ability to represent complex ideas elegantly and safely. Rust really shines in that regard.
> -- [My favorite rust function](https://blog.jabid.in/2019/10/11/drop.html)

> Many programming languages start from a specific niche (like R and statistics, or Maple and mathematics) and grow into larger languages over time. While Rust goal is not to be a scientific language, its focus on being a general purpose language allows a phenomenon similar to what happened with Python, where people from many areas pushed the language in different directions (system scripting, web development, numerical programming...) allowing developers to combine all these things in their systems.

> I love the language. And I actually feel like I’m more productive in it than in other languages.

> "I REALLY want to use this for everything, even if it isn't the best tool for the job."

> Discord has never been afraid of embracing new technologies that look promising. For example, we were early adopters of Elixir, React, React Native, and Scylla. If a piece of technology is promising and gives us an advantage, we do not mind dealing with the inherent difficulties and instability of the bleeding edge. This is one of the ways we’ve quickly reached 250+ million users with less than 50 engineers. Embracing the new async features in Rust nightly is another example of our willingness to embrace new, promising technology.

> I've gotten farther than I thought in a short time. That is a testament to Rust. I absolutely could not have done what I've done so far with C++. I still would be debugging core-dumps. The Rust compiler ensures that my code is safe and can't crash. That said, I still fight a lot with the borrow checker. I suspect the problem isn't the language but that I'm using it wrong. After too many years of using garbage collected languages I don't pay enough attention to where and when I allocate resources.
> – [Building a Rust Web Browser](https://joshondesign.com/2020/03/10/rust_minibrowser)

> However, don’t go around and ask other people to rewrite their projects in Rust (don’t be rude) or think that your own rewrite is magically going to be much faster and less buggy than the existing implementation. While Rust saves you from a big class of possible bugs, it doesn’t save you from yourself and usually rewrites contain bugs that didn’t exist in the original implementation. Also getting good performance in Rust requires, like in every other language, some effort. Before rewriting any software, think about the goals of this rewrite realistically as well as the effort required to actually get it finished.

> Sometimes, our goal isn’t really to write perfect code that is performant, correct, handles all kinds of errors sanely, has great UX and is maintainable. These projects are what we are proud of, sure.
> But sometimes we just need to throw something together really fast and don’t care about the quality as much.

> I haven’t been using Rust for production much; maybe a bit more than a year. The static type checks means I’m getting much less bugs in my code, and spend considerably less time in debugging. I can safely say that, for me, Rust is more productive than JavaScript, PHP or Python and the margin keeps getting larger as I get more acquainted with the ecosystem.
> [Rust as a productive high-level language](https://omarabid.com/rust-high-level-language)

> I used to think that the reason I loved both Haskell and Rust so much was their shared strong typing, ADTs, and pattern matching combination.
> After a recent discussion, I think it may be more about being expression-oriented languages.
> [Michael Snoyman](https://twitter.com/snoyberg/status/1348486654017855489)

> Instead of relying on declare-then-assign patterns, both languages allow conditionals and other constructs to evaluate to values. This reduces the frequency of seeing mutable assignment and avoids cases of uninitialized variables. By restricting mutable assignment to cases where it's actual mutation, we get to free up a lot of head space to focus on the trickier parts of programming.
> Since I started using Haskell, I feel strongly hampered using any language without a rich, flexible, and powerful type system. Rust's embrace of Algebraic Data Types (ADTs) feels natural.
> I see both languages embracing the idea that encouraging programmers to define and use strong typing mechanisms leads to better code. And it's a message I wholeheartedly endorse.
> Rust has fully embraced the concepts of using better approaches to solve problems, and to steal great ideas that have been tried and tested. We believe Rust has the potential to drastically improve software quality in the world, and lead to more maintainable solutions.
> As we're engineers who like playing with shiny tools, if someone wants to have extra fun, Rust is usually it.

> I have to say, the Rust bindings to GTK are much more ergonomic than the C/C++/c#. I think there's something in Rust that makes many implementations much more ergonomic than their C counterpart.
> [Rust GUI](https://dev.to/davidedelpapa/rust-gui-introduction-a-k-a-the-state-of-rust-gui-libraries-as-of-january-2021-40gl)

> As a long-time C++ programmer, and C++ instructor, I am convinced that Rust is better than C++ in all of C++'s application space, that for any new programming project where C++ would make sense as the programming language, Rust would make more sense.
> [Jimmy Hartzell](https://www.thecodedmessage.com/posts/hello-rust/)

> Rust is not just a safer alternative to C++, but, as I continue to argue, unsafe Rust is a better unsafe language than C++.
> [Jimmy Hartzell](https://www.thecodedmessage.com/posts/cpp-move/)

> Rust is the first language that can reliably do everything well.
> When you have 5 different languages... you inevitably implement the same things 5 times. With a single language, you can have a single library of software packages that can be used across all the teams of the organization.
> [Rust is minimalist](https://kerkour.com/rust-is-minimalist)

> So, assuming I’ve convinced you that Rust has a better organized feature-set than C++, we have to discuss what, in the big picture, C++ has done wrong and Rust has done right.
>
> The first and most obvious thing Rust did right was learn from the mistakes of the past. Each new version of C++ has to be compatible with previous versions to a great extent, including (in a lot of ways) C, giving it a legacy back into the early 70’s. Rust started maintaining compatibility in 2015, and so it’s only had 7 years or so of cruft, but knew about all of C++’s later add-ons from the beginning.
>
> And one of the things Rust learned from the experience of others is how to mitigate this effect, so we can hope Rust retains its youthful freshness for longer going forward. Rust has an edition system, so that features actually can be deprecated and phased out, while still maintaining compatibility.
> https://www.thecodedmessage.com/posts/2022-05-11-programming-multiparadigm/

> From now it should be completely understandable why Rust feels like a full of holes from time to time; in fact, it is almost a miracle that it is functioning at all. A computer language is like a system of tightly intertwined components: every time you introduce a new linguistic abstraction, you have to make sure that it plays well with the rest of the system to avoid bugs and inconsistencies.
> https://hirrolot.github.io/posts/rust-is-hard-or-the-misery-of-mainstream-programming.html

> The hype is real. After writing Rust code for sometime I felt like I want to rewrite everything in Rust.
> [creator of Rust Explorer](https://www.rustexplorer.com/b/about)

> The soul of Rust, to my mind, is definitely not being explicit about allocation. Rather, it’s about the struggle between a few key values — especially productivity and versatility1 in tension with transparency. Rust’s goal has always been to feel like a high-level but with the performance and control of a low-level one. Oftentimes, we are able to find a “third way” that removes the tradeoff, solving both goals pretty well.
> https://smallcultfollowing.com/babysteps/blog/2022/09/19/what-i-meant-by-the-soul-of-rust/

> There’s something about Rust that makes it uniquely fun to write. It feels like a wooden jigsaw puzzle: sometimes tedious, sometimes frustrating, but always immensely satisfying when the last piece drops into place. Other languages can feel more like soggy cardboard puzzles where the pieces need to be forcibly smooshed together and you’re never 100% sure if you got it right.
> https://medium.com/source-and-buggy/data-driven-performance-optimization-with-rust-and-miri-70cb6dde0d35

> Rust makes it possible for a small team to build a complex product quickly, and Zed wouldn't have been possible without it. In the past, to write software this performant, you would need to use C++. Rust, for the first time, enables us to write software at that level as a very small team.
> https://zed.dev/tech

> Rust is in a sweet spot in the language design space. It allows us to build efficient and memory-safe programs with concise, portable, and sometimes even pretty code.
> It is almost always a bug to modify the contents of an object if some other part of the program references that object.
> The value of Rust that I like the most is its focus on local reasoning. Looking at the function's type signature often gives you a solid understanding of what the function can do. State mutations are explicit thanks to mutability and lifetime annotations. Error handling is explicit and intuitive thanks to the ubiquitous Result type. When used correctly, these features often lead to the mystical if it compiles—it works effect.
> https://mmapped.blog/posts/15-when-rust-hurts.html

> It is true that Rust is designed to replace C++, and that Rust has a borrow checker, and that the borrow checker is necessary in order to safely perform the kind of memory management people are used to in C++. However, that doesn’t mean that the borrow checker is only useful or necessary when doing C++, or that it isn’t useful in a GC’ed language. Memory management is the least interesting application of borrow checking.
> https://blog.polybdenum.com/2023/03/05/fixing-the-next-10-000-aliasing-bugs.html

> Rust is vertically scalable, in that you can write all kinds of software in it. You can write an advanced zero-alloc image compression library, build a web server exposing the library to the world as an HTTP SAAS, and cobble together a “script” for building, testing, and deploying it to wherever people deploy software these days. And you would only need Rust — while it excels in the lowest half of the stack, it’s pretty ok everywhere else too.
> https://matklad.github.io/2023/03/28/rust-is-a-scalable-language.html

[developer’s praises for Rust](https://brson.github.io/fireflowers/)

> I used to believe the differences between popular programming languages are mostly aesthetic, but learning Rust convinced me it represents something new in a way that will have a meaningful impact on how technology affects everyone in the digital age. —— Jon Bauman

> Building web services in Rust is pleasant (I've been doing it since a pretty long time now) because they are just so much more robust than the ones in any other programming language. The other thing that I really enjoy is when you come back to your code 6+ months later, you can directly understand the intents behind each line of code thanks to Rust's expressiveness: Option, mut and the ownership rules, which is not true in languages that use pointers to express both references and possible nullity.
> https://kerkour.com/rust-http-ecosystem-2024

> There is a certain spectrum of Rust:
    - Sloppy Rust, which allocates and clones left-and-right.
    - Normal Rust, which opportunistically uses pretzels and avoids gratuitous allocations but otherwise doesn’t try to optimize anything specifically.
    - DoD Rust, which thinks a bit about cache-lines, packs things into arenas, uses indexes instead of pointers with an occasional SoA and SIMD.
    - Crazy here-be-dragons Rust with untagged unions, unsafe, inline assembly and other wizardry.
> [On Ousterhout’s Dichotomy](https://matklad.github.io/2024/10/06/ousterhouts-dichotomy.html)

> I’ve talked to a number of people who expected just to use Rust for one thing, say a tail-latency-sensitive data plane service, but they wound up using it for everything. Why? Because it turned out that, once they learned it, Rust was quite productive and using one language meant they could share libraries and support code. 
> https://smallcultfollowing.com/babysteps/blog/2025/03/10/rust-2025-intro/

### 574
The whole point of Rust is that before there were two worlds:

- Inefficient, garbage collected, reliable languages
- Efficient, manually allocated, dangerous languages

And the mark of being a good developer in the first was mitigating the inefficiency well, and for the second it was it didn't crash, corrupt memory, or be riddled with security issues. Rust makes the trade-off instead that being good means understanding how to avoid the compiler yelling at you.

– Simon Buchan on rust-users 

### 568
Writing Rust is pure joy. I can go from an idea to a working, tested, robust, published and packaged implementation in the time it would take me to even begin the first few lines of a C version. The tooling is beautiful, makes programming fun, and the end result usually outperforms the equivalent C.

### 537
My experience with C++ is that, as I’ve become more of an expert in the language, I’ve become more disillusioned with it. It’s incredibly hard to do things that you should be able to do in software. And, it’s a huge problem for me to constantly be helping other engineers debug the same bugs over and over. It’s always another use after free. I’ve probably debugged 300 of those. [...]

In our experience using the Rust ecosystem for almost three years now, I don't think we found a bug in a single Rust crate that we've pulled off the shelf. We found a bug in one of them and that was a Rust crate wrapping a C library and the bug was in the C library. The software quality that you kind of get for free is amazing.

– Carter Schultz interviewed on the filtra blog

### 516

The Rust mission -- let you write software that's fast and correct, productively -- has never been more alive. So next Rustconf, I plan to celebrate:

- All the buffer overflows I didn't create, thanks to Rust
- All the unit tests I didn't have to write, thanks to its type system
- All the null checks I didn't have to write thanks to Option and Result
- All the JS I didn't have to write thanks to WebAssembly
- All the impossible states I didn't have to assert "This can never actually happen"
- All the JSON field keys I didn't have to manually type in thanks to Serde
- All the missing SQL column bugs I caught at compiletime thanks to Diesel
- All the race conditions I never had to worry about thanks to the borrow checker
- All the connections I can accept concurrently thanks to Tokio
- All the formatting comments I didn't have to leave on PRs thanks to Rustfmt
- All the performance footguns I didn't create thanks to Clippy

### 510

In [other languages], I could end up chasing silly bugs and waste time debugging and tracing to find that I made a typo or ran into a language quirk that gave me an unexpected nil pointer. That situation is almost non-existent in Rust, it's just me and the problem. Rust is honest and upfront about its quirks and will yell at you about it before you have a hard to find bug in production.

### 496

I guess the nicest example of this phenomenon is shared mutability. Programmers have been arguing for decades whether it is sharing xor mutability that causes memory safety bugs:

> "It's threads!" – shouted JavaScript and Python, and JS remained single-threaded, and Python introduced the GIL.
> "It's mutability!" – screamed Haskell and Erlang, and they made (almost) everything immutable.

And then along came Rust, and said: "you are fools! You can have both sharing and mutability in the same language, as long as you isolate them from each other."

– H2CO3 on rust-users

### 493

It's as if you've spent an entire career writing assembly, and one day you hear something or other about a brand-new programming language claiming to be a "portable assembler" called C. It sounds too good to be true. And then the years pass, and all of the mystery and disbelief gives way to obviousness and precision engineering. That's sort of how it is when going from C to Rust.

– Jay Oster

### 492

That said, I really like the language. It’s as if someone set out to design a programming language, and just picked all the right answers. Great ecosystem, flawless cross platform, built-in build tools, no “magic”, static binaries, performance-focused, built-in concurrency checks. Maybe these “correct” choices are just laser-targeted at my soul, but in my experience, once you leap over the initial hurdles, it all just works™️, without much fanfare.

– John Austin on his blog

### 483

It’s enjoyable to write Rust, which is maybe kind of weird to say, but it’s just the language is fantastic. It’s fun. You feel like a magician, and that never happens in other languages.

### 471

After many years of writing bugs, and then discovering Rust, I learned to appreciate explicitness in code, and hope you eventually will too.

### 464

All the concurrency bugs just vanish with Rust! Memory gets freed when it needs to be freed! Once you learn to make Rust work with you, I feel like it guides you into writing correct code, even beyond the language's safety promises. It's seriously magic! ✨

### 458

We reached a tipping point. We decided to move our entire codebase to Rust... We all expect[ed] performance and dev processes to improve. Those indeed happened. What we didn’t expect was the extent to which dev velocity increased and operational incidents decreased. Dev velocity ... improved dramatically with Rust.

### 456

Rust is attempting to raise the abstraction in the programming language and ultimately to join computer science and software engineering into one single discipline, an ambition that has been around since these disciplines were created.

## 453

If there’s one lesson from decades of software engineering, it is the failure of “just be careful” as a strategy. C/C++ programmers still experience memory corruption constantly, no matter how careful they are. Java programmers still frequently see NullPointerExceptions, no matter how careful they are. And so on. One of the reasons that Rust is so successful is that it adds automated checks to prevent many common mistakes.

### 442

Rust limits severity of the damage an inexperienced programmer can cause. Once they manage to get the code to compile, it already has lots of correctness guarantees. “Bad” Rust code may just clone more than strictly necessary, or write 10 lines of code for something that has a helper method in the stdlib, but it won’t corrupt memory or blindly run the happy path without checking for errors.

### 390

> You won’t appreciate Rust unless you spend few weeks building something in it. The initial steep learning curve could be frustrating or challenging depending on how you see it, but once past that it’s hard not to love it.
>
> [My second impression of Rust and why I think it's the best general-purpose language!](https://deepu.tech/my-second-impression-of-rust/)

### 377

The main theme of Rust is not systems programming, speed, or memory safety - it's moving runtime problems to compile time. Everything else is incidental. This is an invaluable quality of any language, and is something Rust greatly excels at.

### 376

Having a fast language is not enough (ASM), and having a language with strong type guarantees neither (Haskell), and having a language with ease of use and portability also neither (Python/Java). Combine all of them together, and you get the best of all these worlds.

Rust is not the best option for any coding philosophy, it’s the option that is currently the best at combining all these philosophies.

### 373

> Rust favours security over convenience. Rust does not want you to make silly little mistakes than can waste so much of your time debugging, which in the end makes it more convenient.

### 368

> Writing rust for me is a gradual process of the compiler patiently guiding me towards the program I should have written in the first place, and at the end I take all the credit.
>
> – @felixwatts on Discord

### 362

> what many devs often miss initially when talking about Rust is that it isn't just about the design & details of the language (which is great), Rust's super power is that in combination with its fantastic community & ecosystem, and the amazing friendly people that create & form it.

### 350

> Empowering is the perfect word to describe Rust in 2020. What used to be a rough adventure with many pitfalls has turned into something beautiful, something that can lift your spirit. At least, that’s what it did for me.

[Mathias Lafeldt on his blog](https://sharpend.io/giving-rust-another-shot-in-2020/)

### 339

> The whole motivation behind exceptions is to allow one to write ones business logic, concentrate on what one likes to think ones program will do, without having lots of fiddly error checking and handling code obscuring that logic. Error situations are therefore swept under the carpet with "try" and kept out of sight with "catch".

> However in my world view failure is not exceptional, it is a common happening, it's too important to be hidden away. Therefor failure handling should be in ones face in the code you write. Certainly in the face of those that read it.

– ZiCog on rust-users

## 338

> Ownership is purely conceptual: it is not something you can see in a disassembler.

– Jay Oster on rust-users

## 316

> When I'm writing in Rust, it feels as though I'm actually able to think about the program, rather than wasting half of my effort going through the necessary rituals to stop the language from having a panic attack.
> – [/u/rime-frost on reddit](https://www.reddit.com/r/rust/comments/e8tms0/rust_is_fun/faei257/)

## 288

> I used to think of programs as execution flowing and think about what the CPU is doing. As I moved to rust I started thinking a lot more about memory: how the data was laid out in memory, and how ownership of different parts of memory is given to different parts of the program at run time.

– Oliver Gould on "The Open Source Show: All About Rust

## 272

> I always think of borrow checker as an angel sitting on your shoulder, advising you not to sin against the rules of ownership and borrowing, so your design will be obvious and your code simple and fast.

– llogiq on /r/rust

### 271

> Rust is kind of nice in that it lets you choose between type erasure and monomorphization, or between heap-allocation and stack-allocation, but the downside is that you have to choose.

– Brook Heisler on discord

### 267

> In theory it would be entirely reasonable to guess that most Rust projects would need to use a significant amount of unsafe code to escape the limitations of the borrow checker. However, in practice it turns out (shockingly!) that the overwhelming majority of programs can be implemented perfectly well using only safe Rust.

– PM_ME_UR_MONADS [on reddit](https://www.reddit.com/r/rust/comments/a7kkw9/looking_for_someone_to_change_my_view_on_this/ec3r38n/)

### 266

> Using (traits) for Inheritance was like putting car wheels on a boat because I am used to driving a vehicle with wheels.
>
> – Marco Alka [on Hashnode](https://hashnode.com/post/how-to-become-a-rust-super-developer-cjpv1ee7e000buhs2aqrdw2ym)

### 239

> In Rust it’s the compiler that complains, with C++ it’s the colleagues.

– Michal 'Vorner' Vaner on [gitter](https://gitter.im/rust-lang/rust?at=5b212b1a37a2df7bed398c7c)

## 231

> I’ve become fearless in Rust, but it’s made me fear every other language…

— [u/bluejekyll on reddit](https://www.reddit.com/r/rust/comments/8babua/fearless_rust_bloggers/dx6ay6k/?context=1).

### 230

> Rust is one of those friends that take some time to get along with, but that you'll finally want to engage with for a long term relationship.

— [Sylvain Wallez](https://bluxte.net/musings/2018/04/10/go-good-bad-ugly/).

### 212

> Although rusting is generally a negative aspect of iron, a particular form of rusting, known as “stable rust,” causes the object to have a thin coating of rust over the top, and if kept in low relative humidity, makes the “stable” layer protective to the iron below.

— [Wikipedia on rusting of iron](https://en.wikipedia.org/wiki/Rust).

### 210

> Indeed. I notice even when after some Rust I return to the “main day job” C, I start to think differently, and it is excellent. Rust is like a complement to good diet and exercise.

— [AndrewY on TRPLF](https://users.rust-lang.org/t/solved-what-is-the-best-way-to-dump-sqlite3-row-values-into-sql-text-when-the-table-structure-is-unknown-at-compile-time/14020/7).

### 209

> Rust's abstraction layers feel both transparent and productive. It's like being on a glass-bottomed boat, you see the sharks, but they can't get you.
> It's like a teaching language that you can also use in production. Rust helped me understand C.
> Also Rust people are amazing.

— [@gibfahn on Twitter](https://twitter.com/gibfahn/status/931187143686393863).

### 206

> I feel like I'm doing something wrong because I'm programming faster and nothing has gone wrong yet

— [@0x424c41434b on Rust](https://twitter.com/0x424c41434b/status/923369121844043776).

### 202

> The compiler is grumpy for you so you don’t have to be

— Élisabeth Henry @RustFest Zürich

### 200

> <heycam> one of the best parts about stylo has been how much easier it has been to implement these style system optimizations that we need, because Rust
> <heycam> can you imagine if we needed to implement this all in C++ in the timeframe we have
> <bholley> heycam: yeah srsly
> <bholley> heycam: it's so rare that we get fuzz bugs in rust code
> <bholley> heycam: considering all the complex stuff we're doing \* heycam remembers getting a bunch of fuzzer bugs from all kinds of style system stuff in gecko
> <bholley> heycam: think about how much time we could save if each one of those annoying compiler errors today was swapped for a fuzz bug tomorrow :-)
> <njn> you guys sound like an ad for Rust

— [Conversation between some long-time Firefox developers](http://logs.glob.uno/?c=mozilla%23servo&s=13+Sep+2017&e=13+Sep+2017#c751661).

### 195

> once you can walk barefoot (C), it’s easy to learn to walk with shoes (go) but it will take time to learn to ride a bike (rust)

— [/u/freakhill on Reddit](https://www.reddit.com/r/rust/comments/6srf8h/thoughts_from_a_dumb_person_notes_on_my_threeweek/dlf58jt/).

### 188

> Regarding the C++ discussion, when I started programming the only viable oss version control system was cvs. It was horrible, but better than nothing. Then subversion was created and it was like a breath of fresh air, because it did the same thing well. Then alternatives exploded and among them git emerged as this amazing, amazing game-changer because it changed the whole approach to version control, enabling amazing things.
>
> To me, Rust is that git-like game-changer of systems programming languages because it changes the whole approach, enabling amazing things.

— Nathan Stocks on TRPLF.

### 178

> Rust doesn't end unsafety, it just builds a strong, high-visibility fence around it, with warning signs on the one gate to get inside. As opposed to C's approach, which was to have a sign on the periphery reading "lol good luck".

— Quxxy on reddit.

### 166

> Yeah, it's like learning to dance when your partner [borrow checker] already knows all the steps. When you're just getting started, you step on their toes a lot, but over time you get the motions down. Eventually, you can start to anticipate their movements and start to appreciate the music as part of the dance, instead of just concentrating on getting your feet in the right place.

— QuietMisdreavus on reddit.

### 165

> I really hate the phrase "fighting". Calling it a fight doesn't do justice to the conversations you have with the borrow checker when you use Rust every day. You don't fight with the borrow checker, because there isn't a fight to win. It's far more elegant, more precise. It's fencing; you fence with the borrow checker, with ripostes and parries and well-aimed thrusts. And sometimes, you get to the end and you realize you lose anyway because the thing you were trying to do was fundamentally wrong. And it's okay, because it's just fencing, and you're a little wiser, a little better-honed, a little more practiced for your next bout.

— kaosjester on Hacker News.

### 154

> Every once in a while someone discovers a bug in librsvg that makes it all the way to a CVE security advisory. We've gotten double free()s, wrong casts, and out-of-bounds memory accesses. Recently someone did fuzz-testing with some really pathological SVGs, and found interesting explosions in the library. That's the kind of 1970s bullshit that Rust prevents.

— Federico on porting librsvg to Rust.

### 111

> You have to think about it. You don't have to worry about it.

— SamReidHughes on manual memory management in Rust.
