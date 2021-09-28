# devops-netology
Home work 03-sysadmin-05-fs

Домашнее задание к занятию "3.5. Файловые системы"  

1.Узнайте о sparse (разряженных) файлах.  
Ранее сталкивался с таким понятием при резервном копировании виртуальных машин Proxmox

2.Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?  
Не могут, т.к. hard link - это объект полностью идентичный оригинальному файлу.  

3. Сделал. У меня Proxmox, но думаю, что для данной задачи это не принципиально, ОС Ubuntu 20LTS  
4. Используя fdisk, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.  
lsblk выдал мне такую информацию о новых дисках 
sda                         8:0    0  2.5G  0 disk  
sdb                         8:16   0  2.5G  0 disk  
Далее fdisk /dev/sda   
Command (m for help): n  
Partition type  
   p   primary (0 primary, 0 extended, 4 free)  
   e   extended (container for logical partitions)  
Select (default p): p  
Partition number (1-4, default 1):  
First sector (2048-5242879, default 2048):  
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879): +2G
Created a new partition 1 of type 'Linux' and of size 2 GiB.
Command (m for help): n  
Partition type  
   p   primary (1 primary, 0 extended, 3 free)  
   e   extended (container for logical partitions)  
Select (default p): p  
Partition number (2-4, default 2):  
First sector (4196352-5242879, default 4196352):  
Last sector, +/-sectors or +/-size{K,M,G,T,P} (4196352-5242879, default 5242879):  
Created a new partition 2 of type 'Linux' and of size 511 MiB.  
Command (m for help): w  
The partition table has been altered.  
Calling ioctl() to re-read partition table.  
Syncing disks.  

5. Используя sfdisk, перенесите данную таблицу разделов на второй диск.  
Выгрузил файл для sfdisk с помощью команды O fdisk в /root/sda.txt и далее sfdisk /dev/sdb < /root/sda.txt  
6. Соберите mdadm RAID1 на паре разделов 2 Гб.  
mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sda1 /dev/sdb1  
7. Соберите mdadm RAID0 на второй паре маленьких разделов.  
mdadm --create --verbose /dev/md1 --level=0 --raid-devices=2 /dev/sda2 /dev/sdb2  
8. Создайте 2 независимых PV на получившихся md-устройствах.  
root@lab01:/home/root2# pvcreate /dev/md0  
  Physical volume "/dev/md0" successfully created.  
root@lab01:/home/root2# pvcreate /dev/md1  
  Physical volume "/dev/md1" successfully created.  
9. Создайте общую volume-group на этих двух PV  
vgcreate LVMvgTEST /dev/md0 /dev/md1  
10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.  
lvcreate -L 100 -ntestlv  LVMvgTEST /dev/md1  
11. Создайте mkfs.ext4 ФС на получившемся LV
mkfs.ext4 /dev/LVMvgTEST/testlv  
12. Смонтируйте этот раздел в любую директорию, например, /tmp/new.
mount /dev/LVMvgTEST/testlv /mnt  
13. Поместите туда тестовый файл, например wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz
Сделано
14. Прикрепите вывод lsblk
lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
loop0                       7:0    0 55.4M  1 loop  /snap/core18/2128
loop1                       7:1    0 70.3M  1 loop  /snap/lxd/21029
loop2                       7:2    0 32.3M  1 loop  /snap/snapd/12704
loop3                       7:3    0 32.3M  1 loop  /snap/snapd/13170
loop4                       7:4    0 61.8M  1 loop  /snap/core20/1081
loop5                       7:5    0 67.3M  1 loop  /snap/lxd/21545
sda                         8:0    0  2.5G  0 disk
├─sda1                      8:1    0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sda2                      8:2    0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
    └─LVMvgTEST-testlv    253:1    0  100M  0 lvm   /mnt
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdb2                      8:18   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
    └─LVMvgTEST-testlv    253:1    0  100M  0 lvm   /mnt
sr0                        11:0    1 1024M  0 rom
vda                       252:0    0  101G  0 disk
├─vda1                    252:1    0    1M  0 part
├─vda2                    252:2    0    1G  0 part  /boot
└─vda3                    252:3    0  100G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0   50G  0 lvm   /  
15. Протестируйте целостность файла:
root@lab01:/dev/LVMvgTEST#  gzip -t /mnt/test.gz
root@lab01:/dev/LVMvgTEST# echo $?
0
16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.
pvmove /dev/md1 /dev/md0  
File descriptor 4 (pipe:[56150]) leaked on pvmove invocation. Parent PID 15443: bash  
File descriptor 5 (pipe:[56150]) leaked on pvmove invocation. Parent PID 15443: bash  
File descriptor 9 (pipe:[56153]) leaked on pvmove invocation. Parent PID 15443: bash  
  /dev/md1: Moved: 100.00%  
17. Сделайте --fail на устройство в вашем RAID1 md.
mdadm /dev/md0 --fail /dev/sda1  
mdadm: set /dev/sda1 faulty in /dev/md0  
18. Подтвердите выводом dmesg, что RAID1 работает в деградированном состоянии.  
[39329.053304] md/raid1:md0: Disk failure on sda1, disabling device.  
               md/raid1:md0: Operation continuing on 1 devices.  
19. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:  
root@lab01:/dev/LVMvgTEST# gzip -t /mnt/test.gz  
root@lab01:/dev/LVMvgTEST# echo $?  
0  
20. Сделано.














