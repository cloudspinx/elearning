# Installation of KVM on Linux Systems

In this chapter we will cover the setup process of KVM on various Linux distributions like Debian, Ubuntu, CentOS, AlmaLinux, Rocky Linux, Arch Linux, openSUSE/SUSE, and RHEL.

Before doing any package installation, it is important to check if your CPU supports hardware virtualization. This is done by executing the following commands in your terminal.

```bash
grep -E --color 'vmx|svm' /proc/cpuinfo
```

Analyze the output if it contains "**vmx**" or "**svm**," which will mean your CPU supports hardware virtualization.

You can as well run the commands separately:

```bash
### For Intel-based processors ###
grep -E --color 'vmx' /proc/cpuinfo

### For AMD-based processors ###
grep -E --color 'svm' /proc/cpuinfo
```

You can also run the following commands to check if your system supports hardware virtualization.

```bash
grep -Ec '(vmx|svm)' /proc/cpuinfo
```

If the output provided has a number **greater than** **zero**, it means CPU supports hardware virtualization.

## Installation of KVM on Debian / Ubuntu

Begin by ensuring the system packages are up-to-date:

```bash
sudo apt update && sudo apt upgrade -y
```

If a reboot is required after the packages are upgraded, consider doing it.

```bash
[ -f /var/run/reboot-required ] && sudo reboot -f
```

Install KVM and all necessary packages required to convert your Debian or Ubuntu system into a complete hypervisor.

```bash
sudo apt install libvirt-daemon qemu-kvm \
libvirt-daemon-system virtinst libosinfo-bin \
libguestfs-tools bridge-utils genisoimage
```

Explanation of the packages installed:

> - `libvirt-daemon` - A package that provides the libvirtd daemon, which provides an API for interacting with VMs.
> 
> - `qemu-kvm` - A metapackage that provides QEMU with KVM support. Qemu is used in the creation and the management of virtual machines, and it provides necessary emulation and virtualization capabilities.
> 
> - `libvirt-daemon-system` - It includes the system service files and default configurations for the libvirtd daemon service.
> 
> - `virtinst` - This package provides the command-line utilities used to provision and manage virtual machines. It include `virt-install` and `virt-clone`command-line tools which facilitates VM creation and management automations.
> 
> - `libosinfo-bin` - It provides the command-line tools required for the managment of operating system information. Command line tools like `osinfo-query`, `osinfo-install-script`, and `virt-customize` are provided by this package.
> 
> - `libguestfs-tools` - This provides a set CLI tools that allows you to access and modify virtual machine disk images. Some of these tools are `virt-builder`, `virt-cat`, `virt-ls`, `virt-edit`, `virt-df`, and `guestmount`.
> 
> - `bridge-utils` - Provide the utilities for managing and configuring network bridges. The main command-line tool in this package is `brctl`,

The installed packages collectively empower system administrators to create, configure, and manage virtual machines on KVM.

After installations, run the following command to check of KVM kernel modules are loaded.

```bash
$ sudo lsmod | grep kvm
kvm_intel             344064  8
kvm                   958464  1 kvm_intel
irqbypass              16384  19 kvm
```

If your Linux OS has a graphical user interface (Desktop Environment), you can install

```bash
sudo apt install qemu-system virt-manager 
```

Where:

> - `qemu-system` is a package with full system emulation binaries for QEMU
> 
> - `virt-manager` (Virtual Machine Manager) is a graphical user interface tool that enables you to manage virtual machines through libvirt from an intuitive interface.

If you encounter an error message like below on Ubuntu:

> `Error: error creating libvirt domain: internal error: process exited while connecting to monitor: qemu-system-x86_64: -blockdev {"driver":"file","filename":"/var/lib/libvirt/images/flatcar-base","node-name":"libvirt-2-storage","auto-read-only":true,"discard":"unmap"}: Could not open '/var/lib/libvirt/images/flatcar-base': Permission denied`

The solution is disable QEMU default security driver.

```bash
$ sudo nano /etc/libvirt/qemu.conf
security_driver = "none"
```

The `libvirtd` service needs tp be restarted after the change:

```bash
sudo systemctl restart libvirtd
```

## Install KVM on Rocky / AlmaLinux / CentOS Stream / Fedora

KVM can be installed on Rocky Linux, AlmaLinux, CentOS Stream, or Fedora operating systems using the steps provided in this section. All these distributions mentioned are similar and the same process apply when setting up KVM.

Start with the updating of system packages

```bash
sudo dnf update -y
```

Thereafter install KVM and the related packages for turning your OS into a hypervisor.

```bash
sudo dnf install -y libvirt qemu-kvm virt-install \
virt-top libguestfs-tools guestfs-tools genisoimage
```

In these distros, `bridge-utils` package is contained in the `epel` repository.

```bash
sudo dnf -y install epel-release
sudo dnf -y install bridge-utils
```

If you have a Desktop Environment, you can install `virt-manager`

```bash
sudo dnf -y install virt-viewer virt-manager
```

The explanation of packages installed.

> - `libvirt` is an open-source API written in C programming language. It is the core of the virtualization platform and is used by tools like `virt-manager` and `virt-install`.
> 
> - `qemu-kvm`- This package provides QEMU emulator with KVM (Kernel-based Virtual Machine) support.
> 
> - `virt-install` - A command-line tool for used in the creation of new virtual machines.
> 
> - `virt-top` - This is a top-like utility used to display the stats of your virtualized domains in KVM.
> 
> - `libguestfs-tools` - This package provides a set of libraries and tools that allows you to access and and modify virtual machine (VM) disk images.
> 
> - `virt-manager` (Virtual Machine Manager) is a graphical user interface tool that enables you to manage virtual machines through libvirt from an intuitive interface.
> 
> - `virt-viewer` - A lightweight tool used to display a graphical console of your virtual machines.

Start and enable the `libvirtd` daemon to start at system boot.

```bash
sudo systemctl enable --now libvirtd
```

## Installation of KVM on openSUSE / SUSE

In this section you will learn how to install KVM hypervisor on openSUSE and SUSE Linux distributions.

We always recommend you work on a system that is up to date:

```bash
sudo zypper refresh
sudo zypper -n update
```

To install all KVM tools and utilities run the commands below in your terminal.

```bash
sudo zypper -n install libvirt qemu-kvm libvirt-daemon \
libvirt-daemon-qemu virt-install guestfs-tools virt-top \
bridge-utils libguestfs
```

Packages installed:

> - `libvirt` - Libvirt API package
> 
> - `qemu-kvm` - QEMU emulator and virtualizer
> 
> - `libvirt-daemon` - This contains the libvirtd service
> 
> - `libvirt-daemon-qemu` - provides all the necessary drivers for managing QEMU/KVM guests using libvirt.
> 
> - `virt-install` - A command-line tool use to create new virtual machines
> 
> - `guestfs-tools` - Set of tools used to modify VMs disk images without booting up the VM.
> 
> - `virt-top` - This is a real-time VMs monitoring tool for checking resource usage statistics.
> 
> - `bridge-utils` - A set of utilities that enables you to configure network bridge devices on Linux
> 
> - `libguestfs` - Set of tools for accessing and modifying virtual machine (VM) disk images

If you have Desktop Environment in your system install `virt-manager` package as well.

```bash
sudo zypper -n install virt-manager 
```

After the installation start and enable `libvirtd` service.

```bash
sudo systemctl start libvirtd
sudo systemctl enable libvirtd 
```

## Installation of KVM on RHEL

Login to your RHEL server and make sure it's registered

```bash
$ sudo subscription-manager register --auto-attach
Registering to: subscription.rhsm.redhat.com:443/subscription
Username: <INPUT-USERNAME>
Password: <ENTER-PASSWORD>
```

Install packages that together work to make your RHEL server a functional virtualization platform.

```bash
sudo dnf install -y libvirt qemu-kvm virt-install \
virt-top libguestfs-tools
```

Users running the Desktop edition of RHEL can install `virt-manager` and `virt-viewer` GUI packages.

```bash
sudo dnf -y install virt-manager virt-viewer
```

The `bridge-utils` package is installed from EPEL repository. Add using the commands below.

```bash
# RHEL 9
sudo subscription-manager repos \
 --enable codeready-builder-for-rhel-9-$(arch)-rpms

sudo dnf install \
https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
sudo dnf -y install bridge-utils

# RHEL 8
sudo subscription-manager repos \
 --enable codeready-builder-for-rhel-8-$(arch)-rpms
sudo dnf install \
https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo dnf -y install bridge-utils
```

The explanation of packages installed.

> - `libvirt` is an open-source API written in C programming language. It is the core of the virtualization platform and is used by tools like `virt-manager` and `virt-install`.
> 
> - `qemu-kvm`- This package provides QEMU emulator with KVM (Kernel-based Virtual Machine) support.
> 
> - `virt-install` - A command-line tool for used in the creation of new virtual machines.
> 
> - `virt-top` - This is a top-like utility used to display the stats of your virtualized domains in KVM.
> 
> - `libguestfs-tools` - This package provides a set of libraries and tools that allows you to access and and modify virtual machine (VM) disk images.
> 
> - `bridge-utils` - Utilities used to configure network bridge devices
> 
> - `virt-manager` (Virtual Machine Manager) is a graphical user interface tool that enables you to manage virtual machines through libvirt from an intuitive interface.
> 
> - `virt-viewer` - A lightweight tool used to display a graphical console of your virtual machines.

Start the service and set it to start automatically with system boot.

```bash
sudo systemctl enable --now libvirtd
```

To chek if it' running we run:

```bash
systemctl status libvirtd
```

## Installation of KVM on Arch-based systems

Arch-based Linux distributions are known for their simplicity, flexibility, and user-centric approach. The following are major distributions based on Arch Linux:

- Manjaro

- EndeavourOS

- ArcoLinux

- Garuda Linux

- RebornOS

- ArchBang

- Artix Linux

Follow the steps provided to have KVM installed on any Arch based Linux distribution.

**Step 1:** Ensure the default GPG keys are populated

```bash
sudo pacman-key --init
sudo pacman-key --populate archlinux
```

**Step 2**: Update system

```bash
sudo pacman -Sy archlinux-keyring
sudo pacman -Syyu
```

Next we install KVM and all other tools required.

```bash
sudo pacman -S qemu libvirt virt-manager \
virt-viewer dnsmasq ebtables bridge-utils \
libguestfs guestfs-tools
```

Packages installed:

> - `qemu`: An emulator proving hardware virtualization functionalities.
> 
> - `libvirt`:  Provides Libvirt daemon and tools for managing virtual machines.
> 
> - `virt-manager` (optional package): Provides a graphical user interface where you can manage virtual machines (it is highly recommended for beginners).
> 
> - `virt-viewer`: Tool for viewing the graphical output of VMs in on KVM.
> 
> - `dnsmasq`: It provies DNS server and DHCP server that can be used to provide networking capabilities to your virtual machines.
> 
> - `ebtables`: Firewall utility specifically designed for managing firewall rules on Ethernet bridges, often used for network filtering within virtual environments.
> 
> - `bridge-utils`: Set of utilities for managing network bridge, which essentially connect virtual machines to your physical network.
> 
> - `libguestfs`: Set of tools for accessing and modifying virtual machine disk images
> 
> - `guestfs-tools`: Tools for accessing and modifying guest disk images

Ensure the kernel modules are automatically loaded

```bash
lsmod | grep kvm
```

Enable and start libvirtd service

```bash
sudo systemctl enable --now libvirtd
```

To check service status use the command

```bash
systemctl status libvirtd
```

## Verify Virtualization Host Readiness

To verify that your system is prepared to be a virtualization host, run the command below:

```bash
$ sudo virt-host-validate
  QEMU: Checking for hardware virtualization                                 : PASS
  QEMU: Checking if device /dev/kvm exists                                   : PASS
  QEMU: Checking if device /dev/kvm is accessible                            : PASS
  QEMU: Checking if device /dev/vhost-net exists                             : PASS
  QEMU: Checking if device /dev/net/tun exists                               : PASS
  QEMU: Checking for cgroup 'cpu' controller support                         : PASS
  QEMU: Checking for cgroup 'cpuacct' controller support                     : PASS
  QEMU: Checking for cgroup 'cpuset' controller support                      : PASS
  QEMU: Checking for cgroup 'memory' controller support                      : PASS
  QEMU: Checking for cgroup 'devices' controller support                     : PASS
  QEMU: Checking for cgroup 'blkio' controller support                       : PASS
  QEMU: Checking for device assignment IOMMU support                         : PASS
  QEMU: Checking if IOMMU is enabled by kernel                               : PASS
  QEMU: Checking for secure guest support                                    : WARN
```

For a`FAIL` value from any of the checks, refer to the displayed instructions for how to fix the problem.

If `virt-host-validate`generates generates the following output then your host CPU doesn't support KVM virtualization:

```bash
QEMU: Checking for hardware virtualization: FAIL (Only emulated CPUs are available, performance will be significantly limited)
```

## Libvirt Service Management

Libvirt is an open source virtualization API that provides a consistent interface enabling you to provision, start, stop, migrate, and monitor VMs on KVM hypervisor.

The management of the `libvirtd` service can vary slightly depending on the host operating system - whether it uses `systemd` or the older init systems like `SysVinit` or `Upstart`.

Here's we show you how the management of the `libvirtd` service can be performed on different init systems. The list should cover the common Libvirtd service management operations you might need to perform on various systems.

### 1. Systemd

Enable `libvirtd` service to start on system boot:

```bash
sudo systemctl enable libvirtd
```

Start the `libvirtd` service:

```bash
sudo systemctl start libvirtd
```

Check the `libvirtd` service status:

```bash
sudo systemctl status libvirtd
```

Stop the `libvirtd` service:

```bash
sudo systemctl stop libvirtd
```

Restart the `libvirtd` service:

```bash
sudo systemctl restart libvirtd
```

Reload the `libvirtd` service configuration without stopping it:

```bash
sudo systemctl reload libvirtd
```

Disable the `libvirtd` service so it does not start on boot:

```bash
sudo systemctl disable libvirtd
```

### 2. SysVinit

Enable the `libvirtd` service to start on boot:

```bash
sudo chkconfig libvirtd on
```

Start the `libvirtd` service:

```bash
sudo service libvirtd start
```

Check the status of the `libvirtd` service:

```bash
sudo service libvirtd status
```

Stop the `libvirtd` service:

```bash
sudo service libvirtd stop
```

Restart the `libvirtd` service:

```bash
sudo service libvirtd restart
```

Reload the `libvirtd` service configuration without stopping it:

```bash
sudo service libvirtd reload
```

Disable the `libvirtd` service so it does not start on boot:

```bash
sudo chkconfig libvirtd off
```

### 3. Upstart

Upstart is primarily used in older versions of Ubuntu (from 9.10 to 14.10) and some other distributions.

Start the `libvirtd` service:

```bash
sudo start libvirtd
```

Check the status of the `libvirtd` service:

```bash
sudo status libvirtd
```

Stop the `libvirtd` service:

```bash
sudo stop libvirtd
```

Restart the `libvirtd` service:

```bash
sudo restart libvirtd
```

Reload the `libvirtd` service configuration without stopping it:

```bash
sudo reload libvirtd
```

### Summary of Commands

| Action          | Systemd                      | SysVinit                   | Upstart            |
| --------------- | ---------------------------- | -------------------------- | ------------------ |
| Enable service  | `systemctl enable libvirtd`  | `chkconfig libvirtd on`    | N/A                |
| Start service   | `systemctl start libvirtd`   | `service libvirtd start`   | `start libvirtd`   |
| Check status    | `systemctl status libvirtd`  | `service libvirtd status`  | `status libvirtd`  |
| Stop service    | `systemctl stop libvirtd`    | `service libvirtd stop`    | `stop libvirtd`    |
| Restart service | `systemctl restart libvirtd` | `service libvirtd restart` | `restart libvirtd` |
| Reload service  | `systemctl reload libvirtd`  | `service libvirtd reload`  | `reload libvirtd`  |
| Disable service | `systemctl disable libvirtd` | `chkconfig libvirtd off`   | N/A                |

## Allow standard users to manage KVM

For security reasons, KVM by default restricts management operations to users with root privileges.

This can be demonstrated using the virsh command to list all networks, which typically requires root access. We are running the command as standard user (without any privileges)

```bash
$ virsh net-list
 Name   State   Autostart   Persistent
----------------------------------------
```

We can see the list is empty yet default network exists in this KVM node.

For standard (non-root) Linux users to manage KVM hypervisor we need to setup appropriate configurations and user permissions. Follow these detailed steps to safely grant non-root users powers to perform tasks on KVM.

1. **Create a New User Group**: Let's create a new user group called `libvirt`. This group will have the necessary permissions to administer with KVM. Skip if it exists and go to step 2.
   
   ```bash
   sudo groupadd --system libvirt
   ```

2. **Add Users to the Group**: Next we are adding specific user accounts to the group created.
   
   ```bash
   sudo usermod -a -G libvirt <username>
   ```
   
   Replace `<username>` with the actual username of the standard user you want to grant KVM management permissions. Repeat this step for each user you want to add.

3. **Modify Libvirt Configuration**: Edit the Libvirt configuration file to allow members of the `libvirt` group to manage KVM:
   
   ```bash
   sudo vim /etc/libvirt/libvirtd.conf
   ```
   
   Locate the line `unix_sock_group` in the configuration file:
   
   ```bash
   #unix_sock_group = "libvirt"
   ```

Uncomment the line and set the value to the `libvirt` group:

```bash
unix_sock_group = "libvirt"
```

Also ensure the following lines are uncommented and set as needed:

```bash
unix_sock_group = "libvirt"
unix_sock_ro_perms = "0777"
unix_sock_rw_perms = "0770"
```

Open the `qemu.conf` file and

```bash
sudo vim /etc/libvirt/qemu.conf
```

Uncomment the following lines and set as needed:

```bash
# Around line 519
user = "qemu"
group = "libvirt"
dynamic_ownership = 1
```

4. **Restart Libvirtd Service**: After making the changes you need to restart the Libvirtd service:

```bash
sudo systemctl restart libvirtd
```

5. **Verify Permissions**: Validate that a standard user can now manage KVM without sudo.

```bash
# Switch to standard user account
su - <username>

# List groups the user belongs to, kvm_admins should be in the list.
groups <username>
```

6. **Manager KVM as Standard User**: Finally we can test if a user in the `kvm_admins` group can now run `virsh` commands or use `virt-manager` without sudo privilege escalation.

```bash
newgrp libvirt
virsh net-list
```

The user can also use graphical tools like `virt-manager` to manage the VMs:

```bash
virt-manager
```

## Enabling IOMMU on host system

In KVM, IOMMU (Input-Output Memory Management Unit) is a hardware functionality that gives better security to the VMs by ensuring that they don't interfere with the memory each other.

The host system is also protected in an instance where the VM is compromised through malicios attacks. IOMMU can isolate faults in the system and reduces the likelyhood of a system-wide crashes that can be caused by a single VM or device.

For DMA (Direct Memory Access) capable devices, IOMMU allows VMs to have direct access to physical devices on the host system. This results in a near-native performance on VMs since it reduces overhead by bypassing the host OS.

Check if your system supports IOMMU:

```bash
grep -E --color 'vmx|svm' /proc/cpuinfo
```

If you get any output it means your system likely supports IOMMU.

### Enable IOMMU on Debian based systems

To enable IOMMU we need to modify OS kernel boot parameters. Open the file `/etc/default/grub` with a text editor of your choice.

```bash
# Open with vim
sudo vim /etc/default/grub

# Open with nano
sudo nano /etc/default/grub
```

Locate the line `GRUB_CMDLINE_LINUX_DEFAULT` and add `intel_iommu=on` or `amd_iommu=on` depending on the processor type in your host device.

```bash
# For Intel CPU add
intel_iommu=on


# For AMD CPU add
amd_iommu=on
```

Here are samples of a modified `GRUB_CMDLINE_LINUX_DEFAULT` line.

```bash
# Example 1
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash intel_iommu=on"

# Example 2
GRUB_CMDLINE_LINUX_DEFAULT="consoleblank=0 systemd.show_status=true console=tty1 console=ttyS0 intel_iommu=on"

# Example 3
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash amd_iommu=on"
```

After the file is edited, you need to generate a new GRUB configuration file.

```bash
sudo update-grub
```

For the changes to take effect, you will need to reboot the system.

```bash
sudo reboot
```

### Enable IOMMU on RHEL based systems

As a root or user with administrative privileges, open GRUB configuration file

```bash
# Open with vim
sudo vim /etc/default/grub

# Open with nano
sudo nano /etc/default/grub
```

Locate the line `GRUB_CMDLINE_LINUX_DEFAULT` and add `intel_iommu=on` or `amd_iommu=on` depending on the processor type in your host device.

```bash
# For Intel CPU add
intel_iommu=on


# For AMD CPU add
amd_iommu=on
```

Examples of modified `GRUB_CMDLINE_LINUX` lines:

```bash
# Example 1
GRUB_CMDLINE_LINUX="rhgb quiet intel_iommu=on"

# Example 2
GRUB_CMDLINE_LINUX="crashkernel=auto rd.lvm.lv=rl/root intel_iommu=on"

# Example 3
GRUB_CMDLINE_LINUX="rhgb quiet amd_iommu=on"
```

Run below commands to update GRUB and apply the changes:

```bash
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

For the changes to take effect you must reboot the server.

```bash
sudo shutdown -r now
```

### Confirm if IOMMU is enabled

Once the system has been rebooted, you can verify if IOMMU is enabled by querying the current kernel boot parameters:

```bash
grep -i iommu /proc/cmdline
```

If IOMMU is enabled, then there should be `intel_iommu=on` or `amd_iommu=on` in the output.
