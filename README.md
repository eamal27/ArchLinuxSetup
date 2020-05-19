# ArchLinuxSetup
Guide to setting up Arch Linux in VMWare 

### List block devices and create partitions
```
# lsblk

# cfdisk /dev/sda
```

Create disk type gpt and swap

### Format and mount

```
# mkfs.vfat -F32 /dev/sda1
# mkfs.ext4 /dev/sda2
# mkswap /dev/sda3
# swapon /dev/sda3

# mount /dev/sda2 /mnt
# mkdir /mnt/boot
# mount /dev/sda1 /mnt/boot
```

```
# lsblk

-----output------
sda1  8:1  0  512M  0 part  /mnt/boot
sda2  8:2  0    8G  0 part  /mnt
sda3  8:3  0  1.5G  0 part  [SWAP]
```

### Install packages

```
# pacstrap /mnt base base-devel linux linux-firmware grub vim bash-completion openssh
```

### Generate fstab

```
# genfstab -U /mnt >> /mnt/etc/fstab
```

### arch-chroot into the filesystem

```
# arch-chroot /mnt
```

### Set root password

```
# passwd
```

### Add a user

```
# useradd -m USERNAME -G wheel
# passwd USERNAME
```

### Set Timezone

```
# ln -s /usr/share/zoneinfo/America/Toronto /etc/localtime
```

### Generate languages

uncomment languages in locale.gen to be generated i.e. en_US.UTF-8 UTF-8

```
# vim /etc/locale.gen

# locale-gen
```

### Create initial ramdisk environment for booting the linux kernel

```
# mkinitcpio -p linux
```

### Install Grub

```
# grub-install --efi-directory=/boot
# grub-mkconfig -o /boot/grub/grub.cfg
```

### Enable services to run at startup

```
# systemctl enable dhcpcd sshd
```

### Exit and reboot

```
# exit
# shutdown -r 0
```
