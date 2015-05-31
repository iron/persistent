persistent [![Build Status](https://secure.travis-ci.org/iron/persistent.png?branch=master)](https://travis-ci.org/iron/persistent)
====

> Persistent storage as middleware for the [Iron](https://github.com/iron/iron) web framework.

## Example

```rust
use iron::prelude::*;

use persistent::Write;
use iron::typemap::Key;
use iron::{status};

#[derive(Copy, Clone)]
pub struct HitCounter;

impl Key for HitCounter { type Value = usize; }

fn serve_hits(req: &mut Request) -> IronResult<Response> {
    let mutex = req.get::<Write<HitCounter>>().unwrap();
    let mut count = mutex.lock().unwrap();

    *count += 1;
    Ok(Response::with((status::Ok, format!("Hits: {}", *count))))
}

fn main() {
    let mut chain = Chain::new(serve_hits);
    chain.link(Write::<HitCounter>::both(0));
    Iron::new(chain).http("localhost:3000").unwrap();
}
```

## Overview

persistent is a part of Iron's [core bundle](https://github.com/iron/core).

- Share persistent data across requests
- Read or modify locally stored data

## Installation

If you're using a `Cargo.toml` to manage dependencies, just add persistent to the toml:

```toml
[dependencies.persistent]

git = "https://github.com/iron/persistent.git"
```

Otherwise, `cargo build`, and the rlib will be in your `target` directory.

## [Documentation](http://ironframework.io/doc/persistent)

Along with the [online documentation](http://ironframework.io/doc/persistent),
you can build a local copy with `make doc`.

## [Examples](/examples)

## Get Help

One of us ([@reem](https://github.com/reem/), [@zzmp](https://github.com/zzmp/),
[@theptrk](https://github.com/theptrk/), [@mcreinhard](https://github.com/mcreinhard))
is usually on `#iron` on the mozilla irc. Come say hi and ask any questions you might have.
We are also usually on `#rust` and `#rust-webdev`.
