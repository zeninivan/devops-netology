Домашнее задание к занятию "3.5. Файловые системы"


1. Узнайте о sparse (разряженных) файлах.
Почитал, изучил вопрос.
Хороший способ не записывать на диск последовательность нулевых байт внутри файла.


2. Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?

Все хардлинки одного и того же файла фактически указывают на один цифровой объект на диске, т.е. они будут идентичны. 
У них будет общий inode, один и тот же владелец и одинаковые права доступа.

Создадим тестовый файл и посмотрим его статус:
ivan@HP-Pavilion-dv6:~/Testfile$ touch test_file
ivan@HP-Pavilion-dv6:~/Testfile$ stat test_file
  Файл: test_file
  Размер: 0         	Блоков: 0          Блок В/В: 4096   пустой обычный файл
Устройство: 805h/2053d	Инода: 137663      Ссылки: 1
Доступ: (0664/-rw-rw-r--)  Uid: ( 1000/    ivan)   Gid: ( 1000/    ivan)
Доступ:        2022-02-09 15:59:23.482850790 +0300
Модифицирован: 2022-02-09 15:59:23.482850790 +0300
Изменён:       2022-02-09 15:59:23.482850790 +0300
Создан:        -
ivan@HP-Pavilion-dv6:~/Testfile$

Создадим еще один хардлинк для этого файла:
ivan@HP-Pavilion-dv6:~/Testfile$ ln test_file test_link
ivan@HP-Pavilion-dv6:~/Testfile$ stat test_link
  Файл: test_link
  Размер: 0         	Блоков: 0          Блок В/В: 4096   пустой обычный файл
Устройство: 805h/2053d	Инода: 137663      Ссылки: 2
Доступ: (0664/-rw-rw-r--)  Uid: ( 1000/    ivan)   Gid: ( 1000/    ivan)
Доступ:        2022-02-09 15:59:23.482850790 +0300
Модифицирован: 2022-02-09 15:59:23.482850790 +0300
Изменён:       2022-02-09 16:03:10.106586086 +0300
Создан:        -
ivan@HP-Pavilion-dv6:~/Testfile$

ivan@HP-Pavilion-dv6:~/Testfile$ stat test_file
  Файл: test_file
  Размер: 0         	Блоков: 0          Блок В/В: 4096   пустой обычный файл
Устройство: 805h/2053d	Инода: 137663      Ссылки: 2
Доступ: (0664/-rw-rw-r--)  Uid: ( 1000/    ivan)   Gid: ( 1000/    ivan)
Доступ:        2022-02-09 15:59:23.482850790 +0300
Модифицирован: 2022-02-09 15:59:23.482850790 +0300
Изменён:       2022-02-09 16:03:10.106586086 +0300
Создан:        -
ivan@HP-Pavilion-dv6:~/Testfile$

Видим, что один и тот же цифровой объект на диске соответствует двум записям в файловой таблице. 
У файлов одинаковый владелец, групп-id и права доступа.

ivan@HP-Pavilion-dv6:~/Testfile$ ls -ilh
итого 0
137663 -rw-rw-r-- 2 ivan ivan 0 фев  9 15:59 test_file
137663 -rw-rw-r-- 2 ivan ivan 0 фев  9 15:59 test_link
ivan@HP-Pavilion-dv6:~/Testfile$ 


3.  Сделайте vagrant destroy на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
  config.vm.provider :virtualbox do |vb|
    lvm_experiments_disk0_path = "/tmp/lvm_experiments_disk0.vmdk"
    lvm_experiments_disk1_path = "/tmp/lvm_experiments_disk1.vmdk"
    vb.customize ['createmedium', '--filename', lvm_experiments_disk0_path, '--size', 2560]
    vb.customize ['createmedium', '--filename', lvm_experiments_disk1_path, '--size', 2560]
    vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk0_path]
    vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk1_path]
  end
end
Данная конфигурация создаст новую виртуальную машину с двумя дополнительными неразмеченными дисками по 2.5 Гб.

Новая машина создана и запущена:
ivan@HP-Pavilion-dv6:~/Vagrant$ vagrant ssh
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-91-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed 09 Feb 2022 04:25:48 PM UTC

  System load:  1.75               Processes:             126
  Usage of /:   11.2% of 30.88GB   Users logged in:       0
  Memory usage: 20%                IPv4 address for eth0: 10.0.2.15
  Swap usage:   0%

This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
vagrant@vagrant:~$ lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
loop0                       7:0    0 55.4M  1 loop /snap/core18/2128
loop1                       7:1    0 70.3M  1 loop /snap/lxd/21029
loop2                       7:2    0 32.3M  1 loop /snap/snapd/12704
loop3                       7:3    0 55.5M  1 loop /snap/core18/2284
loop4                       7:4    0 43.4M  1 loop /snap/snapd/14549
sda                         8:0    0   64G  0 disk 
├─sda1                      8:1    0    1M  0 part 
├─sda2                      8:2    0    1G  0 part /boot
└─sda3                      8:3    0   63G  0 part 
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm  /
sdb                         8:16   0  2.5G  0 disk 
sdc                         8:32   0  2.5G  0 disk 
vagrant@vagrant:~$


4. Используя fdisk, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.
2гб = 2147483648 байт = 4194304 секторов. 

vagrant@vagrant:~$ sudo fdisk /dev/sdb

Welcome to fdisk (util-linux 2.34).
Command (m for help): g
Created a new GPT disklabel (GUID: A661BA77-9075-2E45-8F0B-9A246316CE07).

Command (m for help): n
Partition number (1-128, default 1): 1
First sector (2048-5242846, default 2048): 2048
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242846, default 5242846): 4196351
Created a new partition 1 of type 'Linux filesystem' and of size 2 GiB.

Command (m for help): n
Partition number (2-128, default 2): 2
First sector (4196352-5242846, default 4196352): 4196352
Last sector, +/-sectors or +/-size{K,M,G,T,P} (4196352-5242846, default 5242846): 5242846
Created a new partition 2 of type 'Linux filesystem' and of size 511 MiB.

Command (m for help): p
Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK   
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: A661BA77-9075-2E45-8F0B-9A246316CE07

Device       Start     End Sectors  Size Type
/dev/sdb1     2048 4196351 4194304    2G Linux filesystem
/dev/sdb2  4196352 5242846 1046495  511M Linux filesystem


5. Используя sfdisk, перенесите данную таблицу разделов на второй диск.

vagrant@vagrant:~$ sudo bash
root@vagrant:/home/vagrant# sfdisk -d /dev/sdb | sfdisk /dev/sdc
Checking that no-one is using this disk right now ... OK

Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK   
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Created a new GPT disklabel (GUID: A661BA77-9075-2E45-8F0B-9A246316CE07).
/dev/sdc1: Created a new partition 1 of type 'Linux filesystem' and of size 2 GiB.
/dev/sdc2: Created a new partition 2 of type 'Linux filesystem' and of size 511 MiB.
/dev/sdc3: Done.

New situation:
Disklabel type: gpt
Disk identifier: A661BA77-9075-2E45-8F0B-9A246316CE07

Device       Start     End Sectors  Size Type
/dev/sdc1     2048 4196351 4194304    2G Linux filesystem
/dev/sdc2  4196352 5242846 1046495  511M Linux filesystem

The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
root@vagrant:/home/vagrant# 


6. Соберите mdadm RAID1 на паре разделов 2 Гб.

Создаем RAID1:
mdadm --create --verbose /dev/md1 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1

vagrant@vagrant:~$ sudo bash
root@vagrant:/home/vagrant# mdadm --create --verbose /dev/md1 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1
mdadm: Note: this array has metadata at the start and
    may not be suitable as a boot device.  If you plan to
    store '/boot' on this device please ensure that
    your boot-loader understands md/v1.x metadata, or use
    --metadata=0.90
mdadm: size set to 2094080K
Continue creating array? y
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md1 started.
root@vagrant:/home/vagrant# 


7. Соберите mdadm RAID0 на второй паре маленьких разделов.

Создаем RAID0:
mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sdb2 /dev/sdc2

root@vagrant:/home/vagrant# mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sdb2 /dev/sdc2
mdadm: chunk size defaults to 512K
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
root@vagrant:/home/vagrant# 

Проверяем статус созданных RAID-массивов:

root@vagrant:/home/vagrant# cat /proc/mdstat
Personalities : [linear] [multipath] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10] 
md0 : active raid0 sdc2[1] sdb2[0]
      1041408 blocks super 1.2 512k chunks
md1 : active raid1 sdc1[1] sdb1[0]
      2094080 blocks super 1.2 [2/2] [UU]
      unused devices: <none>


8. Создайте 2 независимых PV на получившихся md-устройствах.

root@vagrant:/home/vagrant# pvcreate /dev/md0
  Physical volume "/dev/md0" successfully created.
root@vagrant:/home/vagrant# pvcreate /dev/md1
  Physical volume "/dev/md1" successfully created.

root@vagrant:/home/vagrant# pvs
  PV         VG        Fmt  Attr PSize    PFree   
  /dev/md0             lvm2 ---  1017.00m 1017.00m
  /dev/md1             lvm2 ---    <2.00g   <2.00g
  /dev/sda3  ubuntu-vg lvm2 a--   <63.00g  <31.50g
root@vagrant:/home/vagrant# 


9. Создайте общую volume-group на этих двух PV.

root@vagrant:/home/vagrant# vgcreate vg_main /dev/md0 /dev/md1
  Volume group "vg_main" successfully created
root@vagrant:/home/vagrant# 

root@vagrant:/home/vagrant# vgdisplay
  --- Volume group ---
  VG Name               ubuntu-vg
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  2
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               1
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <63.00 GiB
  PE Size               4.00 MiB
  Total PE              16127
  Alloc PE / Size       8064 / 31.50 GiB
  Free  PE / Size       8063 / <31.50 GiB
  VG UUID               aK7Bd1-JPle-i0h7-5jJa-M60v-WwMk-PFByJ7
   
  --- Volume group ---
  VG Name               vg_main
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               <2.99 GiB
  PE Size               4.00 MiB
  Total PE              765
  Alloc PE / Size       0 / 0   
  Free  PE / Size       765 / <2.99 GiB
  VG UUID               qH5Rhf-fxcE-gMgQ-PExN-Z2Ba-HG3f-3Q2WQY
   

10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.

root@vagrant:/home/vagrant# lvcreate -L 100m vg_main /dev/md0
  Logical volume "lvol0" created.
root@vagrant:/home/vagrant# 

root@vagrant:/home/vagrant# lvs
  LV        VG        Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  ubuntu-lv ubuntu-vg -wi-ao----  31.50g                                                    
  lvol0     vg_main   -wi-a----- 100.00m                                                    
root@vagrant:/home/vagrant# 


11. Создайте mkfs.ext4 ФС на получившемся LV.

root@vagrant:/home/vagrant# mkfs.ext4 /dev/vg_main/lvol0
mke2fs 1.45.5 (07-Jan-2020)
Creating filesystem with 25600 4k blocks and 25600 inodes

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (1024 blocks): done
Writing superblocks and filesystem accounting information: done
root@vagrant:/home/vagrant#


12. Смонтируйте этот раздел в любую директорию, например, /tmp/new.

root@vagrant:/tmp# mkdir new
root@vagrant:/tmp/new# mount /dev/vg_main/lvol0 /tmp/new


13. Поместите туда тестовый файл, например wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.

root@vagrant:/tmp/new# wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz
--2022-02-12 13:34:54--  https://mirror.yandex.ru/ubuntu/ls-lR.gz
Resolving mirror.yandex.ru (mirror.yandex.ru)... 213.180.204.183, 2a02:6b8::183
Connecting to mirror.yandex.ru (mirror.yandex.ru)|213.180.204.183|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 22341182 (21M) [application/octet-stream]
Saving to: ‘/tmp/new/test.gz’

/tmp/new/test.gz                                     100%[==========================================>]  21.31M  2.87MB/s    in 8.7s    

2022-02-12 13:35:04 (2.45 MB/s) - ‘/tmp/new/test.gz’ saved [22341182/22341182]
root@vagrant:/tmp/new#


14. Прикрепите вывод lsblk.

root@vagrant:/tmp/new# lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
loop0                       7:0    0 55.4M  1 loop  /snap/core18/2128
loop1                       7:1    0 70.3M  1 loop  /snap/lxd/21029
loop2                       7:2    0 32.3M  1 loop  /snap/snapd/12704
loop3                       7:3    0 55.5M  1 loop  /snap/core18/2284
loop4                       7:4    0 43.4M  1 loop  /snap/snapd/14549
loop5                       7:5    0 61.9M  1 loop  /snap/core20/1328
loop6                       7:6    0 67.2M  1 loop  /snap/lxd/21835
sda                         8:0    0   64G  0 disk  
├─sda1                      8:1    0    1M  0 part  
├─sda2                      8:2    0    1G  0 part  /boot
└─sda3                      8:3    0   63G  0 part  
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm   /
sdb                         8:16   0  2.5G  0 disk  
├─sdb1                      8:17   0    2G  0 part  
│ └─md1                     9:1    0    2G  0 raid1 
└─sdb2                      8:18   0  511M  0 part  
  └─md0                     9:0    0 1017M  0 raid0 
    └─vg_main-lvol0       253:1    0  100M  0 lvm   /tmp/new
sdc                         8:32   0  2.5G  0 disk  
├─sdc1                      8:33   0    2G  0 part  
│ └─md1                     9:1    0    2G  0 raid1 
└─sdc2                      8:34   0  511M  0 part  
  └─md0                     9:0    0 1017M  0 raid0 
    └─vg_main-lvol0       253:1    0  100M  0 lvm   /tmp/new
root@vagrant:/tmp/new# 


15. Протестируйте целостность файла:

root@vagrant:/tmp/new# gzip -t /tmp/new/test.gz
root@vagrant:/tmp/new# echo $?
0
root@vagrant:/tmp/new# 


16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.

root@vagrant:/tmp/new# pvmove -i1 /dev/md0
  /dev/md0: Moved: 12.00%
  /dev/md0: Moved: 100.00%
root@vagrant:/tmp/new# lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
loop0                       7:0    0 55.4M  1 loop  /snap/core18/2128
loop1                       7:1    0 70.3M  1 loop  /snap/lxd/21029
loop2                       7:2    0 32.3M  1 loop  /snap/snapd/12704
loop3                       7:3    0 55.5M  1 loop  /snap/core18/2284
loop4                       7:4    0 43.4M  1 loop  /snap/snapd/14549
loop5                       7:5    0 61.9M  1 loop  /snap/core20/1328
loop6                       7:6    0 67.2M  1 loop  /snap/lxd/21835
sda                         8:0    0   64G  0 disk  
├─sda1                      8:1    0    1M  0 part  
├─sda2                      8:2    0    1G  0 part  /boot
└─sda3                      8:3    0   63G  0 part  
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm   /
sdb                         8:16   0  2.5G  0 disk  
├─sdb1                      8:17   0    2G  0 part  
│ └─md1                     9:1    0    2G  0 raid1 
│   └─vg_main-lvol0       253:1    0  100M  0 lvm   /tmp/new
└─sdb2                      8:18   0  511M  0 part  
  └─md0                     9:0    0 1017M  0 raid0 
sdc                         8:32   0  2.5G  0 disk  
├─sdc1                      8:33   0    2G  0 part  
│ └─md1                     9:1    0    2G  0 raid1 
│   └─vg_main-lvol0       253:1    0  100M  0 lvm   /tmp/new
└─sdc2                      8:34   0  511M  0 part  
  └─md0                     9:0    0 1017M  0 raid0 
root@vagrant:/tmp/new#


17. Сделайте --fail на устройство в вашем RAID1 md.

root@vagrant:/tmp/new# mdadm --fail /dev/md1 /dev/sdb1
mdadm: set /dev/sdb1 faulty in /dev/md1

root@vagrant:/tmp/new# cat /proc/mdstat
Personalities : [linear] [multipath] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10] 
md0 : active raid0 sdc2[1] sdb2[0]
      1041408 blocks super 1.2 512k chunks
      
md1 : active raid1 sdc1[1] sdb1[0](F)
      2094080 blocks super 1.2 [2/1] [_U]
      
unused devices: <none>
root@vagrant:/tmp/new# 


18. Подтвердите выводом dmesg, что RAID1 работает в деградированном состоянии.

root@vagrant:/tmp/new# dmesg | grep md1
[16032.493297] md/raid1:md1: not clean -- starting background reconstruction
[16032.493300] md/raid1:md1: active with 2 out of 2 mirrors
[16032.493328] md1: detected capacity change from 0 to 2144337920
[16032.497516] md: resync of RAID array md1
[16042.996160] md: md1: resync done.
[197287.415819] md/raid1:md1: Disk failure on sdb1, disabling device.
                md/raid1:md1: Operation continuing on 1 devices.
root@vagrant:/tmp/new#


19. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:

root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0

root@vagrant:/tmp/new# gzip -t /tmp/new/test.gz
root@vagrant:/tmp/new# echo $?
0
root@vagrant:/tmp/new# 


20. Погасите тестовый хост, vagrant destroy.

vagrant@vagrant:~$ exit
logout
Connection to 127.0.0.1 closed.
ivan@HP-Pavilion-dv6:~/Vagrant$ vagrant destroy
    default: Are you sure you want to destroy the 'default' VM? [y/N] y
==> default: Forcing shutdown of VM...
==> default: Destroying VM and associated drives...
ivan@HP-Pavilion-dv6:~/Vagrant$ 

