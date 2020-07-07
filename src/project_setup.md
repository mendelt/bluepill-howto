# Project setup
This chapter shows you step by step how to set up a rust project to target the bluepill (or other embedded) target. This is a great way to introduce all the separate parts of a project and explain them bit-by-bit. 

> ## The easy way
> If you just want to get started programming you can also use cargo generate to use the bluepill-template
> project. This will set all this up for you instead of you having to type it all in manually.
> Cargo generate can be installed with the following command;
> ```
> cargo install cargo-generate
> ```
>
> You can then start a new project;
> ```
> cargo generate --git https://github.com/mendelt/bluepill-template
> ```

You can create a project the same way you do for normal Rust programs using cargo new;
```
cargo new hello-stm32
```
This will initialize a directory with a simple project for you.

We will need to tell cargo to use the `thumbv7m-none-eabi` target among other things. To do this you can create a new text-file in the .cargo folder in your project
directory called `.cargo/config` with the following contents;
```
[build]
target = "thumbv7m-none-eabi"

[target.thumbv7m-none-eabi]
runner = "arm-none-eabi-gdb -q -x openocd.gdb"
# runner = "gdb-multiarch -q -x openocd.gdb"

rustflags = [
    "-C",
    "link-arg=-Tlink.x"
]
```

This will tell cargo to use the thumbv7m-none-eabi target for our project by default. The next section in the file configures some flags for the rust compiler and tells
cargo it should use the gdb debugger for our platform to run our software instead of trying to start compiled programs itself. This will allow us to use the `cargo run`
command to start debugging on our target system.

> If you installed the `gdb-multiarch` package instead of `arm-none-eabi-gdb` in the previous chapter you will need to comment out the runner line and un-comment the next line in this file to make this work for you.

There is one other file that you will need and thats a linker script to tell the linker about the layout of the memory of our target.
Create a file in the project root called `memory.x` for this. It should contain the following text;
```
/* Linker script for the STM32F103C8T6 */
MEMORY
RY
{
  FLASH : ORIGIN = 0x08000000, LENGTH = 64K
  RAM : ORIGIN = 0x20000000, LENGTH = 20K
}
```

Now we are ready to create our first program by editing `src/main.rs`
```
#![no_main]
#![no_std]

#[allow(unused_imports)]
use panic_semihosting;

use cortex_m_rt::{entry};
use cortex_m_semihosting::hprintln;

#[entry]
fn main() -> ! {
    hprintln!("Hello semihosting world");

    loop { }
}
```

First the lines with `#![no_main]` and `#![no_std]` tell the compiler how we want to set up the
program. ``#![no_main]` tells the compiler that there is no main function like in a normal rust
program. We use the `cortex_m_rt` crate that provides a light-weight runtime for our embedded
code. We do have an `fn main()` but that could be named anything. We use the `#[entry]` annotation
to tell the runtime what the entry point to our code is.
`#![no_std]` simply tells the compiler not to add the Rust `std` crate. The `std` crate depends on
a lot of primitives that are normally supplied by the operating system of our computer. We do not
have an operating system to supply things like threading or file-io so we use `#![no_std]` to tell
the compiler to use the Rust `core` crate instead.

The `use panic_semihosting;` line tells the compiler what we want to do when our program panics. For
now we use semihosting. A mechanism to print error messages and debug information over our stlink
debug connection to our host. This is a slow and cumbersome way of debugging. But it's easy to set
up so it's a nice way of getting some information out of your microcontroller when there is nothing
else.

After this we import some dependencies for our program. And we define the entrypoint as the main
function. hprintln uses the same semihosting mechanism described earlier to print a message over
the stlink connection.

The loop at the end is neccesary to keep our code running. The `fn main()` return type is `!` which
indicates that this function should keep running indefinately.

TODO: rewrite to blinky

To make this program work we need to add the dependencies we used to Cargo.toml
```
[dependencies]
cortex-m = "0.6.2"
cortex-m-rt = "0.6.11"
cortex-m-semihosting = "0.3.5"
panic-semihosting = "0.5.3"

[dependencies.hal]
package = "stm32f1xx-hal"
version = "0.5.3"
features = ["rt", "stm32f103", "medium"]
```

TODO: fix dependencies.hal
TODO: explain dependencies
