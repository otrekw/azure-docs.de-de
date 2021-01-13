---
title: Erstellen einer Azure Media Services-Auftragseingabe aus einer lokalen Datei | Microsoft-Dokumentation
description: In diesem Artikel wird gezeigt, wie Sie eine Azure Media Services-Auftragseingabe aus einer lokalen Datei erstellen.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: 02aaae89f7544392765b46361b1617e0fd7dae8c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89296805"
---
# <a name="create-a-job-input-from-a-local-file"></a>Erstellen einer Auftragseingabe aus einer lokalen Datei

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Wenn Sie in Media Services v3 Aufträge zur Verarbeitung Ihrer Videos übermitteln, müssen Sie Media Services mitteilen, wo sich das Eingabevideo befindet. Das Eingabevideo kann als Media Service-Medienobjekt gespeichert werden. In diesem Fall erstellen Sie ein Eingabemedienobjekt basierend auf einer Datei (die lokal oder in Azure Blob Storage gespeichert ist). In diesem Thema wird gezeigt, wie Sie eine Auftragseingabe aus einer lokalen Datei erstellen. Ein vollständiges Beispiel finden Sie in diesem [GitHub-Beispiel](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="prerequisites"></a>Voraussetzungen 

* [Erstellen Sie ein Media Services-Konto.](./create-account-howto.md)

## <a name="net-sample"></a>.NET-Beispiel

Der folgende Code zeigt, wie Sie ein Eingabemedienobjekt erstellen und es als Eingabe für den Auftrag nutzen können. Die „CreateInputAsset“-Funktion führt die folgenden Aktionen aus:

* Das Medienobjekt erstellen
* Abrufen einer nicht schreibgeschützten [SAS-URL](../../storage/common/storage-sas-overview.md) für den [Container](../../storage/blobs/storage-quickstart-blobs-dotnet.md#upload-blobs-to-a-container) des Objekts im Speicher
* Die Datei in den Container im Speicher mithilfe der SAS-URL hochladen

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

Der folgende Codeausschnitt erzeugt ein Ausgabemedienobjekt, wenn es nicht bereits vorhanden ist:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateOutputAsset)]

Der folgende Codeausschnitt übermittelt einen Codierungsauftrag:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="job-error-codes"></a>Auftragsfehlercodes

Weitere Informationen finden Sie unter [Fehlercodes](/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Auftragseingabe aus einer HTTPS-URL](job-input-from-http-how-to.md)
