# Grub2

On a Red Hat Enterprise Linux 9 system, the boot loader is the GRand Unified Bootloader version 2 (GRUB2).
RHEL 9 provides a prebuilt /boot/efi/EFI/redhat/grubx64.efi file, which contains the required authentication signatures for a Secure Boot system.
GRUB2 loads its configuration from the /boot/grub2/grub.cfg file for BIOS, and from the /boot/efi/EFI/redhat/grub.cfg file for UEFI, and displays a menu to select which kernel to boot.


# 
Select a Different Target at Boot Time
To select a different target at boot time, append the systemd.unit=target.target option to the kernel command line from the boot loader.

To use this method to select a different target, use the following procedure:
1. Boot or reboot the system.
2. Interrupt the boot loader menu countdown by pressing any key (except Enter, which would initiate a normal boot).
3. Move the cursor to the kernel entry to start.
4. Press e to edit the current entry.
5. Move the cursor to the line that starts with linux which is the kernel command line.
6. Append systemd.unit=target.target, for example, systemd.unit=emergency.target.
7. Press Ctrl+x to boot with these changes.


On Red Hat Enterprise Linux 9, the scripts that run from the initramfs image can be paused at certain points, to provide a root shell, and then continue when that shell exits. This script is mostly meant for debugging, and also to reset a lost root password.

To access that root shell, follow these steps:
1. Reboot the system.
2. Interrupt the boot-loader countdown by pressing any key, except Enter.
3. Move the cursor to the rescue kernel entry to boot (the entry with the rescue word in its name).
4. Press e to edit the selected entry.
5. Move the cursor to the kernel command line (the line that starts with linux).
6. Append rd.break. With that option, the system breaks just before the system hands control from the initramfs image to the actual system.
7. Press Ctrl+x to boot with the changes.
8. Press Enter to perform maintenance when prompted.
9. At this point, the system presents a root shell, and the root file system on the disk is mounted read-only on /sysroot. Because troubleshooting often requires modifying the root file system, you must remount the root file system as read/write. The following step shows how the remount,rw option to the mount command remounts the file system where the new option (rw) is set.

````
mount -o remount,rw /sysroot
chroot /sysroot
passwd root
touch /.autorelabel
exit
exit 
````
