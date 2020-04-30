---
title: Abrufen der Absicht mit einem REST-Aufruf in Java
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/20/2020
ms.author: diberry
ms.openlocfilehash: d59b7ebd1376d0bee10482cfe5faac1c53d1bde0
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733254"
---
## <a name="prerequisites"></a>Voraussetzungen

* [JDK SE](https://aka.ms/azure-jdks) (Java Development Kit, Standard Edition)
* [Visual Studio Code](https://code.visualstudio.com/) oder Ihre bevorzugte IDE
* Eine LUIS-App-ID: Verwenden Sie die öffentliche IoT-App-ID `df67dcdb-c37d-46af-88e1-8b97951ca1c2`. Die im Schnellstartcode verwendete Benutzerabfrage ist spezifisch für diese App.

## <a name="create-luis-runtime-key-for-predictions"></a>Erstellen eines LUIS-Runtimeschlüssels für Vorhersagen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie auf [**Language Understanding** erstellen](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne).
1. Geben Sie alle erforderlichen Einstellungen für den **Runtimeschlüssel** ein:

    |Einstellung|Wert|
    |--|--|
    |Name|Gewünschter Name (2 bis 64 Zeichen)|
    |Subscription|Wählen Sie ein passendes Abonnement aus|
    |Standort|Wählen Sie einen nahe gelegenen und verfügbaren Speicherort aus|
    |Preisstufe|`F0` – der Mindesttarif|
    |Ressourcengruppe|Wählen Sie eine verfügbare Ressourcengruppe aus|

1. Klicken Sie auf **Erstellen**, und warten Sie, bis die Ressource erstellt wird. Nachdem sie erstellt wurde, navigieren Sie zur Ressourcenseite.
1. Erfassen Sie den konfigurierten Endpunkt (`endpoint`) und einen Schlüssel (`key`).

## <a name="get-intent-programmatically"></a>Programmgesteuertes Abrufen der Absicht

Verwenden Sie Java, um den [Vorhersageendpunkt](https://aka.ms/luis-apim-v3-prediction) abzurufen und ein Vorhersageergebnis zu erhalten.

1. Erstellen Sie ein Unterverzeichnis mit dem Namen `lib`, und kopieren Sie die folgenden Java-Bibliotheken hinein:

    * [commons-logging-1.2.jar](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/quickstarts/analyze-text/java/lib/commons-logging-1.2.jar)
    * [httpclient-4.5.3.jar](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/quickstarts/analyze-text/java/lib/httpclient-4.5.3.jar)
    * [httpcore-4.4.6.jar](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/quickstarts/analyze-text/java/lib/httpcore-4.4.6.jar)

1. Kopieren Sie den folgenden Code, um eine Klasse in einer Datei namens `Predict.java` zu erstellen:

    ```java
    import java.io.*;
    import java.net.URI;
    import org.apache.http.HttpEntity;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.HttpClient;
    import org.apache.http.client.methods.HttpGet;
    import org.apache.http.client.utils.URIBuilder;
    import org.apache.http.impl.client.HttpClients;
    import org.apache.http.util.EntityUtils;

    public class Predict {

        public static void main(String[] args)
        {
            HttpClient httpclient = HttpClients.createDefault();

            try
            {

                // The ID of a public sample LUIS app that recognizes intents for turning on and off lights
                String AppId = "df67dcdb-c37d-46af-88e1-8b97951ca1c2";

                // Add your prediction Runtime key
                String Key = "YOUR-KEY";

                // Add your endpoint, example is your-resource-name.api.cognitive.microsoft.com
                String Endpoint = "YOUR-ENDPOINT";

                String Utterance = "turn on all lights";

                // Begin endpoint URL string building
                URIBuilder endpointURLbuilder = new URIBuilder("https://" + Endpoint + "/luis/prediction/v3.0/apps/" + AppId + "/slots/production/predict?");

                // query string params
                endpointURLbuilder.setParameter("query", Utterance);
                endpointURLbuilder.setParameter("subscription-key", Key);
                endpointURLbuilder.setParameter("show-all-intents", "true");
                endpointURLbuilder.setParameter("verbose", "true");

                // create URL from string
                URI endpointURL = endpointURLbuilder.build();

                // create HTTP object from URL
                HttpGet request = new HttpGet(endpointURL);

                // access LUIS endpoint - analyze text
                HttpResponse response = httpclient.execute(request);

                // get response
                HttpEntity entity = response.getEntity();


                if (entity != null)
                {
                    System.out.println(EntityUtils.toString(entity));
                }
            }

            catch (Exception e)
            {
                System.out.println(e.getMessage());
            }
        }
    }
    ```

1. Ersetzen Sie die Werte `YOUR-KEY` und `YOUR-ENDPOINT` durch Ihren eigenen Schlüssel- bzw. Endpunktwert für die **Vorhersageruntime**.

    |Information|Zweck|
    |--|--|
    |`YOUR-KEY`|Ihr 32-stelliger Schlüssel für die **Vorhersageruntime**.|
    |`YOUR-ENDPOINT`| Ihr URL-Endpunkt für die Vorhersage. Beispiel: `replace-with-your-resource-name.api.cognitive.microsoft.com`.|


1. Kompilieren Sie das Java-Programm über die Befehlszeile:

    ```console
    javac -cp ":lib/*" Predict.java
    ```

1. Führen Sie das Java-Programm über die Befehlszeile aus:

    ```console
    java -cp ":lib/*" Predict
    ```

1. Überprüfen Sie die Vorhersageantwort, die im JSON-Format zurückgegeben wird:

    ```console
    {'query': 'turn on all lights', 'prediction': {'topIntent': 'HomeAutomation.TurnOn', 'intents': {'HomeAutomation.TurnOn': {'score': 0.5375382}, 'None': {'score': 0.08687421}, 'HomeAutomation.TurnOff': {'score': 0.0207554}}, 'entities': {'HomeAutomation.Operation': ['on'], '$instance': {'HomeAutomation.Operation': [{'type': 'HomeAutomation.Operation', 'text': 'on', 'startIndex': 5, 'length': 2, 'score': 0.724984169, 'modelTypeId': -1, 'modelType': 'Unknown', 'recognitionSources': ['model']}]}}}}
    ```

    Die zur besseren Lesbarkeit formatierte JSON-Antwort:

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "None": {
                    "score": 0.08687421
                },
                "HomeAutomation.TurnOff": {
                    "score": 0.0207554
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ],
                "$instance": {
                    "HomeAutomation.Operation": [
                        {
                            "type": "HomeAutomation.Operation",
                            "text": "on",
                            "startIndex": 5,
                            "length": 2,
                            "score": 0.724984169,
                            "modelTypeId": -1,
                            "modelType": "Unknown",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Datei aus dem Dateisystem, nachdem Sie diese Schnellstartanleitung durchgearbeitet haben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen und Trainieren mit Java](../get-started-get-model-rest-apis.md)