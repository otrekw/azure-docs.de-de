---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 06/18/2019
ms.author: alkohli
ms.openlocfilehash: 3e1f9c225a57e7d41f85c2a92dac989453057c51
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2020
ms.locfileid: "82736976"
---
Dies sind die Grenzwerte für die Größe der Daten, die in das Speicherkonto kopiert werden. Stellen Sie sicher, dass die von Ihnen hochgeladenen Daten diesen Grenzwerten entsprechen. Aktuelle Informationen zu diesen Grenzwerten finden Sie unter [Skalierbarkeits- und Leistungsziele für Blob Storage](../articles/storage/blobs/scalability-targets.md) und [Skalierbarkeits- und Leistungsziele für Azure Files](../articles/storage/files/storage-files-scale-targets.md).

| Größe der in das Azure-Speicherkonto kopierten Daten                      | Standardlimit          |
|---------------------------------------------------------------------|------------------------|
| Blockblob und Seitenblob                                            | 2 PB für USA und Europa.<br>500 TB für alle anderen Regionen, einschließlich VK.  <br> Dies schließt Daten aus allen Quellen einschließlich Data Box ein.|
| Azure Files                                                          | Maximale Größe der Standarddateifreigaben 100 TiB*, 5 TB, Premium-Dateifreigaben 100 TiB pro Freigabe.<br> Alle Ordner unter *StorageAccount_AzureFiles* müssen diese Beschränkung einhalten.       |
