---
title: VM-Größen in Azure
description: Listet die verschiedenen verfügbaren Größen für virtuelle Computer in Azure auf.
author: ju-shim
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 16d6bfb34489df49ffccb87822ce885d8aa42d37
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84248871"
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a>Größen für virtuelle Windows-Computer in Azure

Dieser Artikel beschreibt die verfügbaren Größen und Optionen für die virtuellen Azure-Computer, die Sie zum Ausführen Ihrer Windows-Apps und Workloads verwenden können. Darüber hinaus werden Überlegungen zur Bereitstellung angestellt, die Sie berücksichtigen sollten, wenn Sie eine Verwendung dieser Ressourcen planen.  Dieser Artikel gilt für Windows- und Linux-VMs.

| type | Größen | BESCHREIBUNG |
|------|-------|-------------|
| [Allgemeiner Zweck](../sizes-general.md) | B, Dsv3, Dv3, Dasv4, Dav4, DSv2, Dv2, Av2, DC, DCv2, Ddv4, Ddsv4 | Ausgewogenes Verhältnis von CPU zu Arbeitsspeicher. Ideal für Tests und Entwicklung, kleine bis mittlere Datenbanken sowie Webserver mit geringer bis mittlerer Auslastung. |
| [Computeoptimiert](../sizes-compute.md) | Fsv2 | Hohes Verhältnis von CPU zu Arbeitsspeicher. Ideal für Webserver, Network Appliances, Stapelverarbeitungsvorgänge und Anwendungsserver mit mittlerer Auslastung. |
| [Arbeitsspeicheroptimiert](../sizes-memory.md) | Esv3, Ev3, Easv4, Eav4, Edv4, Edsv4, Mv2, M, DSv2, Dv2 | Hohes Verhältnis von Speicher zu CPU. Hervorragend geeignet für relationale Datenbankserver, mittlere bis große Caches und In-Memory-Analysen. |
| [Speicheroptimiert](../sizes-storage.md)  | Lsv2 | Hoher Datenträgerdurchsatz und E/A, ideal für Big Data, SQL, NoSQL-Datenbanken, Datawarehousing und große transaktionale Datenbanken.  |
| [GPU](../sizes-gpu.md) | NC, NCv2, NCv3, ND, NDv2 (Vorschau), NV, NVv3, NVv4 | Spezielle virtuelle Computer als Ziel für aufwendiges Grafikrendering und Videobearbeitung sowie für Modelltraining und Rückschlüsse (ND) mit Deep Learning. Mit einem oder mehreren GPUs verfügbar. |
| [High Performance Computing](../sizes-hpc.md) | HB, HBv2, HC, H | Unsere virtuellen Computer mit den schnellsten und leistungsfähigsten CPUs, die optional über Netzwerkschnittstellen mit hohem Durchsatz (RDMA) verfügen. |

- Informationen zu den Preisen der unterschiedlichen Größen finden Sie unter [Virtual Machines – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows).
- Informationen zu den allgemeinen Einschränkungen von virtuellen Azure-Computern finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../../azure-subscription-service-limits.md).
- Speicherkosten werden separat basierend auf bereits verwendete Seiten im Speicherkonto berechnet. Weitere Informationen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
- Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](../acu.md) die Computeleistung von Azure-SKUs vergleichen können.

## <a name="rest-api"></a>REST-API

Informationen zur Verwendung der REST-API, um VM-Größen abzufragen, finden Sie nachstehend:

- [List available virtual machine sizes for resizing (Auflisten der verfügbaren Größen virtueller Computer zur Größenänderung)](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)
- [List available virtual machine sizes for a subscription (Auflisten der verfügbaren Größen virtueller Computer für ein Abonnement)](https://docs.microsoft.com/rest/api/compute/resourceskus/list)
- [List available virtual machine sizes in an availability set (Auflisten der verfügbaren Größen virtueller Computer in einer Verfügbarkeitsgruppe)](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)

## <a name="acu"></a>ACU

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](../acu.md) die Computeleistung von Azure-SKUs vergleichen können.

## <a name="benchmark-scores"></a>Benchmarkergebnisse

Weitere Informationen zur Computeleistung für virtuelle Windows-Computer unter Verwendung der [CoreMark-Benchmarkergebnisse](compute-benchmark-scores.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den verschiedenen verfügbaren Größen von virtuellen Computern:

- [Allgemeiner Zweck](../sizes-general.md)
- [Computeoptimiert](../sizes-compute.md)
- [Arbeitsspeicheroptimiert](../sizes-memory.md)
- [Speicheroptimiert](../sizes-storage.md)
- [GPU-optimiert](../sizes-gpu.md)
- [High Performance Computing](../sizes-hpc.md)
- Auf der Seite [Vorherige Generation](../sizes-previous-gen.md) finden Sie die Serien A Standard, Dv1 (D1–4 und D11–14 v1) sowie A8–A11.
