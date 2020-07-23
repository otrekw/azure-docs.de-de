---
title: Aktivieren von InifinBand mit SR-IOV – Azure Virtual Machines | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie InfiniBand mit SR-IOV aktivieren.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 10/17/2019
ms.author: amverma
ms.openlocfilehash: de61403b62f80bea7872d5ab3561567ae2109590
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86500067"
---
# <a name="enable-infiniband-with-sr-iov"></a>Aktivieren von InfiniBand mit SR-IOV

Die Azure NC-, ND- und H-Reihen von VMs sind alle von einem dedizierten InfiniBand-Netzwerk gestützt. Alle RDMA-fähigen Größen können dieses Netzwerk mithilfe von Intel MPI nutzen. Einige VM-Reihen bieten erweiterte Unterstützung für alle MPI-Implementierungen und RDMA-Aktionsarten mithilfe von SR-IOV. Zu den RDMA-fähigen VMs zählen die [GPU-optimierten](../../sizes-gpu.md) und die [HPC](../../sizes-hpc.md)-VMs (High Performance Compute).

## <a name="choose-your-installation-path"></a>Auswählen Ihres Installationspfads

Die einfachste Option für den Einstieg besteht in der Verwendung eines für InfiniBand vorkonfigurierten Plattformimages, sofern verfügbar:

- **HPC IaaS-VMs**: Die einfachste Lösung, um in IaaS-VMs für HPC einzusteigen, bietet das [CentOS-HPC 7.6 VM-Betriebssystemimage](https://techcommunity.microsoft.com/t5/Azure-Compute/CentOS-HPC-VM-Image-for-SR-IOV-enabled-Azure-HPC-VMs/ba-p/665557), das bereits mit InfiniBand konfiguriert ist. Da dieses Image bereits mit InfiniBand konfiguriert ist, brauchen Sie es nicht manuell zu konfigurieren. Informationen zu kompatiblen Windows-Versionen finden Sie unter [RDMA-fähige Windows-Instanzen](../../sizes-hpc.md#rdma-capable-instances).

- **GPU IaaS-VMs**: Für GPU-optimierte VMs sind zurzeit bis auf das [CentOS-HPC 7.6 VM-Betriebssystemimage](https://techcommunity.microsoft.com/t5/Azure-Compute/CentOS-HPC-VM-Image-for-SR-IOV-enabled-Azure-HPC-VMs/ba-p/665557) keine vorkonfigurierten Plattformimages verfügbar. Informationen zum Konfigurieren eines benutzerdefinierten Images mit InfiniBand finden Sie unter [Manuelle Installation von Mellanox OFED](#manually-install-mellanox-ofed).

Wenn Sie Ihr benutzerdefiniertes VM-Image oder eine [GPU-optimierte](../../sizes-gpu.md) VM verwenden, sollten Sie dieses mit InfiniBand konfigurieren, indem Sie Ihrer Bereitstellung die VM-Erweiterung „InfiniBandDriverLinux“ oder „InfiniBandDriverWindows“ hinzufügen. Informationen zur Verwendung dieser VM-Erweiterungen finden Sie [hier](../../sizes-hpc.md#rdma-capable-instances) (Linux) bzw. [hier](../../sizes-hpc.md#rdma-capable-instances) (Windows).

## <a name="manually-install-mellanox-ofed"></a>Manuelle Installation von Mellanox OFED

Führen Sie die folgenden Schritte aus, um InfiniBand mit SR-IOV manuell zu konfigurieren. Das Beispiel in diesen Schritten zeigt die Syntax für RHEL/CentOS, die Schritte sind jedoch allgemein und können für jedes kompatible Betriebssystem verwendet werden, wie Ubuntu (16.04, 18.04, 19.04) und SLES (12 SP4 and 15). Die enthaltenen Treiber funktionieren ebenfalls, die Mellanox OpenFabrics-Treiber bieten aber mehr Funktionen.

Weitere Informationen zu den unterstützten Distributionen für den Mellanox-Treiber finden Sie bei den aktuellen [Mellanox OpenFabrics-Treibern](https://www.mellanox.com/page/products_dyn?product_family=26). Weitere Informationen zum Mellanox OpenFabrics-Treiber finden Sie im [Mellanox-Benutzerhandbuch](https://docs.mellanox.com/category/mlnxofedib).

In diesem Beispiel sehen Sie, wie InfiniBand für Linux konfiguriert wird:

```bash
# Modify the variable to desired Mellanox OFED version
MOFED_VERSION=#4.7-1.0.0.1
# Modify the variable to desired OS
MOFED_OS=#rhel7.6
pushd /tmp
curl -fSsL https://www.mellanox.com/downloads/ofed/MLNX_OFED-${MOFED_VERSION}/MLNX_OFED_LINUX-${MOFED_VERSION}-${MOFED_OS}-x86_64.tgz | tar -zxpf -
cd MLNX_OFED_LINUX-*
sudo ./mlnxofedinstall
popd
```

Laden Sie für Windows den [Mellanox OFED für Windows-Treiber](https://www.mellanox.com/page/products_dyn?product_family=32&menu_section=34) herunter, und installieren Sie ihn.

## <a name="enable-ip-over-infiniband"></a>Aktivieren von IP über InfiniBand

Verwenden Sie die folgenden Befehle, um IP über InfiniBand zu aktivieren.

```bash
sudo sed -i -e 's/# OS.EnableRDMA=y/OS.EnableRDMA=y/g' /etc/waagent.conf
sudo systemctl restart waagent
```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich ausführlicher über [HPC](/azure/architecture/topics/high-performance-computing/) in Azure.
