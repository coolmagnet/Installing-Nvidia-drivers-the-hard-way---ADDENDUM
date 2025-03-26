# [openSUSE] Installing Nvidia drivers - the hard way - ADDENDUM
[openSUSE] Installing Nvidia drivers — the hard way [ADDENDUM]<br>
March 25, 2025


Download the NVIDIA Linux driver


[Optional] Install latest kernel<br>
```bash
zypper in kernel-default
```

Reboot


Install the Kernel Sources and Kernel Development<br>
```bash
KERNEL_VERSION=$(zypper se -s -t package /^kernel-source$/ |grep -E ".+`uname -r | sed 's/-default//'`.+" |cut -d'|' -f4 |xargs); zypper install dkms libglvnd* kernel-devel=$KERNEL_VERSION kernel-default-devel=$KERNEL_VERSION kernel-source=$KERNEL_VERSION
```

Install the rest of Kernel Development and Base Development<br>
```bash
zypper install -t pattern devel_kernel devel_basis
```

Disable Nouveau<br>
```bash
cat <<EOF | sudo tee /etc/modprobe.d/blacklist-nouveau.conf
blacklist nouveau
options nouveau modeset=0
EOF
```

Rebuild Initial Ramdisk<br>
```bash
dracut -f
```

Reboot and boot OS in Multi-user mode by adding these options to the boot kernel at Grub.<br>
```bash
nomodset 3
```

Sudo to root and run the NVIDIA installer<br>
```bash
sh NVIDIA*.run
```


-------------------------

Additional notes:

Build Initial Ramdisk for a specific kernel version<br>
```bash
dracut -f --kver 6.4.0-150600.23.17-default
```
