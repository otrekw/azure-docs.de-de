---
title: 'Aktivieren von InifinBand auf HPC-VMs: Azure Virtual Machines | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie InfiniBand auf HPC-Azure-VMs aktivieren.
author: vermagit
ms.service: virtual-machines
ms.subservice: hpc
ms.topic: article
ms.date: 03/18/2021
ms.author: amverma
ms.reviewer: cynthn
ms.openlocfilehash: 089976f2009e006f53dd2a77f09f57d5090429b7
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104721240"
---
# <a name="enable-infiniband"></a>Aktivieren von InfiniBand

[RDMA-fähige](../../sizes-hpc.md#rdma-capable-instances) virtuelle Computer der [H-Serie](../../sizes-hpc.md) und der [N-Serie](../../sizes-gpu.md) kommunizieren über das InfiniBand-Netzwerk, das sich durch seine geringe Wartezeit und seine hohe Bandbreite auszeichnet. Die RDMA-Funktion über eine solche Verbindung ist entscheidend, um die Skalierbarkeit und Leistung von HPC- und KI-Workloads mit verteilten Knoten zu steigern. Die InfiniBand-fähigen virtuellen Computer der H- und N-Serie sind über einen blockierungsfreien Fat Tree mit einem Design mit geringem Durchmesser verbunden, um eine optimierte und konsistente RDMA-Leistung zu erzielen.

Es gibt verschiedene Möglichkeiten, InfiniBand für die dazu geeigneten VM-Größen zu aktivieren.

## <a name="vm-images-with-infiniband-drivers"></a>VM-Images mit InfiniBand-Treibern
Unter [Konfigurieren und Optimieren von virtuellen Computern](configure.md#vm-images) finden Sie eine Liste der unterstützten VM-Images im Marketplace, die vorab mit InfiniBand-Treibern (für SR-IOV- oder Nicht-SR-IOV-VMs) geladen werden oder mit den entsprechenden Treibern für [RDMA-fähige VMs](../../sizes-hpc.md#rdma-capable-instances) konfiguriert werden können.
- Die [CentOS-HPC](configure.md#centos-hpc-vm-images)-VM-Images im Marketplace sind die einfachste Möglichkeit für den Einstieg.
- Die [Ubuntu](configure.md#ubuntu-vm-images)-VM-Images können mit den entsprechenden IB-Treibern konfiguriert werden.

## <a name="infiniband-driver-vm-extensions"></a>InfiniBand-Treibererweiterungen für VMs
Unter Linux kann die [InfiniBandDriverLinux-VM-Erweiterung](../../extensions/hpc-compute-infiniband-linux.md) verwendet werden, um die Mellanox OFED-Treiber zu installieren und InfiniBand auf den SR-IOV-fähigen VMs der H- und N-Serie zu aktivieren.

Unter Windows installiert die VM-Erweiterung [InfiniBandDriverWindows](../../extensions/hpc-compute-infiniband-windows.md) Windows Network Direct-Treiber (auf VMs ohne SR-IOV) oder Mellanox OFED-Treiber (auf VMs mit SR-IOV), um RDMA-Konnektivität herzustellen. In bestimmten Bereitstellungen von A8- und A9-Instanzen wird die Erweiterung HpcVmDrivers automatisch hinzugefügt. Beachten Sie, dass die VM-Erweiterung „HpcVmDrivers“ eingestellt wird. Sie wird nicht mehr aktualisiert.

Um die VM-Erweiterung einer VM hinzuzufügen, können Sie [Azure PowerShell](/powershell/azure/)-Cmdlets verwenden. Weitere Informationen finden Sie unter [Erweiterungen und Features für virtuelle Computer](../../extensions/overview.md). Erweiterungen können auch für virtuelle Computer verwendet werden, die mit dem [klassischen Bereitstellungsmodell](/previous-versions/azure/virtual-machines/windows/classic/agents-and-extensions-classic) bereitgestellt wurden.

## <a name="manual-installation"></a>Manuelle Installation
[Mellanox OpenFabrics-Treiber (OFED)](https://www.mellanox.com/products/InfiniBand-VPI-Software) können manuell auf den [SR-IOV-fähigen](../../sizes-hpc.md#rdma-capable-instances) VMs der [H-Serie](../../sizes-hpc.md) und der [N-Serie](../../sizes-gpu.md) installiert werden.

### <a name="linux"></a>Linux
Die [OFED-Treiber für Linux](https://www.mellanox.com/products/infiniband-drivers/linux/mlnx_ofed) können mithilfe des folgenden Beispiels installiert werden. Obwohl das Beispiel hier für RHEL/CentOS gilt, sind die Schritte allgemein und können für jedes kompatible Linux-Betriebssystem wie Ubuntu (16.04, 18.04, 19.04, 20.04) und SLES (12 SP4 and 15) verwendet werden. Weitere Beispiele für andere Distributionen finden Sie im [azhpc-images-Repository](https://github.com/Azure/azhpc-images/blob/master/ubuntu/ubuntu-18.x/ubuntu-18.04-hpc/install_mellanoxofed.sh). Die enthaltenen Treiber funktionieren ebenfalls, die Mellanox OFED-Treiber bieten aber mehr Features.

```bash
MLNX_OFED_DOWNLOAD_URL=http://content.mellanox.com/ofed/MLNX_OFED-5.0-2.1.8.0/MLNX_OFED_LINUX-5.0-2.1.8.0-rhel7.7-x86_64.tgz
# Optionally verify checksum
wget --retry-connrefused --tries=3 --waitretry=5 $MLNX_OFED_DOWNLOAD_URL
tar zxvf MLNX_OFED_LINUX-5.0-2.1.8.0-rhel7.7-x86_64.tgz

KERNEL=( $(rpm -q kernel | sed 's/kernel\-//g') )
KERNEL=${KERNEL[-1]}
# Uncomment the lines below if you are running this on a VM
#RELEASE=( $(cat /etc/centos-release | awk '{print $4}') )
#yum -y install http://olcentgbl.trafficmanager.net/centos/${RELEASE}/updates/x86_64/kernel-devel-${KERNEL}.rpm
yum install -y kernel-devel-${KERNEL}
./MLNX_OFED_LINUX-5.0-2.1.8.0-rhel7.7-x86_64/mlnxofedinstall --kernel $KERNEL --kernel-sources /usr/src/kernels/${KERNEL} --add-kernel-support --skip-repo
```

### <a name="windows"></a>Windows
Laden Sie für Windows den [Mellanox OFED für Windows-Treiber](https://www.mellanox.com/products/adapter-software/ethernet/windows/winof-2) herunter, und installieren Sie ihn.

## <a name="enable-ip-over-infiniband-ib"></a>Aktivieren von IP über InfiniBand (IB)
Wenn Sie MPI-Aufträge ausführen möchten, wird IPoIB in der Regel nicht benötigt. Die MPI-Bibliothek verwendet die Verbenschnittstelle für die IB-Kommunikation (es sei denn, Sie verwenden explizit den TCP/IP-Kanal der MPI-Bibliothek). Wenn Sie jedoch über eine App verfügen, die TCP/IP für die Kommunikation verwendet und die Sie über IB ausführen möchten, können Sie IPoIB über die IB-Schnittstelle verwenden. Verwenden Sie die folgenden Befehle (für RHEL/CentOS), um IP über InfiniBand zu aktivieren.

```bash
sudo sed -i -e 's/# OS.EnableRDMA=n/OS.EnableRDMA=y/g' /etc/waagent.conf
sudo systemctl restart waagent
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Installation verschiedener unterstützter MPI-Bibliotheken sowie zur optimalen Konfiguration auf der VM finden Sie unter [Einrichten von Message Passing Interface für HPC](setup-mpi.md).
- Weitere Informationen finden Sie in den Artikeln [Übersicht über virtuelle Computer der HBv3-Serie](hbv3-series-overview.md) und [Übersicht über virtuelle Computer der HC-Serie](hc-series-overview.md).
- Informieren Sie sich über die neuesten Ankündigungen, HPC-Workloadbeispiele und Leistungsergebnisse in den [Tech Community-Blogs zu Azure Compute](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute).
- Eine allgemeinere Übersicht über die Architektur für die Ausführung von HPC-Workloads finden Sie unter [High Performance Computing (HPC) in Azure](/azure/architecture/topics/high-performance-computing/).
