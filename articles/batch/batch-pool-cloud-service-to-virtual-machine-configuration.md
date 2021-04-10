---
title: Migrieren der Batch-Poolkonfiguration von Cloud Services zu Virtual Machines
description: Erfahren Sie, wie Sie Ihre Poolkonfiguration auf die neueste und empfohlene Konfiguration aktualisieren.
ms.topic: how-to
ms.date: 03/11/2021
ms.openlocfilehash: a176c4df1737a340a546b4ab7926447cd821350d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103200557"
---
# <a name="migrate-batch-pool-configuration-from-cloud-services-to-virtual-machine"></a>Migrieren der Batch-Poolkonfiguration von Cloud Services zu Virtual Machines

Batch-Pools können derzeit entweder mit [virtualMachineConfiguration](/rest/api/batchservice/pool/add#virtualmachineconfiguration) oder [cloudServiceConfiguration](/rest/api/batchservice/pool/add#cloudserviceconfiguration) erstellt werden. Sie sollten nur die Konfiguration des virtuellen Computers verwenden, da diese Konfiguration alle Batch-Funktionen unterstützt.

Cloud Services-Konfigurationspools unterstützen einige der aktuellen Batch-Features nicht und keine neu hinzugefügten Features. Sie können [nach dem 29. Februar 2024](https://azure.microsoft.com/updates/azure-batch-cloudserviceconfiguration-pools-will-be-retired-on-29-february-2024/) weder neue CloudServiceConfiguration-Pools erstellen noch vorhandenen Pools neue Knoten hinzuzufügen.

Wenn Ihre Batch-Lösungen derzeit cloudServiceConfiguration-Pools verwenden, sollten Sie so bald wie möglich zu virtualMachineConfiguration wechseln. So können Sie von allen Batch-Funktionen profitieren, wie z. B.: erweiterte [Auswahl von VM-Serien](batch-pool-vm-sizes.md), Linux-VMs, [Container](batch-docker-container-workloads.md), [virtuelle Azure Resource Manager-Netzwerke](batch-virtual-network.md) und [Datenträgerverschlüsselung auf Knoten](disk-encryption.md).

## <a name="create-a-pool-using-virtual-machine-configuration"></a>Erstellen eines Pools mithilfe der Konfiguration des virtuellen Computers

Sie können einen vorhandenen aktiven Pool, der cloudServiceConfiguration verwendet, nicht zum Verwenden von virtualMachineConfiguration ändern. Stattdessen müssen Sie neue Pools erstellen. Nachdem Sie die neuen virtualMachineConfiguration-Pools erstellt und alle Aufträge und Aufgaben repliziert haben, können Sie die alten cloudServiceConfiguration-Pools löschen, die Sie nicht mehr verwenden.

Mit allen Batch-APIs, Befehlszeilentools, im Azure-Portal und auf der Batch Explorer-Benutzeroberfläche können Sie Pools mithilfe von virtualMachineConfiguration erstellen.

Eine exemplarische Vorgehensweise zum Erstellen von Pools, die virtualMachineConfiguration verwenden, finden Sie im [.NET-Tutorial](tutorial-parallel-dotnet.md) oder [Python-Tutorial](tutorial-parallel-python.md).

## <a name="pool-configuration-differences"></a>Unterschiede bei der Poolkonfiguration

Einige der Hauptunterschiede zwischen den beiden Konfigurationen sind:

- cloudServiceConfiguration-Pool-Knoten verwenden nur das Windows-Betriebssystem. virtualMachineConfiguration-Pools können entweder Linux oder Windows als Betriebssystem verwenden.
- Im Vergleich zu Pools mit „cloudServiceConfiguration“ bieten Pools mit „virtualMachineConfiguration“ mehr Funktionen, wie z. B. Containerunterstützung, Datenträger für Daten und Datenträgerverschlüsselung.
- Die Start- und Löschzeiten für Pools und Knoten können sich bei Pools mit „cloudServiceConfiguration“ und Pools mit „virtualMachineConfiguration“ geringfügig unterscheiden.
- Poolknoten mit „virtualMachineConfiguration“ verwenden verwaltete Betriebssystem-Datenträger. Der für die jeweiligen Knoten verwendete [Typ der verwalteten Datenträger](../virtual-machines/disks-types.md) richtet sich nach der für den Pool ausgewählten VM-Größe. Wenn die für den Pool angegebene VM-Größe ein „s“ enthält (z. B. Standard_D2s_v3), wird ein Premium-SSD verwendet. Wenn die VM-Größe kein „s“ enthält (z. B. Standard_D2_v3), wird ein Standard-Festplattenlaufwerk verwendet.

   > [!IMPORTANT]
   > Ebenso wie bei Virtual Machines und Virtual Machine Scale Sets verursachen die verwalteten Betriebssystem-Datenträger der einzelnen Knoten Kosten, die zusätzlich zu den VM-Gebühren anfallen. Da die Betriebssystem-Datenträger auf den lokalen SSDs der Knoten erstellt werden, fallen für cloudServiceConfiguration-Knoten keine Kosten für diese Datenträger an.

## <a name="azure-data-factory-custom-activity-pools"></a>Pools benutzerdefinierter Azure Data Factory-Aktivitäten

Mithilfe von Azure Batch-Pools können benutzerdefinierte Data Factory-Aktivitäten ausgeführt werden. Alle Pools mit „cloudServiceConfiguration“, die zum Ausführen von benutzerdefinierten Aktivitäten verwendet werden, müssen gelöscht werden, und neue Pools mit „virtualMachineConfiguration“ müssen erstellt werden.

Wenn Sie neue Pools erstellen, um benutzerdefinierte Data Factory-Aktivitäten auszuführen, befolgen Sie die folgenden Vorgehensweisen:

- Halten Sie vor dem Erstellen der neuen Pools und Löschen der alten alle Pipelines an, um sicherzustellen, dass keine Ausführungen unterbrochen werden.
- Die gleiche Pool-ID kann verwendet werden, um Änderungen an der Konfiguration von verknüpften Diensten zu vermeiden.
- Setzen Sie die Pipelines fort, nachdem die neuen Pools erstellt wurden.

Weitere Informationen zur Verwendung von Azure Batch, um benutzerdefinierte Data Factory-Aktivitäten auszuführen, finden Sie unter [Verknüpfter Azure Batch-Dienst](../data-factory/compute-linked-services.md#azure-batch-linked-service) und [Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline](../data-factory/transform-data-using-dotnet-custom-activity.md).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Poolkonfigurationen](nodes-and-pools.md#configurations).
- Erfahren Sie mehr über [bewährte Methoden für Pools](best-practices.md#pools).
- Lesen Sie die REST-API-Referenz zum [Hinzufügen von Pools](/rest/api/batchservice/pool/add) und zu [virtualMachineConfiguration](/rest/api/batchservice/pool/add#virtualmachineconfiguration).