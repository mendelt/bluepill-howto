# Blink a led
In this chapter we will walk through a simle but complete project from start to finish. We will first set up a Rust project to target the stm32 on the bluepill board. We will create a simple program to blink a led. Then we will demonstrate how to build the code and program the board and we will show how to run the program in a debugger.

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

