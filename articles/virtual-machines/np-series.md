---
title: NP-Serie – Azure Virtual Machines
description: Spezifikationen für die VMs der NP-Serie.
author: vikancha-MSFT
ms.service: virtual-machines
ms.subservice: vm-sizes-gpu
ms.topic: conceptual
ms.date: 02/09/2021
ms.author: vikancha
ms.openlocfilehash: 61488b88b00206cb78beed4fe773bf9377848701
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107861047"
---
# <a name="np-series"></a>NP-Serie 
Die virtuellen Computer der NP-Serie basieren auf [Xilinx U250](https://www.xilinx.com/products/boards-and-kits/alveo/u250.html)-FPGAs zum Beschleunigen von Workloads einschließlich Machine Learning-Rückschluss, Videotranscodierung und Datenbanksuche und Analysen. VMs der NP-Serie werden auch mit Intel Xeon 8171m-CPUs (Skylake) mit einem Turbotakt von 3,2 GHz für alle Kerne betrieben.

[Storage Premium](premium-storage-performance.md): Unterstützt<br>
[Storage Premium-Zwischenspeicherung:](premium-storage-performance.md) Unterstützt<br>
[Livemigration:](maintenance-and-updates.md) Nicht unterstützt<br>
[Updates mit Speicherbeibehaltung:](maintenance-and-updates.md) Nicht unterstützt<br>
Unterstützung von VM-Generationen: Generation 1<br>
[Beschleunigter Netzwerkbetrieb](../virtual-network/create-vm-accelerated-networking-cli.md): Unterstützt<br>
[Kurzlebige Betriebssystemdatenträger:](ephemeral-os-disks.md) Nicht unterstützt <br>
<br>

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | FPGA | FPGA-Speicher: GiB | Max. Anzahl Datenträger | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBit/s) | 
|---|---|---|---|---|---|---|---|
| Standard_NP10s | 10 | 168 | 736  | 1 | 64  | 8 | 1/7500 | 
| Standard_NP20s | 20 | 336 | 1474 | 2 | 128 | 16 | 2/15000 | 
| Standard_NP40s | 40 | 672 | 2948 | 4 | 256 | 32 | 4/30000 | 



[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]


##  <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**F:** Welche Version von Vitis sollte ich verwenden? 

**A:** Xilinx empfiehlt [Vitis 2020.2](https://www.xilinx.com/products/design-tools/vitis/vitis-platform.html)


**F:** Muss ich NP VMs verwenden, um meine Lösung zu entwickeln? 

**A:** Nein, Sie können lokal entwickeln und in der Cloud bereitstellen! Stellen Sie sicher, dass Sie die Nachweisdokumentation befolgen, um auf NP-VMS bereitzustellen. 

**F:** Welche Version von XRT sollte ich verwenden?

**A:** xrrt_202020.2.8.832 

**F:** Was ist die Ziel-Bereitsstellungsplattform?

**A:** Verwenden Sie die folgenden Plattformen.
- xilinx-u250-gen3x16-xdma-platform-2.1-3_alle
- xilinx-u250-gen3x16-xdma-validiert_2.1-3005608.1 

**F:** Welche Plattform sollte ich für die Entwicklung anvisieren?

**A:** xilinx-u250-gen3x16-xdma-2.1-202010-1-dev_1-2954688_alle 

**F:** Welche sind die unterstützten OS (Betriebssysteme)? 

**A:** Xilinx und Microsoft haben Ubuntu 18.04 LTS und CentOS 7.8 validiert.

 Xilinx hat die folgenden Marketplace-Images erstellt, um die Bereitstellung dieser VMs zu vereinfachen. 

[Xilinx Alveo U250 Bereitstellung VM – Ubuntu18.04](https://ms.portal.azure.com/#blade/Microsoft_Azure_Marketplace/GalleryItemDetailsBladeNopdl/id/xilinx.xilinx_alveo_u250_deployment_vm_ubuntu1804_032321)

[Xilinx Alveo U250 Bereitstellung VM – CentOS7.8](https://ms.portal.azure.com/#blade/Microsoft_Azure_Marketplace/GalleryItemDetailsBladeNopdl/id/xilinx.xilinx_alveo_u250_deployment_vm_centos78_032321)

**V:** Kann ich meine eigenen Ubuntu/CentOS-VMs bereitstellen und XRT/Deployment Target Platform installieren? 

**A:** Ja.

**F:** Wenn ich meine eigene Ubuntu18.04 VM bereitstellen, was sind die erforderlichen Pakete und Schritte?

**A:** Verwenden Sie Kernel 4.1X per [Xilinx XRT Dokumentation](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2020_2/ug1451-xrt-release-notes.pdf)
       
Installieren Sie die folgenden Pakete.
- xrrt_202020.2.8.832_18.04-amd64-xrt.deb
       
- xrrt_202020.2.8.832_18.04-amd64-azure.deb
       
- xilinx-u250-gen3x16-xdma-platform-2.1-3_alle_18.04.deb.tar.gz
       
- xilinx-u250-gen3x16-xdma-validiert_2.1-3005608.1_all.deb  

**F:** Unter Ubuntu kann ich nach dem Neustart meiner VM meine FPGA(s) nicht finden: 

**A:** Vergewissern Sie sich, dass Ihr Kernel nicht aktualisiert wurde (uname-a). Wenn dies der Fall ist, führen Sie ein Downgrade auf Kernel 4.1X durch. 

**V:** Wenn ich meine eigene CentOS7.8 VM bereitstelle, was sind dann die erforderlichen Pakete und Schritte?

**A:** Kernel Version verwenden: 3.10.0-1160.15.2.el7.x86_64

 Installieren Sie die folgenden Pakete.
   
 - xrt_202020.2.8.832_7.4.1708-x86_64-xrt.rpm 
      
 - xrrt_202020.2.8.832_7.4.1708-x86_64-azure.rpm 
     
 - xilinx-u250-gen3x16-xdma-plattform-2.1-3.noarch.rpm.tar.gz 
      
 - xilinx-u250-gen3x16-xdma-validiert-2.1-3005608.1.noarch.rpm  

**Host_Mem(SB): sudo xbutil host_mem --disable:** Wenn ich xbutil validate auf CentOS ausführe, erhalte ich diese Warnung: "WARNUNG: Kernel-Version 3.10.0-1160.15.2.el7.x86_64 wird nicht offiziell unterstützt. 4.18.0-193 ist die neueste unterstützte Version.” 

**A:** Dies kann getrost ignoriert werden. 

**F:** Was sind die Unterschiede zwischen OnPrem und NP VMs bezüglich XRT? 

**A:** In Azure, unterstützt die XDMA 2,1-Plattform nur Host_Mem (SB) und DDR-Daten Rückhaltungsfunktionen. 

Aktivieren von Host_Mem (SB) (1 GB RAM): sudo xbutil host_mem--enable--size 1 g 

Deaktivieren von Host_Mem(SB): sudo xbutil host_mem --disable 

**F:** Kann ich xbmgmt-Befehle ausführen? 

**A:** Nein, auf Azure-VMS gibt es keine direkte Management-Unterstützung von der Azure-VM. 

 **F:** Muss ich eine PLP laden? 

**A:** Nein, das PLP wird automatisch für Sie geladen, sodass es nicht erforderlich ist, über xbmgmt-Befehle zu laden. 

 
**F:** Unterstützt Azure verschiedene plps? 

**A:** Derzeit leider nicht. Wir unterstützen nur die in den Bereitstellungs-Plattform-Paketen bereitgestellte PLP. 

**F:** Wie kann ich die PLP-Informationen Abfragen? 

**A:** Sie müssen die xbutil-Abfrage ausführen und den unteren Teil überprüfen. 

**F:** Wenn ich meine eigene VM erstelle und XRT manuell bereitstelle, welche zusätzlichen Änderungen muss ich dann vornehmen? 

**A:** Fügen Sie in der Datei /opt/xilinx/xrt/setup.sh einen Eintrag für XRT_INI_PATH ein, der auf /opt/xilinx/xrt/xrt.ini hinweist

 
Der Inhalt von /opt/xilinx/xrt/xrt.ini sollte das folgende enthalten: <br>
[Ausführungsdauer]<br>
ert=falsch <br>

## <a name="other-sizes"></a>Andere Größen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.
