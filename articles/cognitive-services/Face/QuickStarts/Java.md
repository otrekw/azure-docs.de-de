---
title: 'Schnellstart: Erkennen von Gesichtern in einem Bild mit der Azure-REST-API und Java'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart verwenden Sie die Azure-Gesichtserkennungs-REST-API mit Java, um Gesichter in einem Bild zu erkennen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 11/23/2020
ms.custom: devx-track-java
ms.author: pafarley
ms.openlocfilehash: b8ea2908711497db5e86a7dd665548e33b9d9f25
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/24/2020
ms.locfileid: "96020856"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-java"></a>Schnellstart: Erkennen von Gesichtern in einem Bild mit der REST-API und Java

In dieser Schnellstartanleitung verwenden Sie die Azure-Gesichtserkennungs-REST-API mit Java, um menschliche Gesichter in einem Bild zu erkennen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/cognitive-services/) erstellen, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services/)
* Sobald Sie über Ihr Azure-Abonnement verfügen, erstellen Sie über <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title="Erstellen einer Gesichtserkennungsressource"  target="_blank"> eine Gesichtserkennungsressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Gesichtserkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.
* Eine Java-IDE Ihrer Wahl.

## <a name="create-the-java-project"></a>Erstellen des Java-Projekts

1. Erstellen Sie eine neue Java-Befehlszeilen-App in Ihrer IDE, und fügen eine **Main**-Klasse mit einer **main**-Methode hinzu.
1. Importieren Sie die folgenden Bibliotheken in Ihr Java-Projekt. Wenn Sie Maven verwenden, werden die Maven-Koordinaten für jede Bibliothek bereitgestellt.
   - [Apache-HTTP-Client](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.6)
   - [Apache-HTTP-Core](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.10)
   - [JSON-Bibliothek](https://github.com/stleary/JSON-java) (org.json:json:20180130)
   - [Apache Commons-Protokollierung](https://commons.apache.org/proper/commons-logging/download_logging.cgi) (commons-logging:commons-logging:1.1.2)

## <a name="add-face-detection-code"></a>Hinzufügen von Code für die Gesichtserkennung

Öffnen Sie die Main-Klasse Ihres Projekts. Hier fügen Sie den Code hinzu, der zum Laden von Bildern und Erkennen von Gesichtern erforderlich ist.

### <a name="import-packages"></a>Importieren von Paketen

Fügen Sie am Anfang der Datei die folgenden `import`-Anweisungen hinzu.

:::code language="java" source="~/cognitive-services-quickstart-code/java/Face/rest/detect.java" id="dependencies":::

### <a name="add-essential-fields"></a>Hinzufügen wichtiger Felder

Ersetzen Sie die **Main**-Klasse durch den folgenden Code: Mit diesen Daten wird angegeben, wie eine Verbindung mit dem Gesichtserkennungsdienst hergestellt wird und wo die Eingabedaten abgerufen werden. Sie müssen das Feld `subscriptionKey` mit dem Wert Ihres Abonnementschlüssels aktualisieren. Ändern Sie außerdem die Zeichenfolge `uriBase`, sodass sie die korrekte Endpunktzeichenfolge enthält. Sie können den Wert von `imageWithFaces` ggf. auch auf einen Pfad festlegen, der auf eine andere Bilddatei verweist.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

Das Feld `faceAttributes` ist einfach eine Liste bestimmter Attributtypen. In diesem Feld wird angegeben, welche Informationen zu den erkannten Gesichtern abgerufen werden sollen.

:::code language="java" source="~/cognitive-services-quickstart-code/java/Face/rest/detect.java" id="environment":::

### <a name="call-the-face-detection-rest-api"></a>Aufrufen der Gesichtserkennungs-REST-API

Fügen Sie die **main**-Methode mit dem folgenden Code hinzu. Diese Methode erstellt einen REST-Aufruf der Gesichtserkennungs-API, um Gesichtsinformationen im Remotebild zu erkennen. (Die Zeichenfolge `faceAttributes` gibt an, welche Gesichtsattribute abgerufen werden sollen.) Anschließend werden die Ausgabedaten in eine JSON-Zeichenfolge geschrieben.

:::code language="java" source="~/cognitive-services-quickstart-code/java/Face/rest/detect.java" id="main":::

### <a name="parse-the-json-response"></a>Analysieren der JSON-Antwort

Fügen Sie direkt unter dem obigen Code den folgenden Block hinzu, der die zurückgegebenen JSON-Daten vor der Ausgabe an die Konsole in ein leichter lesbares Format konvertiert. Schließen Sie zum Schluss den try-catch-Block, die **main**-Methode und die **Main**-Klasse.

:::code language="java" source="~/cognitive-services-quickstart-code/java/Face/rest/detect.java" id="print":::

## <a name="run-the-app"></a>Ausführen der App

Kompilieren Sie den Code, und führen Sie ihn aus. Bei einer erfolgreichen Antwort werden Gesichtserkennungsdaten in einem leicht lesbaren JSON-Format im Konsolenfenster angezeigt. Beispiel:

```json
[{
  "faceRectangle": {
    "top": 131,
    "left": 177,
    "width": 162,
    "height": 162
  }
}]
```

## <a name="extract-face-attributes"></a>Extrahieren von Gesichtsattributen
 
Verwenden Sie zum Extrahieren von Gesichtsattributen das Erkennungsmodell 1, und fügen Sie den Abfrageparameter `returnFaceAttributes` hinzu.

```java
builder.setParameter("detectionModel", "detection_01");
builder.setParameter("returnFaceAttributes", "age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise");
```

Die Antwort enthält jetzt Gesichtsattribute. Beispiel:

```json
[{
  "faceRectangle": {
    "top": 131,
    "left": 177,
    "width": 162,
    "height": 162
  },
  "faceAttributes": {
    "makeup": {
      "eyeMakeup": true,
      "lipMakeup": true
    },
    "facialHair": {
      "sideburns": 0,
      "beard": 0,
      "moustache": 0
    },
    "gender": "female",
    "accessories": [],
    "blur": {
      "blurLevel": "low",
      "value": 0.06
    },
    "headPose": {
      "roll": 0.1,
      "pitch": 0,
      "yaw": -32.9
    },
    "smile": 0,
    "glasses": "NoGlasses",
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
    },
    "emotion": {
      "contempt": 0,
      "surprise": 0.005,
      "happiness": 0,
      "neutral": 0.986,
      "sadness": 0.009,
      "disgust": 0,
      "anger": 0,
      "fear": 0
    },
    "exposure": {
      "value": 0.67,
      "exposureLevel": "goodExposure"
    },
    "occlusion": {
      "eyeOccluded": false,
      "mouthOccluded": false,
      "foreheadOccluded": false
    },
    "noise": {
      "noiseLevel": "low",
      "value": 0
    },
    "age": 22.9
  },
  "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f"
}]
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine einfache Java-Konsolenanwendung erstellt, die REST-Aufrufe für die Azure-Gesichtserkennungs-API verwendet, um Gesichter in einem Bild zu erkennen und deren Attribute zurückzugeben. Sehen Sie sich als Nächstes die Referenzdokumentation zur Gesichtserkennungs-API an, um mehr über die unterstützten Szenarien zu erfahren.

> [!div class="nextstepaction"]
> [Gesichtserkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
