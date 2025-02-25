# linux-cheat-list
Чит-лист шпаргалка Линукс & Винда

## Linux (Debian / Ubuntu / Kali-Linux)

### Bash Commands
* [Download new stable Debian-Linux Live-Distribution](https://www.debian.org/distrib/)
  or Ubuntu, Kali-linux, etc...
* USB Boot-Disk
  * `sudo diskutil list`
  * `sudo diskutil eraseDisk FAT32 USB /dev/disk3`
  * `sudo diskutil umountDisk /dev/disk3`
  * `cd Downloads`
  * `ls -l`
  * `sudo dd if=./debian-12...iso of=/dev/disk3 bs=5M status=progress`
  * ... CD, DVD `bs=2048`

* Live-CD:
  * `user: user`
  * `passwd: live`
* `sudo nano /etc/apt/sources.list`
* [Source list Debian](https://wiki.debian.org/SourcesList)
* `sudo apt update`
* Show system and kernel
`uname -a`
* Show distribution
 * `cat /etc/debian_version`
 * `lsb_release -a`
 * `lsb_release -d`
* Show your username
`whoami`
* add username to sudo
* `su - root`
* `usermod -aG sudo username`
* `reboot`
* login username
* `su - username`

### Directory Operations
* Show all incl. hidden `ls -a`
* Show all as a list `ls -lh`
* Recover and split the copy of a big data (>= 4G) to a fat32 Disk (max. 4G file)
`split -b 4000M /source-dest/big_file_name ~/archive/big-file-name-split_`
* Merge a splitted files named like split_aa, split_ab, ..., split_az
`cat ~/archive/big-file-name-split_* > ~/target-dest/big-file-name`

### Users / groups
* Show all users / groups `cat /etc/passwd`

### Clean a linux distr.
* `sudo apt autoremove`
* `sudo apt autoclean`
* Check with Ncdu
  * Install Ncdu `sudo apt install ncdu`
  * and start `ncdu /`
  * Clean dir with `d` (to delete). Be carefull!
  * `/var/log/journal/`
  * `/var/tmp/`
  * `/var/spool/`
  * `/tmp/`

### PDF
* PDF convert gs
 * `sudo apt-get install ghostscript libtiff-tools`
 * `ghostscript -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf input.pdf`
  * print (300 dpi)
  * ebook (150 dpi)
  * screen (75 dpi)
 * `gs -o out.pdf -sDEVICE=pdfwrite -sPageList=1,5,7,12 -f input.pdf output.pdf`

* PDF convert pdftk
 * `sudo apt install pdftk`
 * `pdftk A=input.pdf cat A1-2 A5 A7-8 A11-12 output output.pdf`
  * A1-2 A5 pages no.

* PDF merge pdftk
 * `pdftk file1.pdf file2.pdf cat output mergedfile.pdf`

* convert tiff to pdf
 * `tiff2pdf -o output.pdf input.tif`

### .iso boot disk
* `lsblk`
* `sudo umount /dev/sd<?><?>`
* `sudo dd bs=4M if=path/to/input.iso of=/dev/sd<?> conv=fdatasync status=progress`

### grub-pc / refind
* grub
 * `apt install grub-pc`
 * `grub-install /dev/sda`
 * `update-grub`

* boot efi
 * `root@dlinux:/boot/efi/EFI/Debian# cat /boot/efi/EFI/Debian/grub.cfg`
 * `search.fs_uuid disk-0000-111c-b222-9a...b root hd0,msdos4`
 * `set prefix=($root)'/boot/grub'`
 * `configfile $prefix/grub.cfg`

* refind
 * `apt search refind` or download
 * `bash /home/../../refind-bin-0.14.0.2/refind-install`

### Macbook <-> Linux
* WiFi / Broadcom Corporation BCM4360 802.11ac ...
  * `lspci -nn | grep Network`
  * `sudo apt install wireless-tools`
  * `sudo apt install wpasupplicant`
  * `sudo apt install linux-image-$(uname -r|sed 's,[^-]*-[^-]*-,,') linux-headers-$(uname -r|sed 's,[^-]*-[^-]*-,,') broadcom-sta-dkms`
  * `sudo modprobe -r b44 b43 b43legacy ssb brcmsmac bcma`
  * `aptitude install network-manager-vpnc vpnc`

* Bluetooth
  * `dmesg | grep -i bluetooth`
  * `lsusb | grep Bluetooth`
  * blueman
  * `modprobe btusb`
  * `sudo systemctl status bluetooth`
  * `sudo systemctl enable bluetooth`
  * `sudo systemctl start bluetooth`

* Facetimehd
  * `lspci -v`
    * Broadcom Inc. 720p FaceTime HD Camera
   
  * steps to install Facetimehd
    ```
    sudo apt install git
    cd /etc/local/src/
    sudo git clone https://github.com/patjak/bcwc_pcie.git
    ls
    cd bcwc_pcie/firmware
    ls
    sudo git clone https://github.com/patjak/facetimehd-firmware.git
    cd facetimehd-firmware
    sudo make install
    cd ../..
    sudo make install
    sudo depmod    
    sudo modprobe -r bdc-pci
    sudo modprobe facetimehd
    
    (---
    sudo apt install kmod
    cd ~/bcwc_pcie
    sudo make install
    sudo depmod
    ---)
    ```

  * [Facetimehd wiki / github](https://github.com/patjak/facetimehd/wiki)
  * Install the missing Debian dependencies to extract the firmware
    * `sudo apt install xz-utils curl cpio make`
  * Extract and install the firmware file as described in Firmware extraction.
  * Install the dependencies: 
    * `sudo apt install linux-headers-generic git kmod libssl-dev checkinstall`
  * Clone the driver's code: 
    * `git clone https://github.com/patjak/facetimehd.git`
  * Change into that dir: `$ cd facetimehd`
  * Build the kernel module: `$ make`
  * Generate dkpg and install the kernel module, this is easy to uninstall later:
    * `sudo checkinstall`
  * Alternatively if you are really lazy just:
    * `sudo make install`
  * Run depmod for the kernel to be able to find and load it:
    * `sudo depmod`
  * Load kernel module: `#sudo modprobe facetimehd`
  * try it out: `$ mplayer tv://`
 

### Network
* Tools
 ```cmd
 su -s
 ip addr
 netstat -tl (прослушать TCP-порты)
 dig geekbrains.ru (службы DNS)
 nmap geekbrains.ru
 nmcli c show
 iptables -t nat -L
 ```

`cat /etc/network/interfaces` (if not exist, than)
 ```cmd
 apt install ifupdown
 nano /etc/network/interfaces
 service networking restart
 ip addr
 cat /etc/resolv.conf
 ```
* Ручная настройка сети:
 ```cmd
 ip link set enp0s3 up
 ip addr add 192.IP.../255.255.255.0 broadcast 192.IP... dev enp0s3
 ip route add default via 192.IP...
 ip a s
 ```
 * Ручная настройка DNS:
 ```cmd
 dig geekbrains.ru
 ping -c 4 geekbrains.ru
 nano /etc/resolv.conf
 ```

* DHCP, проверить получение адреса
 ```cmd
 cat /etc/network/interfaces
 auto enp0s3 … dhcp
 service networking restart
 dig geekbrains.ru
 ping -c 4 geekbrains.ru
 ```

* Изменить адрес DNS
 ```cmd
 nano /etc/resolv.conf
 nano /etc/network/interfaces
 service networking restart
 dig geekbrains.ru
 ping -c 4 geekbrains.ru
 ```

### Server Apache2 Mysql Phpmyadmin Ufw Security
* Apache2 Serv `sudo apt install apache2`

* Firewall ufw `sudo apt install ufw`
 * `sudo ufw enable`
 * `sudo enable status`
 * `sudo ufw app list`

* Linux DrWeb Antivir
  [Download DrWeb](https://www.drweb.by/saas/support/install/)
  Далее
  ```cmd
  wget -O - http://repo.drweb.com/drweb/drweb.key | apt-key add -
  sudo nano /etc/apt/sources.list
   ```
  [Add to source list](https://repo.drweb.com/drweb/debian/dists/11.1/non-free/binary-amd64/)
  ```cmd
  deb https://repo.drweb.com/drweb/debian 11.1 non-free
   ```
  Istall demo per. 30 d
  ```cmd
  sudo apt update
  sudo apt install drweb-workstations
  ```
  
* MySQL install
   ```cmd
   sudo apt-get install mysql-server mysql-client mysql-common php7.0-mysql
   mysql_secure_installation
   mysql -u root -p
   1234
   ```
* Установить php7.4 or higher и phpmyadmin
  ```cmd
  sudo apt-get -y install php7.0 libapache2-mod-php7.0 php7.0-mysql php7.0-curl php7.0-json
  sudo apt install phpmyadmin -y
  ```

### Usefull
* `apt install firmware-linux`
* `apt install gparted`
* `apt install openssh-server`
* `apt install mc`
* `apt install putty`
* `apt install build-essential libssl-dev libffi-dev python3-dev python3 pandas`
* `pip install pandas`
* `sudo apt search network-manager-vpnc`
* `sudo apt install vpnc`
* `sudo apt install network-manager-openconnect`
* `sudo apt search network-manager`
* `sudo apt install sane-utils`
* `sudo apt install alien`
* `sudo dpkg -i PACKAGE.deb`
* dpkg Package if failure / errors 
* `sudo dpkg -i --force-all PACKAGE.deb`
* `sudo dpkg --configure -a`
* `sudo apt-get install -f`
* `sudo autoremove PACKAGE`
* `sudo purge PACKAGE`
* `gunzip GZTARFILE.tar.gz`
* `tar -xvjf TARFILE.tar.gz`
* `sudo ./install.sh`
* locale
* `sudo tasksel`
* `sudo dpkg-reconfigure keyboard-configuration`
* `sudo service keyboard-setup restart`
* `sudo nano /etc/default/keyboard`
* `sudo setxkbmap de`
* `sudo setxkbmap ru`
* `sudo apt update`
* `sudo nano /etc/apt/sources.list`
* `sudo apt update`
* `ping ip-address`
* `history 1000`
* `clear`

### Format W95 FAT32
* `lslbk` list disks
* `sudo apt install dosfstools`
* `sudo umount /dev/sda1` my sd-card on disk /dev/sda
* `sudo fdisk /dev/sda`
> `o` (Enter) - `d` (delete partition & Enter) - `n` (Enter) - `p` (Enter) - `t` (Enter) - `c` (W95 FAT32 (LBA) & Enter) - `w` (write & Enter)
* `sudo mkfs.vfat -F 32 -n name /dev/sda1`
* `sudo fdisk -l` W95 FAT32 (LBA)

---

## Windows 10 MRB repair / BCD
`/media/USER/DISK/Windows/System32/config/`

BCD-Template [1]
`WS\System32\config\BCD-Template`

* Если BCD шаблоны в этой папке повреждены или удалены, попробуйте проверить целостность системных файлов в офлайн режиме с помощью утилиты sfc (понадобится установочный диск с Windows – диск D:): `sfc /scanow /OFFBOOTDIR=C:\ /OFFWINDIR=D:\WINDOWS`
  * BFSVC Error: Error copying boot files Last Error = 0x570 – попробуйте выполнить проверку диска с помощью команды `CHKDSK M: /F`
  * BFSVC Error: Failed to set element application device. Status = [c000000bb] – проверьте с помощью chkdsk.exe разделы с EFI и Windows 10. Проверьте, что снят атрибут скрытый и системный у файла BCD. Удалите его: `attrib -s -h \EFI\Microsoft\Boot\BCD 
  del \EFI\Microsoft\Boot\BCD`
  * `bcdboot` ошибка BFSVC Error
  * Failure when initializing library system volume – проверьте, что вы используете правильный FAT32 раздел с EFI (возможно у вас из несколько);
  * Failure when attempting to copy boot files – проверьте букву диска Windows в команде. На скриншоте ошибка появилась при попытке скопировать файлы загрузки с диска C:. В данном случае диску с Windows назначена другая буква, например D:. Вы можете найти диск с Windows и назначенную букву с помощью diskpart (описано выше).bcdboot ошибка копирования загрузочных файлов

Перезагрузите компьютер, отключите загрузочный диск. Если вы все сделали правильно, в выборе устройств загрузки должен появиться пункт Windows Boot Manager в котором можно выбрать загрузку нужной операционной системы. Ваш EFI загрузчик и конфигурация BCD успешно восстановлены!
В некоторых случаях после восстановления BCD загрузчика, при загрузке Windows появляется ошибка BAD SYSTEM CONFIG INFO . Чтобы исправить ошибку:
* Убедитесь, что вы не вносили недавно изменения в настройки UEFI
* Загрузитесь с установочной/загрузочной флешки и измените конфигурацию загрузчика командами:
  * `bcdedit /deletevalue {default} numproc`
  * `bcdedit /deletevalue {default} truncatememory` 

> Благодарность автору 1. [winitpro](https://winitpro.ru/index.php/2014/03/20/repair-bootloader-windows-8-uefi) 2. [remontka.pro](https://remontka.pro/files-integrity-windows-10/)

---
_vers. 1.0_
