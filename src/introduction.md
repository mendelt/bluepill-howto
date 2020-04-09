# Introduction and Setup

This page documents how to write firmware for the bluepill board in Rust. For now it is assumed that Linux is used on the host used for development and debugging.
We'll use an STLink II compatible programmer for programming and debugging.

## The Hardware

### Bluepill


### STLink v2


## The Software
This paragraph describes setting up the tools used to program and debug the BluePill board. We'll use the normal Rust tools to write and compile software. Cargo, Rust's
build tool can cross-compile to target other targets out-of-the-box. We'll just need to add the right build target for the stm32 on the Bluepill board.
We'll use GDB and OpenOCD to connect to our target hardware for programming and debugging

### Setting up Rust using rustup


### OpenOCD


### GDB





Now that we have all the prerequisites set up we can start programming. In the next chapter we'll show how easy it is to create our first embedded software to blink
a led.
