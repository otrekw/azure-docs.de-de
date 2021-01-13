---
title: Objekte
titleSuffix: Azure Media Services
description: Erfahren Sie, was Medienobjekte sind und wie sie von Azure Media Services verwendet werden.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: seodec18
ms.openlocfilehash: 99101f31ec955c822c784180dd8b04bfb8e5f73c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89291601"
---
# <a name="assets-in-azure-media-services-v3"></a>Medienobjekte in Azure Media Services v3

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

In Azure Media Services ist ein [Medienobjekt](/rest/api/media/assets) ein zentrales Konzept. Sie können darin Medien eingeben (z. B. durch Hochladen oder Liveerfassung), Medien ausgeben (von einer Auftragsausgabe) und Medien veröffentlichen (für Streaming). 

Ein Medienobjekt ist einem Blobcontainer im [Azure Storage-Konto](storage-account-concept.md) zugeordnet, und die im Medienobjekt enthaltenen Dateien werden als Blockblobs in diesem Container gespeichert. Medienobjekte enthalten Informationen zu digitalen in Azure Storage gespeicherten Dateien (z. B. Video- und Audiodateien, Bilder, Sammlungen von Miniaturansichten, Textspuren und Untertiteldateien).

Azure Media Services unterstützt Blobebenen, wenn das Konto einen Speicher vom Typ „Allgemein v2“ (GPv2) verwendet. In GPv2-Speichern können Sie Dateien auf die [kalte Speicherebene oder die Archivspeicherebene](../../storage/blobs/storage-blob-storage-tiers.md) verschieben. Der **Archivspeicher** eignet sich für die Archivierung von Quelldateien, wenn diese nicht mehr benötigt werden (z. B. nach der Codierung).

Die **Archivspeicherebene** wird nur empfohlen, wenn Sie über sehr große Quelldateien verfügen, die bereits codiert wurden, und wenn der Codierungsauftrag in einem Ausgabeblobcontainer platziert wurde. Die Blobs in dem Ausgabecontainer, den Sie einem Medienobjekt zuordnen und zum Streamen oder Analysieren Ihrer Inhalte verwenden möchten, müssen auf den Speicherebenen **heiß** oder **kalt** vorhanden sein.

## <a name="naming"></a>Benennung 

### <a name="assets"></a>Objekte

Ressourcennamen müssen eindeutig sein. Media Services v3-Ressourcennamen (beispielsweise Objekte, Aufträge und Transformationen) unterliegen den Namenseinschränkungen von Azure Resource Manager. Weitere Informationen finden Sie unter [Namenskonventionen](media-services-apis-overview.md#naming-conventions).

### <a name="blobs"></a>BLOBs

Die Namen von Dateien/Blobs innerhalb einer Ressource müssen die [Anforderungen an Blobnamen](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) und die [Anforderungen an NTFS-Namen](/windows/win32/fileio/naming-a-file) erfüllen. Diese Anforderungen bestehen, da die Dateien zur Verarbeitung aus Blob Storage auf einen lokalen NTFS-Datenträger kopiert werden können.

## <a name="next-steps"></a>Nächste Schritte

[Media Services: Übersicht](media-services-overview.md)

## <a name="see-also"></a>Weitere Informationen

[Unterschiede zwischen Media Services v2 und v3](migrate-from-v2-to-v3.md)
