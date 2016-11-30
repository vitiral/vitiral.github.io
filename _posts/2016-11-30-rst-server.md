# rst 0.3 released, now with a web ui and windows, linux and mac osx binaries!

I have just released the 0.3beta release of [rst](https://github.com/vitiral/rst/releases),
the requirements tracking tool made for developers. This release brings many benefits,
including support for all major platforms and a web-ui for viewing requirements. In
addition, I have a clear path forward for how to make the web-ui be able to edit
the requirements as well.

There are many points I would like to cover in this post, in summary they are:
- rst's licensing changes
- learning a new programing langugage to write the front end in, [elm](http://elm-lang.org/)
- using [nickel.rs](https://github.com/nickel-org/nickel.rs) and the
    [jsonrpc_core](https://github.com/ethcore/jsonrpc-core) rust libraries
    for the backend
- packaging the static html files directly into the released rust binary

**rst**'s primary goal is to be *simple* -- to make it easy to track requirements and
integrate into development tools. It will always run locally on your machine

## Change to a copy-left license
Just before the 0.3 release I switched rst's license from the MIT permissive
license to the LGPLv3+ copy-left license and I would like to talk a little bit
about why.

rst is an application tool that I strongly feel developers need. Requirements
tracking and detailed design are core pillars to effective software development,
and yet there are really no tools to express these ideas that improve a
developer's *day to day* work. In summary, there is nothing like git for
requirements tracking.

But there should be! What are requirements but a name,
a piece of text and links to other requirements? This is a simple problem with
a simple solution, but the industry has not pursued simplicity -- all we've
gotten is large web tools with database backends.

By having your requirements be simple text files that live in your code,
you get another benefit -- the requirements can easily link **to** your code
and be revision controlled **with** your code. This allows you to integrate
your requirements with tools you already use for tracking your code development
(github, Atlassian, etc).

Because rst's very design is to enable simplicity, it was important to me
that the core library always remain open source. That is why I chose LGPL, it
allows propriety companies to use rst as a library and build on it, but if
they want new features then they need to share. Free developer tools should
remain free and companies that want to improve them should contribute.

On the other hand, I fully support using rst within a larger application,
and that is why I am using the "Lesser" GPL which allows others to build
on rst and why the format for the artifact files are public domain.

## Learning Elm
The rst backend and cmd line tool are written in
[rust](https://www.rust-lang.org/en-US/) a systems programming language that
runs blazingly fast, prevents segfaults, and guarantees thread safety. There
really is no replacing rust -- it is an amazing language for the backend.
The safety guarantees make runtime errors practically impossible.

Elm is a functional programming language designed from the ground up to make
WebUIs that are maintainable and fun to program. I have to say they accomplished
all those goals, and learning elm after learning rust was pretty much a matter
of just learning new syntax. Many of the concepts that rust taught such as
[expressions returning a value](http://rustbyexample.com/expression.html)
and [pattern matching](http://rustbyexample.com/flow_control/match.html)
were utilized for exactly the same benefit in elm.

A frontend in elm with a backend in rust is a match made in heaven: fun,
performant and safe.

## Rust web development
Before I got too far, I would like to give a shoutout to
[nickel.rs](https://github.com/nickel-org/nickel.rs) and
[jsonrpc_core](https://github.com/ethcore/jsonrpc-core). They made developing
a json-rpc backend about as easy as anyone could ask for.

What was really fun was learning how to package an elm/node app into
a rust binary -- it turned out to be surprisingly easy! I used
[just](https://github.com/casey/just) to create a simple makefile which
compiles my node/elm frontend into a tar file that I included in the source.
I then then use the `include_bytes!` macro to include the tar file directly in
the rust binary during the build. I then followed the directions on
[rust-everywhere](https://github.com/japaric/rust-everywhere) to compile
rust for linux, mac and windows. It was a pain figuring out how to
package the node-app in a *cross platform* manner, but other than that
it went really smoothly!

With nickel, hosting was as simple as
`server.utilize(StaticFilesHandler::new(&tmp_dir))`, where `tmp_dir` was
just the unpacked [tarfile](https://github.com/alexcrichton/tar-rs)
in a temporary directory created by
[tempdir](https://github.com/rust-lang-nursery/tempdir). Big shoutout to
[alexcrichton](https://github.com/alexcrichton), along with your
[toml](https://github.com/alexcrichton/toml-rs) library I have used an
incredible number of your tools for core development needs -- you have
done an incredible amount to make rust useful for developing complex
applications!

If you want more details, check out the source code at
https://github.com/vitiral/rst.

## Summary
Huge steps have been made in the development of rst, with a read-only web-ui
deployed, and a fully supported one which includes editing just around the
corner. I simply cannot be thankful enough to the wonderful people who have made
rust and elm what they are today. These languages make development easy and
*fun*, and anything that I can do to support them I will do.
