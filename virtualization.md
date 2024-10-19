# Virtualization
Virtual machines are useful for learning and testing the operating system install process.

## QEMU
QEMU is an open source tool for emulating entire systems.  It can also use hypervisor software such as KVM to run a guest OS using the CPU of the host instead of purely emulating.

[QEMU Documentation](https://www.qemu.org/docs/master/index.html)

Simple installation on a system with `apt`, including the OVMF package to allow UEFI firmware support:
`sudo apt install qemu-system ovmf`

### Create a Virtual Disk
Virtual disks represent the emulated system.  Initially they are created empty, and operating systems can be installed on them.

The following is a basic command to create a 100GB virtual disk named "VirtualDisk" in the QEMU Copy on Write (QCOW2) format.
```bash
qemu-img create -f qcow2 VirtualDisk.qcow2 100G
```

### Run a Virtual Disk
The following command will run the "VirtualDisk.cow2" virtual disk and load the "your-os-image.iso" for use if you want to install it.
- 4G RAM
- 2 CPUs
- Basic networking allowing wired access within the VM
- Graphics and sound
- UEFI

See the documentation for more customization. These work fine for testing installs but may not be suitable for regular use.
```bash
qemu-system-x86_64                                             \
-enable-kvm                                                    \
-m 4G                                                          \
-smp 2                                                         \
-hda VirtualDisk.qcow2                                         \
-boot menu=on                                                  \
-cdrom your-os-image.iso                                       \
-netdev user,id=net0,net=192.168.0.0/24,dhcpstart=192.168.0.9  \
-device virtio-net-pci,netdev=net0                             \
-vga qxl                                                       \
-device AC97                                                   \
-bios /usr/share/ovmf/OVMF.fd
```


