# Blink a led
Now we will set up a first working program for the bluepill to blink a led. This will show how to set up a Rust project to target the board. And how to quickly build and
run your software on the board using OpenOCD and the GDB debugger.

But first we will connect our hardware so we are able to talk to it.

## Connect the STM32 and the bluepill

TODO: explain connecting the stm32 to the debug header and usb
TODO: test the connection

## Set up the project
> ### The easy way
> This chapter shows you step by step how to set up a rust project to target the bluepill (or other embedded) target. This is a great way to introduce all the
> separate parts of a project and explain them bit-by-bit. If you just want to get started programming you can also use cargo generate to use the bluepill-template
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

If you installed the `gdb-multiarch` package instead of `arm-none-eabi-gdb` in the previous chapter you will need to comment out the runner line and un-comment the next
line in this file to make this work for you.

