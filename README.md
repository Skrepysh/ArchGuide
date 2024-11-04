# ArchGuide
Тебе оно не надо, правда

# Подготовка

timedatectl set-timezone Europe/Moscow
timedatectl set-ntp true 
cfdisk /dev/sda # 
mkfs.ext4 /dev/root # форматируем корень
mkswap /dev/swap # создаем свап
mkfs.fat -F 32 /dev/efi # форматируем ефи
mount /dev/root /mnt # монтируем корень
mount —mkdir /dev/efi /mnt/boot/efi # монтируем ефи
swapon /dev/swap # вкл свап

# ставим систему

pacstrap -K /mnt base base-devel linux linux-firmware linux-headers grub efibootmgr git sudo nano networkmanager plasma sddm konsole ttf-ubuntu-font-family ttf-hack ttf-opensans ttf-dejavu noto-fonts-cjk net-tools plymouth gnome chromium dolphin kate 

genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt

# Настраиваем систему

ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime
hwclock --systohc
nano /etc/locale.gen # выбираем локали
locale-gen
nano /etc/locale.conf # выбираем язык системы LANG=ru_RU.UTF-8
nano /etc/hostname # ставим хостнейм
useradd -m -G wheel archik
passwd # root password
passwd archik # user password
sudo EDITOR=nano visudo
systemctl enable gdm
systemctl enable networkmanager

# Ставим загрузчик

grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
exit

# Завершение

umount -R /mnt
reboot
