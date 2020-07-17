---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 06/24/2020
ms.author: alkohli
ms.openlocfilehash: 5c5732a825116d4762c3a27bcbf9eac195327afb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85378425"
---
Dies sind die Grenzwerte für die Größe der Daten, die in das Speicherkonto kopiert werden. Stellen Sie sicher, dass die von Ihnen hochgeladenen Daten diesen Grenzwerten entsprechen. Aktuelle Informationen zu diesen Grenzwerten finden Sie unter [Skalierbarkeits- und Leistungsziele für Blob Storage](../articles/storage/blobs/scalability-targets.md) und [Skalierbarkeits- und Leistungsziele für Azure Files](../articles/storage/files/storage-files-scale-targets.md).

| Größe der in das Azure-Speicherkonto kopierten Daten                      | Standardlimit          |
|---------------------------------------------------------------------|------------------------|
| Blockblob und Seitenblob                                            | Der maximale Grenzwert ist identisch mit dem [Speicherlimit, das für das Azure-Abonnement definiert ist](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#storage-limits), und enthält Daten aus allen Quellen(einschließlich Data Box).|
| Azure Files                                                          | Die maximale Größe von Standarddateifreigaben beträgt 5 TB. <br> Die maximale Größe von Premium-Dateifreigaben beträgt 100 TiB pro Freigabe.<br> Alle Ordner unter *StorageAccount_AzureFiles* müssen diese Beschränkung einhalten.       |
