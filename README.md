# ArchGuide
Тебе оно не надо, правда

# Подготовка

```
timedatectl set-timezone Europe/Moscow
timedatectl set-ntp true 
```
```
cfdisk /dev/sda 
```
```
mkfs.ext4 /dev/root
mkswap /dev/swap
mkfs.fat -F 32 /dev/efi
```
```
mount /dev/root /mnt
mount —mkdir /dev/efi /mnt/boot/efi 
swapon /dev/swap
```

# Ставим систему

```
pacstrap -K /mnt base base-devel linux linux-firmware linux-headers grub efibootmgr git sudo nano networkmanager plasma sddm konsole ttf-ubuntu-font-family ttf-hack ttf-opensans ttf-dejavu noto-fonts-cjk net-tools plymouth gnome chromium dolphin kate 
```

```
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
```

# Настраиваем систему
```
ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime
hwclock --systohc
```
### Выбираем локали
```
nano /etc/locale.gen
```
### Генерируем локали
```
locale-gen
```
### Выбираем язык системы LANG=ru_RU.UTF-8
```
nano /etc/locale.conf
```
### Ставим хостнейм
```
nano /etc/hostname
```
### Настраиваем юзеров
```
useradd -m -G wheel archik
```
```
passwd
```
```
passwd archik
```
### Даем группе wheel доступ к sudo
```
sudo EDITOR=nano visudo
```
### Включаем службы
```
systemctl enable gdm
systemctl enable networkmanager
```

# Ставим загрузчик

```
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
```
```
grub-mkconfig -o /boot/grub/grub.cfg
```
### Выходим из chroot
```
exit
```

# Завершение

```
umount -R /mnt
reboot
```
