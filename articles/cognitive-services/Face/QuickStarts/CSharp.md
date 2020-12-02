---
title: 'Schnellstart: Erkennen von Gesichtern in einem Bild mit der Azure-REST-API und C#'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung verwenden Sie die Azure-Gesichtserkennungs-REST-API mit C#, um Gesichter in einem Bild zu erkennen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 11/23/2020
ms.author: pafarley
ms.openlocfilehash: f15fbc340aafa325f772a1fd50a2129fa5044c47
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/24/2020
ms.locfileid: "96020873"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-c"></a>Schnellstart: Erkennen von Gesichtern in einem Bild mit der Gesichtserkennungs-REST-API und C#

In dieser Schnellstartanleitung verwenden Sie die Azure-Gesichtserkennungs-REST-API mit C#, um menschliche Gesichter in einem Bild zu erkennen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/cognitive-services/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services/)
* Sobald Sie über Ihr Azure-Abonnement verfügen, erstellen Sie über <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title="Erstellen einer Gesichtserkennungsressource"  target="_blank"> eine Gesichtserkennungsressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Gesichtserkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.
- Eine beliebige Edition von [Visual Studio](https://www.visualstudio.com/downloads/).

## <a name="create-the-visual-studio-project"></a>Erstellen des Visual Studio-Projekts

1. Erstellen Sie in Visual Studio ein neues Projekt vom Typ **Konsolen-App (.NET Framework)** , und nennen Sie es **FaceDetection**.
1. Sollte Ihre Projektmappe noch andere Projekte enthalten, wählen Sie dieses Projekt als das alleinige Startprojekt aus.

## <a name="add-face-detection-code"></a>Hinzufügen von Code für die Gesichtserkennung

Öffnen Sie die Datei *Program.cs* des neuen Projekts. Hier fügen Sie den Code hinzu, der zum Laden von Bildern und Erkennen von Gesichtern erforderlich ist.

### <a name="include-namespaces"></a>Einschließen von Namespaces

Fügen Sie am Anfang der Datei *Program.cs* die folgenden `using`-Anweisungen hinzu:

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/rest/detect.cs" id="dependencies":::

### <a name="add-essential-fields"></a>Hinzufügen wichtiger Felder

Fügen Sie die **Program**-Klasse mit den folgenden Felder hinzu. Mit diesen Daten wird angegeben, wie eine Verbindung mit dem Gesichtserkennungsdienst hergestellt wird und wo die Eingabedaten abgerufen werden. Sie müssen das Feld `subscriptionKey` mit dem Wert Ihres Abonnementschlüssels aktualisieren. Unter Umständen müssen Sie die Zeichenfolge `uriBase` ändern, sodass sie die Zeichenfolge des Ressourcenendpunkts enthält.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/rest/detect.cs" id="environment":::

### <a name="receive-image-input"></a>Empfangen der Bildeingabe

Fügen Sie der **Main**-Methode der **Program**-Klasse den folgenden Code hinzu. Durch diesen Code wird der Benutzer über die Konsole aufgefordert, seinen lokalen Pfad zur Bilddatei einzugeben. Anschließend wird eine weitere Methode (**MakeAnalysisRequest**) aufgerufen, um das Bild an diesem Speicherort zu verarbeiten.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/rest/detect.cs" id="main":::

### <a name="call-the-face-detection-rest-api"></a>Aufrufen der Gesichtserkennungs-REST-API

Fügen Sie der **Program**-Klasse die folgende Methode hinzu. Diese Methode erstellt einen REST-Aufruf der Gesichtserkennungs-API, um Gesichtsinformationen im Remotebild zu erkennen. (Die Zeichenfolge `requestParameters` gibt an, welche Gesichtsattribute abgerufen werden sollen.) Anschließend werden die Ausgabedaten in eine JSON-Zeichenfolge geschrieben.

Sie definieren die Hilfsmethoden in den folgenden Schritten.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/rest/detect.cs" id="request":::

### <a name="process-the-input-image-data"></a>Verarbeiten der Eingabebilddaten

Fügen Sie der **Program**-Klasse die folgende Methode hinzu. Durch diese Methode wird das Bild unter dem angegebenen Dateipfad in ein Bytearray konvertiert.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/rest/detect.cs" id="getimage":::

### <a name="parse-the-json-response"></a>Analysieren der JSON-Antwort

Fügen Sie der **Program**-Klasse die folgende Methode hinzu. Durch diese Methode wird die JSON-Antwort formatiert, damit sie besser lesbar ist. Ihre App schreibt diese Zeichenfolgendaten in die Konsole. Anschließend können Sie die Klasse und den Namespace schließen.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/rest/detect.cs" id="print":::

## <a name="run-the-app"></a>Ausführen der App

Eine erfolgreiche Antwort zeigt Gesichtserkennungsdaten in leicht lesbarem JSON-Format an. Beispiel:

```json
[
   {
      "faceId": "f7eda569-4603-44b4-8add-cd73c6dec644",
      "faceRectangle": {
         "top": 131,
         "left": 177,
         "width": 162,
         "height": 162
      }
   }
]
```

## <a name="extract-face-attributes"></a>Extrahieren von Gesichtsattributen
 
Verwenden Sie zum Extrahieren von Gesichtsattributen das Erkennungsmodell 1, und fügen Sie den Abfrageparameter `returnFaceAttributes` hinzu.

```csharp
string requestParameters = "detectionModel=detection_01&returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise";
```

Die Antwort enthält jetzt Gesichtsattribute. Beispiel:

```json
[
   {
      "faceId": "f7eda569-4603-44b4-8add-cd73c6dec644",
      "faceRectangle": {
         "top": 131,
         "left": 177,
         "width": 162,
         "height": 162
      },
      "faceAttributes": {
         "smile": 0.0,
         "headPose": {
            "pitch": 0.0,
            "roll": 0.1,
            "yaw": -32.9
         },
         "gender": "female",
         "age": 22.9,
         "facialHair": {
            "moustache": 0.0,
            "beard": 0.0,
            "sideburns": 0.0
         },
         "glasses": "NoGlasses",
         "emotion": {
            "anger": 0.0,
            "contempt": 0.0,
            "disgust": 0.0,
            "fear": 0.0,
            "happiness": 0.0,
            "neutral": 0.986,
            "sadness": 0.009,
            "surprise": 0.005
         },
         "blur": {
            "blurLevel": "low",
            "value": 0.06
         },
         "exposure": {
            "exposureLevel": "goodExposure",
            "value": 0.67
         },
         "noise": {
            "noiseLevel": "low",
            "value": 0.0
         },
         "makeup": {
            "eyeMakeup": true,
            "lipMakeup": true
         },
         "accessories": [

         ],
         "occlusion": {
            "foreheadOccluded": false,
            "eyeOccluded": false,
            "mouthOccluded": false
         },
         "hair": {
            "bald": 0.0,
            "invisible": false,
            "hairColor": [
               {
                  "color": "brown",
                  "confidence": 1.0
               },
               {
                  "color": "black",
                  "confidence": 0.87
               },
               {
                  "color": "other",
                  "confidence": 0.51
               },
               {
                  "color": "blond",
                  "confidence": 0.08
               },
               {
                  "color": "red",
                  "confidence": 0.08
               },
               {
                  "color": "gray",
                  "confidence": 0.02
               }
            ]
         }
      }
   }
]
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine einfache .NET-Konsolenanwendung erstellt, die REST-Aufrufe mit dem Azure-Gesichtserkennungsdienst verwendet, um Gesichter in einem Bild zu erkennen und deren Attribute zurückzugeben. Sehen Sie sich als Nächstes die Referenzdokumentation zur Gesichtserkennungs-API an, um mehr über die unterstützten Szenarien zu erfahren.

> [!div class="nextstepaction"]
> [Gesichtserkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
