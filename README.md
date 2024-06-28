# Kamakiri *(kernel.patch)* compatibility on available linux kernels
<br>

I only created this because im bored, also helps those newbies out there how to patch *devio.c* ***(kamakiri patch)*** with their current kernel-release ver. and to be "User friendly".
<br>
<br>

> REMEMBER, MUST HAVE A PC WITH 4 cores 4 threads CPU, SSD/SDD/M.2 drive, incredibly Fast RAM `2xxxMhz ram is fine` **(or better)** For faster compilation!!<br><br>
> REMEMBER, im not responsible for corrupted or un-bootable linux **It can be restored if you backed them up** so BACK UP THEM!
<br>
<br>
<br>

## Let's start!
<br>

> Requirements for compiling linux kernel after patching

### Common Requirements Across All Distributions
<br>

**Build Essentials**: Basic tools for compiling software.<br>
**Kernel Headers**: Necessary for building kernel modules and other kernel-related components.<br>
**Make**: The build automation tool.<br>
**GCC**: The GNU Compiler Collection.<br>
**Bison**: A parser generator used in the configuration process.<br>
**Flex**: A tool for generating scanners (programs that recognize lexical patterns in text).<br>
**Libssl-dev**: Development files for OpenSSL libraries, needed for kernel features involving cryptography.<br>
**Ncurses**: Libraries for text-based user interfaces, used in kernel menuconfig.<br>
**Dracut**: Used for creating initramfs.img for vmlinuz (also known as bzImage). It's particularly useful for including various installed packages and drivers into the initial RAM filesystem (initramfs)
<br><br><br>
## whats my current linux-release version?

To know what's your current linux version run this command:
```
uname -r
```
This would show your `linux-release version` mine was `6.8.7-zen1` Just focus on the 6.8.7, ***-zen1** is a patch for my Ryzen AMD CPU
<br><br><br>

## Download your fresh unmodified-linux kernel

To download go to this link [linux-kernel](https://mirrors.edge.kernel.org/pub/linux/kernel/) and you should see some version identifiers *e.g.* `v6.x/`
<br><br><br>

## Extract and tidy up things!

1. Extract to `/home/username/Downloads` folder.<br>
2. Go to the current forked github `Release` Section and find the compatible `kernel.patch` for your current/compiling linux-kernel release version<br>
3. Take your current kernel `.config` file, They're typically found at:<br>

> `/usr/src/` You must know your current linux kernel name, do `uname -s` then you should find a folder same as the `uname -s` typed out.<br>
> Go to here `/proc/` then search config without dots, you should find a file called `config.gz`, extract it and name it `.config`<br>
> If ever you cant find it check `/boot/` find it there.

<br>

## Start compiling
<br>

Before we start compiling, make sure that everything is in the Linux-`kernel version` folder, both must be in the folder (`.config` and `kernel.patch`).

<br><br>

# Lets start!
<br>

Open your terminal in the Linux-`kernel version` folder, then do the following command:
```
patch -p1 < kernel.patch
```
<br>

Then we configure the config to be implemented to the kernel and updates them abit for better `make build` compatibility and for the kernel `.config` compatibility.<br>
To update the `.config` do this command:
```
make menuconfig
```











