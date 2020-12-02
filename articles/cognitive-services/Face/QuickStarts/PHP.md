---
title: 'Schnellstart: Erkennen von Gesichtern in einem Bild mit der REST-API und PHP'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung verwenden Sie die Gesichtserkennungs-REST-API mit PHP, um Gesichter in einem Bild zu erkennen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 11/23/2020
ms.author: pafarley
ms.openlocfilehash: 649c456309d2524fec8d44f13c0f3081725c4361
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/24/2020
ms.locfileid: "95994082"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-php"></a>Schnellstart: Erkennen von Gesichtern in einem Bild mit der REST-API und PHP

In dieser Schnellstartanleitung verwenden Sie die Azure-Gesichtserkennungs-REST-API mit PHP, um menschliche Gesichter in einem Bild zu erkennen.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services/)
* Sobald Sie über Ihr Azure-Abonnement verfügen, erstellen Sie über <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title="Erstellen einer Gesichtserkennungsressource"  target="_blank"> eine Gesichtserkennungsressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Gesichtserkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.
* Ein Code-Editor wie [Visual Studio Code](https://code.visualstudio.com/download)
* Das PHP-Paket [HTTP_Request2](https://pear.php.net/package/HTTP_Request2)
* Ein PHP-fähiger Webbrowser. Falls Sie noch nicht über einen Browser dieser Art verfügen, können Sie [XAMPP](https://www.apachefriends.org/) auf Ihrem Computer installieren und einrichten.

## <a name="initialize-the-html-file"></a>Initialisieren der HTML-Datei

Erstellen Sie eine neue HTML-Datei namens *detectFaces.html*, und fügen Sie ihr den folgenden Code hinzu:

```html
<html>
    <head>
        <title>Face Detect Sample</title>
    </head>
    <body></body>
</html>
```

## <a name="write-the-php-script"></a>Schreiben des PHP-Skripts

Fügen Sie innerhalb des Elements `body` des Dokuments den folgenden Code hinzu. Mit diesem Code wird eine einfache Benutzeroberfläche mit einem URL-Feld, einer Schaltfläche für die Gesichtsanalyse (**Analyze face**), einem Antwortbereich und einem Bereich für die Bildanzeige eingerichtet.

:::code language="php" source="~/cognitive-services-quickstart-code/php/face/rest/detect.php":::

Sie müssen das Feld `subscriptionKey` mit dem Wert Ihres Abonnementschlüssels aktualisieren und die Zeichenfolge `uriBase` ändern, sodass sie die korrekte Endpunktzeichenfolge enthält. Das Feld `returnFaceAttributes` gibt an, welche Gesichtsattribute abgerufen werden sollen. Sie können diese Zeichenfolge je nach beabsichtigter Verwendung ändern.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="run-the-script"></a>Führen Sie das Skript aus.

Öffnen Sie die Datei in einem PHP-fähigen Webbrowser. Daraufhin sollte eine JSON-Zeichenfolge mit Gesichtsdaten zurückgegeben werden, die in etwa wie folgt aussieht:

```json
[
    {
        "faceId": "e93e0db1-036e-4819-b5b6-4f39e0f73509",
        "faceRectangle": {
            "top": 621,
            "left": 616,
            "width": 195,
            "height": 195
        }
    }
]
```

## <a name="extract-face-attributes"></a>Extrahieren von Gesichtsattributen
 
Verwenden Sie zum Extrahieren von Gesichtsattributen das Erkennungsmodell 1, und fügen Sie den Abfrageparameter `returnFaceAttributes` hinzu.

```php
$parameters = array(
    // Request parameters
    'detectionModel' => 'detection_01',
    'returnFaceAttributes' => 'age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise',
   'returnFaceId' => 'true');
```

Die Antwort enthält jetzt Gesichtsattribute. Beispiel:

```json
[
    {
        "faceId": "e93e0db1-036e-4819-b5b6-4f39e0f73509",
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
        "faceId": "37c7c4bc-fda3-4d8d-94e8-b85b8deaf878",
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

Machen Sie sich mit der Gesichtserkennungs-API vertraut, die verwendet wird, um in einem Bild menschliche Gesichter zu erkennen, mit Rechtecken zu kennzeichnen und Attribute wie Alter und Geschlecht zurückzugeben.

> [!div class="nextstepaction"]
> [Gesichtserkennungs-APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
 