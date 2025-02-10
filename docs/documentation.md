# LSM

An OS and Cyber-security based project that enforces Mandatory Access Control (MAC) security model as a Linux Security Module (LSM).

This differentiates from Discretionary Access Control (DAC) where users can grant or change file permissions. MAC enforces predefined policies that cannot be altered by individual users.

## Environment

This project is developed and tested in Ubuntu 22.04 using a kernel emulator to load the distro as a Virtual Machine (VM).

To achieve this, VMware workstation and KVM will be used.

### KVM:

Installs:

`sudo apt install -y qemu-kvm libvirt-daemon-system virt-manager bridge-utils`

Verify KVM:

`kvm-ok`

Enable and start libvirt:

`sudo systemctl enable --now libvirtd`

starting KVM:

`sudo virt-install \
--name security_os \
--memory 4096 \
--disk size=20 \
--cdrom /var/lib/libvirt/images/ubuntu-22.04.5-desktop-amd64.iso`

Pakages installed inside VM:

`sudo apt install build-essential linux-headers-$(uname -r) qemu-guest-agent`

Kernel Development Tools:

`sudo apt install -y gcc make flex bison libelf-dev \
  libssl-dev clang llvm`

`sudo apt install -y linux-source`
