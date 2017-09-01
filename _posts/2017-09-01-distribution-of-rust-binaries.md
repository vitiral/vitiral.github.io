# Rust ergonomics 101: fast and easy access to the tools

## The Problem
One of the pain points often mentioned around rust is the long compilation
times. While this is certainly an issue I would like addressed, it is often
over-hyped in my opinion when talking about developing in rust.

However, when trying to download applications distributed in rust, the compile
times become a *huge pain* since the standard as of today seems to be to
distribute applications via cargo. `cargo install <crate>` requires the full
download and compilation of all crate dependencies, which can easily take more
than half an hour for several applications.

This is unnacceptable in my opinion, and it really harms rust as a language
that tools like `rustfix`, `cargo-edit`, etc have to be installed through
cargo. When I want to create a fake virtual env I require all my contributors
to have these in their local path -- but setting up all these tools takes
upwards of 40 minutes on my laptop (!).

## One possible solution: 0install
The solution is simple: rust allows extremly easy cross compilation and
deployment of binaries through the amazing template [trust][0] which uses
[cross][1]. In the future I would like to make a script for setting up this
template, but for now it works quite well and [many][2] [rust][3] projects use
it.

However, distributing a binary is not enough -- there needs to be an easy way
to install it for any platform -- whether that be included in a build script,
a "virtual environment", or a user's desktop. I thought about going out and
creating such a tool and then I came accross [0install][4].

0install relies on only a *webpage* for knowing how to install software.
The webpage simply needs to include the xml data of the binaries and
dependencies -- and *that's it*.

I like the design philosophy of 0install, but I had an impossible time
*actually installing it* on my arch linux system. There is an [open bug][5]
about this. In addition, I opened [another one][6] requesting a pre-compiled
binary. Since 0install is written in OCaml, I don't know how likely
this is to be fulfilled.

## Another Solution: where rust can shine
Distributing an easy to install binary is an area where rust can really shine.
I [already distribute artifact][2] as a pre-compiled binary and it is
works really well.

So this blog post might be the lanch of my next project: a simple binary
package manager written in rust. The requirements are similar to
0install but I am trying to make it even simpler. I call the tool
wpkg ("web package", like wget but for package installation). I've started
the initial design approach and would love to know who is interested
and if anyone might want to join the effort.

https://github.com/vitiral/wpkg

If you have an opinion (either positive or negative) please
[open an issue](https://github.com/vitiral/wpkg/issues) and let me know!

[0]: https://github.com/japaric/trust
[1]: https://github.com/japaric/cross
[2]: https://github.com/vitiral/artifact
[3]: https://github.com/BurntSushi/ripgrep
[4]: http://0install.net/
[5]: https://github.com/0install/0install/issues/58
[6]: https://github.com/0install/0install/issues/59
