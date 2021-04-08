---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 09/30/2020
ms.author: alkohli
ms.openlocfilehash: 80e34c117c02c2126b46297c4219effa85e9caa6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98792344"
---
- Kopieren Sie Dateien nicht direkt in eine der vorab erstellten Freigaben. Sie müssen zunächst einen Ordner unter der Freigabe erstellen und dann Dateien in diesen Ordner kopieren.
- Ein Ordner unter *StorageAccount_BlockBlob* und *StorageAccount_PageBlob* ist ein Container. Container werden beispielsweise als *StorageAccount_BlockBlob/container* und *StorageAccount_PageBlob/container* erstellt.
- Jeder Ordner, der direkt unter *StorageAccount_AzureFiles* erstellt wird, wird in eine Azure-Dateifreigabe umgewandelt.
- Wenn ein Objekt, das kopiert wird, den gleichen Namen hat wie ein Azure-Objekt (z. B. ein Blob oder eine Datei), das bereits in der Cloud vorhanden ist, überschreibt Data Box die Datei in der Cloud.
- Jede Datei, die in die Freigaben *StorageAccount_BlockBlob* und *StorageAccount_PageBlob* geschrieben wird, wird als Blockblob bzw. Seitenblob hochgeladen.
- Azure Blob Storage unterstützt keine Verzeichnisse. Wenn Sie einen Ordner unter dem Ordner *StorageAccount_BlockBlob* erstellen, werden im Blobnamen virtuelle Ordner erstellt. Für Azure Files wird die tatsächliche Verzeichnisstruktur beibehalten.
- Eine unter den Ordnern *StorageAccount_BlockBlob* und *StorageAccount_PageBlob* erstellte leere Verzeichnishierarchie (ohne jegliche Dateien) wird nicht hochgeladen.
- Wenn beim Hochladen von Daten in Azure Fehler auftreten, wird im Zielspeicherkonto ein Fehlerprotokoll erstellt. Der Pfad zu diesem Fehlerprotokoll ist verfügbar, sobald das Hochladen abgeschlossen ist, und Sie können das Protokoll überprüfen, um Korrekturmaßnahmen zu ergreifen. Löschen Sie keine Daten aus der Quelle, ohne die hochgeladenen Daten zu überprüfen.
- Dateimetadaten und NTFS-Berechtigungen können beim Hochladen der Daten in Azure Files beibehalten werden. Siehe hierzu die Anleitung unter [Beibehalten von ACLs, Attributen und Zeitstempeln für Dateien mit Azure Data Box](../articles/databox/data-box-file-acls-preservation.md).
