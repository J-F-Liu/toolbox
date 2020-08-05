# qemu

https://www.qemu.org/

### Install

```
pacman -S qemu 	virt-manager
```

### Command Line

```
curl -O https://nl.alpinelinux.org/alpine/v3.8/releases/x86_64/alpine-standard-3.8.0-x86_64.iso
qemu-img create -f qcow2 alpine.qcow2 16G
qemu-system-x86_64 \
    -enable-kvm \
    -m 2048 \
    -nic user,model=virtio \
    -drive file=alpine.qcow2,media=disk,if=virtio \
    -cdrom alpine-standard-3.8.0-x86_64.iso \
    -sdl
```

-enable-kvm: This enables use of the KVM (kernel virtual machine) subsystem to use hardware accelerated virtualisation on Linux hosts.

-m 2048: This specifies 2048M (2G) of RAM to provide to the guest.

-nic user,model=virtio: Adds a virtual network interface controller, using a virtual LAN emulated by qemu. This is the most straightforward way to get internet in a guest, but there are other options (for example, you will probably want to use -nic tap if you want the guest to do networking directly on the host NIC). model=virtio specifies a special virtio NIC model, which is used by the virtio kernel module in the guest to provide faster networking.

-drive file=alpine.qcow2,media=disk,if=virtio: This attaches our virtual disk to the guest. It’ll show up as /dev/vda. We specify if=virtio for the same reason we did for -nic: it’s the fastest interface, but requires special guest support from the Linux virtio kernel module.

-cdrom alpine-standard-3.8.0-x86_64.iso connects a virtual CD drive to the guest and loads our install media into it.

-sdl finally specifies the graphical configuration. We’re using the SDL backend, which is the simplest usable graphical backend. It attaches a display to the guest and shows it in an SDL window on the host.

### GUI Frontends

- [virt-manager](https://virt-manager.org/)
- [Gnome Boxes](https://help.gnome.org/users/gnome-boxes/stable/)
- [AQEMU](https://github.com/tobimensch/aqemu)
