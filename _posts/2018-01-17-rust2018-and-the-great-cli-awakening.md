# #Rust2018 and the Great CLI Awakening
This is a response to the [#Rust2018][rust2018] call for blog posts with a little bit of my
experience and how I see the 2018 year moving forward.


## The Experience of Writing a CLI in Rust
> **spoiler**: simultaniously refreshing and painful

Rust is a fantastic language for writing a Command Line Application (CLI). For the
ergonomics of hacking, it has one of the best [argument parsers
ever][structopt], has seriously the [best serialization library ever][serde]
and it compiles to almost any target and goes _fast_ when it runs.

For testing and robustness, its type system prevents you from making stupid
errors, hooking into [fuzz-testing][proptest] is trivial, and there are [better
and better][assert_cli] libraries for testing CLI applications. It is simply
a fantastic language and will be my goto for writing CLIs from now on (and I
come from python!).

However, there are some serious difficulties. The std library is small, and while
adding functionality from crates with [cargo-edit][cargo-edit] is a simple
`cargo add` away, there are *so* many crates that make up a typical CLI, each
one requring you to add `extern crate foo` and `use foo` in every file you want
to use it.

Probably the most significant paper cut is that rust's error handling semantics
of `Result<T, E>` don't work well for the CLI use case. In most cases a CLI
works like a compiler: I want to show the user _all_ the things that are wrong,
not fail on the first error I find! There needs to be a standard crate for
handling a "stream of errors and warnings", probably via a channel, which I can
query "were there errors in the last function?".


## Filling the Gaps
I have a strong desire to fill the gaps that exist in the ecosystem today. I am
currently doing a complete rewrite of my command line and web application
[artifact][artifact], and am using that use case to "fill the holes" that I see.

Here are some of the things I am working on to fill the gaps. I would like
help from the community as well.


### Gap 1: Working with Paths
I found working directly with paths to be *extremely annoying*. Does my path
exist? Is it equal to this other path? Is my path a file or a directory and why
can't a type tell me? Why is creating/writing/appending a string to my path not
possible in a single line of code?

To solve this I wrote the crate [path_abs][path_abs]. It exports types which
guarantee that all paths exist and are absolute. Even better, it has `PathFile`
and `PathDir` types with commonly used methods attached to them.

Oh and did I mention that you can serialize the paths with serde? That was
one of my biggest pain points -- if I have a path I can't stick it in a struct
which I want to send anywhere. This library solves that and more.

This was one of my biggest pain points developing a CLI in rust, and it was
filled (by me...) in early 2018 -- so the year is already looking good for
CLI ergonomics!


### Gap 2: writing tests
Rust makes it extremely simple to write _unit tests_, but once you want to
do a more end-to-end test there isn't a lot out there. Unfortunately, testing
a CLI application requires end to end tests to be confident you are giving
the user the experience you expected.

If all your text is plain (not colored or formatted) then
[assert_cli][assert_cli] is the crate for you. However, the second you put
style or color -- or even a table -- in your application it becomes a tedious
affair of editing a string of raw bytes every time you make a minor change to
your application.

I'm currently working on a CLI testing _framework_ built in rust, using the new
crate I wrote called [termstyle][termstyle] to aid in being able to easily
express your styles (when you write your app) and then make it easy to test
them as well. The goal is to be able to write your CLI tests in a simple YAML
file and get clear diffs against the expected vs result. One of the benefits:
while you are writing your tests there is no compilation time if you are not
touching your source code -- and running the tests takes almost no time at all!

> Shoutout to [pretty_assertions][pretty_assertions] for making reading diffs
> in regular rust tests SO much easier. This is a life saver for CLI
> applications!
>
> Also shoutout to the [`"{:#?}"`][pretty_print] pretty printing formatter.  When
> I discovered it I felt like the largest pain point of rust just vanished.


### Gap 3: working with errors
This one is a little bit more difficult and I am currently trying out solutions
in my rewrite of artifact.

The basic idea is this:
- Instead of returning errors, I get passed a [`Sender`][sync_sender] object
  where I push errors/warnings/info to that may or may not end up getting
  displayed to the user. (note: I use a sender so that multithreading is
  trivial)
- If I don't get any useful work done, I return `None`, but there might have
  been _several_ errors that contributed to that instead of only one.
- The function above me checks for errors and can choose to exit early.
  At the highest levels, the errors are sorted and displayed to the user.

What I would like to do is leverage something like [slog][slog] for this
purpose. `slog` has a similar design: it wants you to pass a "log sender"
into every function/struct and log to that. What I need is a way to *query*
what logs have been passed (in particular whether they are errors). More
investigation is necessary, but I (personally) found slog very difficult to get
running. Maybe simply improving `slog`'s ergonomics could fill this gap and
simultaniously provide built-in logging?


### Gap 4: bringing it all together
The fourth major gap is the ergonomics _of actually using_ the ecosystem. It is
often stated that rust's ecosystem is immature. While this is somewhat true,
the real issue is in _finding_ and _using_ the pieces you need.

My final major project this year (along with artifact itself) is to flush out
the [stdcli][stdcli] crate. The goal (unlike the similar crate [stdx][stdx]) is
to have a _specific use case in mind_ -- which is to create a Command Line SDK.

I have made significant progress already, and will flush out the first release
as I rewrite artifact, but the primary goal is to reduce the number of `use foo`
and `external crate foo` statements that have to be made (especially regarding
traits), by providing a _standard prelude_ for developing CLI applications. The
idea is that developing a CLI using stdcli should almost feel like a completely
different language. Everything you need should be directly within reach, and
the only crates you need to add should be application specific.

Unlike stdx, I plan on using the `>=X.Y.Z` syntax for all dependencies. I then
plan on setting up something similar to Crater. If your application depends on
stdcli and you have a solid set of tests then you can have your project run
automated tests with it's dependencies continuously updated. This will give
you a feeling of security that you can stay on the cutting edge, because stdcli
will be detecting any regressions and opening bugs with the relevant libraries.


## Gap 5 (stretch): Distributing Your Application
This one is a bit of a stretch, but I would like to have a simple way to
distribute applications written in artifact that doesn't require me to maintain
a build systems on Travis and Appveyor for 3 platforms. I am seriously looking
at the Nix project as the savior to all my woes. I don't have much more to say
except that the rust team's goal of making it easier to integrate cargo into
build system is _much_ appreciated.


## Conclusion
I think CLI applications are (perhaps surprisingly) one of rust's strong suites,
and it needs only a few relatively minor tweaks to make it the best language
for developing CLIs bar none.

The introduction of webassembly and being able to write client-side web
applications will only make this case *stronger*. Imagine being able to expose
a CLI application that can launch a static (or dynamic+backend) webpage
whenever that makes sense. Imagine `ps` (for monitoring processes), but you
could do `ps serve` and it would pop open a dynamically updating processor
status in your browser, with complex searching and graphing functionality built
in.

Rust will push this frontier. We are going to see more and more hybrid CLI/Web
applications being built every day and rust could be the language of choice. I
think it will be, and I think #Rust2018 will be what turns the tide.

[stdx]: https://github.com/brson/stdx
[structopt]: https://github.com/TeXitoi/structopt
[walkdir]: https://github.com/BurntSushi/walkdir
[std_prelude]: https://github.com/vitiral/std_prelude
[stdcli]: https://github.com/vitiral/stdcli
[termstyle]: https://github.com/vitiral/termstyle
[fern]: https://github.com/daboross/fern
[assert_cli]: https://github.com/killercup/assert_cli
[proptest]: https://github.com/AltSysrq/proptest
[cargo-edit]: https://github.com/killercup/cargo-edit
[artifact]: https://github.com/vitiral/artifact
[path_abs]: https://github.com/vitiral/path_abs
[serde]: https://github.com/serde-rs/serde
[rust2018]: https://blog.rust-lang.org/2018/01/03/new-years-rust-a-call-for-community-blogposts.html
[pretty_assertions]: https://github.com/colin-kiegel/rust-pretty-assertions
[sync_sender]: https://doc.rust-lang.org/std/sync/mpsc/struct.Sender.html
[slog]: https://github.com/slog-rs/slog
[pretty_print]: https://doc.rust-lang.org/std/fmt/#sign0

