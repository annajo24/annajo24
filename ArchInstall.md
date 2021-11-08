# Anna Jordan - Installation Documentation

## Start
Open and configure a new virtual machine with VMWare \
Launch with archiso \
Boot in Arch Install Medium
## Configure the Arch VM 
verify the boot mode 
```
ls /sys/firmware/efi/efivars
```
check network connection 
```
ping archlinux.org
```
Update system clock
```
timedatectl set-ntp true
```
Partition the disks
*note: swap partition is not required*
```
mklabel gpt
mkpart "efi_partition" fat32 1MiB 261MiB
set 1 esp true
mkpart "swap_partition" ext4 261MiB 774MiB
mkpart "root_partition" ext4 774MiB 100%
```
Format Partions
```
mkfs.ext4 /dev/sda3
mkswap /dev/sda2
```
Mount the File Systems 
```
mount /dev/sda3 /mnt
swapon /dev/sda2
```
Install Essential Packages
- install base package 
```
pacstrap /mnt base linux linux-firmware
``` 
- change root into new system 
```
arch-chroot /mnt
```
- Install packages 
```
pacman -S man-db
pacman -S man-pages
pacman -S texinfo
pacman -S sudo
```
Install a network manager 
```
pacman -S NewtworkManager
```
Enable the network manager
```
enable NetworkManager
```
Install Grub
```
pacman -S grub
pacman -S efibootmgr
```
Confirm EFI Format
```
pacman -S dosfstools
mkfs.fat -F32 /dev/sda1
```
Mount EFI 
```
mount /dev/sda1 /efi
```
Configure Grub
```
grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```
Set Time Zone
```
ln -sf /usr/share/zoneinfo/Tulsa /etc/localtime
hwclock --systohc
locale-gen
```
Confirm language and keymap defaults
```
echo 'LANG=en_US.UTF-8' > /etc/locale.conf
echo 'KEYMAP=de_latin1' > /etc/vconsole.conf
```
Verify file creation
```
ls /etc 
```
Create host name
```
echo 'aljArchVM' > /etc/hostname
```
Set root password
```
passwd
```
Install a default environment
- *note: I chose LXDE*
- Install  package
```
$ sudo pacman -Sy --noconfirm lxde lxdm
```
-  Enable the DE
```
systemctl enable lxdm
```
- Set as deafult 
```
sudo sed -i /etc/lxdm/lxdm.conf \-e 's;^# session=/usr/bin/startlxde;session=/usr/bin/startlxde;g'
```
Reboot into the new environment
```
reboot
```
## Inside The New Environment
Login as root with root password \
Create Users
```
useradd --create-home anna
useradd --create-home codi
useradd --create-home sal
```
Set password for accounts 
```
passwd anna
passwd codi
passwd sal
```
Force Users to change password on login by setting password status to expired
```
passwd -e anna
passwd -e codi
passwd -e sal
```
Give sudo permissions to users
- Add users to the wheel group
```
usermod -aG wheel anna
usermod -aG wheel codi
usermod -aG wheel sal
```
- Edit sudoers file
```
Visudo 
```
- Remove the comment (#) from this line in the file to activate it
```
# %wheel ALL = (ALL) ALL 
```
- Save and exit 
-- ESC + code
```
:wq 
```
Confirm sudo privilidges 
- *note: looking for "User may run all commands"*
```
sudo -lU anna
sudo -lU codi
sudo -lU sal
```
Install zsh shell
- Install package
```
pacman -S zsh
```
- Set default
``` 
chsh sal -s /usr/bin/zsh
chsh anna -s /usr/bin/zsh
chsh codi -s /usr/bin/zsh
chsh root -s /usr/bin/zsh
```
- Confirm shell
```
echo $SHELL
```
Install ssh
```
pacman -S openssh
```
Add color to terminal
- *for this I installed oh-my-zsh*
```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"e
```
Add Aliases
```
alias c='clear'
alias e='echo'
alias t='touch'
```
Install Web Browser \
*note: I chose Google Chrome*
- Install essential base package
```
sudo pacman -S --needed base-devel git
```
- Install Chrome
```
git clone https://aur.archlinux.org/google-chrome.git
cd google-chrome
makepkg -si
```

















