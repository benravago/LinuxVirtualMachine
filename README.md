# LinuxVirtualMachine
_A basic linux KVM guest for MacOS (and others)_

The project contains some linux kernel config files to complement Apple's [Running Linux in a Virtual Machine](https://developer.apple.com/documentation/virtualization/running_linux_in_a_virtual_machine).

The base config is mostly from Adam Chester's [Bring Your Own VM - Mac Edition](https://blog.xpnsec.com/bring-your-own-vm-mac-edition/) blog post with a variation
to add container/namespace support (for lxc) and another to add VFAT support.

To build the LinuxVirtualMachine example app, you need to install Xcode.

To build the kernel, you'll need a host linux environment.
I used the [UTM](https://mac.getutm.app/) app with an [Ubuntu](https://mac.getutm.app/gallery/ubuntu-20-04) vm.
For a kernel build, you will also need to install a gcc toolchain via, for example,

`sudo apt-get install gcc make flex bison bc libncurses-dev`

To build the linux kernel:
1. download a kernel tarball, e.g. from https://kernel.org/
2. unpack the kernel source tree and copy the [kvm guest config](https://github.com/benravago/LinuxVirtualMachine/blob/main/etc/config.kvm) to `.config`
3. run `make`
4. copy the **uncompressed boot image** file from `./arch/arm64/boot/Image` to your MacOS host
5. make a simple initramfs file using the [mk.initramfs](https://github.com/benravago/LinuxVirtualMachine/blob/main/etc/mk.initramfs) script
6. copy the generated `initramfs` **uncompressed CPIO** file to your MacOS host

Then, build the Apple sample app:
1. download the [project zip file](https://docs-assets.developer.apple.com/published/7fa857c589/RunningLinuxInAVirtualMachine.zip) and unzip it
2. import the project into Xcode and build it
3. copy the `build/Release/LinuxVirtualMachine` binary to where the linux Image and initramfs files are
4. run `./LinuxVirtualMachine Image initramfs` and you should see something like this:

```
# ./LinuxVirtualMachine Image.kvm initramfs.cpio
cacheinfo: Unable to detect cache hierarchy for CPU 0
loop: module loaded
NET: Registered PF_PACKET protocol family
9pnet: Installing 9P2000 support
Freeing unused kernel memory: 320K
Run /init as init process
Hello world

```

After the 'Hello world' message, do Ctrl-C to exit the LVM app.

------

### TODO:

1. add an example showing how to use the [toybox-aarch64](http://landley.net/toybox/bin/toybox-aarch64) bundle for a user environment
2. [LinuxFromScratch](https://linuxfromscratch.org/) in the kvm (possibly bootstrapped from the [PiLFS](https://intestinate.com/pilfs/) aarch64 image
3. etc.

  


