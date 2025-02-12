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

Restarting domain:

`virsh --connect qemu:///system start security_os`

Graphical Interface to manage VMS:

`virt-manager`

### VMware Workstation:

## LSM development:

### Kernel hooks:

Kernel hook is a technique where a system call is slightly modified by a "hook". This can be done for legitimate puposes but can also be exploited by malware.

### LSMs:

LSMs are not modules, but extensions which provide a framework for kernel hooks

In order to see the list of running LSMs, the following command can be executed.

`cat /sys/kernel/security/lsm`

The LSM interface is four functions:

- int register_security (struct security_operations *ops);
- int unregister_security (struct security_operations *ops);
- int mod_reg_security (const char *name, struct security_operations *ops);
- int mod_unreg_security (const char *name, struct security_operations *ops);

If the security_operations function fails, it means some other security module has already been loaded. When this happens the mod_reg_seurity function is called in an attempt to registed with this security module.

First we will download the Linux kernel:

`git clone --depth=1 https://github.com/torvalds/linux.git`

Next, test_lsm directroy is created for our custom LSM. To this we will add our test_lsm.c file which contains the skeleton for our lsm module.

In order to tell the kernel to build the LSM, the following three steps are followed:

1. Modify `security/Kconfig` to register our LSM
2. Modify `security/Makefile` to add our LSM module
3. Modify `security.c` to add our hook
3. Tell the security Makefile and Kconfig files about the LSM, editing security/Makefile and security/Kconfig

Now we compile and install the kernel with the LSM included.

`make menuconfig`

Navigate to Security Options and enable Test LSM

Then compile the kernel:
`
make -j$(nproc)
sudo make modules_install
sudo make install
`

Update GRUB:
`
sudo update-grub
sudo reboot
`