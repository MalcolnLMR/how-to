# My notes to easily install Arch linux

## Installation on Live Enviroment

### 1 - Download Iso and Prepare media installation

- download Arch iso: https://archlinux.org/download/
- create USB flash installation medium: https://wiki.archlinux.org/title/USB_flash_installation_medium

#### 1.1 Boot live enviroment

Just set your mobo bios to start the pendrive first

### 2 - Setup enviroment

#### 2.1 - Setup keyboard and font

```bash
setfont ter-132n
```
```bash
loadkeys br-latin1-us
```
or `br-abnt2`

#### 2.2 - Create partition table

check which disk will be partitioned with:
```bash
fdisk -l
```
then start partitioning it
```bash
fdisk /dev/the_disk_to_be_partitioned
```
*while in fdisk, use "m" for help, and create the partitions you need, then write it (AKA save and exit)

##### Example layout for GPT
- /boot  | /dev/efi_system_partition | EFI System Partition (1) | At least 1 GiB
- [SWAP] | /dev/swap_partition       | Linux Swap               | At least 4 GiB
- /      | /dev/root_partition       | Linux x86-64 root        | At least 32 GiB

#### 2.3 - Format and Mount the partitions

for root:
```bash
mkfs.ext4 /dv/root_partition
```

for swap:
```bash
mkswap /dev/swap_partition
```

for EFI (Only if its not a Dual Boot):
```bash
mkfs.fat -F 32 /dev/efi_system_partition
```

then mount the partitions:
```bash
mount /dev/root_partition /mnt
```
```bash
mount --mkdir /dev/efi_system_partition /mnt/efi
```
```bash
swapon /dev/swap_partition
```

### 3 - Install Linux

for this step, use pacstrap as below:
```bash
pacstrap -K /mnt base linux linux-firmware amd-ucode neovim networkmanager git sudo man-db
```
or `intel-ucode`

#### 3.1 - Generate file system table (FSTAB)

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```
*if you have done this step twice, remove the last fstab ```rm /mnt/etc/fstab```

#### 3.2 - Chroot to new installed system and set a password

```bash
arch-chroot /mnt
```
```bash
passwd
```

### 4 - Setup user

```bash
useradd -m -g users -G wheel,storage,power,video,audio -s /bin/bash malcolnlmr
```
```bash
passwd malcolnlmr
```
```bash
EDITOR=nano visudo
```
then go to `#%whell ALL=(ALL:ALL) NOPASSWD: ALL` and uncomment it, remind yourself to save and exit.
```bash
su - malcolnlmr
```
```bash
sudo pacman -Syu
```
```bash
exit
```

### 5 - Setup localization
#### 5.1 Timezone
```bash
ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime
```
```bash
hwclock --systohc
```
#### 5.2 Locale
```bash
nano /etc/locale.gen
```
and uncomment `en_US.UTF-8` and any other UTF-8 needed. The generate locale:
```bash
locale-gen
```
check in `/etc/locale.conf` if `LANG=en_US.UTF-8`.<br>
Then setup the console keyboard layout:
```bash
nano /etc/vconsole.conf
```
then set `KEYMAP=br-latin1-us`

### 6 - Network configuration
#### 6.1 - setup hostname
```bash
vim /etc/hostname
```
just write `malcoln-arch`, and its going to be the hostname.
#### 6.2 - setup hosts
```bash
vim /etc/hosts
```
set it to something like: 
- 127.0.0.1     localhost
- ::1           localhost
- 127.0.1.1     malcoln-arch.localdomain    malcoln-arch

### 7 - Installing Bootmanager and grub
```bash
pacman -S grub efibootmgr dosfstools mtools
```
#### 7.1 - Setup grub
```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```
```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

### 8 - Setup and finish live enviroment
```bash
systemctl enable NetworkManager
```
```bash
exit
```
*to exit `arch-chroot /mnt`, then unmount drives:
```bash
umount -lR /mnt
```
```bash
shutdown now
```
now u can unplug your pendrive :D, if windows starts first on reboot, go to BIOS and set the boot order to GRUB be the first.

## Installation on the new system

Your first command (after you login), gonna be:
```bash
setfont -d
```
Gonna be easier to read :D

### 1 - Installing a GUI

```bash
sudo pacman -S xorg sddm plasma-meta plasma-workspace kde-applications
```
its a full, install, if you need less apps, remove `kde-applications` and install only what you'll need.

#### 1.1 - Enable SDDM

```bash
sudo systemctl enable sddm
```
```bash
sudo systemctl start sddm
```
YIPEEEE now you have a fully installed 


