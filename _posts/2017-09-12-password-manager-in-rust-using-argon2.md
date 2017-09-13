# My second CLI application in rust was so fun!

I've had an itch to write a better password manager using my favorite
language (rust), and this last weekend I decided to give it a go.
I've spent the last year writing the [artifact][1] command line
application (which just hit 1.0, check it out!).

I had already written a "vault" password manager in python, called
[litevault][2] using scrypt to decrypt AND encrypt the passwords.
That was cool, but had several issues:
- Didn't run on python3. I'm pretty good at writing for both but
  I kept hitting minor bugs and finally just gave up.
- Adding passwords was a pain -- I wrote a helper to "merge" two
  vaults but it was a nerve wracking experience.
- The thought of forgetting to commit the vault was scarry -- the
  passwords were randomly generated so they would be just *gone*.

I have kept my eye on the application [masterpassword][3] for some time, but
I've never been quite happy with its mixture of features. Why does it need
to know my name? What is up with the colorful symbols coresponding to a
password?

All I wanted was something that using a hash of the password for
validation purposes, and then site passwords are just a hash of the
same password. The setings, reference hash and existing sites should
be stored in a simple toml file, which can be put on github.

So I set out to write it and have completed the working implementation:
[novault][4].

## Rust is da best
The first thing that hit me was how quickly I was up and running.  I started by
thinking I would use SHA512 hashing and quickly [found a library][5]. I used
[cargo-edit][6] to achieve an impresive pace of adding libraries -- every
library was a quick cli command away. Having written a cli app before, I knew
what I wanted. I used a [list of cli libraries][7] I had compiled to cut
through my requirements like butter. Within a few hours of work I had a working
prototype that compiled and *actually ran*.

I spent the next few hours cleaning up the docs. I discovered that maybe SHA512
wasn't secure enough for storing the hash in plain-text on the internet so
instead settled on [Argon2, the winner of the 2015 Password Hashing
Competition][8]. The `argon2rs` library was a quick `cargo add` away, and the docs
could be read with `cargo doc --open -p argon2rs`.

I refactored the library to use types for everything I cared about. It is now
IMPOSSIBLE to serialize the user's master password (outside of `secure.rs`), and
serializing a site password (even PRINTING it) requires using it's
`password.audit_this` attribute.

## The Application
The NoVault application itself is now in a pretty good state.  I would really
appreciate some people more versed in security to give it an audit, as well as
any contributions or tests anyone wants to contribute. It can currently be
installed by installing rust and using

```
cargo install novault
```

I will probably start actually using it in the comming month. For now I am
just using it to store my reddit password, which is [on github][9]. Feedback
and audits would be great!

[1]: https://github.com/vitiral/artifact
[2]: https://github.com/vitiral/litevault
[3]: http://masterpasswordapp.com/
[4]: https://github.com/vitiral/novault
[5]: https://github.com/sfackler/rust-openssl
[6]: https://www.google.com/search?q=rust+cargo+add&ie=utf-8&oe=utf-8
[7]: https://github.com/vitiral/stdcli
[8]: https://en.wikipedia.org/wiki/Argon2
[9]: https://github.com/vitiral/dotfiles
