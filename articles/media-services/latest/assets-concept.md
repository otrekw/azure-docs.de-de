---
title: Objekte
titleSuffix: Azure Media Services
description: Erfahren Sie, was Medienobjekte sind und wie sie von Azure Media Services verwendet werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 08/29/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: ce32343faefbcf2484ec0b1b39f752654a2d8514
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303612"
---
# <a name="assets-in-azure-media-services"></a>Medienobjekte in Azure Media Services

In Azure Media Services ist ein [Medienobjekt](https://docs.microsoft.com/rest/api/media/assets) ein zentrales Konzept. Sie können darin Medien eingeben (z. B. durch Hochladen oder Liveerfassung), Medien ausgeben (von einer Auftragsausgabe) und Medien veröffentlichen (für Streaming). 

Ein Medienobjekt ist einem Blobcontainer im [Azure Storage-Konto](storage-account-concept.md) zugeordnet, und die im Medienobjekt enthaltenen Dateien werden als Blockblobs in diesem Container gespeichert. Medienobjekte enthalten Informationen zu digitalen in Azure Storage gespeicherten Dateien (z. B. Video- und Audiodateien, Bilder, Sammlungen von Miniaturansichten, Textspuren und Untertiteldateien).

Azure Media Services unterstützt Blobebenen, wenn das Konto einen Speicher vom Typ „Allgemein v2“ (GPv2) verwendet. In GPv2-Speichern können Sie Dateien auf die [kalte Speicherebene oder die Archivspeicherebene](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) verschieben. Der **Archivspeicher** eignet sich für die Archivierung von Quelldateien, wenn diese nicht mehr benötigt werden (z. B. nach der Codierung).

Die **Archivspeicherebene** wird nur empfohlen, wenn Sie über sehr große Quelldateien verfügen, die bereits codiert wurden, und wenn der Codierungsauftrag in einem Ausgabeblobcontainer platziert wurde. Die Blobs in dem Ausgabecontainer, den Sie einem Medienobjekt zuordnen und zum Streamen oder Analysieren Ihrer Inhalte verwenden möchten, müssen auf den Speicherebenen **heiß** oder **kalt** vorhanden sein.

## <a name="naming"></a>Benennung 

### <a name="assets"></a>Objekte

Ressourcennamen müssen eindeutig sein. Media Services v3-Ressourcennamen (beispielsweise Objekte, Aufträge und Transformationen) unterliegen den Namenseinschränkungen von Azure Resource Manager. Weitere Informationen finden Sie unter [Namenskonventionen](media-services-apis-overview.md#naming-conventions).

### <a name="blobs"></a>BLOBs

Die Namen von Dateien/Blobs innerhalb einer Ressource müssen die [Anforderungen an Blobnamen](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) und die [Anforderungen an NTFS-Namen](https://docs.microsoft.com/windows/win32/fileio/naming-a-file) erfüllen. Diese Anforderungen bestehen, da die Dateien zur Verarbeitung aus Blob Storage auf einen lokalen NTFS-Datenträger kopiert werden können.

## <a name="map-v3-asset-properties-to-v2"></a>Zuordnung der Eigenschaften des Medienobjekts in v3 zu v2

Die folgende Tabelle zeigt, wie die Eigenschaften des [Medienobjekts](https://docs.microsoft.com/rest/api/media/assets/createorupdate#asset) in v3 den Eigenschaften in v2 zugeordnet werden.

|v3-Eigenschaften|v2-Eigenschaften|
|---|---|
|`id` – (eindeutig) der vollständige Azure Resource Manager-Pfad, siehe Beispiele unter [Medienobjekt](https://docs.microsoft.com/rest/api/media/assets/createorupdate)||
|`name` – (eindeutig) siehe [Benennungskonventionen](media-services-apis-overview.md#naming-conventions) ||
|`alternateId`|`AlternateId`|
|`assetId`|`Id` – (eindeutiger) Wert beginnt mit dem `nb:cid:UUID:`-Präfix.|
|`created`|`Created`|
|`description`|`Name`|
|`lastModified`|`LastModified`|
|`storageAccountName`|`StorageAccountName`|
|`storageEncryptionFormat`| `Options` (Optionen für die Erstellung)|
|`type`||

## <a name="storage-side-encryption"></a>Speicherseitige Verschlüsselung

Zum Schutz Ihrer im Ruhezustand befindlichen Ressourcen sollten die Ressourcen durch die speicherseitige Verschlüsselung verschlüsselt werden. Die folgende Tabelle zeigt, wie die speicherseitige Verschlüsselung in Media Services funktioniert:

|Verschlüsselungsoption|BESCHREIBUNG|Media Services v2|Media Services v3|
|---|---|---|---|
|Media Services-Speicherverschlüsselung|AES-256-Verschlüsselung, Schlüssel von Media Services verwaltet.|Unterstützt<sup>(1)</sup>|Nicht unterstützt<sup>(2)</sup>|
|[Speicherdienstverschlüsselung für ruhende Daten](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Durch Azure Storage angebotene serverseitige Verschlüsselung – Schlüssel wird von Azure oder vom Kunden verwaltet.|Unterstützt|Unterstützt|
|[Clientseitige Storage-Verschlüsselung](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Durch Azure Storage angebotene clientseitige Verschlüsselung – Schlüssel wird vom Kunden in Key Vault verwaltet.|Nicht unterstützt|Nicht unterstützt|

<sup>1</sup> Media Services unterstützt zwar die Behandlung von Inhalten in Klartext/ohne jede Form der Verschlüsselung, doch wird davon abgeraten.

<sup>2</sup> In Media Services v3 wird Speicherverschlüsselung (AES-256-Verschlüsselung) nur für die Abwärtskompatibilität unterstützt, wenn Ihre Ressourcen mit Media Services v2 erstellt wurden. Dies bedeutet, dass v3 mit vorhandenen speicherverschlüsselten Ressourcen funktioniert, jedoch nicht die Erstellung neuer zulässt.

## <a name="next-steps"></a>Nächste Schritte

[Verwalten von Medienobjekten in Media Services](manage-asset-concept.md)

## <a name="see-also"></a>Weitere Informationen

[Unterschiede zwischen Media Services v2 und v3](migrate-from-v2-to-v3.md)
