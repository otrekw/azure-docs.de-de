---
title: 'Schnellstart: Erkennen von Gesichtern in einem Bild mit der Azure-REST-API und Node.js'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart verwenden Sie die Azure-Gesichtserkennungs-REST-API mit Node.js, um Gesichter in einem Bild zu erkennen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 11/23/2020
ms.author: pafarley
ms.custom: devx-track-js
ms.openlocfilehash: a245dae4405706c37b3c142963d325925d089830
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/24/2020
ms.locfileid: "96001438"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-nodejs"></a>Schnellstart: Erkennen von Gesichtern in einem Bild mit der Gesichtserkennungs-REST-API und Node.js

In dieser Schnellstartanleitung verwenden Sie die Azure-Gesichtserkennungs-REST-API mit Node.js, um menschliche Gesichter in einem Bild zu erkennen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/cognitive-services/) erstellen, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services/)
* Sobald Sie über Ihr Azure-Abonnement verfügen, erstellen Sie über <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title="Erstellen einer Gesichtserkennungsressource"  target="_blank"> eine Gesichtserkennungsressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Gesichtserkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.
- Ein Code-Editor wie [Visual Studio Code](https://code.visualstudio.com/download)

## <a name="set-up-the-node-environment"></a>Einrichten der Node.js-Umgebung

Navigieren Sie zu dem Ordner, in dem Sie Ihr Projekt erstellen möchten, und erstellen Sie eine neue Datei namens *facedetection.js*. Installieren Sie dann das Modul `axios` für dieses Projekt. Dadurch können Ihre Skripts HTTP-Anforderungen ausführen.

```shell
npm install axios --save
```

## <a name="write-the-nodejs-script"></a>Schreiben des Node.js-Skripts

Fügen Sie den folgenden Code in *facedetection.js* ein. Mit diesen Feldern wird angegeben, wie eine Verbindung mit dem Gesichtserkennungsdienst hergestellt wird und wo die Eingabedaten abgerufen werden. [Erstellen Sie Umgebungsvariablen](../../cognitive-services-apis-create-account.md?tabs=multiservice%252cwindows#configure-an-environment-variable-for-authentication), und fügen Sie Schlüssel und Endpunkt Ihres Gesichtserkennungsabonnements hinzu. Ändern Sie bei Bedarf das Feld `imageUrl`, um auf Ihr eigenes Eingabebild zu verweisen.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/Face/rest/detect.js" id="environment":::

Fügen Sie anschließend den folgenden Code hinzu, um die Gesichtserkennungs-API aufzurufen und Gesichtsattributdaten aus dem Eingabebild abzurufen. Das Feld `returnFaceAttributes` gibt an, welche Gesichtsattribute abgerufen werden sollen. Sie können diese Zeichenfolge je nach beabsichtigter Verwendung ändern.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/Face/rest/detect.js" id="main":::

## <a name="save-and-run-the-script"></a>Speichern und Ausführen des Skripts

Nachdem Sie die gewünschten Änderungen vorgenommen haben, öffnen Sie eine Eingabeaufforderung, und führen Sie die Datei mit dem Befehl `node` aus.

```
node facedetection.js
```

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
 
Verwenden Sie zum Extrahieren von Gesichtsattributen das Erkennungsmodell 1, und fügen Sie den Abfrageparameter `returnFaceAttributes` hinzu. Bearbeiten Sie die Parameter wie folgt:

```javascript
    params : {
        detectionModel: 'detection_01',
        returnFaceAttributes: 'age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise',
        returnFaceId: true
    },
```

Die Antwort enthält jetzt Gesichtsattribute. Beispiel:

```json
[
  {
    "faceId": "ae8952c1-7b5e-4a5a-a330-a6aa351262c9",
    "faceRectangle": {
      "top": 621,
      "left": 616,
      "width": 195,
      "height": 195
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 6.8,
        "yaw": 3.7
      },
      "gender": "male",
      "age": 37,
      "facialHair": {
        "moustache": 0.4,
        "beard": 0.4,
        "sideburns": 0.1
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.999,
        "sadness": 0.001,
        "surprise": 0
      },
      "blur": {
        "blurLevel": "high",
        "value": 0.89
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.51
      },
      "noise": {
        "noiseLevel": "medium",
        "value": 0.59
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": false
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0.04,
        "invisible": false,
        "hairColor": [
          {
            "color": "black",
            "confidence": 0.98
          },
          {
            "color": "brown",
            "confidence": 0.87
          },
          {
            "color": "gray",
            "confidence": 0.85
          },
          {
            "color": "other",
            "confidence": 0.25
          },
          {
            "color": "blond",
            "confidence": 0.07
          },
          {
            "color": "red",
            "confidence": 0.02
          }
        ]
      }
    }
  },
  {
    "faceId": "b1bb3cbe-5a73-4f8d-96c8-836a5aca9415",
    "faceRectangle": {
      "top": 693,
      "left": 1503,
      "width": 180,
      "height": 180
    },
    "faceAttributes": {
      "smile": 0.003,
      "headPose": {
        "pitch": 0,
        "roll": 2,
        "yaw": -2.2
      },
      "gender": "female",
      "age": 56,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0.001,
        "disgust": 0,
        "fear": 0,
        "happiness": 0.003,
        "neutral": 0.984,
        "sadness": 0.011,
        "surprise": 0
      },
      "blur": {
        "blurLevel": "high",
        "value": 0.83
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.41
      },
      "noise": {
        "noiseLevel": "high",
        "value": 0.76
      },
      "makeup": {
        "eyeMakeup": false,
        "lipMakeup": false
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0.06,
        "invisible": false,
        "hairColor": [
          {
            "color": "black",
            "confidence": 0.99
          },
          {
            "color": "gray",
            "confidence": 0.89
          },
          {
            "color": "other",
            "confidence": 0.64
          },
          {
            "color": "brown",
            "confidence": 0.34
          },
          {
            "color": "blond",
            "confidence": 0.07
          },
          {
            "color": "red",
            "confidence": 0.03
          }
        ]
      }
    }
  }
]
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein Node.js-Skript geschrieben, das den Azure-Gesichtserkennungsdienst aufruft, um Gesichter in einem Bild zu erkennen und deren Attribute zurückzugeben. Sehen Sie sich als Nächstes die Referenzdokumentation zur Gesichtserkennungs-API an, um mehr zu erfahren.

> [!div class="nextstepaction"]
> [Gesichtserkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) 