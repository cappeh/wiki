# Password Recovery

You need an Arch Linux Live USB to boot the system

```bash
# check which partition is the root partition
lsblk

# mount the root partition
mount /dev/nvme0n1p3 /mnt

# enter the installed system
arch-chroot /mnt

# change root password
passwd
# follow on screen prompts to set new password

# change password of user account
passwd <user account>
# follow on screen prompts to set new password

# exit the installed system
exit

# unmount /mnt
umount /mnt

reboot
```

My root filesystem is ext4 and `/dev/nvme0n1p3` is my root partition.
These steps could be different if using a different filesystem or disk encryption
