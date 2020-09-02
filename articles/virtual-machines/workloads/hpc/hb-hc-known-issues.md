---
title: Bekannte Probleme bei virtuellen Computern der HB- und HC-Serie – Azure Virtual Machines | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu bekannten Problemen bei VM-Größen der HB-Serie in Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 08/19/2020
ms.author: amverma
ms.reviewer: cynthn
ms.openlocfilehash: 6316bcc91bb381facb4f77b2d8dbd8b22f9ed387
ms.sourcegitcommit: d18a59b2efff67934650f6ad3a2e1fe9f8269f21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88660094"
---
# <a name="known-issues-with-h-series-and-n-series-vms"></a>Bekannte Probleme bei virtuellen Computern der H-Serie und der N-Serie

Dieser Artikel enthält Informationen zu den häufigsten Problemen bei der Verwendung virtueller Computer der [H-Serie](../../sizes-hpc.md) und der [N-Serie](../../sizes-gpu.md) sowie entsprechende Lösungen.

## <a name="dram-on-hb-series"></a>DRAM in der HB-Serie

Virtuelle Computer der HB-Serie können momentan nur 228 GB RAM für virtuelle Gastcomputer verfügbar machen. Der Grund ist eine bekannte Einschränkung des Azure-Hypervisors, die verhindert, dass Seiten dem lokalen DRAM der AMD CCXs (NUMA-Domänen) zugewiesen werden, die für den virtuellen Gastcomputer reserviert sind.

## <a name="accelerated-networking"></a>Beschleunigter Netzwerkbetrieb

Der beschleunigte Netzwerkbetrieb von Azure ist momentan nicht aktiviert. Dies ändert sich im Laufe des Vorschauzeitraums aber noch. Kunden erhalten eine entsprechende Benachrichtigung, wenn dieses Feature unterstützt wird.

## <a name="qp0-access-restriction"></a>qp0-Zugriffseinschränkung

Um Zugriffe auf Low-Level-Hardware zu verhindern, die zu Sicherheitsrisiken führen können, haben virtuelle Gastcomputer keinen Zugriff auf das Warteschlangenpaar 0. Dies ist in der Regel nur für Aktionen im Zusammenhang mit der Verwaltung der ConnectX-5-NIC oder mit der Ausführung einiger InfiniBand-Diagnosen (beispielsweise „ibdiagnet“) relevant, nicht aber für Endbenutzeranwendungen.

## <a name="gss-proxy"></a>GSS Proxy

Für GSS Proxy liegt in CentOS/RHEL 7.5 ein bekannter Fehler vor, der bei Verwendung mit NFS zu erheblichen Leistungs- und Reaktionseinbußen führen kann. Die Auswirkungen können auf folgende Weise gemindert werden:

```console
sed -i 's/GSS_USE_PROXY="yes"/GSS_USE_PROXY="no"/g' /etc/sysconfig/nfs
```

## <a name="cache-cleaning"></a>Cachebereinigung

Auf HPC-Systemen empfiehlt es sich häufig, nach Abschluss eines Auftrags den Arbeitsspeicher zu bereinigen, bevor dem nächsten Benutzer der gleiche Knoten zugewiesen wird. Nach der Ausführung von Anwendungen unter Linux ist unter Umständen ein Rückgang des verfügbaren Arbeitsspeichers sowie eine Zunahme des Pufferspeichers zu beobachten, obwohl gar keine Anwendungen ausgeführt werden.

![Screenshot der Eingabeaufforderung](./media/known-issues/cache-cleaning-1.png)

`numactl -H` zeigt, mit welchen NUMA-Knoten der Arbeitsspeicher gepuffert wird. (Das können unter Umständen alle sein.) Unter Linux können Benutzer die Caches auf drei Arten bereinigen, um gepufferten Arbeitsspeicher oder CPU-Cache freizugeben. Dazu sind root- oder sudo-Berechtigungen erforderlich.

```console
echo 1 > /proc/sys/vm/drop_caches [frees page-cache]
echo 2 > /proc/sys/vm/drop_caches [frees slab objects e.g. dentries, inodes]
echo 3 > /proc/sys/vm/drop_caches [cleans page-cache and slab objects]
```

![Screenshot der Eingabeaufforderung](./media/known-issues/cache-cleaning-2.png)

## <a name="kernel-warnings"></a>Kernelwarnungen

Wenn Sie einen virtuellen Computer der HB-Serie unter Linux starten, werden unter Umständen folgende Kernelwarnmeldungen angezeigt:

```console
[  0.004000] WARNING: CPU: 4 PID: 0 at arch/x86/kernel/smpboot.c:376 topology_sane.isra.3+0x80/0x90
[  0.004000] sched: CPU #4's llc-sibling CPU #0 is not on the same node! [node: 1 != 0]. Ignoring dependency.
[  0.004000] Modules linked in:
[  0.004000] CPU: 4 PID: 0 Comm: swapper/4 Not tainted 3.10.0-957.el7.x86_64 #1
[  0.004000] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007 05/18/2018
[  0.004000] Call Trace:
[  0.004000] [<ffffffffb8361dc1>] dump_stack+0x19/0x1b
[  0.004000] [<ffffffffb7c97648>] __warn+0xd8/0x100
[  0.004000] [<ffffffffb7c976cf>] warn_slowpath_fmt+0x5f/0x80
[  0.004000] [<ffffffffb7c02b34>] ? calibrate_delay+0x3e4/0x8b0
[  0.004000] [<ffffffffb7c574c0>] topology_sane.isra.3+0x80/0x90
[  0.004000] [<ffffffffb7c57782>] set_cpu_sibling_map+0x172/0x5b0
[  0.004000] [<ffffffffb7c57ce1>] start_secondary+0x121/0x270
[  0.004000] [<ffffffffb7c000d5>] start_cpu+0x5/0x14
[  0.004000] ---[ end trace 73fc0e0825d4ca1f ]---
```

Diese Warnung kann ignoriert werden. Sie ist auf eine bekannte Einschränkung des Azure-Hypervisors zurückzuführen, die im Laufe der Zeit behandelt wird.


## <a name="infiniband-driver-installation-on-infiniband-enabled-n-series-vm-sizes"></a>InfiniBand-Treiberinstallation auf InfiniBand-fähigen VM-Größen der N-Serie

NC24r_v3 und ND40r_v2 sind SR-IOV-fähig, während NC24r und NC24r_v2 nicht SR-IOV-fähig sind. Details dazu finden Sie [hier](../../sizes-hpc.md#rdma-capable-instances).
InfiniBand (IB) kann für die SR-IOV-fähigen VM-Größen mit den OFED-Treibern konfiguriert werden, während die nicht SR-IOV-fähigen VM-Größen ND-Treiber erfordern. Diese IB-Unterstützung ist entsprechend auf [CentOS-HPC-VMIs](configure.md) verfügbar. Informationen zu Ubuntu finden Sie in der [dieser Anweisung](https://techcommunity.microsoft.com/t5/azure-compute/configuring-infiniband-for-ubuntu-hpc-and-gpu-vms/ba-p/1221351) zum Installieren der OFED- und ND-Treiber, wie in der [Dokumentation](enable-infiniband.md#vm-images-with-infiniband-drivers) beschrieben.


## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Übersicht über virtuelle Computer der HB-Serie](hb-series-overview.md) und die [Übersicht über virtuelle Computer der HC-Serie](hc-series-overview.md) an, um zu erfahren, wie Sie Workloads optimal konfigurieren, um die bestmögliche Leistung und Skalierbarkeit zu erzielen.
- Informieren Sie sich in den [Tech Community-Blogs zu Azure Compute](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute) über die neuesten Ankündigungen, und machen Sie sich mit einigen HPC-Beispielen und Ergebnissen vertraut.
- Eine allgemeinere Architekturübersicht zur Ausführung von HPC-Workloads finden Sie unter [High Performance Computing (HPC) in Azure](/azure/architecture/topics/high-performance-computing/).
