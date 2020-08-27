---
title: Hochladen von Medien
titleSuffix: Azure Media Services
description: Hier erfahren Sie, wie Sie Medien für Streaming und Codierung hochladen.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.date: 08/11/2020
ms.author: inhenkel
ms.openlocfilehash: a046a3caba9a0909d873356bda8d1fa6cf1f9860
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2020
ms.locfileid: "88719256"
---
# <a name="upload-media-for-streaming-or-encoding"></a>Hochladen von Medien für Streaming und Codierung

In Media Services laden Sie Ihre digitalen Dateien (Medien) in einen Blobcontainer hoch, der einem Medienobjekt zugeordnet ist. Die [Asset](/rest/api/media/operations/asset)-Entität kann Videos, Audiodateien, Bilder, Miniaturansichtssammlungen, Texttitel und Untertiteldateien (und die Metadaten zu diesen Dateien) enthalten. Nachdem die Dateien in den Container des Medienobjekts hochgeladen wurden, werden Ihre Inhalte zur weiteren Verarbeitung und zum Streaming sicher in der Cloud gespeichert.

Bevor Sie beginnen, müssen Sie jedoch einige Werte erfassen bzw. berücksichtigen.

1. Lokaler Dateipfad zu der Datei, die Sie hochladen möchten
1. Asset-ID für das Medienobjekt (Container)
1. Name, den Sie für die hochgeladene Datei verwenden möchten, einschließlich der Erweiterung
1. Name des von Ihnen verwendeten Speicherkontos
1. Zugriffsschlüssel für Ihr Speicherkonto

## <a name="portal"></a>[Portal](#tab/portal/)

[!INCLUDE [Upload files with the portal](./includes/task-upload-file-to-asset-portal.md)]

## <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/cli/)

[!INCLUDE [Upload files with the portal](./includes/task-upload-file-to-asset-cli.md)]

## <a name="rest"></a>[REST](#tab/rest/)

Nachdem Sie [mithilfe von Postman oder einer anderen REST-Methode ein Medienobjekt erstellt und die SAS-URL für das Medienobjekt abgerufen haben](how-to-create-asset.md?tabs=rest), verwenden Sie die Azure Storage-APIs oder SDKs (etwa die [Storage-REST-API](../../storage/common/storage-rest-api-auth.md) oder das [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)).

---
<!-- add these to the tabs when available -->
Informationen zu weiteren Methoden finden Sie in der [Azure Storage-Dokumentation](https://docs.microsoft.com/azure/storage/blobs/) zum Arbeiten mit Blobs in [.NET](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet), [Java](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-java), [Python](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-python) und [JavaScript (Node.js)](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-nodejs).

## <a name="next-steps"></a>Nächste Schritte

> [Media Services: Übersicht](media-services-overview.md)
