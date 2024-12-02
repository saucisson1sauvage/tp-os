### 🌞 Lister les périphériques de stockage branchés à la machine

```powershell
aa@vbox:~$ lsblk
NAME           MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda              8:0    0   20G  0 disk
└─sda1           8:1    0   20G  0 part
   ├─zizi-root  254:0    0  9.3G  0 lvm  /
   ├─zizi-home  254:1    0  1.9G  0 lvm  [SWAP]
   ├─zizi-var   254:2    0  2.8G  0 lvm  /var
   └─zizi-swap  254:3    0  1.9G  0 lvm  /home
sr0             11:0    1 1024M  0 rom
```

### 🌞 Lister les partitions en cours d'utilisation
```powershell
aa@vbox:~$ df -h
Filesystem               Size  Used Avail Use% Mounted on
udev                    961M     0  961M   0% /dev
tmpfs                   198M  1.2M  196M   1% /run
/dev/mapper/zizi-root   9.1G  3.7G  4.9G  43% /
tmpfs                   986M     0  986M   0% /dev/shm
tmpfs                   5.0M  4.0K  5.0M   1% /run/lock
/dev/mapper/zizi-swap   1.8G   18M  1.7G   2% /home
/dev/mapper/zizi-var    2.7G  420M  2.2G  17% /var
tmpfs                   198M  112K  197M   1% /run/user/1000
```

### 🌞 Lister la quantité d'inodes disponibles sur chaque partition

```powershell
aa@vbox:~$ df -i
Filesystem            Inodes  IUsed  IFree IUse% Mounted on
udev                  245784    397 245387    1% /dev
tmpfs                 252303    700 251603    1% /run
/dev/mapper/zizi-root 610800 140089 470711   23% /
tmpfs                 252303      1 252302    1% /dev/shm
tmpfs                 252303      4 252299    1% /run/lock
/dev/mapper/zizi-swap 121920    416 121504    1% /home
/dev/mapper/zizi-var  183264  11025 172239    7% /var
tmpfs                  50460    121  50339    1% /run/user/1000
```

### 🌞 Déterminer la taille (avec du) et l'inode (avec ls) de chacun des fichiers suivants

```powershell 
aa@vbox:~$ ls -i /boot/vmlinuz* & du /boot/vmlinuz*
[1] 3248
260612 /boot/vmlinuz-5.10.0-32-amd64
6908 /boot/vmlinuz-5.10.0-32-amd64
aa@vbox:~$ which bash
/usr/bin/bash
aa@vbox:~$ ls -i /usr/bin/bash & du /usr/bin/bash
[1] 3276
392241 /usr/bin/bash
1208 /usr/bin/bash
aa@vbox:~$ ls -i /etc/passwd
171247 /etc/passwd
aa@vbox:~$ du /etc/passwd
4 /etc/passwd
```

### 🌞 Repérer le nom des nouveaux disques

```powershell

a@vbox:~$ lsblk
NAME          MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda             8:0    0   20G  0 disk
└─sda1          8:1    0   20G  0 part
  ├─zizi-root 254:0    0  9.3G  0 lvm  /
  ├─zizi-home 254:1    0  1.9G  0 lvm  [SWAP]
  ├─zizi-var  254:2    0  2.8G  0 lvm  /var
  └─zizi-swap 254:3    0  1.9G  0 lvm  /home
sdb             8:16   0   10G  0 disk
sdc             8:32   0   10G  0 disk
sr0            11:0    1 1024M  0 rom

```
### 🌞 Ajouter ces deux disques comme des PV LVM
```powershell

aa@vbox:~$ sudo pvcreate /dev/sdb
  Physical volume "/dev/sdb" successfully created.
aa@vbox:~$ sudo pvcreate /dev/sdc
  Physical volume "/dev/sdc" successfully created.


```

### 🌞 Créer un VG nommé cat

```powershell

aa@vbox:~$ sudo vgcreate cat /dev/sdb
  Volume group "cat" successfully created
aa@vbox:~$ sudo vgextend cat /dev/sdc
  Volume group "cat" successfully extended

  ```

### 🌞 Créer un LV nommé meoooow

```powershell
aa@vbox:~$ sudo lvcreate -L 3G cat -n meoooow
  Logical volume "meoooow" created.

```

### 🌞 Formater la partition (le LV) en autre chose que ext4

```powershell
aa@vbox:~$ sudo mkfs -t btrfs -f /dev/cat/meoooow
btrfs-progs v5.10.1
See http://btrfs.wiki.kernel.org for more information.

Label:              (null)
UUID:               7cfbc188-c503-4072-bc14-0170532e2536
Node size:          16384
Sector size:        4096
Filesystem size:    3.00GiB
Block group profiles:
  Data:             single            8.00MiB
  Metadata:         DUP             256.00MiB
  System:           DUP               8.00MiB
SSD detected:       no
Incompat features:  extref, skinny-metadata
Runtime features:  
Checksum:           crc32c
Number of devices:  1
Devices:
   ID        SIZE  PATH
    1     3.00GiB  /dev/cat/meoooow
```

### 🌞 Créer le point de montage /mnt/meow

```powershell
aa@vbox:~$ sudo mkdir /mnt/meow
```

### 🌞 Monter la partition meoooow sur le point de montage que vous avez créé

```powershell

aa@vbox:~$ sudo mount /dev/cat/meoooow /mnt/meow

```

### 🌞 Vérifier avec la commande adaptée que la partition est utilisable et qu'elle fait 3G

```powershell

aa@vbox:~$ sudo mount | grep /mnt/meow
/dev/mapper/cat-meoooow on /mnt/meow type btrfs (rw,relatime,space_cache,subvolid=5,subvol=/)
aa@vbox:~$ sudo lsblk | grep cat
└─cat-meoooow 254:4    0    3G  0 lvm  /mnt/meow
aa@vbox:~$ sudo mkdir /mnt/meow/jcp
aa@vbox:~$ sudo ls /mnt/meow
jcp

```

### 🌞 Il reste 2G sur le disque dur principal

```powershell
aa@vbox:~$ sudo lvextend -L+2G /dev/mapper/zizi-home
  Size of logical volume zizi/home changed from <1.86 GiB (476 extents) to <3.86 GiB (988 extents).
  Logical volume zizi/home successfully resized.

```
(des trucs chelous se sont passées detruisant 2/3 vms dcp je vais continuer en travaillant sur meoooow en ext4 xd)

```powershell

me@Debian11:~$ sudo resize2fs /dev/cat/meoooow
resize2fs 1.46.2 (28-Feb-2021)
Resizing the filesystem on /dev/cat/meoooow to 1310720 (4k) blocks.
The filesystem on /dev/cat/meoooow is now 1310720 (4k) blocks long.

```

### 🌞 Prouvez en affichant la liste des partitions que la partition fait désormais 2G de plus

```powershell

me@Debian11:/$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            901M     0  901M   0% /dev
tmpfs           186M  904K  185M   1% /run
/dev/sda1        19G  4.1G   14G  23% /
tmpfs           927M     0  927M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           186M   44K  186M   1% /run/user/115
tmpfs           186M   40K  186M   1% /run/user/1000
```
ouai bon il y a plus rien qui fonctionne comme ca devrait...


### 🌞 Agrandir la partition meoooow pour qu'elle occupe tout l'espace libre de son VG

```powershell

me@Debian11:/$ sudo lvextend -l +100%FREE /dev/cat/meoooow
  Size of logical volume cat/meoooow changed from 5.00 GiB (1280 extents) to 19.99 GiB (5118 extents).
    Logical volume cat/meoooow successfully resized.
me@Debian11:/$ sudo resize2fs /dev/cat/meoooow
resize2fs 1.46.2 (28-Feb-2021)
Resizing the filesystem on /dev/cat/meoooow to 5240832 (4k) blocks.
The filesystem on /dev/cat/meoooow is now 5240832 (4k) blocks long.
```

### 🌞 Déterminer la taille de la nouvelle partition

```powershell

me@Debian11:/$ lsblk
NAME          MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda             8:0    0   20G  0 disk
├─sda1          8:1    0   19G  0 part /
├─sda2          8:2    0    1K  0 part
└─sda5          8:5    0  975M  0 part [SWAP]
sdb             8:16   0   10G  0 disk
└─cat-meoooow 254:0    0   20G  0 lvm
sdc             8:32   0   10G  0 disk
└─cat-meoooow 254:0    0   20G  0 lvm
sdd             8:48   0   10G  0 disk
sr0            11:0    1 1024M  0 rom
```

### 🌞 Modifier le fichier /etc/fstab pour que la partition soit montée /mnt/meow automatiquement

```powershell
me@Debian11:/$ sudo blkid /dev/cat/meoooow
/dev/cat/meoooow: UUID="f2fd70f3-bf44-4a29-956c-202c519e3035" BLOCK_SIZE="4096" TYPE="ext4"
me@Debian11:/$ sudo nano /etc/fstab
```
```
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# systemd generates mount units based on this file, see systemd.mount(5).
# Please run 'systemctl daemon-reload' after making changes here.
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda1 during installation
UUID=6f10f11f-3f5f-43c6-8e76-1800aa0def43 /               ext4    errors=remount-ro 0       1
# swap was on /dev/sda5 during installation
UUID=95a98959-9cd9-466e-841c-ce4ef0fc30fb none            swap    sw              0       0
/dev/sr0        /media/cdrom0   udf,iso9660 user,noauto     0       0
UUID=f2fd70f3-bf44-4a29-956c-202c519e3035 /mnt/meow ext4 defaults
```
```powershell
me@Debian11:/$ sudo mount -a
me@Debian11:/$ lsblk
NAME          MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda             8:0    0   20G  0 disk
├─sda1          8:1    0   19G  0 part /
├─sda2          8:2    0    1K  0 part
└─sda5          8:5    0  975M  0 part [SWAP]
sdb             8:16   0   10G  0 disk
└─cat-meoooow 254:0    0   20G  0 lvm
sdc             8:32   0   10G  0 disk
└─cat-meoooow 254:0    0   20G  0 lvm
sdd             8:48   0   10G  0 disk
sr0            11:0    1 1024M  0 rom
```