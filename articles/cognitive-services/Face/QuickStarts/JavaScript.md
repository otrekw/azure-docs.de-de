---
title: 'Schnellstart: Erkennen von Gesichtern in einem Bild mit der REST-API und JavaScript'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart verwenden Sie die Gesichtserkennungs-API mit JavaScript in Cognitive Services, um Gesichter in einem Bild zu erkennen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.custom: devx-track-js
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 08/05/2020
ms.author: pafarley
ms.openlocfilehash: 6992b6abb8ab54d5f08903f1b1393111bbd78c09
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91322987"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-javascript"></a>Schnellstart: Erkennen von Gesichtern in einem Bild mit der REST-API und JavaScript

In dieser Schnellstartanleitung verwenden Sie die Azure-Gesichtserkennungs-REST-API mit JavaScript, um menschliche Gesichter in einem Bild zu erkennen.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services/)
* Sobald Sie über Ihr Azure-Abonnement verfügen, erstellen Sie über <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title="Erstellen einer Gesichtserkennungsressource"  target="_blank"> eine Gesichtserkennungsressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Gesichtserkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.
* Ein Code-Editor wie [Visual Studio Code](https://code.visualstudio.com/download)

## <a name="initialize-the-html-file"></a>Initialisieren der HTML-Datei

Erstellen Sie eine neue HTML-Datei namens *detectFaces.html*, und fügen Sie ihr den folgenden Code hinzu:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Detect Faces Sample</title>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
    </head>
    <body></body>
</html>
```

Fügen Sie anschließend innerhalb des Elements `body` des Dokuments den folgenden Code hinzu. Mit diesem Code wird eine einfache Benutzeroberfläche mit einem URL-Feld, einer Schaltfläche für die Gesichtsanalyse (**Analyze face**), einem Antwortbereich und einem Bereich für die Bildanzeige eingerichtet.

```html
<h1>Detect Faces:</h1>
Enter the URL to an image that includes a face or faces, then click
the <strong>Analyze face</strong> button.<br><br>
Image to analyze: <input type="text" name="inputImage" id="inputImage"
    value="https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg" />
<button onclick="processImage()">Analyze face</button><br><br>
<div id="wrapper" style="width:1020px; display:table;">
    <div id="jsonOutput" style="width:600px; display:table-cell;">
        Response:<br><br>
        <textarea id="responseTextArea" class="UIInput"
            style="width:580px; height:400px;"></textarea>
    </div>
    <div id="imageDiv" style="width:420px; display:table-cell;">
        Source image:<br><br>
        <img id="sourceImage" width="400" />
    </div>
</div>
```

## <a name="write-the-javascript-script"></a>Schreiben des JavaScript-Skripts

Fügen Sie in Ihrem Dokument den folgenden Code direkt über dem Element `h1` hinzu. Mit diesem Code wird der JavaScript-Code zum Aufrufen der Gesichtserkennungs-API eingerichtet.

```html
<script type="text/javascript">
    function processImage() {
        // Replace <Subscription Key> with your valid subscription key.
        var subscriptionKey = "<Subscription Key>";
    
        var uriBase =
            "https://<My Endpoint String>.com/face/v1.0/detect";
    
        // Request parameters.
        var params = {
            "returnFaceId": "true",
            "returnFaceLandmarks": "false",
            "returnFaceAttributes":
                "age,gender,headPose,smile,facialHair,glasses,emotion," +
                "hair,makeup,occlusion,accessories,blur,exposure,noise"
        };
    
        // Display the image.
        var sourceImageUrl = document.getElementById("inputImage").value;
        document.querySelector("#sourceImage").src = sourceImageUrl;
    
        // Perform the REST API call.
        $.ajax({
            url: uriBase + "?" + $.param(params),
    
            // Request headers.
            beforeSend: function(xhrObj){
                xhrObj.setRequestHeader("Content-Type","application/json");
                xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            },
    
            type: "POST",
    
            // Request body.
            data: '{"url": ' + '"' + sourceImageUrl + '"}',
        })
    
        .done(function(data) {
            // Show formatted JSON on webpage.
            $("#responseTextArea").val(JSON.stringify(data, null, 2));
        })
    
        .fail(function(jqXHR, textStatus, errorThrown) {
            // Display error message.
            var errorString = (errorThrown === "") ?
                "Error. " : errorThrown + " (" + jqXHR.status + "): ";
            errorString += (jqXHR.responseText === "") ?
                "" : (jQuery.parseJSON(jqXHR.responseText).message) ?
                    jQuery.parseJSON(jqXHR.responseText).message :
                        jQuery.parseJSON(jqXHR.responseText).error.message;
            alert(errorString);
        });
    };
</script>
```

Sie müssen das Feld `subscriptionKey` mit dem Wert Ihres Abonnementschlüssels aktualisieren und die Zeichenfolge `uriBase` ändern, sodass sie die korrekte Endpunktzeichenfolge enthält. Das Feld `returnFaceAttributes` gibt an, welche Gesichtsattribute abgerufen werden sollen. Sie können diese Zeichenfolge je nach beabsichtigter Verwendung ändern.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="run-the-script"></a>Führen Sie das Skript aus.

Öffnen Sie *detectFaces.html* in Ihrem Browser. Wenn Sie auf die Schaltfläche für die Gesichtsanalyse (**Analyze face**) klicken, zeigt die App das Bild an, das sich unter der angegebenen URL befindet, und gibt eine JSON-Zeichenfolge mit Gesichtserkennungsdaten aus.

![GettingStartCSharpScreenshot](../Images/face-detect-javascript.png)

Der folgende Text ist ein Beispiel für eine erfolgreiche JSON-Antwort.

```json
[
  {
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "top": 131,
      "left": 177,
      "width": 162,
      "height": 162
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 0.1,
        "yaw": -32.9
      },
      "gender": "female",
      "age": 22.9,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
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
        "value": 0
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": true
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0,
        "invisible": false,
        "hairColor": [
          {
            "color": "brown",
            "confidence": 1
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

In dieser Schnellstartanleitung haben Sie ein JavaScript-Skript geschrieben, das den Azure-Gesichtserkennungsdienst aufruft, um Gesichter in einem Bild zu erkennen und deren Attribute zurückzugeben. Sehen Sie sich als Nächstes die Referenzdokumentation zur Gesichtserkennungs-API an, um mehr zu erfahren.

> [!div class="nextstepaction"]
> [Gesichtserkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
