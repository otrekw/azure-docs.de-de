---
title: 'Azure Storage Premium: Entwurf für hohe Leistung auf Windows-VMs | Microsoft-Dokumentation'
description: Entwerfen Sie Hochleistungsanwendungen mithilfe von verwalteten Azure SSD Premium-Datenträgern. Storage Premium bietet Datenträgerunterstützung für hohe Leistung mit geringer Latenz für E/A-intensive Workloads, die auf virtuellen Azure-Computern ausgeführt werden.
author: roygara
ms.service: virtual-machines-windows
ms.topic: conceptual
ms.date: 06/27/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 06ac680ef48d9602f6e7487eb9ebecee9f3ae080
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84017601"
---
# <a name="azure-premium-storage-design-for-high-performance"></a>Azure Storage Premium: Entwurf für hohe Leistung
[!INCLUDE [virtual-machines-common-premium-storage-introduction](../../../includes/virtual-machines-common-premium-storage-introduction.md)]

> [!NOTE]
> Manchmal ist ein vermutetes Problem mit der Datenträgerleistung tatsächlich ein Engpass. In solchen Fällen sollten Sie Ihre [Netzwerkleistung](../../virtual-network/virtual-network-optimize-network-bandwidth.md) optimieren.
>
> Wenn Sie für Ihren Datenträger einen Benchmarktest durchführen möchten, finden Sie weitere Informationen in unserem Artikel [Benchmarktests für einen Datenträger](disks-benchmarks.md).
>
> Wenn Ihr virtueller Computer den beschleunigten Netzwerkbetrieb unterstützt, stellen Sie sicher, dass dieser aktiviert ist. Wenn er nicht aktiviert ist, können Sie ihn sowohl unter [Windows](../../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) als auch [Linux](../../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms) auf bereits bereitgestellten virtuellen Computern aktivieren.

Falls Sie noch nicht mit Storage Premium vertraut sind, lesen Sie zunächst die Artikel [Auswählen eines Azure-Datenträgertyps für IaaS VMs](disks-types.md) und [Skalierbarkeitsziele für Seitenblobspeicher mit Premium-Leistung](../../storage/blobs/scalability-targets-premium-page-blobs.md).

[!INCLUDE [virtual-machines-common-premium-storage-performance.md](../../../includes/virtual-machines-common-premium-storage-performance.md)]

Wenn Sie für Ihren Datenträger einen Benchmarktest durchführen möchten, finden Sie weitere Informationen in unserem Artikel [Benchmarktests für einen Datenträger](disks-benchmarks.md).

Weitere Informationen zu den verfügbaren Datenträgertypen finden Sie unter [Auswählen eines Datenträgertyps](disks-types.md).  

Für SQL Server-Benutzer bietet sich das Lesen von Artikeln zu den bewährten Methoden für die Leistung von SQL Server an:

* [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](../../azure-sql/virtual-machines/windows/performance-guidelines-best-practices.md)
* [Azure Storage Premium bietet höchste Leistung für SQL Server in Azure-VM](https://cloudblogs.microsoft.com/sqlserver/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm/)
