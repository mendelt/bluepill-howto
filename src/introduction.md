# Introduction and Setup

This document describes how to use the Rust programming language to write firmware for the bluepill board. It is assumed that Linux is used on the host used for development and debugging.
We'll use an STLink II compatible programmer for programming and debugging.

## The Hardware

### Bluepill
The blue pill board is an inexpensive stm32f103 based microcontroller board that is available from many resellers. It is inexpensive and relatively powerful so it's
perfect for learning. The stm32f103 comes with plenty of I/O, decent amounts of flash and RAM memory and is quite fast.

### STLink v2 or equivalent
The STLink v2 is a USB programmer and debugger for the stm32 range of microcontrollers by STMicroelectronics. The STLink itself is not expensive but even cheaper
imitations can be bought that will also do the job.

## The Software
This paragraph describes setting up the tools used to program and debug the BluePill board. We'll use the normal Rust tools to write and compile software. Cargo, Rust's
build tool can cross-compile to target other targets out-of-the-box. We'll just need to add the right build target for the stm32 on the Bluepill board.
We'll use GDB and OpenOCD to connect to our target hardware for programming and debugging.

### Rust, Cargo and Rustup
We'll assume you already have a working and up-to-date Rust setup on your machine. If not a description on how to install it can be found in the [official Rust book](https://doc.rust-lang.org/book/ch01-01-installation.html). To cross compile sofware for the stm32f103 microcontroller on the bluepill board the additional target `thumbv7m-none-eabi` needs to be added. This can be done with the following command;
```
rustup target add thumbv7m-none-eabi
```

### OpenOCD
We'll need OpenOCD to be able to connect to your device so we can program and debug it. A recent (0.10.0 is the newest at the time of writing this document) version can
probably be installed using the package manager of your choice.
on Debian based systems this can be done this way;
```
sudo apt-get install openocd
```

On arch or manjaro this will do the trick;
```
sudo pacman -S openocd
```

### GDB
GDB is the Gnu debugger. There is a special version for debugging Arm devices that we can use to remotely connect to the blue pill and step though your code. There is
probably a package for your package manager available that can be installed similar to this;
```
sudo apt-get install arm-none-eabi-gdb
```
or
```
sudo pacman -S arm-none-eabi-gdb
```

On Ubuntu versions 18.04 and higher this package was replaced by the gdb-multiarch package
```
sudo apt-get install gdb-multiarch
```
This means that in later chapters you will need to specify a different debugger in some scripts, so keep this in mind.

Now that we have all the prerequisites set up we can start programming. In the next chapter we'll show how easy it is to create our first embedded software to blink
a led.
