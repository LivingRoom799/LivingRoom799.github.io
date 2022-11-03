
# Arch Begin
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
mount /dev/sda1 /mnt
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
switch to chroot
```bash
arch-chroot /mnt
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
pacman -S intel-ucode
```
install xorg for the Desktop env
```bash
pacman -S xorg-server
```
install video driver (vm)
```bash
pacman -S virtualbox-guest-utils xf86-video-vmware
```
enable vmbox video card on boot
```bash
systemctl enable vboxservice
```
## Setting up GNOME DE
installing
```bash
pacman -S gnome
```
say yes to all
install gnometweaks as well
```bash
pacman -S gnome-tweaks
```
enable display env
```bash
systemctl enable gdm
```
now reboot into brand new DE!!!
and login
in GNOME
settings changed to dark theme in appearance
display set to native
and language set to english and relog
## other tweaks for the assignment
added a codi user
```bash
sudo useradd -m -g users -G wheel codi
```
changed codi user password
```bash
sudo passwd -e codi
#changed to GraceHopper1906
```
install fish shell
```bash
pacman -S fish
```
in a terminal use fish by:
```bash
fish
```
- changed theme on terminal to dark in the terminal prefrences as well as a color theme for my terminal i picked Tango.
- ssh was installed in my linux packages earlier so to ssh into the class gateway using my ip 10.10.1.129 and password. The VPN was open and
connected on my host computer allowing me to see the private network on my VM.
- System already boots into DE.
- add aliases to .fish
```fish
alias c='clear'
alias e='echo'
alias mkdir='mkdir -pv'
alias cal='gnome-calculator'
```

### ScreenShots
![image](https://user-images.githubusercontent.com/90875780/199777785-ac2e3f3b-a9d6-46bb-8b59-de517e999056.png)
![image](https://user-images.githubusercontent.com/90875780/199777931-4d2fc46d-2467-49f8-9783-7685e105b6f0.png)
![image](https://user-images.githubusercontent.com/90875780/199778033-84717e51-3a70-4f92-a858-8fad938f926d.png)


