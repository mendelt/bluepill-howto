# Runing code in the debugger

This document will show you two ways of getting code to run on the bluepill. Eventually the code should be compiled to a `.hex` file that can be distributed and flashed
onto your board. But this won't be the way you program your board during development. During development it will be more useful to have a tight deployment and test cycle
where you can incrementally write your code, compile it and test it out. Also it will be useful to be able to debug your code to quickly find and solve problems.
In this chapter we'll show you how to set up your project to quickly use `cargo run` to build and deploy your program to your board using `openocd` and the `gdb` debugger.

First we need to set up a script to start `openocd` and connect to your stlink in-circuit-debugger. We described how to connect this debugger to your board at the start of this book.

```
#!/bin/sh
openocd -f interface/stlink-v2.cfg -f target/stm32f1x.cfg
```

next we'll set up cargo to use gdb as runner when starting our application. We do this by creating a `.cargo` directory at the root level in our project and creating the file `.cargo/config` with the following content.

```
[target.thumbv7m-none-eabi]
runner = "arm-none-eabi-gdb -q -x openocd.gdb"
# runner = "gdb-multiarch -q -x openocd.gdb"

rustflags = [
    "-C",
    "link-arg=-Tlink.x"
]

[build]
target = "thumbv7m-none-eabi"
```

This file first configures the `thumbv7m-none-eabi` target, the target for our platform, to use gdb as the runner using the file `openocd.gdb` with some initial commands.
We'll create this file shortly.

After this it specifies some flags for the linker and it configures the right default target.

If you installed gdb-multiarch in the previous chapter you'll need to comment out the runner line and replace it with the line below it.

Now we'll add the `openocd.gdb` file with some convenient set-up commands for gdb whenever we run our code. It should look a bit like this but you can customize it later;
```
target remote :3333
set print asm-demangle on
monitor arm semihosting enable

# detect unhandled exceptions, hard faults and panics
break DefaultHandler
break HardFault
break rust_begin_unwind

load
```

TODO: expand on this file and explain commands.

Now that you've set this up you should be able to compile and run your code. You do this by first starting `openocd` by running the script we created earlier; `./openocd.sh`. Preferably run this in a separate terminal. You can just leave this running when you're developing, so you'll only need to start this once.

Running your code should be as easy as running `cargo run` in your project directory.

