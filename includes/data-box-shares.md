---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 06/05/2020
ms.author: alkohli
ms.openlocfilehash: 5aaf0ce747b14b2fa9f2fcd9a65b774aa7d2db3b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "87102682"
---
Je nach ausgewähltem Speicherkonto erstellt Data Box bis zu:

* Drei Freigaben für jedes verknüpfte Speicherkonto für GPv1 und GPv2
* Eine Freigabe für Storage Premium
* Eine Freigabe für das Blob Storage-Konto

Unter Blockblob- und Seitenblobfreigaben sind Entitäten der ersten Ebene Container, und Entitäten der zweiten Ebene sind Blobs. Unter Freigaben für Azure Files sind Entitäten erster Ebene Freigaben. Entitäten zweiter Ebene sind Dateien.

In der folgenden Tabelle sind der UNC-Pfad zu den Freigaben auf Ihrer Data Box und die Azure Storage-Pfad-URL aufgeführt, wohin die Daten hochgeladen werden. Die endgültige URL des Azure Storage-Pfads kann aus dem UNC-Freigabepfad abgeleitet werden.
 
| Blobs und Dateien | Pfade und URLs |
| --------------- | -------------- |
| Azure-Blockblobs | <li>UNC-Pfad zu den Freigaben: `\\<DeviceIPAddress>\<StorageAccountName_BlockBlob>\<ContainerName>\files\a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li> |  
| Azure-Seitenblobs  | <li>UNC-Pfad zu den Freigaben: `\\<DeviceIPAddres>\<StorageAccountName_PageBlob>\<ContainerName>\files\a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li>   |  
| Azure Files       |<li>UNC-Pfad zu den Freigaben: `\\<DeviceIPAddres>\<StorageAccountName_AzFile>\<ShareName>\files\a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.file.core.windows.net/<ShareName>/files/a.txt`</li>        |      

