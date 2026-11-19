# Grub2

On a Red Hat Enterprise Linux 9 system, the boot loader is the GRand Unified Bootloader version 2 (GRUB2).
RHEL 9 provides a prebuilt /boot/efi/EFI/redhat/grubx64.efi file, which contains the required authentication signatures for a Secure Boot system.
GRUB2 loads its configuration from the /boot/grub2/grub.cfg file for BIOS, and from the /boot/efi/EFI/redhat/grub.cfg file for UEFI, and displays a menu to select which kernel to boot.


# Select a Different Target at Boot Time
To select a different target at boot time, append the systemd.unit=target.target option to the kernel command line from the boot loader.

To use this method to select a different target, use the following procedure:
1. Boot or reboot the system.
2. Interrupt the boot loader menu countdown by pressing any key (except Enter, which would initiate a normal boot).
3. Move the cursor to the kernel entry to start.
4. Press e to edit the current entry.
5. Move the cursor to the line that starts with linux which is the kernel command line.
6. Append systemd.unit=target.target, for example, systemd.unit=emergency.target.
7. Press Ctrl+x to boot with these changes.


# Reset the Root Password

On Red Hat Enterprise Linux 9, the scripts that run from the initramfs image can be paused at certain points, to provide a root shell, and then continue when that shell exits. This script is mostly meant for debugging, and also to reset a lost root password.

1. Reboot server
2. Interrupt the countdown in the boot-loader menu.
3. Edit rescue kernet entry to abort boot process just after kernel mounts all filesystems and before systemd takes control.
4. Press "e" to edit, and append rd.break at the end of the line that starts with linux.
5. Press Ctrl+X to boot ising modified config
6. Press enter to perform tasks.
7.  At this point, the system presents a root shell, and the root file system on the disk is mounted read-only on /sysroot. Because troubleshooting often requires modifying the root file system, you must remount the root file system as read/write.
8. Remount /sysroot ````mount -o remount,rw /sysroot````
9. Chroot to /sysroot ````chroot /sysroot````
10. Change password to root  ````passwd root````
11. Config system to perform a system SELinux relabeling ````touch /.autorelabel````
12. Exit twice con continue booting````exit; exit````
   
