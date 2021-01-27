---
title: Migrieren der Batch-Poolkonfiguration von Cloud Services zu Virtual Machines
description: Erfahren Sie, wie Sie Ihre Poolkonfiguration auf die neueste und empfohlene Konfiguration aktualisieren.
ms.topic: how-to
ms.date: 1/6/2021
ms.openlocfilehash: 417738be2c69101129079b8ff3a3d80634f9f99c
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98731498"
---
# <a name="migrate-batch-pool-configuration-from-cloud-services-to-virtual-machines"></a>Migrieren der Batch-Poolkonfiguration von Cloud Services zu Virtual Machines

Batch-Pools können entweder mit [cloudServiceConfiguration](/rest/api/batchservice/pool/add#cloudserviceconfiguration) oder mit [virtualMachineConfiguration](/rest/api/batchservice/pool/add#virtualmachineconfiguration) erstellt werden. „virtualMachineConfiguration“ ist die empfohlene Konfiguration, da sie alle Batch-Funktionen unterstützt. Pools mit „cloudServiceConfiguration“ unterstützen nicht alle Features, und es sind keine neuen Features geplant.

Wenn Sie Pools mit „cloudServiceConfiguration“ verwenden, wird dringend empfohlen, zu Pools mit „virtualMachineConfiguration“ zu wechseln. So können Sie von allen Batch-Funktionen profitieren, wie z. B.: erweiterte [Auswahl von VM-Serien](batch-pool-vm-sizes.md), Linux-VMs, [Container](batch-docker-container-workloads.md), [virtuelle Azure Resource Manager-Netzwerke](batch-virtual-network.md) und [Datenträgerverschlüsselung auf Knoten](disk-encryption.md).

In diesem Artikel wird die Migration zu „virtualMachineConfiguration“ beschrieben.

## <a name="new-pools-are-required"></a>Neue Pools erforderlich

Vorhandene Pools können nicht von „cloudServiceConfiguration“ auf „virtualMachineConfiguration“ aktualisiert werden. Es müssen neue Pools erstellt werden. Das Erstellen von Pools mithilfe von „virtualMachineConfiguration“ wird von allen Batch-APIs, Befehlszeilentools sowie im Azure-Portal und auf der Batch Explorer-Benutzeroberfläche unterstützt.

**Die Tutorials zu [.NET](tutorial-parallel-dotnet.md) und [Python](tutorial-parallel-python.md) enthalten Beispiele für die Erstellung von Pools mit „virtualMachineConfiguration“.**

## <a name="pool-configuration-differences"></a>Unterschiede bei der Poolkonfiguration

Beim Aktualisieren von Poolkonfiguration sind folgende Aspekte zu berücksichtigen:

- Poolknoten mit „cloudServiceConfiguration“ werden immer unter einem Windows-Betriebssystem ausgeführt, Poolknoten mit „virtualMachineConfiguration“ können unter Linux oder Windows ausgeführt werden.
- Im Vergleich zu Pools mit „cloudServiceConfiguration“ bieten Pools mit „virtualMachineConfiguration“ mehr Funktionen, wie z. B. Containerunterstützung, Datenträger für Daten und Datenträgerverschlüsselung.
- Poolknoten mit „virtualMachineConfiguration“ verwenden verwaltete Betriebssystem-Datenträger. Der für die jeweiligen Knoten verwendete [Typ der verwalteten Datenträger](../virtual-machines/disks-types.md) richtet sich nach der für den Pool ausgewählten VM-Größe. Wenn die für den Pool angegebene VM-Größe ein „s“ enthält (z. B. Standard_D2s_v3), wird ein Premium-SSD verwendet. Wenn die VM-Größe kein „s“ enthält (z. B. Standard_D2_v3), wird ein Standard-Festplattenlaufwerk verwendet.

   > [!IMPORTANT]
   > Ebenso wie bei Virtual Machines und Virtual Machine Scale Sets verursachen die verwalteten Betriebssystem-Datenträger der einzelnen Knoten Kosten, die zusätzlich zu den VM-Gebühren anfallen. Da die Betriebssystem-Datenträger auf den lokalen SSDs der Knoten erstellt werden, fallen für Knoten mit „cloudServiceConfiguration“ keine Kosten für diese Datenträger an.

- Die Start- und Löschzeiten für Pools und Knoten können sich bei Pools mit „cloudServiceConfiguration“ und Pools mit „virtualMachineConfiguration“ geringfügig unterscheiden.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Poolkonfigurationen](nodes-and-pools.md#configurations).
- Erfahren Sie mehr über [bewährte Methoden für Pools](best-practices.md#pools).
- Lesen Sie die REST-API-Referenz zum [Hinzufügen von Pools](/rest/api/batchservice/pool/add) und zu [virtualMachineConfiguration](/rest/api/batchservice/pool/add#virtualmachineconfiguration).