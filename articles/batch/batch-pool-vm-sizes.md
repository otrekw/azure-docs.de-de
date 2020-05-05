---
title: Auswählen von VM-Größen für Pools
description: Wählen aus den verfügbaren VM-Größen für Computeknoten in Azure Batch-Pools
ms.topic: article
ms.date: 09/12/2019
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: d33b97790d2ce9a276a552fd02fe28c796234b15
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82115855"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Auswählen einer VM-Größe für Computeknoten in einem Azure Batch-Pool

Wenn Sie eine Knotengröße für einen Azure Batch-Pool wählen, können Sie aus fast allen in Azure verfügbaren VM-Größen wählen. Azure bietet eine Reihe von Größen für virtuelle Linux- und Windows-Computer für verschiedene Workloads.

Bei der Auswahl einer VM-Größe gelten einige Ausnahmen und Einschränkungen:

* Einige VM-Serien oder VM-Größen werden in Batch nicht unterstützt.
* Einige VM-Größen sind eingeschränkt und müssen explizit aktiviert werden, damit sie zugeordnet werden können.

## <a name="supported-vm-series-and-sizes"></a>Unterstützte VM-Serien und -Größen

### <a name="pools-in-virtual-machine-configuration"></a>Pools in der Konfiguration des virtuellen Computers

Batch-Pools in der Konfiguration des virtuellen Computers unterstützen nahezu alle VM-Größen ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)). In der folgenden Tabelle finden Sie weitere Informationen zu unterstützten Größen und Einschränkungen.

| VM-Serie  | Unterstützte Größen |
|------------|---------|
| Basic A | Alle Größen *außer* Basic_A0 (A0) |
| Ein | Alle Größen *außer* Standard_A0 |
| Av2 | Alle Größen |
| B | Keine |
| SL | Keine |
| Dv2, DSv2 | Alle Größen |
| Dv3, Dsv3 | Alle Größen |
| Dav4, Dasv4 | Keine: Noch nicht verfügbar |
| Ev3, Esv3 | Alle Größen, mit Ausnahme von E64is_v3 und E64i_v3 |
| Eav4, Easv4 | Keine: Noch nicht verfügbar |
| F, Fs | Alle Größen |
| Fsv2 | Alle Größen |
| G, Gs | Alle Größen |
| H | Alle Größen |
| HB<sup>1</sup> | Alle Größen |
| HBv2<sup>1</sup> | Alle Größen |
| HC<sup>1</sup> | Alle Größen |
| Ls | Alle Größen |
| Lsv2 | Keine: Noch nicht verfügbar |
| M<sup>1</sup> | Alle Größen, mit Ausnahme von M64, M64m, M128, M128m |
| Mv2 | Keine: Noch nicht verfügbar |
| NC | Alle Größen |
| NCv2<sup>1</sup> | Alle Größen |
| NCv3<sup>1</sup> | Alle Größen |
| ND<sup>1</sup> | Alle Größen |
| NDv2<sup>1</sup> | Keine: Noch nicht verfügbar |
| SH | Alle Größen |
| NVv3<sup>1</sup> | Alle Größen |
| NVv4 | Keine |
| SAP HANA | Keine |

<sup>1</sup> Diese VM-Größen können bei der Konfiguration des virtuellen Computers in Batch-Pools zugewiesen werden. Sie müssen aber ein neues Batch-Konto erstellen und eine bestimmte [Kontingenterhöhung](batch-quota-limit.md#increase-a-quota) anfordern. Diese Einschränkung wird aufgehoben, sobald das vCPU-Kontingent pro VM-Serie für Batch-Konten vollständig unterstützt wird.

### <a name="pools-in-cloud-service-configuration"></a>Pools in der Clouddienstkonfiguration

Batch-Pools in der Clouddienstkonfiguration unterstützen alle [VM-Größen für Cloud Services](../cloud-services/cloud-services-sizes-specs.md) **mit Ausnahme** der folgenden:

| VM-Serie  | Nicht unterstützte Größen |
|------------|-------------------|
| A-Serie   | Sehr klein       |
| Av2-Serie | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

## <a name="size-considerations"></a>Überlegungen zu Größen

* **Anwendungsanforderungen**: Berücksichtigen Sie die Merkmale und Anforderungen der Anwendungen, die auf den Knoten ausgeführt werden sollen. Die Beantwortung der Fragen, ob es sich beispielsweise um eine Multithreadanwendung handelt und wie viel Arbeitsspeicher sie beansprucht, kann Ihnen dabei behilflich sein, die am besten geeignete und kostengünstigste Knotengröße zu bestimmen. Ziehen Sie für [MPI-Workloads](batch-mpi.md) mit mehreren Instanzen oder CUDA-Anwendungen spezielle [HPC](../virtual-machines/linux/sizes-hpc.md)- bzw. [GPU-fähige ](../virtual-machines/linux/sizes-gpu.md) VM-Größen in Betracht. (Informationen dazu finden Sie unter [Verwenden RDMA-fähiger oder GPU-fähiger Instanzen in Batch-Pools](batch-pool-compute-intensive-sizes.md).)

* **Aufgaben pro Knoten**: Die Knotengröße wird normalerweise unter der Annahme ausgewählt, dass jeweils nur eine Aufgabe auf einem Knoten ausgeführt wird. Es kann jedoch von Vorteil sein, mehrere Aufgaben (und somit mehrere Anwendungsinstanzen) während der Auftragsausführung auf Computeknoten [parallel zu nutzen](batch-parallel-node-tasks.md). In diesem Fall wird häufig eine Knotengröße mit mehreren Kernen gewählt, um den höheren Bedarf an parallelen Aufgabenausführungen decken zu können.

* **Auslastungsgrade für verschiedene Aufgaben**: Alle Knoten in einem Pool haben die gleiche Größe. Wenn Sie Anwendungen mit unterschiedlichen Systemanforderungen bzw. Auslastungsgraden ausführen möchten, empfehlen wir Ihnen die Verwendung von separaten Pools.

* **Regionale Verfügbarkeit**: Eine VM-Serie oder -Größe ist in den Regionen, in denen Sie die Batch-Konten erstellen, möglicherweise nicht verfügbar. Informationen dazu, welche Größen verfügbar sind, finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).

* **Kontingente**: Die [Kernkontingente](batch-quota-limit.md#resource-quotas) in Ihrem Batch-Konto können die Anzahl der Knoten einer bestimmten Größe beschränken, die Sie einem Batch-Pool hinzufügen können. Informationen zum Anfordern einer Kontingenterhöhung finden Sie in [diesem Artikel](batch-quota-limit.md#increase-a-quota). 

* **Poolkonfiguration**: Im Allgemeinen stehen Ihnen mehr VM-Größenoptionen zur Verfügung, wenn Sie einen Pool in der Konfiguration des virtuellen Computers anstatt in der Clouddienstkonfiguration erstellen.

## <a name="next-steps"></a>Nächste Schritte

* Eine detaillierte Übersicht über Batch finden Sie unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](batch-api-basics.md).
* Informationen zur Verwendung von rechenintensiven VM-Größen finden Sie unter [Verwenden RDMA-fähiger oder GPU-fähiger Instanzen in Batch-Pools](batch-pool-compute-intensive-sizes.md).
