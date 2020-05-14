# The Software
This paragraph describes setting up the tools used to program and debug the BluePill board. We'll use the normal Rust tools to write and compile software. Cargo, Rust's
build tool can cross-compile to target other targets out-of-the-box. We'll just need to add the right build target for the stm32 on the Bluepill board.
We'll use GDB and OpenOCD to connect to our target hardware for programming and debugging.

## Rust, Cargo and Rustup
We'll assume you already have a working and up-to-date Rust setup on your machine. If not a description on how to install it can be found in the [official Rust book](https://doc.rust-lang.org/book/ch01-01-installation.html). To cross compile sofware for the stm32f103 microcontroller on the bluepill board the additional target `thumbv7m-none-eabi` needs to be added. This can be done with the following command;
```
rustup target add thumbv7m-none-eabi
```

## OpenOCD
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

## GDB
GDB is the Gnu debugger. There is a special version for debugging Arm devices that we can use to remotely connect to the blue pill and step though your code. There is
probably a package for your package manager available that can be installed similar to this;
```
sudo apt-get install arm-none-eabi-gdb
```
or
```
sudo pacman -S arm-none-eabi-gdb
```

> If the `arm-none-eabi-gdb` package does not exist in your package manager it might be that you're using a system based on Ubuntu 18.04 or later. where this package
> was replaced by the gdb-multiarch package;
> ```
> sudo apt-get install gdb-multiarch
> ```
> Keep in mind that in later chapters you will need to specify a different debugger in some scripts.

Now that we have all the prerequisites set up we can start programming. In the next chapter we'll show how easy it is to create our first embedded software to blink
a led.
