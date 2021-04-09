---
title: Verringern der Wartezeit bei Verwendung des Diensts „Gesichtserkennung“
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie die Wartezeit bei Verwendung des Diensts „Gesichtserkennung“ verringern.
services: cognitive-services
author: v-jaswel
manager: chrhoder
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 1/5/2021
ms.author: v-jawe
ms.openlocfilehash: 2c771509de5ac246bac0d8e006a5d0b884a410b0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "101706808"
---
# <a name="how-to-mitigate-latency-when-using-the-face-service"></a>Vorgehensweise: Verringern der Wartezeit bei Verwendung des Diensts „Gesichtserkennung“

Bei Verwendung des Diensts „Gesichtserkennung“ können Wartezeiten auftreten. Als Wartezeit wird jede Art von Verzögerung bezeichnet, die bei der Kommunikation über ein Netzwerk auftritt. Im Allgemeinen können Wartezeiten u. a. folgende Ursachen haben:
- Die räumliche Distanz, die die einzelnen Pakete von der Quelle bis zum Ziel überwinden müssen
- Probleme mit dem Übertragungsmedium
- Fehler in Routern oder Switches entlang des Übertragungswegs
- Die Zeit, die Antivirenanwendungen, Firewalls und andere Sicherheitsmechanismen benötigen, um Pakete zu überprüfen
- Funktionsstörungen in Client- oder Serveranwendungen

In diesem Artikel geht es um mögliche Ursachen von Wartezeiten bei der Verwendung von Azure Cognitive Services sowie darum, wie Sie diese beheben können.

> [!NOTE]
> Azure Cognitive Services bietet keine Vereinbarung zum Servicelevel (Service Level Agreement, SLA) in Bezug auf Wartezeiten.

## <a name="possible-causes-of-latency"></a>Mögliche Ursachen für Wartezeiten

### <a name="slow-connection-between-the-cognitive-service-and-a-remote-url"></a>Langsame Verbindung zwischen Cognitive Services und einer Remote-URL

Einige Azure Cognitive Services-Instanzen bieten Methoden, die Daten von einer von Ihnen bereitgestellten Remote-URL abrufen. Wenn Sie z. B. die [Methode „DetectWithUrlAsync“](/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync#Microsoft_Azure_CognitiveServices_Vision_Face_FaceOperationsExtensions_DetectWithUrlAsync_Microsoft_Azure_CognitiveServices_Vision_Face_IFaceOperations_System_String_System_Nullable_System_Boolean__System_Nullable_System_Boolean__System_Collections_Generic_IList_System_Nullable_Microsoft_Azure_CognitiveServices_Vision_Face_Models_FaceAttributeType___System_String_System_Nullable_System_Boolean__System_String_System_Threading_CancellationToken_) des Diensts „Gesichtserkennung“ aufrufen, können Sie die URL eines Bilds angeben, in dem der Dienst Gesichter erkennen soll.

```csharp
var faces = await client.Face.DetectWithUrlAsync("https://www.biography.com/.image/t_share/MTQ1MzAyNzYzOTgxNTE0NTEz/john-f-kennedy---mini-biography.jpg");
```

Der Dienst „Gesichtserkennung“ muss das Bild dann vom Remoteserver herunterladen. Wenn die Verbindung zwischen dem Dienst „Gesichtserkennung“ und dem Remoteserver langsam ist, wirkt sich dies auf die Antwortzeit der Erkennungsmethode aus.

[Das Bild sollte ggf. in Azure Blob Storage Premium gespeichert werden](../../../storage/blobs/storage-upload-process-images.md?tabs=dotnet), um diese Zeit zu verkürzen. Beispiel:

``` csharp
var faces = await client.Face.DetectWithUrlAsync("https://csdx.blob.core.windows.net/resources/Face/Images/Family1-Daughter1.jpg");
```

### <a name="large-upload-size"></a>Große Uploadgröße

Einige Azure Cognitive Services-Instanzen bieten Methoden, die Daten aus einer von Ihnen hochgeladenen Datei abrufen. Wenn Sie z. B. die [Methode „DetectWithStreamAsync“](/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync#Microsoft_Azure_CognitiveServices_Vision_Face_FaceOperationsExtensions_DetectWithStreamAsync_Microsoft_Azure_CognitiveServices_Vision_Face_IFaceOperations_System_IO_Stream_System_Nullable_System_Boolean__System_Nullable_System_Boolean__System_Collections_Generic_IList_System_Nullable_Microsoft_Azure_CognitiveServices_Vision_Face_Models_FaceAttributeType___System_String_System_Nullable_System_Boolean__System_String_System_Threading_CancellationToken_) des Diensts „Gesichtserkennung“ aufrufen, können Sie ein Bild hochladen, in dem der Dienst Gesichter erkennen soll.

```csharp
using FileStream fs = File.OpenRead(@"C:\images\face.jpg");
System.Collections.Generic.IList<DetectedFace> faces = await client.Face.DetectWithStreamAsync(fs, detectionModel: DetectionModel.Detection02);
```

Wenn die hochzuladende Datei groß ist, wirkt sich dies aus den folgenden Gründen auf die Antwortzeit der Methode `DetectWithStreamAsync` aus:
- Das Hochladen der Datei dauert länger.
- Der Dienst benötigt proportional zur Dateigröße mehr Zeit für die Verarbeitung der Datei.

Gegenmaßnahmen:
- [Das Bild sollte ggf. in Azure Blob Storage Premium gespeichert werden.](../../../storage/blobs/storage-upload-process-images.md?tabs=dotnet) Beispiel:
``` csharp
var faces = await client.Face.DetectWithUrlAsync("https://csdx.blob.core.windows.net/resources/Face/Images/Family1-Daughter1.jpg");
```
- Laden Sie ggf. eine kleinere Datei hoch.
    - Lesen Sie die Richtlinien hinsichtlich der Eingabedaten für die Gesichtserkennung unter [Gesichtserkennung und -attribute](../concepts/face-detection.md#input-data) und [Konzepte der Gesichtserkennung](../concepts/face-recognition.md#input-data).
    - Wird bei der Gesichtserkennung das Erkennungsmodell `DetectionModel.Detection01` verwendet, erhöht sich die Verarbeitungsgeschwindigkeit bei einer Verkleinerung der Bildgröße. Wenn Sie das Erkennungsmodell `DetectionModel.Detection02` verwenden, führt eine Verringerung der Bildgröße nur zu einer Erhöhung der Verarbeitungsgeschwindigkeit, wenn die Bilddatei kleiner als 1920 × 1080 ist.
    - Eine Verringerung der Gesichtsgröße auf 200 × 200 Pixel hat bei der Gesichtserkennung keine Auswirkungen auf die Genauigkeit des Erkennungsmodells.
    - Die Leistung der Methoden `DetectWithUrlAsync` und `DetectWithStreamAsync` hängt auch davon ab, wie viele Gesichter in einem Bild vorhanden sind. Der Dienst „Gesichtserkennung“ kann bis zu 100 Gesichter in einem Bild erkennen. Gesichter werden dabei nach der Rechteckgröße der Gesichter von groß nach klein bewertet.
    - Wenn Sie mehrere Dienstmethoden aufrufen müssen, empfiehlt es sich, diese parallel aufzurufen, sofern Ihr Anwendungsentwurf dies zulässt. Dies bietet sich z. B. an, wenn für einen Gesichtsvergleich Gesichter in zwei Bildern erkannt werden müssen:
```csharp
var faces_1 = client.Face.DetectWithUrlAsync("https://www.biography.com/.image/t_share/MTQ1MzAyNzYzOTgxNTE0NTEz/john-f-kennedy---mini-biography.jpg");
var faces_2 = client.Face.DetectWithUrlAsync("https://www.biography.com/.image/t_share/MTQ1NDY3OTIxMzExNzM3NjE3/john-f-kennedy---debating-richard-nixon.jpg");
Task.WaitAll (new Task<IList<DetectedFace>>[] { faces_1, faces_2 });
IEnumerable<DetectedFace> results = faces_1.Result.Concat (faces_2.Result);
```

### <a name="slow-connection-between-your-compute-resource-and-the-face-service"></a>Langsame Verbindung zwischen Ihrer Computeressource und dem Dienst „Gesichtserkennung“

Wenn der Computer über eine langsame Verbindung mit dem Dienst „Gesichtserkennung“ verfügt, wirkt sich dies auf die Antwortzeit der Dienstmethoden aus.

Gegenmaßnahmen:
- Wenn Sie Ihr Abonnement für den Dienst „Gesichtserkennung“ erstellen, sollten Sie darauf achten, die Region auszuwählen, in der Ihre Anwendung gehostet wird.
- Wenn Sie mehrere Dienstmethoden aufrufen müssen, empfiehlt es sich, diese parallel aufzurufen, sofern Ihr Anwendungsentwurf dies zulässt. Ein Beispiel finden Sie im vorherigen Abschnitt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie die Wartezeit bei Verwendung des Diensts „Gesichtserkennung“ verringern. Im nächsten Schritt erfahren Sie, wie Sie vorhandene PersonGroup- und FaceList-Objekte auf die Objekte „LargePersonGroup“ bzw. „LargeFaceList“ hochskalieren.

> [!div class="nextstepaction"]
> [Beispiel: Verwenden des Features für die Verarbeitung in großem Umfang](how-to-use-large-scale.md)

## <a name="related-topics"></a>Verwandte Themen

- [Referenzdokumentation (REST)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Referenzdokumentation (.NET SDK)](/dotnet/api/overview/azure/cognitiveservices/client/faceapi)