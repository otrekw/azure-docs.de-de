---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 06/18/2019
ms.author: alkohli
ms.openlocfilehash: 6a9c31eb46a457433834d5940b3f7e190ebe1476
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75468517"
---
Dies sind die Grenzwerte für die Größe der Daten, die in das Speicherkonto kopiert werden. Stellen Sie sicher, dass die von Ihnen hochgeladenen Daten diesen Grenzwerten entsprechen. Aktuelle Informationen zu diesen Grenzwerten finden Sie unter [Skalierbarkeits- und Leistungsziele für Blob Storage](../articles/storage/blobs/scalability-targets.md) und [Skalierbarkeits- und Leistungsziele für Azure Files](../articles/storage/files/storage-files-scale-targets.md).

| Größe der in das Azure-Speicherkonto kopierten Daten                      | Standardlimit          |
|---------------------------------------------------------------------|------------------------|
| Blockblob und Seitenblob                                            | 2 PB für USA und Europa.<br>500 TB für alle anderen Regionen, einschließlich VK.  <br> Dies schließt Daten aus allen Quellen einschließlich Data Box ein.|
| Azure Files                                                          | 5 TB pro Freigabe.<br> Alle Ordner unter *StorageAccount_AzureFiles* müssen diese Beschränkung einhalten.       |
