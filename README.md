# Kamakiri Patch Tutorial for Linux Kernels

### Kamakiri *(kernel.patch)* compatibility on available linux kernels
<br>

I only created this because im bored, also helps those newbies out there how to patch *devio.c* ***(kamakiri patch)*** with their current kernel-release ver. and to be "User friendly".
<br>
<br>

### IMPORTANT

> **System Requirements**: A PC with at least 4 cores/4 threads CPU, SSD/SDD/M.2 drive, and fast RAM (2xxx MHz) or ***more better*** is recommended for faster compilation.<br><br>
> **Backup**: Always back up your system before making changes. I am not responsible for corrupted or un-bootable Linux systems.
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
> Go to here `/proc/` then search config without dots, you should find a file called `config.gz`, extract it and name it `.config`.<br>
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

Then we configure the config to be implemented to the kernel and updates them abit for better `make build` compatibility and for the kernel `.config` compatibility.<br><br>
To update the `.config` do this command:
```
make menuconfig
```
Wait for abit and it should open up an interactive menu, Now go and select `Load` and leave the text as `.config` as you already renamed it inside the linux-`kernel version` folder.<br><br>
<br>

## GREAT! Lets compile
<br>

Simply put this command:
```
make
```
<br>

Also remember what i said? `..BER, MUST HAVE A PC WITH 4 cores 4 threads CPU...` Do this if you dont like to fiddle around or use the pc when compiling:

```
make -j$(nproc)
```

or if you know how much cpu your pc has. e.g. `4 cores CPU`.

```
make -j4
```

> The compiling takes around hours approx. 1 - 2.3 Hours, the efficieny reduce this depending how much your CPUfreq and Cores are.

<br><br>

## After compiling
<br>

After compiling, Run this command:
```
make install modules
```
<br>

After this is done check the directory `/lib/modules` and find the most recent "Newly" added directories, you would know it because of the `last modified/time was modified` on `folder/s properties`.
> at this point i dont even know what im saying, but i hope you get what i say

<br><br><br>

## Configure initramfs.img and vmlinuz
<br>

> This might be abit tricky, so stay with me.

<br>

### We're close!
<br>

To configure the initramfs, first lets copy the vmlinuz we made to `/boot/`.<br>
To find this, just go to the main folder of ***linux-`kernel version`/arch/`current system architecture`/boot/bzImage***
> the `current system architecture` is the part what architecture youre running, x86 (32Bit), x86-x64 (32-64Bit or 64Bit)

<br>

After finding it, copy it and paste it to `/boot/` make sure you rename it, rename it whatever you like. just make sure you would remember it.
in this case i name it `vmlinuz-kamakiri`
> doesnt matter if you forget it after forgetting it

<br>

now open up the terminal and run dracut, run this command:

```
sudo dracut --force --verbose /boot/initramfs-custom name.img module
```

<br>

heres what the `custom name` and `module` means. the `custom name` is what you named your vmlinuz, in this case i named mine `vmlinuz-kamakiri` and for the module like i said find the most recent "Newly" added folder on `/lib/modules`, copy that folder name and put it on the `module`
in this case mine was `6.8.7-zen1` 
> zen1 is just a patch i added for my AMD cpu

<br>

like this
```
sudo dracut --force --verbose /boot/initramfs-kamakiri.img 6.8.7-zen1
```
When you're done with that, back up your current vmlinuz or stock vmlinuz/initramfs to somewhat unrecognizable for the system to read/write.

<br><br><br>

## Last Part


Now the last part is to update your bootloader to read and configure your modified kernel and initramfs.
<br>

In this case i use GRUB, run this command:
```
sudo update-grub
```

> Also look closely, you must find that it processed your vmlinuz. in my case it showed `vmlinuz-kamakiri` and the `initramfs-kamakiri.img`.
> depends on it, But make sure while updating your bootloader check if the modified vmlinuz is inside your bootloader `cfg` file(s)


