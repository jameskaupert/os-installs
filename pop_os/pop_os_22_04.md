# Pop!_OS
[Download from System76](https://pop.system76.com/)

This install process assumes version 22.04 and uses the image with the NVIDIA driver already installed.

This install process assumes you want to install Pop!_OS over the existing operating system and makes no considerations for preserving existing data.  Use carefully!

**Secure Boot must be disabled for the installation process.**

**Always back up your data before working with operating systems!**

## Installation - Real Computer
If you are installing to a real computer instead of a virtual machine, use these steps.

Below is a summary of the [official install instructions](https://support.system76.com/articles/install-pop/).

1. Download an ISO from the System76 website.
2. Verify the checksum against the one provided on the website.
3. Create a bootable USB drive.  The official website has [good instructions](https://support.system76.com/articles/live-disk/) for all operating systems.
4. Insert the USB drive into the computer you want to install to.
5. Turn the computer on (or reboot it if on already).
6. Enter the BIOS/UEFI/boot menu (could be F1, F2, F7, F8, F10, F12, or ESC key during initial boot)
7. Ensure Secure Boot is not enabled.
8. Select the live USB as the boot device.
9. At this point, the install process is nearly self-explanatory. Choose the options you want, with the two caveats below:
  - The only really critical point to be aware of is the "Install" screen where the option of "Clean Install" is provided. Choosing this WILL erase your entire existing system! The Custom (Advanced) option can be used to manage partitions if you know what you're doing.
  - Strongly consider encrypting the device option after the "Create User Account" page. Pop!_OS makes the process simple and it protects your data if the device is physically stolen.


## Installation - Virtual Machine (QEMU)
Other virtualization methods will work - these just assume QEMU.
1. Download an ISO from the System76 website.
2. Verify the checksum against the one provided on the website.
3. Create a virtual disk for the VM using QEMU.
```bash
qemu-img create -f qcow2 VirtualDisk.qcow2 100G
```
4. Replace the iso in the following command with the path to your ISO containing the Pop!_OS installer and run it to start QEMU.
```bash
qemu-system-x86_64                                             \
-enable-kvm                                                    \
-m 8G                                                          \
-smp 2                                                         \
-hda VirtualDisk.qcow2                                         \
-boot menu=on                                                  \
-cdrom pop-os_22.04_amd64_nvidia_43.iso                        \
-netdev user,id=net0,net=192.168.0.0/24,dhcpstart=192.168.0.9  \
-device virtio-net-pci,netdev=net0                             \
-vga qxl                                                       \
-device AC97                                                   \
-bios /usr/share/ovmf/OVMF.fd
```
9. At this point, the install process is nearly self-explanatory. Choose the options you want, with the two caveats below:
  - The only really critical point to be aware of is the "Install" screen where the option of "Clean Install" is provided. This is less important on a fresh VM than on a real machine, but if you want to partition things more specifically, you may want the Custom (Advanced) option.
  - Strongly consider encrypting the device option after the "Create User Account" page. Pop!_OS makes the process simple and it protects your data if the device is physically stolen.
