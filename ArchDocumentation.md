
# Begin
Download from the website using us mirror
booted with the BIOS mode (First option)
was not ufei
didnt have to connect to the internet or set up keyboard since I was in VM
i partitioned the one sda to a primary and its the same size and now im out of space for other primary
drives using:
```bash
fdisk -l /dev/sda
n
sda
defualt
```
formatted the only partition with:
```bash
mkfs.ext4 /dev/sda1
```
mounting the storage using:
```bash
sda1 /mnt
```
## File storage and installing kernel
install linux packages:
```bash
pacstrap -i /mnt base
```
Fstab: generate and fstab to get UUID
```bash
genfstab -U /mnt >> /mnt/etc/fstab
```
add the linux kernel
```bash
pacman -S linux-lts linux-lts-headers
```
add nano
```bash
pacman -S nano
```
add ssh to boot
```bash
systemctl enable sshd
```
network packages
```bash
pacman -S networkmanager wpa_supplicant wireless_tools netctl
```
enable network manager on boot
```bash
systemctl enable NetworkManager
```
install harddrive package
```bash
pacman -S lvm2
```
edit a conf file for hook to add lvm2 support
```bash
nano /etc/mkinitcpio.conf
```
find the link that starts with hook not comment and inbtween block and filesystens add lvm2
mkinitcpio
```bash
mkinitcpio -p linux-lts
```
Localization create the localization files by
```bash
nano /etc/locale.gen
#uncommented en_US.UTF-8 UTF-8
```
generate locale
```bash
locale-gen
```
changed root password
```bash
passwd
#changed it to password
```
added a user for myself
```bash
useradd -m -g users -G wheel eli
```
changed password for eli
```bash
passwd eli
#changed to password
```
install sudo
```bash
pacman -S sudo
```
did a command to give wheel group sudo
```bash
EDITOR=nano visudo
#uncmmend the line about %wheel = all
```
packages for grub
```bash
pacman -S grub dosfstools os-prober mtools
```
install grub
```bash
grub-install --target=i386-pc --recheck /dev/sda
```
generate english boot messages
```bash
cp /usr/share/locale/en\@quot/LC_MESSAGE/grub.mo /boot/grub/locale/en.mo
```
generate grub config
```bash
grub-mkconfig -o /boot/grub/grub.cfg
```
reboot
```bash
reboot
```
login as eli
### post installation tweaks
creating a swap file
```bash
dd if=/dev/zero of=/swapfile bs=1M count=2048 status=progress
```
change perms for swapfile
```bash
chmod 600 /swapfile
```
make a real swap file
```bash
mkswap /swapfile
```
make a copy of the fstab
```bash
cp /etc/fstab /etc/fstab.bak
```
appending line to fstab
```bash
echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab
```
turning on swap
```bash
swapon -a
```
changing time zones
```bash
systemctl set-timezone America/Detriot
```
enable the sync
```bash
systemctl enable systemd-timesyncd
```
set hostname
```bash
hostnamectl set-hostname eliarch
```
edit etc/hosts to include the hostname
```bash
nano /etc/hosts
```
add line for
127.0.0.1 localhost
127.0.1.1 eliarch
install microcode for cpu
```bash
