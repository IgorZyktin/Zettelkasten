# Samba

Samba обеспечивает возможность раздавать файлы по сети с локальной машины.



- [{{ компьютеры }}](../../__tags/kompytery.md)
- [{{ программное обеспечение }}](../../__tags/programmnoe_obespechenie.md)
- [{{ linux }}](../../__tags/linux.md)
- [{{ полезные рецепты }}](../../__tags/poleznye_retsepty.md)
- [{{ системное администрирование }}](../../__tags/sistemnoe_administrirovanie.md)
- [{{ Raspberry Pi }}](../../__tags/raspberry_pi.md)
- [{{ Samba }}](../../__tags/samba.md)


## Запуск Samba на raspberry Pi4

Samba обеспечивает возможность раздавать файлы по сети с локальной машины.

### Ход установки

Обновить систему:

```shell
sudo apt-get update
sudo apt-get upgrade
```

Установить поддержку NTFS:

```shell
sudo apt-get install ntfs-3g
```

Найти подключенные жёсткие диски:

```shell
sudo fdisk -l
```

Выхлоп будет примерно таким:

```shell
Device         Boot  Start      End  Sectors  Size Id Type
/dev/mmcblk0p1        8192   532480   524289  256M  c W95 FAT32 (LBA)
/dev/mmcblk0p2      540672 30572543 30031872 14,3G 83 Linux


Disk /dev/sda: 3,8 GiB, 4026531840 bytes, 7864320 sectors
Disk model: Flash Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x196cbb88

Device     Boot Start     End Sectors  Size Id Type
/dev/sda1          63 7864319 7864257  3,8G  7 HPFS/NTFS/exFAT
```

Нас интересует конкретный девайс, в данном случае /dev/sda1.

Добавляем нового пользователя:

```shell
sudo useradd lan_user -m -G users
sudo passwd lan_user
```

Узнаём его параметры:

```shell
id -u lan_user
id -g lan_user
```

Создаём каталог:

```shell
sudo mkdir /media/folder
```

Далее надо отредактировать файл **/etc/fstab**, в конец добавить:

```shell
/dev/sda1 /media/folder auto uid=<user_id>,gid=<group id>,noatime 0 0
```

Сохранить, потом sudo reboot.

Дальше надо ставить самбу:

```shell
sudo apt-get install samba samba-common-bin
```

Сделать копию настроек:

```shell
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.old
```

Дальше в файле **smb.conf** в конце надо добавить примерно вот это:

```
[Anonymous]
comment = Anonymous File Server Share
path = /srv/samba/anonymous_shares
browsable =yes
writable = yes
guest ok = yes
read only = no
force user = nobody
```

В квадратных скобках это имя каталога, как он будет виден.

Перезапустить самбу:

```shell
sudo systemctl restart smbd
```

Добавить пароль пользователя:

```shell
sudo smbpasswd -a lan_user
```

Ставим клиент:

```shell
sudo apt-get install smbclient
sudo apt-get install cifs-utils
smbclient -L //<HOST_IP_OR_NAME>/<folder_name> -U <user>
smbclient //<HOST_IP_OR_NAME>/<folder_name> -U <user>
```

Для удобства можно подмонтировать каталог:

```shell
sudo mount -t cifs -o username=**user**,password=***** //**ip address**/folder /home/user/folder
```

В особо тугих случаях Win 10 может плохо подключаться. Тогда надо пойти в:

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa
```

И выставить **LmCompatibilityLevel** минимум в 3 (можно 5).
