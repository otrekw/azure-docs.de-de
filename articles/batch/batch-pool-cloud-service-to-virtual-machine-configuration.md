---
title: Migrieren der Batch-Poolkonfiguration von Cloud Services zu Virtual Machines
description: Erfahren Sie, wie Sie Ihre Poolkonfiguration auf die neueste und empfohlene Konfiguration aktualisieren.
ms.topic: how-to
ms.date: 2/16/2021
ms.openlocfilehash: 9cbcf3864526bd8f8132f3b0f729e2d728e07bb8
ms.sourcegitcommit: 5a999764e98bd71653ad12918c09def7ecd92cf6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2021
ms.locfileid: "100546039"
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

## <a name="azure-data-factory-custom-activity-pools"></a>Pools benutzerdefinierter Azure Data Factory-Aktivitäten

Mithilfe von Azure Batch-Pools können benutzerdefinierte Data Factory-Aktivitäten ausgeführt werden. Alle Pools mit „cloudServiceConfiguration“, die zum Ausführen von benutzerdefinierten Aktivitäten verwendet werden, müssen gelöscht werden, und neue Pools mit „virtualMachineConfiguration“ müssen erstellt werden.

- Pipelines sollten vor dem Löschen und Neuerstellen angehalten werden, um sicherzustellen, dass keine Ausführungen unterbrochen werden.
- Die gleiche Pool-ID kann verwendet werden, um Änderungen an der Konfiguration von verknüpften Diensten zu vermeiden.
- Setzen Sie die Pipelines fort, nachdem die neuen Pools erstellt wurden.

Weitere Informationen zur Verwendung von Azure Batch zum Ausführen von benutzerdefinierten Data Factory-Aktivitäten:

- [Verknüpfter Azure Batch-Dienst](../data-factory/compute-linked-services.md#azure-batch-linked-service)
- [Verwenden von benutzerdefinierten Aktivitäten in der Data Factory-Pipeline](../data-factory/transform-data-using-dotnet-custom-activity.md)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Poolkonfigurationen](nodes-and-pools.md#configurations).
- Erfahren Sie mehr über [bewährte Methoden für Pools](best-practices.md#pools).
- Lesen Sie die REST-API-Referenz zum [Hinzufügen von Pools](/rest/api/batchservice/pool/add) und zu [virtualMachineConfiguration](/rest/api/batchservice/pool/add#virtualmachineconfiguration).