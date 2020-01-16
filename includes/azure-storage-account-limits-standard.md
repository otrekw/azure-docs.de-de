---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/13/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a0ecb4f5fe54e9895898156893c1e686c6cc24e6
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75392284"
---
In der folgenden Tabelle werden die für Azure GPv1, GPv2 und Blob-Speicherkonten geltenden Standardgrenzwerte beschrieben. Das Limit für den *Eingang* bezieht sich auf alle Daten aus Anforderungen, die an ein Speicherkonto gesendet werden. Das Limit für den *Ausgang* bezieht sich auf alle Daten aus Anforderungen, die von einem Speicherkonto empfangen werden.

| Resource | Standardlimit |
| --- | --- |
| Anzahl von Speicherkonten pro Region und Abonnement, einschließlich Standard- und Premium-Konten. | 250 |
| Maximale Speicherkontokapazität | 2 PiB für USA und Europa, und 500 TiB für alle anderen Regionen (einschließlich Vereinigtes Königreich)<sup>1</sup>|
| Maximale Anzahl an Blobcontainern, Blobs, Dateifreigaben, Tabellen, Warteschlangen, Entitäten oder Meldungen pro Speicherkonto | Keine Begrenzung |
| Maximale Anforderungsrate<sup>1</sup> pro Speicherkonto | 20.000 Anforderungen pro Sekunde |
| Maximaler Eingang<sup>1</sup> pro Speicherkonto (Regionen US, Europa) | 25GBit/s |
| Maximaler Eingang<sup>1</sup> pro Speicherkonto (Regionen außerhalb von USA und Europa) | 5 GBit/s bei aktiviertem RA-GRS/GRS, 10 GBit/s für LRS/ZRS<sup>2</sup> |
| Maximaler Ausgang für universelle v2-Speicherkonten und Blobspeicherkonten (alle Regionen) | 50 GBit/s |
| Maximaler Ausgang für universelle v1-Speicherkonten (US-Regionen) | 20 GBit/s bei aktiviertem RA-GRS/GRS, 30 GBit/s für LRS/ZRS<sup>2</sup> |
| Maximaler Ausgang für universelle v1-Speicherkonten (US-fremde Regionen) | 10 GBit/s bei aktiviertem RA-GRS/GRS, 15 GBit/s für LRS/ZRS<sup>2</sup> |
| Maximale Anzahl von VNET-Regeln pro Speicherkonto | 200 |
| Maximale Anzahl von Regeln für IP-Adressen pro Speicherkonto | 200 |

<sup>1</sup> Azure Storage Standard-Konten unterstützen höhere Grenzwerte für Kapazität und Eingang auf Anforderung. Wenden Sie sich an den [Azure-Support](https://azure.microsoft.com/support/faq/), um eine Erhöhung der Kontogrenzwerte für Eingang anzufordern. Weitere Informationen finden Sie unter [Ankündigung größerer Speicherkonten mit höherer Skalierung](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/).

<sup>2</sup> Wenn für Ihr Speicherkonto Lesezugriff mit georedundantem Speicher (RA-GRS) oder geozonenredundantem Speicher (RA-GZRS) aktiviert ist, sind die Ausgangsziele für den sekundären Standort mit denen des primären Standorts identisch. Für die [Azure Storage-Replikation](https://docs.microsoft.com/azure/storage/common/storage-redundancy) sind folgende Optionen verfügbar:

[!INCLUDE [azure-storage-redundancy](azure-storage-redundancy.md)]

> [!NOTE]
> Microsoft empfiehlt, für die meisten Szenarien Speicherkonten vom Typ „Allgemein v2“ zu verwenden. Sie können ganz einfach ein Upgrade von einem Konto vom Typ „Allgemein v1“ oder einem Blob Storage-Konto auf ein Konto vom Typ „Allgemein v2“ durchführen. Dabei treten keine Ausfallzeiten auf, und Sie müssen keine Daten kopieren. Weitere Informationen finden Sie unter [Durchführen eines Upgrades auf ein Speicherkonto vom Typ „Allgemein v2“](../articles/storage/common/storage-account-upgrade.md).

Wenn die Anforderungen Ihrer Anwendung die Skalierbarkeitsziele eines einzelnen Speicherkontos überschreiten, können Sie die Anwendung so erstellen, dass mehrere Speicherkonten verwendet werden. Sie können Ihre Datenobjekte dann basierend auf diesen Speicherkonten partitionieren. Informationen zu Volumenpreisen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

Alle Speicherkonten werden in einer flachen Netzwerktopologie ausgeführt und unterstützen die in diesem Artikel beschriebenen Skalierbarkeits- und Leistungsziele, unabhängig vom Zeitpunkt ihrer Erstellung. Weitere Informationen zur flachen Netzwerkarchitektur von Azure Storage sowie zur Skalierbarkeit finden Sie unter [Microsoft Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx) (Hochverfügbarer Cloud-Speicherdienst mit starker Konsistenz).
