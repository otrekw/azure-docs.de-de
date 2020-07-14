---
title: 'Beispiel: Identifizieren von Gesichtern in Bildern – Gesichtserkennung'
titleSuffix: Azure Cognitive Services
description: In diesem Leitfaden wird veranschaulicht, wie Sie mithilfe von PersonGroup-Objekten, die im Voraus anhand bekannter Personen erstellt werden, unbekannte Gesichter identifizieren.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 07/02/2020
ms.author: sbowles
ms.openlocfilehash: 607f67258c5d069590f934891c09ccada780c977
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85918741"
---
# <a name="example-identify-faces-in-images"></a>Beispiel: Identifizieren von Gesichtern in Bildern

In diesem Leitfaden wird veranschaulicht, wie Sie mithilfe von PersonGroup-Objekten, die im Voraus anhand bekannter Personen erstellt werden, unbekannte Gesichter identifizieren. Die Beispiele wurden in C# unter Verwendung der Clientbibliothek für die Gesichtserkennung von Azure Cognitive Services geschrieben.

In diesem Beispiel wird Folgendes veranschaulicht:

- Erstellen einer PersonGroup: Diese PersonGroup enthält eine Liste mit bekannten Personen.
- Zuweisen von Gesichtern zu einzelnen Personen: Diese Gesichter dienen als Grundlage zur Identifizierung von Personen. Es wird empfohlen, deutlich erkennbare frontale Ansichten von Gesichtern zu verwenden. Ein Beispiel ist ein Ausweisfoto. Am besten sollten für eine Person mehrere Bilder mit unterschiedlichen Posen, Kleidungsfarben oder Frisuren vorliegen.

## <a name="prerequisites"></a>Voraussetzungen
* Aktuelle Version von [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).
* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services/)
* Sobald Sie über Ihr Azure-Abonnement verfügen, sollten Sie über <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title="Erstellen einer Gesichtserkennungsressource"  target="_blank"> im Azure-Portal eine Gesichtserkennungsressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> erstellen, um Ihren Schlüssel und Endpunkt abzurufen. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**, und kopieren Sie Ihren Schlüssel.
* Einige Fotos mit den Gesichtern von identifizierten Personen. [Laden Sie Beispielfotos für Anna, Bill und Clare herunter.](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data)
* Eine Reihe von Testfotos. Die Fotos können die Gesichter der identifizierten Personen enthalten, dies muss aber nicht der Fall sein. Verwenden Sie einige Bilder aus dem Link zu Beispielfotos.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-c-application"></a>Erstellen einer neuen C#-Anwendung

Erstellen Sie eine neue .NET Core-Anwendung in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. 

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl `dotnet new` zum Erstellen einer neuen Konsolen-App mit dem Namen `face-identify`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: *Program.cs*. 

```dotnetcli
dotnet new console -n face-quickstart
```

Wechseln Sie zum Ordner der neu erstellten App. Sie können die Anwendung mit folgendem Befehl erstellen:

```dotnetcli
dotnet build
```

Die Buildausgabe sollte keine Warnungen oder Fehler enthalten. 

```output
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

## <a name="step-1-authorize-the-api-call"></a>Schritt 1: Autorisieren des API-Aufrufs

Für jeden Aufruf der Gesichtserkennungs-API ist ein Abonnementschlüssel erforderlich. Dieser Schlüssel kann entweder über einen Parameter mit der Abfragezeichenfolge übergeben oder im Anforderungsheader angegeben werden. Wenn Sie eine Clientbibliothek verwenden, wird der Abonnementschlüssel durch den Konstruktor der **FaceClient**-Klasse übergeben. Fügen Sie der **Main**-Methode der Datei *Program.cs* den folgenden Code hinzu.
 
```csharp 
private readonly IFaceClient faceClient = new FaceClient(
    new ApiKeyServiceClientCredentials("<subscription key>"),
    new System.Net.Http.DelegatingHandler[] { });
```

## <a name="step-2-create-the-persongroup"></a>Schritt 2: Erstellen von „PersonGroup“

In diesem Schritt enthält eine **PersonGroup** mit dem Namen „MyFriends“ die Personen Anna, Bill und Clare. Für jede Person werden mehrere Gesichtsabbildungen registriert. Die Gesichter müssen auf den Bildern erkannt werden. Nachdem Sie alle Schritte ausgeführt haben, sieht Ihre **PersonGroup** etwa folgendermaßen aus:

![MyFriends](../Images/group.image.1.jpg)

### <a name="step-21-define-people-for-the-persongroup"></a>Schritt 2.1: Definieren von Personen für die PersonGroup
Eine Person ist die Basiseinheit für die Identifizierung. Für eine Person können eine oder mehrere bekannte Gesichtsabbildungen registriert werden. Eine **PersonGroup** ist eine Sammlung von Personen. Jede Person wird innerhalb einer bestimmten **PersonGroup** definiert. Die Identifizierung erfolgt über die jeweilige **PersonGroup**. Daher müssen zunächst eine **PersonGroup** und anschließend die zugehörigen Personen (z. B. Anna, Bill und Clare) erstellt werden.

Erstellen Sie zunächst eine neue **PersonGroup** mithilfe der API [PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244). Die entsprechende API für die Clientbibliothek ist die **CreatePersonGroupAsync**-Methode für die **FaceClient**-Klasse. Die Gruppen-ID, die beim Erstellen der Gruppe angegeben wird, ist für jedes Abonnement eindeutig. Sie können **PersonGroups** mit anderen **PersonGroup**-APIs auch abrufen, aktualisieren oder löschen. 

Nachdem die Gruppe definiert ist, können Sie mit der API [PersonGroup Person – Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) Personen innerhalb der Gruppe definieren. Die Methode der Clientbibliothek lautet **CreatePersonAsync**. Nachdem Sie eine Person erstellt haben, können Sie ihr Gesichter hinzufügen.

```csharp 
// Create an empty PersonGroup
string personGroupId = "myfriends";
await faceClient.PersonGroup.CreateAsync(personGroupId, "My Friends");
 
// Define Anna
CreatePersonResult friend1 = await faceClient.PersonGroupPerson.CreateAsync(
    // Id of the PersonGroup that the person belonged to
    personGroupId,    
    // Name of the person
    "Anna"            
);
 
// Define Bill and Clare in the same way
```
### <a name="step-22-detect-faces-and-register-them-to-the-correct-person"></a><a name="step2-2"></a> Schritt 2.2: Erkennen von Gesichtern und Zuordnen zur richtigen Person
Senden Sie zur Gesichtserkennung eine POST-Webanforderung an die API [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) mit der Bilddatei im Text der HTTP-Anforderung. Bei Verwendung der Clientbibliothek erfolgt die Gesichtserkennung über eine der DetectAsync-Methoden für die FaceClient-Klasse.

Rufen Sie für jedes erkannte Gesicht [PersonGroup Person – Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) auf, um es der richtigen Person hinzuzufügen.

Der folgende Code veranschaulicht, wie ein Gesicht auf einem Bild erkannt und einer Person hinzugefügt wird:

```csharp 
// Directory contains image files of Anna
const string friend1ImageDir = @"D:\Pictures\MyFriends\Anna\";
 
foreach (string imagePath in Directory.GetFiles(friend1ImageDir, "*.jpg"))
{
    using (Stream s = File.OpenRead(imagePath))
    {
        // Detect faces in the image and add to Anna
        await faceClient.PersonGroupPerson.AddFaceFromStreamAsync(
            personGroupId, friend1.PersonId, s);
    }
}
// Do the same for Bill and Clare
``` 
Bei mehreren Gesichtern auf einem Bild wird nur das größte hinzugefügt. Sie können einer Person noch andere Gesichter hinzufügen. Übergeben Sie eine Zeichenfolge im Format „targetFace = links, oben, Breite, Höhe“ im Abfrageparameter targetFace der API [PersonGroup Person – Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b). Sie können auch den optionalen Parameter targetFace für die AddPersonFaceAsync-Methode verwenden, um andere Gesichter hinzuzufügen. Jedes Gesicht, das einer Person hinzugefügt wird, erhält eine eindeutige, persistente Gesichtserkennungs-ID. Sie können diese ID in [PersonGroup Person – Delete Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e) und [Face – Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) verwenden.

## <a name="step-3-train-the-persongroup"></a>Schritt 3: Trainieren der „PersonGroup“

Eine **PersonGroup** muss vor ihrer Verwendung für die Identifikation trainiert werden. Die **PersonGroup** muss außerdem neu trainiert werden, wenn Sie darin eine Person hinzufügen oder entfernen oder wenn Sie das registrierte Gesicht einer Person bearbeiten. Das Training erfolgt über die API [PersonGroup – Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249). Bei Verwendung der Clientbibliothek müssen Sie hierzu die **TrainPersonGroupAsync**-Methode aufrufen:
 
```csharp 
await faceClient.PersonGroup.TrainAsync(personGroupId);
```
 
Das Training ist ein asynchroner Prozess. Es ist durchaus möglich, dass es selbst nach Rückgabe der **TrainPersonGroupAsync**-Methode noch nicht abgeschlossen ist. Sie müssen eventuell den Trainingsstatus abfragen. Verwenden Sie die API [PersonGroup - Get Training Status](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) oder die **GetPersonGroupTrainingStatusAsync**-Methode der Clientbibliothek. Das folgende Codebeispiel veranschaulicht eine einfache Logik zum Warten, bis das Training der **PersonGroup** abgeschlossen ist:
 
```csharp 
TrainingStatus trainingStatus = null;
while(true)
{
    trainingStatus = await faceClient.PersonGroup.GetTrainingStatusAsync(personGroupId);
 
    if (trainingStatus.Status != TrainingStatusType.Running)
    {
        break;
    }
 
    await Task.Delay(1000);
} 
``` 

## <a name="step-4-identify-a-face-against-a-defined-persongroup"></a>Schritt 4: Identifizieren eines Gesichts anhand einer definierten „PersonGroup“

Wenn der Gesichtserkennungsdienst Identifizierungen ausführt, berechnet er die Ähnlichkeit mit einem Testgesicht für alle Gesichter innerhalb einer Gruppe. Sie gibt die ähnlichste Personen für das Testgesicht zurück. Dieser Prozess erfolgt über die API [Face – Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) oder die IdentifyAsync-Methode der Clientbibliothek.

Das Testgesicht muss mithilfe der vorherigen Schritte erkannt werden. Anschließend wird die Gesichtserkennungs-ID als zweites Argument an die Identifizierungs-API übergeben. Es können mehrere Gesichtserkennungs-IDs gleichzeitig identifiziert werden. Das Ergebnis enthält alle ermittelten Übereinstimmungen. Als Standardeinstellung wird beim Identifikationsprozess nur die Person zurückgegeben, die dem Testgesicht am meisten ähnelt. Sie können aber auch den optionalen Parameter _maxNumOfCandidatesReturned_ angeben, damit beim Identifikationsprozess mehr als ein Kandidat zurückgegeben wird.

Der folgende Code veranschaulicht den Identifikationsprozess:

```csharp 
string testImageFile = @"D:\Pictures\test_img1.jpg";

using (Stream s = File.OpenRead(testImageFile))
{
    var faces = await faceClient.Face.DetectWithStreamAsync(s);
    var faceIds = faces.Select(face => face.FaceId.Value).ToArray();
 
    var results = await faceClient.Face.IdentifyAsync(faceIds, personGroupId);
    foreach (var identifyResult in results)
    {
        Console.WriteLine("Result of face: {0}", identifyResult.FaceId);
        if (identifyResult.Candidates.Length == 0)
        {
            Console.WriteLine("No one identified");
        }
        else
        {
            // Get top 1 among all candidates returned
            var candidateId = identifyResult.Candidates[0].PersonId;
            var person = await faceClient.PersonGroupPerson.GetAsync(personGroupId, candidateId);
            Console.WriteLine("Identified as {0}", person.Name);
        }
    }
}
``` 

Nachdem Sie die Schritte abgeschlossen haben, versuchen Sie, andere Gesichter zu erkennen. Finden Sie heraus, ob die Gesichter von Anna, Bill oder Clare anhand der für die Gesichtserkennung hochgeladenen Bilder korrekt identifiziert werden können. Hierzu folgende Beispiele:

![Identifizieren verschiedener Gesichter](../Images/identificationResult.1.jpg )

## <a name="step-5-request-for-large-scale"></a>Schritt 5: Umfangreiche Anforderungen

Eine **PersonGroup** kann aufgrund der Einschränkung des vorherigen Entwurfs bis zu 10.000 Personen enthalten. Weitere Informationen zu Szenarios mit bis zu einer Million finden Sie unter [How to use the large-scale feature (Verwenden der Funktion für die Verarbeitung in großem Umfang)](how-to-use-large-scale.md).

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden haben Sie erfahren, wie eine **PersonGroup** erstellt und eine Person identifiziert wird. Die folgenden Funktionen wurden erläutert und veranschaulicht:

- Erkennen von Gesichtern mithilfe der API [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d)
- Erstellen von PersonGroups mithilfe der API [PersonGroup – Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)
- Erstellen von Personen mithilfe der API [PersonGroup Person – Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)
- Trainieren einer PersonGroup mithilfe der API [PersonGroup – Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)
- Identifizieren von unbekannten Gesichtern über eine PersonGroup mithilfe der API [Face – Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)

## <a name="related-topics"></a>Verwandte Themen

- [Konzepte der Gesichtserkennung](../concepts/face-recognition.md)
- [Gesichtserkennung in einem Bild](HowtoDetectFacesinImage.md)
- [Hinzufügen von Gesichtern](how-to-add-faces.md)
- [Verwenden der Funktion für die Verarbeitung in großem Umfang](how-to-use-large-scale.md)
