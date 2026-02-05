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
* Recover and split the copy of a big data (~ 4G) to a fat32 Disk (max. 4G file)
`split -b 4000M /source-dest/big_file_name ~/archive/big-file-name-split_`
* Merge a splitted files named like split_aa, split_ab, ..., split_az
`cat ~/archive/big-file-name-split_* > ~/target-dest/big-file-name`

### Create a tar archive and split into blocks 4000M (~ 4G)
```cmd
tar -czvf archive.tar.gz /home/disk`
# or
tar -zcvf - file_large.zip | split -b 4000M - files.tar.gz
# then
cat files.tar.gz* | tar -zxv
# to extract use terminal or mc (midnight commander)
tar -xvf files.tar.gz 
unzip files.zip
```

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

### LaTex / TeX / TeXstudio
* Install TeXLive
```cmd
sudo apt update && sudo apt upgrade
sudo apt install texlive
$ 339 M
sudo apt install texlive-latex-extra -y
$ 60+ M
latex --version
sudo apt install texlive-lang-cyrillic
sudo apt install texlive-lang-german
```
* Install TeXstudio
```cmd
sudo apt install texstudio
```
* Start TeXstudio
* TeXstudio -> Optionen -> TeXstudio konfigurieren -> / Erzeugen -> Standardcompiler: PdfLaTeX or XeLaTeX / Editor -> Zeilennummern anzeigen: Alle Zeilennummern
* Write .tex and compile: F5

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

* refind / MacBook <-> Windows <-> Linux
```cmd
# download from https://www.rodsbooks.com/refind
# Run from the terminal to find the EFI partition:
diskutil list
# copy unziped refind-bin-0.14.2 to (exFAT) usb-stick refind-bin-0.14.2
# reboot MacBook and hold Cmd+R
# when the OS has booted, selected Utilities -> Terminal
cd /Volumes/Your-usb-stick/refind-bin-0.14.2
# Run the rEFInd install script:
./refind-install
... Installation has completed successfully. 
reboot
# To Fix rEFInd by problems
# https://gist.github.com/rowanphipps/e4c0e6037b71e9ea96dd8fe403461ee3
# Whenever you install a new OS or somtimes when you install updates, one of the operating systems may decide that you EFI boot selection is all broken and that it needs to be fixed. When this happens then when you reboot it will boot straight into that OS and skip rEFInd. Instructions for how to fix this are below and there is a script in the attached file.
# Restart and hold down the option key or Cmd+R
# This tells the hardware to skip straight to the mac bootloader, allowing you to bypass your broken EFI settings.
# Select your macOS partition to boot from.
# Open terminal
# Mount the ESP volume:
mkdir /Volumes/ESP
mount -t msdos /dev/disk0s1 /Volumes/ESP
#Bless the reFind program
bless --mount /Volumes/ESP --setBoot --file /Volumes/ESP/EFI/REFIND/refind_x64.efi --shortform
```

### Macbook <-> Linux

* Show PC modell
```cmd
sudo dmidecode -s system-product-name
```

* WiFi / Broadcom Corporation BCM4360 802.11ac ...
```cmd
lspci -nn | grep Network
sudo apt install wireless-tools
sudo apt install wpasupplicant
sudo apt install linux-image-$(uname -r|sed 's,[^-]*-[^-]*-,,') linux-headers-$(uname -r|sed 's,[^-]*-[^-]*-,,') broadcom-sta-dkms
sudo modprobe -r b44 b43 b43legacy ssb brcmsmac bcma
sudo apt install network-manager-vpnc vpnc
```

* Bluetooth
```cmd
dmesg | grep -i bluetooth
lsusb | grep Bluetooth
blueman
modprobe btusb
sudo systemctl status bluetooth
sudo systemctl enable bluetooth
sudo systemctl start bluetooth
```

* Facetimehd
```cmd
lspci -v
#Broadcom Inc. 720p FaceTime HD Camera
#steps to install Facetimehd:
#https://github.com/patjak/facetimehd/wiki/Installation
sudo apt install linux-headers-generic git kmod libssl-dev checkinstall
git clone https://github.com/patjak/facetimehd.git
cd facetimehd/
make
sudo checkinstall
sudo make install
sudo depmod
sudo modprobe facetimehd
sudo apt install mplayer
mplayer tv://
#it works...
```

### Network
* Tools
 ```cmd
 su -s
 ip addr
 ip a
 ping ip-address
 netstat -tl (прослушать TCP-порты)
 dig geekbrains.ru (службы DNS)
 nmap geekbrains.ru
 nmcli c show
 more /etc/network/interfaces
 # Ctrl + c
 ```

* if not exist, than:
 ```cmd
 cat /etc/network/interfaces
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
* Apache2 Serv Security
* [Apache HTTP Server vulnerabilities](https://httpd.apache.org/security)
  ```cmd
  $ nano /etc/apache2/apache2.conf (Debian/Ubuntu)
  ServerSignature Off
  ServerTokens Prod
  $ service apache2 restart (Debian/Ubuntu)
  # Indexes off - apache2.conf
  <Directory /var/www/html>
  Options -Indexes
  </Directory>
  $ httpd -v
  # update
  apt-get install apache2
  # Deactivate with comments # :  mod_imap, mod_include, mod_info, mod_userdir, mod_autoindex, ...
  # grep LoadModule /etc/httpd/conf/httpd.conf
  # have to place corresponding 'LoadModule' lines at this location so the
  # LoadModule foo_module modules/mod_foo.so
  LoadModule auth_basic_module modules/mod_auth_basic.so
  ...
  # separate group / user for Apache2
  # /etc/httpd/conf/httpd.conf 
  # groupadd http_web
  # useradd -d /var/www/ -g http-web -s /bin/nologin http_web
  User http_web
  Group http_web
  # httpd.conf.
  <Directory />
  Options None
  Order deny,allow
  Deny from all
  </Directory>
  $ sudo apt-get install libapache2-modsecurity
  $ sudo a2enmod mod-security
  $ sudo /etc/init.d/apache2 force-reload
  # Deactivate links .htaccess
  Options -FollowSymLinks
  # Enable symbolic links
  Options +FollowSymLinks
  # mod_include
  Options -Includes
  Options -ExecCGI
  /var/www/html/web».
  <Directory "/var/www/html/web">
  Options -Includes -ExecCGI
  </Directory>
  #  LimitRequestBody 0 (unlimited) / 500К / 2G
  <Directory "/var/www/myweb/user_uploads">
  LimitRequestBody 512000
  </Directory>
  # DDOS
  TimeOut
  MaxClients
  KeepAliveTimeout
  LimitRequestFields 100 (10 ?)
  LimitRequestFieldSize
  # log mod_log_config
  <VirtualHost *:80>
  DocumentRoot /var/www/html/example.com/
  ServerName www.example.com
  DirectoryIndex index.htm index.html index.php
  ServerAlias example.com
  ErrorDocument 404 /story.php
  ErrorLog /var/log/httpd/example.com_error_log
  CustomLog /var/log/httpd/example.com_access_log combined
  </VirtualHost>
  # SSL mod_ssl
  # openssl genrsa -des3 -out example.com.key 1024
  # openssl req -new -key example.com.key -out exmaple.csr
  # openssl x509 -req -days 365 -in example.com.com.csr -signkey example.com.com.key -out example.com.com.crt
  # add cert to conf Appache
  <VirtualHost 172.16.25.125:443>
  SSLEngine on
  SSLCertificateFile /etc/pki/tls/certs/example.com.crt
  SSLCertificateKeyFile /etc/pki/tls/certs/example.com.key
  SSLCertificateChainFile /etc/pki/tls/certs/sf_bundle.crt
  ServerAdmin ravi.saive@example.com
  ServerName example.com
  DocumentRoot /var/www/html/example/
  ErrorLog /var/log/httpd/example.com-error_log
  CustomLog /var/log/httpd/example.com-access_log common
  </VirtualHost>
  ```
* Server as a router
```cmd
 sudo apt install iptables
 modprobe iptable_nat
 echo 1 > /proc/sys/net/ipv4/ip_forward
 iptables -t nat -A POSTROUTING -o enp3s0 -j MASQUERADE
 iptables -A FORWARD -i enp1s0 -j ACCEPT
 nano /etc/modules
 iptable_nat
 nano /etc/sysctl.conf
 net.ipv4.ip_forward=1
 apt install iptables-persistent
 iptables-save > /etc/iptables/rules.v4
 ping 8.8.8.8
 ping 1.1.1.1
 iptables -t nat -L
```

* Security scan
   ```cmd
   sudo apt install nmap
   nmap -O ip-address/homepage
   nmap -sV ip-address/homepage
   nmap -A -T4 ip-address/homepage
   dpkg -l
   cd ~
   find . -maxdepth 1 -type f -name ".*"
   sudo netstat -tulpn   # open ports
   ```

* Firewall ufw:
  ```cmd
  sudo apt install ufw
  sudo ufw enable
  sudo ufw enable status
  sudo ufw app list
  sudo ufw default deny incoming   # sudo ufw default allow incoming
  sudo ufw default allow outgoing
  sudo ufw status verbose 
  sudo ufw disable   #disable Firewall
  sudo ufw status numbered
  sudo ufw allow OpenSSH
  sudo ufw reset   # reset Firewall
  ```

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
* `sudo dpkg -i PACKAGE.deb`
* list
* `dpkg -l | grep ProgName`
* remove
* `sudo dpkg -P ProgName1`
* dpkg Package if failure / errors 
* `sudo dpkg -i --force-all PACKAGE.deb`
* `sudo dpkg --configure -a`
* `sudo apt-get install -f`
* `sudo autoremove PACKAGE`
* `sudo purge PACKAGE`
* `gunzip GZTARFILE.tar.gz`
* `tar -xvjf TARFILE.tar.gz`
* `sudo ./install.sh`
* Reconfig. Desktop / Server / GUI
* `sudo tasksel`
* time autom.
  ```CMD
  systemd --version
  # systemd 252
  cat /etc/timezone
  cat /etc/localtime
  sudo apt-get autoremove ntp chrony openntpd
  sudo apt-get install systemd-timesyncd
  systemctl status systemd-timesyncd.service
  timedatectl status
  sudo lsof -i
  # no ntp service (OK)
  ```
* locales UTF-8 ... de_DE ru_RU en_EN
  ```CMD
  sudo dpkg-reconfigure locales
  sudo dpkg-reconfigure keyboard-configuration`
  sudo service keyboard-setup restart`
  sudo nano /etc/default/keyboard`
  sudo setxkbmap de`
  sudo setxkbmap ru
  ```
* `sudo apt update`
* `sudo nano /etc/apt/sources.list`
* `sudo apt update`
* history bash show / copy / delete
  ```CMD
  history 1000
  cat ~/.bash_history
  clear
  history -c
  cp ~/.bash_history ~/bash_history-bak_2025-4
  cat /dev/null > ~/.bash_history
  ```
* samba / NAS
* `sudo apt install samba cifs-utils`
* VirtualBox
  ```CMD
  sudo apt install virtualbox
  sudo apt install virtualbox-ext-pack
  #https://www.virtualbox.org/wiki/Downloads
  #Resize the VDI image to MB-SIZE
  cd /home/.../VirtualBox/Your_disk.vdi
  VBoxManage modifyhd Your_disk.vdi –resize size_in_Mb
  #Clone the VMDK image to VDI format
  VBoxManage clonehd source.vmdk cloned.vdi --format vdi
  #Clone back to VMDK format
  VBoxManage clonehd cloned.vdi resized.vmdk --format vmdk
  #Virtualbox Linux
  vboxmanage list -l hdds
  GParted
  resize
  # Windows
  cd C:\Program files\Oracle\VirtualBox
  VBoxManage modifyhd «C:\Users\NameUser\VirtualBox VMs\Staffcop\Staffcop.vdi» --resize X
  ...
  #Virtualbox Win:
  Win+R + diskmgmt.msc
  Расширить том
  ```

### Format W95 FAT32 (LBA)
  ```
  lsblk   #list disks /sdX
  sudo apt install dosfstools
  sudo umount /dev/sdX1  #my sd-card on disk /dev/sdX
  sudo fdisk /dev/sdX
  # o (Enter) - d (delete partition & Enter) - n (Enter) - p (Enter) - ...
  ... - t (Enter) - c (W95 FAT32 (LBA) & Enter) - w (write & Enter)
  # -c set to W95 FAT32 (LBA)
  sudo umount /dev/sdX1
  sudo mkfs.vfat -F 32 -s 2 -S 4096 -v /dev/sdX1   # Format to fat32 with a logical sector size is 4096  # -n nameSD
  sudo fdisk -l #W95 FAT32 (LBA)
  ```

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
