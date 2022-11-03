## Download from the website using us mirror

booted with the BIOS mode (First option)
	
was not ufei

didnt have to connect to the internet or set up keyboard since I was in VM

i partitioned the one sda to a primary and its the same size and now im out of space for other primary drives using:

```
fdisk -l /dev/sda
n
sda
defualt
```

formatted the only partition with:

```
mkfs.ext4 /dev/sda1 
```

mounting the storage using:

```
mount /dev/sda1 /mnt
```

install linux packages:

```
pacstrap -i /mnt base
```

change root into the new system:

```
arch-chroot /mnt
```

add the linux kernel

```
pacman -S linux-lts linux-lts-headers
```

add nano

```
pacman -S nano
```

add ssh to boot

```
systemctl enable sshd
```

network packages

```
pacman -S networkmanager wpa_supplicant wireless_tools netctl
```

enable network manager on boot

```
systemctl enable NetworkManager
```

install harddrive package

```
pacman -S lvm2
```

edit a conf file for hook to add lvm2 support

```
nano /etc/mkinitcpio.conf
```

find the link that starts with hook not comment and inbtween block and filesystens add lvm2 mkinitcpio

```
mkinitcpio -p linux-lts
```

Localization create the localization files by

```
nano /etc/locale.gen
#uncommented en_US.UTF-8 UTF-8 
```

generate locale

```
locale-gen
```

changed root password

```
passwd
#changed it to password 
```

added a user for myself

```
useradd -m -g users -G wheel eli
```

changed password for eli

```
passwd eli
#changed to password
```

install sudo

```
pacman -S sudo
```

did a command to give wheel group sudo

```
EDITOR=nano visudo
#uncmmend the line about %wheel = all
```

packages for grub

```
pacman -S grub dosfstools os-prober mtools
```

install grub

```
grub-install --target=i386-pc --recheck /dev/sda
```

generate english boot messages

```
cp /usr/share/locale/en\@quot/LC_MESSAGE/grub.mo /boot/grub/locale/en.mo
```

generate grub config

```
grub-mkconfig -o /boot/grub/grub.cfg
```

reboot

```
reboot
```

login as eli

### [](https://github.com/LivingRoom799/LivingRoom799.github.io/blob/main/README.md#post-installation-tweaks)

### post installation tweaks

creating a swap file

```
dd if=/dev/zero of=/swapfile bs=1M count=2048 status=progress
```

change perms for swapfile

```
chmod 600 /swapfile
```

make a real swap file

```
mkswap /swapfile
```

make a copy of the fstab

```
cp /etc/fstab /etc/fstab.bak
```

appending line to fstab

```
echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab 
```

turning on swap

```
swapon -a
```

changing time zones

```
systemctl set-timezone America/Detriot
```

enable the sync

```
systemctl enable systemd-timesyncd
```

set hostname

```
hostnamectl set-hostname eliarch
```

edit etc/hosts to include the hostname

```
nano /etc/hosts
```

add line for 127.0.0.1 localhost 127.0.1.1 eliarch

install microcode for cpu

```
pacman -S intel-ucode
```

install xorg for the Desktop env

```
pacman -S xorg-server
```

install video driver (vm)

```
pacman -S virtualbox-guest-utils xf86-video-vmware
```

enable vmbox video card on boot

```
systemctl enable vboxservice
```

## [](https://github.com/LivingRoom799/LivingRoom799.github.io/blob/main/README.md#setting-up-gnome-de)

## Setting up GNOME DE

installing

```
pacman -S gnome
```

say yes to all

install gnometweaks as well

```
pacman -S gnome-tweaks
```

enable display env

```
systemctl enable gdm
```

now reboot into brand new DE!!! and login

in GNOME settings changed to dark theme in appearance display set to native and language set to english and relog

todo... 
add codi usr with sudo 
color code terminal 
install a new terminal 
make github pages