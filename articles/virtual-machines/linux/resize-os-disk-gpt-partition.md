---
title: Ändern der Größe eines Betriebssystem-Datenträgers, der eine GPT-Partition aufweist | Microsoft-Dokumentation
description: Dieser Artikel enthält Anweisungen zum Ändern der Größe eines Betriebssystem-Datenträgers, der eine GPT-Partition aufweist.
services: virtual-machines-linux
documentationcenter: ''
author: kailashmsft
manager: dcscontentpm
editor: ''
tags: ''
ms.service: security
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.devlang: azurecli
ms.date: 05/03/2020
ms.author: kaib
ms.custom: seodec18
ms.openlocfilehash: 30a960c3ed76788158b15022947fec49a95ae299
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89375209"
---
# <a name="resize-an-os-disk-that-has-a-gpt-partition"></a>Ändern der Größe eines Betriebssystem-Datenträgers, der eine GPT-Partition aufweist

> [!NOTE]
> Dieses Szenario gilt nur für Betriebssystem Datenträger mit einer GPT-Partition (GUID-Partitionstabelle).

In diesem Artikel wird beschrieben, wie Sie die Größe eines Betriebssystem-Datenträgers heraufsetzen, der eine GPT-Partition unter Linux aufweist. 

## <a name="identify-whether-the-os-disk-has-an-mbr-or-gpt-partition"></a>Bestimmen, ob der Betriebssystem-Datenträger eine MBR- oder eine GPT-Partition aufweist

Verwenden Sie den Befehl `parted`, um zu ermitteln, ob die Datenträgerpartition mit einer MBR-Partition (Master Boot Record) oder einer GPT-Partition erstellt wurde.

### <a name="mbr-partition"></a>MBR-Partition

In der folgenden Ausgabe zeigt **Partition Table**  den Wert **msdos**. Dieser Wert kennzeichnet eine MBR-Partition.

```
[user@myvm ~]# parted -l /dev/sda
Model: Msft Virtual Disk (scsi)
Disk /dev/sda: 107GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Number  Start   End     Size    Type     File system  Flags
1       1049kB  525MB   524MB   primary  ext4         boot
2       525MB   34.4GB  33.8GB  primary  ext4
[user@myvm ~]#
```

### <a name="gpt-partition"></a>GPT-Partition

In der folgenden Ausgabe zeigt **Partition Table**  den Wert **gpt**. Dieser Wert kennzeichnet eine GPT-Partition.

```
[user@myvm ~]# parted -l /dev/sda
Model: Msft Virtual Disk (scsi)
Disk /dev/sda: 68.7GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system  Name                  Flags
1       1049kB  525MB   524MB   fat16        EFI System Partition  boot
2       525MB   1050MB  524MB   xfs
3       1050MB  1052MB  2097kB                                     bios_grub
4       1052MB  68.7GB  67.7GB                                     lvm
```

Wenn Ihr virtueller Computer (VM) eine GPT-Partition für Ihren Betriebssystem-Datenträger aufweist, setzen Sie die Größe des Betriebssystem-Datenträgers herauf.

## <a name="increase-the-size-of-the-os-disk"></a>Heraufsetzen der Größe des Betriebssystem-Datenträgers

Die folgenden Anweisungen betreffen Linux-gestützte Verteilungen.

> [!NOTE]
> Bevor Sie fortfahren, erstellen Sie eine Sicherungskopie Ihrer VM oder eine Momentaufnahme Ihres Betriebssystem-Datenträgers.

### <a name="ubuntu"></a>Ubuntu

So setzen Sie die Größe des Betriebssystem-Datenträgers in Ubuntu 16.x und 18.x herauf:

1. Beenden Sie den virtuellen Computer.
1. Heraufsetzen der Größe des Betriebssystem-Datenträgers im Portal.
1. Führen Sie einen Neustart der VM aus, und melden Sie sich dann bei der VM als **Root**-Benutzer an.
1. Vergewissern Sie sich, dass der Betriebssystem-Datenträger jetzt ein größeres Dateisystem anzeigt.

Wie im folgenden Beispiel gezeigt, wurde der Betriebssystem-Datenträger im Portal auf 100 GB vergrößert. Das auf **/** eingebundene Dateisystem **/dev/sda1** zeigt jetzt 97 GB an.

```
user@myvm:~# df -Th
Filesystem     Type      Size  Used Avail Use% Mounted on
udev           devtmpfs  314M     0  314M   0% /dev
tmpfs          tmpfs      65M  2.3M   63M   4% /run
/dev/sda1      ext4       97G  1.8G   95G   2% /
tmpfs          tmpfs     324M     0  324M   0% /dev/shm
tmpfs          tmpfs     5.0M     0  5.0M   0% /run/lock
tmpfs          tmpfs     324M     0  324M   0% /sys/fs/cgroup
/dev/sda15     vfat      105M  3.6M  101M   4% /boot/efi
/dev/sdb1      ext4       20G   44M   19G   1% /mnt
tmpfs          tmpfs      65M     0   65M   0% /run/user/1000
user@myvm:~#
```

### <a name="suse"></a>SUSE

So setzen Sie die Größe des Betriebssystem-Datenträgers in SUSE 12 SP4, SUSE SLES 12 for SAP, SUSE SLES 15 und SUSE SLES 15 for SAP herauf:

1. Beenden Sie den virtuellen Computer.
1. Heraufsetzen der Größe des Betriebssystem-Datenträgers im Portal.
1. Starten Sie den virtuellen Computer neu.

Wenn der Neustart der VM erfolgt ist, führen Sie die folgenden Schritte aus:

1. Greifen Sie als **Root**-Benutzer auf Ihre VM zu, und verwenden Sie dazu den folgenden Befehl:

   ```
   # sudo -i
   ```

1. Verwenden Sie den folgenden Befehl, um das Paket **growpart** zu installieren, das zum Ändern der Größe der Partition verwendet wird:

   ```
   # zypper install growpart
   ```

1. Verwenden Sie den Befehl `lsblk`, um die Partition zu suchen, die im Stammverzeichnis des Dateisystems („/“) eingebunden ist. In diesem Fall sehen Sie, dass Partition 4 des Geräts „sda“ unter „/“ eingebunden ist:

   ```
   # lsblk
   NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   sda      8:0    0   48G  0 disk
   ├─sda1   8:1    0    2M  0 part
   ├─sda2   8:2    0  512M  0 part /boot/efi
   ├─sda3   8:3    0    1G  0 part /boot
   └─sda4   8:4    0 28.5G  0 part /
   sdb      8:16   0    4G  0 disk
   └─sdb1   8:17   0    4G  0 part /mnt/resource
   ```

1. Ändern Sie die Größe der erforderlichen Partition mit dem Befehl `growpart` unter Verwendung der im vorherigen Schritt gefundenen Partitionsnummer.

   ```
   # growpart /dev/sda 4
   CHANGED: partition=4 start=3151872 old: size=59762655 end=62914527 new: size=97511391 end=100663263
   ```

1. Führen Sie erneut den Befehl `lsblk` aus, um zu überprüfen, ob die Partition vergrößert wurde.

   Die folgende Ausgabe zeigt, dass die Größe der Partition **/dev/sda4** in 46,5 GB geändert wurde:
   
   ```
   linux:~ # lsblk
   NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   sda      8:0    0   48G  0 disk
   ├─sda1   8:1    0    2M  0 part
   ├─sda2   8:2    0  512M  0 part /boot/efi
   ├─sda3   8:3    0    1G  0 part /boot
   └─sda4   8:4    0 46.5G  0 part /
   sdb      8:16   0    4G  0 disk
   └─sdb1   8:17   0    4G  0 part /mnt/resource
   ```

1. Identifizieren Sie den Typ des Dateisystems auf dem Betriebssystemdatenträger mithilfe des Befehls `lsblk` mit dem Flag `-f`:

   ```
   linux:~ # lsblk -f
   NAME   FSTYPE LABEL UUID                                 MOUNTPOINT
   sda
   ├─sda1
   ├─sda2 vfat   EFI   AC67-D22D                            /boot/efi
   ├─sda3 xfs    BOOT  5731a128-db36-4899-b3d2-eb5ae8126188 /boot
   └─sda4 xfs    ROOT  70f83359-c7f2-4409-bba5-37b07534af96 /
   sdb
   └─sdb1 ext4         8c4ca904-cd93-4939-b240-fb45401e2ec6 /mnt/resource
   ```

1. Verwenden Sie die für den Typ des Dateisystems passenden Befehle, um die Größe des Dateisystems zu ändern.
   
   Verwenden Sie für **xfs** den folgenden Befehl:
   
   ```
   #xfs_growfs /
   ```
   
   Beispielausgabe:
   
   ```
   linux:~ # xfs_growfs /
   meta-data=/dev/sda4              isize=512    agcount=4, agsize=1867583 blks
            =                       sectsz=512   attr=2, projid32bit=1
            =                       crc=1        finobt=0 spinodes=0 rmapbt=0
            =                       reflink=0
   data     =                       bsize=4096   blocks=7470331, imaxpct=25
            =                       sunit=0      swidth=0 blks
   naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
   log      =internal               bsize=4096   blocks=3647, version=2
            =                       sectsz=512   sunit=0 blks, lazy-count=1
   realtime =none                   extsz=4096   blocks=0, rtextents=0
   data blocks changed from 7470331 to 12188923
   ```
   
   Verwenden Sie für **ext4** den folgenden Befehl:
   
   ```
   #resize2fs /dev/sda4
   ```
   
1. Überprüfen Sie die heraufgesetzte Größe des Dateisystems für **df -Th** mit dem folgenden Befehl:
   
   ```
   #df -Thl
   ```
   
   Beispielausgabe:
   
   ```
   linux:~ # df -Thl
   Filesystem     Type      Size  Used Avail Use% Mounted on
   devtmpfs       devtmpfs  445M  4.0K  445M   1% /dev
   tmpfs          tmpfs     458M     0  458M   0% /dev/shm
   tmpfs          tmpfs     458M   14M  445M   3% /run
   tmpfs          tmpfs     458M     0  458M   0% /sys/fs/cgroup
   /dev/sda4      xfs        47G  2.2G   45G   5% /
   /dev/sda3      xfs      1014M   86M  929M   9% /boot
   /dev/sda2      vfat      512M  1.1M  511M   1% /boot/efi
   /dev/sdb1      ext4      3.9G   16M  3.7G   1% /mnt/resource
   tmpfs          tmpfs      92M     0   92M   0% /run/user/1000
   tmpfs          tmpfs      92M     0   92M   0% /run/user/490
   ```
   
   Im vorherigen Beispiel können wir sehen, dass die Größe des Dateisystems für den Betriebssystem-Datenträger heraufgesetzt wurde.

### <a name="rhel"></a>RHEL

So setzen Sie die Größe des Betriebssystem-Datenträgers in RHEL 7.x mit LVM herauf:

1. Beenden Sie den virtuellen Computer.
1. Heraufsetzen der Größe des Betriebssystem-Datenträgers im Portal.
1. Starten Sie den virtuellen Computer.

Wenn der Neustart der VM erfolgt ist, führen Sie die folgenden Schritte aus:

1. Greifen Sie als **Root**-Benutzer auf Ihre VM zu, und verwenden Sie dazu den folgenden Befehl:
 
   ```
   #sudo su
   ```

1. Installieren Sie das **gptfdisk**-Paket, das erforderlich ist, um die Größe des Betriebssystem-Datenträgers heraufzusetzen.

   ```
   #yum install gdisk -y
   ```

1. Um den größten Sektor anzuzeigen, der auf dem Datenträger verfügbar ist, führen Sie den folgenden Befehl aus:

   ```
   #sgdisk -e /dev/sda
   ```

1. Ändern Sie mithilfe des folgenden Befehls die Größe der Partition, ohne sie zu löschen. Der **parted**-Befehl weist eine Option mit der Bezeichnung **resizepart** auf, um die Größe einer Partition zu ändern, ohne sie zu löschen. Die Zahl 4 nach **resizepart** ist die Angabe, dass die Größe der vierten Partition geändert werden soll.

   ```
   #parted -s /dev/sda "resizepart 4 -1" quit
   ```
    
1. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob die Partition erweitert wurde:

   ```
   #lsblk
   ```

   Die folgende Ausgabe zeigt, dass die Partition **/dev/sda4** auf 99 GB vergrößert wurde.

   ```
   [user@myvm ~]# lsblk
   NAME              MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   fd0                 2:0    1    4K  0 disk
   sda                 8:0    0  100G  0 disk
   ├─sda1              8:1    0  500M  0 part /boot/efi
   ├─sda2              8:2    0  500M  0 part /boot
   ├─sda3              8:3    0    2M  0 part
   └─sda4              8:4    0   99G  0 part
   ├─rootvg-tmplv    253:0    0    2G  0 lvm  /tmp
   ├─rootvg-usrlv    253:1    0   10G  0 lvm  /usr
   ├─rootvg-optlv    253:2    0    2G  0 lvm  /opt
   ├─rootvg-homelv   253:3    0    1G  0 lvm  /home
   ├─rootvg-varlv    253:4    0    8G  0 lvm  /var
   └─rootvg-rootlv   253:5    0    2G  0 lvm  /
   sdb                 8:16   0   50G  0 disk
   └─sdb1              8:17   0   50G  0 part /mnt/resource
   ```

1. Verwenden Sie den folgenden Befehl, um die Größe des physischen Volumes (PV) zu ändern:

   ```
   #pvresize /dev/sda4
   ```

   Die folgende Ausgabe zeigt, dass das PV auf 99,02 GB vergrößert wurde.

   ```
   [user@myvm ~]# pvresize /dev/sda4
   Physical volume "/dev/sda4" changed
   1 physical volume(s) resized or updated / 0 physical volume(s) not resized

   [user@myvm ~]# pvs
   PV         VG     Fmt  Attr PSize   PFree
   /dev/sda4  rootvg lvm2 a--  <99.02g <74.02g
   ```

1. Im folgenden Beispiel wird die Größe von **/dev/mapper/rootvg-rootlv** von 2 GB auf 12 GB heraufgesetzt (eine Erhöhung um 10 GB), mit dem folgenden Befehl. Mit diesem Befehl wird außerdem die Größe des Dateisystems heraufgesetzt.

   ```
   #lvresize -r -L +10G /dev/mapper/rootvg-rootlv
   ```

   Beispielausgabe:

   ```
   [user@myvm ~]# lvresize -r -L +10G /dev/mapper/rootvg-rootlv
   Size of logical volume rootvg/rootlv changed from 2.00 GiB (512 extents) to 12.00 GiB (3072 extents).
   Logical volume rootvg/rootlv successfully resized.
   meta-data=/dev/mapper/rootvg-rootlv isize=512    agcount=4, agsize=131072 blks
            =                       sectsz=4096  attr=2, projid32bit=1
            =                       crc=1        finobt=0 spinodes=0
   data     =                       bsize=4096   blocks=524288, imaxpct=25
            =                       sunit=0      swidth=0 blks
   naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
   log      =internal               bsize=4096   blocks=2560, version=2
            =                       sectsz=4096  sunit=1 blks, lazy-count=1
   realtime =none                   extsz=4096   blocks=0, rtextents=0
   data blocks changed from 524288 to 3145728
   ```
         
1. Überprüfen Sie mithilfe des folgenden Befehls, ob **/dev/mapper/rootvg-rootlv** eine erweiterte Größe des Dateisystems aufweist:

   ```
   #df -Th /
   ```

   Beispielausgabe:

   ```
   [user@myvm ~]# df -Th /
   Filesystem                Type  Size  Used Avail Use% Mounted on
   /dev/mapper/rootvg-rootlv xfs    12G   71M   12G   1% /
   [user@myvm ~]#
   ```

> [!NOTE]
> Um die gleiche Vorgehensweise zum Ändern der Größe jedes beliebigen anderen logischen Volumes zu verwenden, ändern Sie in Schritt 7 den **lv**-Namen.

## <a name="next-steps"></a>Nächste Schritte

- [Ändern der Datenträgergröße](expand-disks.md)
