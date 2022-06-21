# Rustbox2

Rustbox2 is a Rust implementation of [termbox2](http://github.com/yymirror/termbox2).

Currently, this is just a wrapper of the C library by nsf, though my plan is to convert it to be a pure Rust implementation and remove the requirement on the C library.

The original implementation of this was inspired by [Aaron Pribadi](http://github.com/apribadi/rust-termbox), so big props to him for the original work.

**NOTE** This is under development, and the APIs may change as I figure out more how Rust works and as the language itself changes


## Usage

In your `Cargo.toml` add the following:

```toml
[dependencies.rustbox]
git = "https://github.com/yymirror/rustbox2.git"
```

Then, in your `src/example.rs`:

```rust
#![feature(core)]

extern crate rustbox2;

use std::error::Error;
use std::default::Default;

use rustbox2::{Color, RustBox};
use rustbox2::Key;

fn main() {
    let rustbox = match RustBox::init(Default::default()) {
        Result::Ok(v) => v,
        Result::Err(e) => panic!("{}", e),
    };

    rustbox.print(1, 1, rustbox::RB_BOLD, Color::White, Color::Black, "Hello, world!");
    rustbox.print(1, 3, rustbox::RB_BOLD, Color::White, Color::Black,
                  "Press 'q' to quit.");
    rustbox.present();
    loop {
        match rustbox.poll_event(false) {
            Ok(rustbox::Event::KeyEvent(key)) => {
                match key {
                    Key::Char('q') => { break; }
                    _ => { }
                }
            },
            Err(e) => panic!("{}", e.description()),
            _ => { }
        }
    }
}
```

**NOTE:** this example can also be run with `cargo run --example hello-world`.
