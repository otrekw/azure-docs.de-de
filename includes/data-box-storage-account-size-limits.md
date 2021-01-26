---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 01/12/2021
ms.author: alkohli
ms.openlocfilehash: 6cabac4c59b09d146c1e42762402043622edb60e
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98225173"
---
Dies sind die Grenzwerte für die Größe der Daten, die in ein Speicherkonto kopiert werden. Stellen Sie sicher, dass die von Ihnen hochgeladenen Daten diesen Grenzwerten entsprechen. Aktuelle Informationen zu diesen Grenzwerten finden Sie unter [Skalierbarkeits- und Leistungsziele für Blob Storage](../articles/storage/blobs/scalability-targets.md) und [Skalierbarkeits- und Leistungsziele für Azure Files](../articles/storage/files/storage-files-scale-targets.md).

| Größe der in das Azure-Speicherkonto kopierten Daten                      | Standardlimit          |
|---------------------------------------------------------------------|------------------------|
| Blockblob und Seitenblob                                            | Der maximale Grenzwert ist identisch mit dem [Speicherlimit, das für das Azure-Abonnement definiert ist](../articles/azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits), und enthält Daten aus allen Quellen(einschließlich Data Box).   |
| Azure Files                                                          | Data Box unterstützt große Dateifreigaben (100 TiB), wenn dies vor Erstellung des Data Box-Auftrags aktiviert wurde. <br> Wenn diese Option nicht vor der Auftragserstellung nicht aktiviert wurde, werden Dateifreigaben von maximal 5 TiB unterstützt. <br> Data Box unterstützt Azure Premium-Dateifreigaben, die insgesamt 100 TiB für alle Freigaben im Speicherkonto ermöglichen. <br> Die maximal nutzbare Kapazität ist aufgrund des von Kopier- und Überwachungsprotokollen verwendeten Speicherplatzes etwas geringer. Mindestens jeweils 100 GiB wird für das Kopier- und das Überwachungsprotokoll reserviert. Weitere Informationen finden Sie unter [Überwachungsprotokolle für Azure Data Box und Azure Data Box Heavy](../articles/databox/data-box-audit-logs.md). <br> Alle Ordner unter *StorageAccount_AzureFiles* müssen diese Beschränkung einhalten. <br> Weitere Informationen finden Sie unter [Aktivieren und Konfigurieren von FILESTREAM](../articles/storage/files/storage-files-how-to-create-large-file-share.md).      |
