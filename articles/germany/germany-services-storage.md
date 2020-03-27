---
title: Speicherdienste von Azure Deutschland | Microsoft-Dokumentation
description: Dieses Thema enthält einen Vergleich der Speicherdienste für Azure Deutschland. Außerdem finden Sie hier weitere relevante Informationen.
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2019
ms.author: ralfwi
ms.openlocfilehash: 33f5235941f5e274c9c17929e527534054f76e23
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "75436645"
---
# <a name="azure-germany-storage-services"></a>Speicherdienste von Azure Deutschland

> [!IMPORTANT]
> Seit [August 2018](https://news.microsoft.com/europe/2018/08/31/microsoft-to-deliver-cloud-services-from-new-datacentres-in-germany-in-2019-to-meet-evolving-customer-needs/) haben wir keine neuen Kunden akzeptiert und keine neuen Features und Dienste an den ursprünglichen Microsoft Cloud Deutschland-Standorten bereitgestellt.
>
> Aufgrund der Weiterentwicklung der Kundenbedürfnisse haben wir vor Kurzem zwei neue Rechenzentrumsregionen in Deutschland [gestartet](https://azure.microsoft.com/blog/microsoft-azure-available-from-new-cloud-regions-in-germany/), die Datenresidenz für Kundendaten, umfassende Konnektivität mit dem globalen Cloudnetzwerk von Microsoft sowie wettbewerbsfähige Preise bieten. 
>
> Profitieren Sie von der Vielfalt der Funktionen, Sicherheit auf Unternehmensniveau und den umfangreichen Features, die in unseren neuen deutschen Rechenzentrumsregionen zur Verfügung stehen, und [migrieren](germany-migration-main.md) Sie noch heute.

## <a name="storage"></a>Storage
Einzelheiten zu Azure Storage und seiner Verwendung finden Sie in der [globalen Dokumentation zu Storage](../storage/index.yml).

In Azure Storage gespeicherte Daten werden repliziert, um Hochverfügbarkeit sicherzustellen. Bei georedundanten Speichern und georedundanten Speichern mit Lesezugriff repliziert Azure Daten zwischen den *Regionspaaren*. Bei Azure Deutschland sind folgende Regionspaare verfügbar:

| Primäre Region | Sekundäre (zugehörige) Region |
| --- | --- |
| Deutschland, Mitte | Deutschland, Nordosten |
| Deutschland, Nordosten | Deutschland, Mitte |

Bei der Datenreplikation werden die Daten innerhalb deutscher Grenzen gehalten. Die primäre und sekundäre Region werden einander zugeordnet, um sicherzustellen, dass ausreichend Abstand zwischen den Rechenzentren besteht, um bei einem Ausfall eines großen Gebiets oder bei einem Notfall die Verfügbarkeit sicherzustellen. Wählen Sie bei georedundanten Speichern mit hoher Verfügbarkeit beim Erstellen eines Speicherkontos entweder georedundante Speicher oder georedundante Speicher mit Lesezugriff aus.  

Die Speicherdienstverschlüsselung schützt ruhende Daten in Azure Storage-Konten. Wenn Sie diese Funktion aktivieren, verschlüsselt Azure automatisch Daten, bevor sie in den Speicher eingefügt werden. Die Daten werden mithilfe einer 256-Bit-AES-Verschlüsselung verschlüsselt. Die Speicherdienstverschlüsselung unterstützt die Verschlüsselung von Block-, Anfüge- und Seitenblobs.

### <a name="storage-service-availability-by-azure-germany-region"></a>Speicherdienstverfügbarkeit nach Azure Deutschland-Region

| Dienst | Deutschland, Mitte | Deutschland, Nordosten |
| --- | --- | --- |
| [Blob Storage](../storage/common/storage-introduction.md#blob-storage) |Allgemein verfügbar |Allgemein verfügbar |
| [Azure Files](../storage/common/storage-introduction.md#azure-files) | Allgemein verfügbar | Allgemein verfügbar |
| [Tabellenspeicherung](../storage/common/storage-introduction.md#table-storage) |Allgemein verfügbar  |Allgemein verfügbar |
| [Queue Storage](../storage/common/storage-introduction.md#queue-storage) |Allgemein verfügbar | Allgemein verfügbar |
| [Hot/Cold Blob Storage](../storage/blobs/storage-blob-storage-tiers.md) |Allgemein verfügbar |Allgemein verfügbar |
| [Storage Service Encryption](../storage/common/storage-service-encryption.md) |Allgemein verfügbar |Allgemein verfügbar |
| Importieren/Exportieren |Nicht verfügbar |Nicht verfügbar |
| StorSimple |Nicht verfügbar |Nicht verfügbar |

### <a name="variations"></a>Abweichungen
Die URLs für Speicherkonten in Azure Deutschland unterscheiden sich von denen in der globalen Azure-Umgebung:

| Dienstart | Globale Azure-Umgebung | Azure Deutschland |
| --- | --- | --- |
| Blob Storage | *.blob.core.windows.net | *.blob.core.cloudapi.de |
| Azure Files | *.file.core.windows.net | *.file.core.cloudapi.de | 
| Queue Storage | *.queue.core.windows.net | *.queue.core.cloudapi.de |
| Table Storage | *.table.core.windows.net | *.table.core.cloudapi.de |

> [!NOTE]
> Ihre gesamten Skripts und Ihr Code müssen die passenden Endpunkte berücksichtigen. Weitere Informationen hierzu finden Sie unter [Konfigurieren von Azure Storage-Verbindungszeichenfolgen](../storage/common/storage-configure-connection-string.md). 
>
>

Weitere Informationen zu APIs finden Sie unter [CloudStorageAccount-Konstruktor](/dotnet/api/microsoft.azure.cosmos.table.cloudstorageaccount.-ctor).

Das zu verwendende Endpunktsuffix für diese Überladungen lautet *core.cloudapi.de*.

> [!NOTE]
> Wenn während der [Einbindung der Dateifreigabe](../storage/files/storage-dotnet-how-to-use-files.md) der Fehler 53 („Der Netzwerkpfad wurde nicht gefunden.“) zurückgegeben wird, blockiert möglicherweise eine Firewall den ausgehenden Port. Versuchen Sie, die Dateifreigabe auf einem virtuellen Computer einzubinden, der sich im selben Azure-Abonnement wie das Speicherkonto befindet.
>
>


## <a name="next-steps"></a>Nächste Schritte
Abonnieren Sie den [Azure Deutschland-Blog](https://blogs.msdn.microsoft.com/azuregermany/), um weitere Informationen und Updates zu erhalten.
