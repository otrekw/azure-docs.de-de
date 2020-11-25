---
title: 'Schnellstart: Extrahieren von gedrucktem Text – REST, Java'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung extrahieren Sie gedruckten Text aus einem Bild, indem Sie die Maschinelles Sehen-API mit Java verwenden.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 11/23/2020
ms.author: pafarley
ms.custom: seodec18, devx-track-java
ms.openlocfilehash: 4e000d87d1f61120f05dc147d2183899c36ef69d
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/24/2020
ms.locfileid: "95746284"
---
# <a name="quickstart-extract-printed-text-ocr-using-the-computer-vision-rest-api-and-java"></a>Schnellstart: Extrahieren von gedrucktem Text (OCR) mithilfe der Maschinelles Sehen-REST-API und Java

> [!NOTE]
> Wenn Sie englischsprachigen Text extrahieren, sollten Sie den neuen [Lesevorgang](../concept-recognizing-text.md) verwenden. Es steht ein [Java-Schnellstart](./java-hand-text.md) zur Verfügung. 

In dieser Schnellstartanleitung verwenden Sie die Maschinelles Sehen-REST-API, um mittels optischer Zeichenerkennung (Optical Character Recognition, OCR) gedruckten Text aus einem Bild zu extrahieren. Mit der [OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f20d)-Methode können Sie gedruckten Text in einem Bild erkennen und erkannte Zeichen als computerlesbare Zeichenfolge extrahieren.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: [Kostenloses Azure-Konto](https://azure.microsoft.com/free/cognitive-services/)
* [Java&trade;-Plattform, Standard Edition Development Kit 7 oder 8](/azure/developer/java/fundamentals/java-jdk-long-term-support) (JDK 7 oder 8)
* Sobald Sie über Ihr Azure-Abonnement verfügen, sollten Sie über <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title="Erstellen einer Ressource für maschinelles Sehen"  target="_blank"> im Azure-Portal eine Ressource für maschinelles Sehen <span class="docon docon-navigate-external x-hidden-focus"></span></a> erstellen, um Ihren Schlüssel und Endpunkt abzurufen. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um eine Verbindung Ihrer Anwendung mit dem Dienst für maschinelles Sehen herzustellen. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.
* [Erstellen Sie Umgebungsvariablen](../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) für den Schlüssel und die Endpunkt-URL, die Sie `COMPUTER_VISION_SUBSCRIPTION_KEY` bzw. `COMPUTER_VISION_ENDPOINT` benennen.

## <a name="create-and-run-the-sample-application"></a>Erstellen und Ausführen der Beispielanwendung

Führen Sie zum Erstellen und Ausführen des Beispiels die folgenden Schritte aus:

1. Erstellen Sie in Ihrer bevorzugten IDE oder in Ihrem bevorzugten Editor ein neues Java-Projekt. Wenn die Option verfügbar ist, erstellen Sie das Java-Projekt anhand einer Befehlszeilen-Anwendungsvorlage.
1. Importieren Sie die folgenden Bibliotheken in Ihr Java-Projekt. Wenn Sie Maven verwenden, werden die Maven-Koordinaten für jede Bibliothek bereitgestellt.
   - [Apache-HTTP-Client](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.5)
   - [Apache-HTTP-Core](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.9)
   - [JSON-Bibliothek](https://github.com/stleary/JSON-java) (org.json:json:20180130)
1. Fügen Sie der Datei, die die öffentliche Klasse `Main` für Ihr Projekt enthält, die folgenden `import`-Anweisungen hinzu.  

   ```java
   import java.net.URI;
   import org.apache.http.HttpEntity;
   import org.apache.http.HttpResponse;
   import org.apache.http.client.methods.HttpPost;
   import org.apache.http.entity.StringEntity;
   import org.apache.http.client.utils.URIBuilder;
   import org.apache.http.impl.client.CloseableHttpClient;
   import org.apache.http.impl.client.HttpClientBuilder;
   import org.apache.http.util.EntityUtils;
   import org.json.JSONObject;
   ```

1. Ersetzen Sie die öffentliche Klasse `Main` durch den folgenden Code.
1. Ersetzen Sie optional den Wert von `imageToAnalyze` durch die URL eines anderen Bilds, aus dem gedruckter Text extrahiert werden soll.
1. Speichern Sie die Änderungen, und erstellen Sie dann das Java-Projekt.
1. Wenn Sie eine IDE verwenden, führen Sie `Main` aus. Öffnen Sie andernfalls ein Eingabeaufforderungsfenster, und verwenden Sie dann den `java`-Befehl, um die kompilierte Klasse auszuführen. Beispiel: `java Main`.

```java
public class Main {

    // Add your Computer Vision subscription key and endpoint to your environment variables.
    private static String subscriptionKey = System.getenv("COMPUTER_VISION_SUBSCRIPTION_KEY");
    private static String endpoint = System.getenv("COMPUTER_VISION_ENDPOINT");

    private static final String uriBase = endpoint + "vision/v3.1/ocr";

    private static final String imageToAnalyze =
        "https://upload.wikimedia.org/wikipedia/commons/thumb/a/af/" +
            "Atomist_quote_from_Democritus.png/338px-Atomist_quote_from_Democritus.png";

    public static void main(String[] args) {
        CloseableHttpClient httpClient = HttpClientBuilder.create().build();

        try {
            URIBuilder uriBuilder = new URIBuilder(uriBase);

            uriBuilder.setParameter("language", "unk");
            uriBuilder.setParameter("detectOrientation", "true");

            // Request parameters.
            URI uri = uriBuilder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity requestEntity =
                    new StringEntity("{\"url\":\"" + imageToAnalyze + "\"}");
            request.setEntity(requestEntity);

            // Call the REST API method and get the response entity.
            HttpResponse response = httpClient.execute(request);
            HttpEntity entity = response.getEntity();

            if (entity != null) {
                // Format and display the JSON response.
                String jsonString = EntityUtils.toString(entity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("REST Response:\n");
                System.out.println(json.toString(2));
            }
        } catch (Exception e) {
            // Display error message.
            System.out.println(e.getMessage());
        }
    }
}
```

## <a name="examine-the-response"></a>Untersuchen der Antwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Die Beispielanwendung analysiert eine Antwort und zeigt diese bei erfolgreicher Ausführung im Konsolenfenster an, ähnlich wie im folgenden Beispiel:

```json
REST Response:

{
  "orientation": "Up",
  "regions": [{
    "boundingBox": "21,16,304,451",
    "lines": [
      {
        "boundingBox": "28,16,288,41",
        "words": [{
          "boundingBox": "28,16,288,41",
          "text": "NOTHING"
        }]
      },
      {
        "boundingBox": "27,66,283,52",
        "words": [{
          "boundingBox": "27,66,283,52",
          "text": "EXISTS"
        }]
      },
      {
        "boundingBox": "27,128,292,49",
        "words": [{
          "boundingBox": "27,128,292,49",
          "text": "EXCEPT"
        }]
      },
      {
        "boundingBox": "24,188,292,54",
        "words": [{
          "boundingBox": "24,188,292,54",
          "text": "ATOMS"
        }]
      },
      {
        "boundingBox": "22,253,297,32",
        "words": [
          {
            "boundingBox": "22,253,105,32",
            "text": "AND"
          },
          {
            "boundingBox": "144,253,175,32",
            "text": "EMPTY"
          }
        ]
      },
      {
        "boundingBox": "21,298,304,60",
        "words": [{
          "boundingBox": "21,298,304,60",
          "text": "SPACE."
        }]
      },
      {
        "boundingBox": "26,387,294,37",
        "words": [
          {
            "boundingBox": "26,387,210,37",
            "text": "Everything"
          },
          {
            "boundingBox": "249,389,71,27",
            "text": "else"
          }
        ]
      },
      {
        "boundingBox": "127,431,198,36",
        "words": [
          {
            "boundingBox": "127,431,31,29",
            "text": "is"
          },
          {
            "boundingBox": "172,431,153,36",
            "text": "opinion."
          }
        ]
      }
    ]
  }],
  "textAngle": 0,
  "language": "en"
}
```

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie eine Java Swing-Anwendung kennen, die maschinelles Sehen verwendet, um eine optische Zeichenerkennung (Optical Character Recognition, OCR) durchzuführen, intelligent zugeschnittene Miniaturansichten zu erstellen sowie visuelle Merkmale in Bildern zu erkennen, zu kategorisieren, zu markieren und zu beschreiben.

> [!div class="nextstepaction"]
> [Computer Vision API Java Tutorial](https://github.com/Azure-Samples/cognitive-services-java-computer-vision-tutorial) (Java-Tutorial zur Maschinelles Sehen-API)

* Um schnell mit der Maschinelles Sehen-API zu experimentieren, probieren Sie die [Open API-Testkonsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f21b/console) aus.
