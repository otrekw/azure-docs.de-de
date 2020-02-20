---
title: Abrufen eines Modells mit einem REST-Aufruf in C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/31/2020
ms.author: diberry
ms.openlocfilehash: e4f995e888d261e1a1a7cb1e63d0d222c165060f
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2020
ms.locfileid: "77368385"
---
## <a name="prerequisites"></a>Voraussetzungen

* Azure Language Understanding: Ressourcenschlüssel mit 32 Zeichen und Endpunkt-URL für die Erstellung. Führen Sie die Erstellung mit dem [Azure-Portal](../luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) oder der [Azure CLI](../luis-how-to-azure-subscription.md#create-resources-in-azure-cli) durch.
* Importieren Sie die App [TravelAgent](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/change-model/TravelAgent.json) aus dem GitHub-Repository „cognitive-services-language-understanding“.
* Die LUIS-Anwendungs-ID für die importierte TravelAgent-App. Die Anwendungs-ID wird auf dem Anwendungsdashboard angezeigt.
* Die Versions-ID der Anwendung, die die Äußerungen empfängt. Die Standard-ID lautet „0.1“.
* [.NET Core V2.2+](https://dotnet.microsoft.com/download)
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>JSON-Datei mit Beispieläußerungen

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]

## <a name="change-model-programmatically"></a>Programmgesteuertes Ändern des Modells

1. Erstellen Sie eine neue Konsolenanwendung für die Sprache C# mit dem Projekt- und Ordnernamen `model-with-rest`.

    ```console
    dotnet new console -lang C# -n model-with-rest
    ```

1. Installieren Sie erforderliche Abhängigkeiten mit den folgenden dotnet-CLI-Befehlen.

    ```console
    dotnet add package System.Net.Http
    dotnet add package JsonFormatterPlus
    ```
1. Überschreiben Sie „Program.cs“ mit dem folgenden Code:

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using System.Linq;

    // 3rd party NuGet packages
    using JsonFormatterPlus;

    namespace AddUtterances
    {
        class Program
        {
            // NOTE: use your LUIS authoring key - 32 character value
            static string authoringKey = "YOUR-KEY";

            // NOTE: Replace this endpoint with your authoring key endpoint
            // for example, your-resource-name.api.cognitive.microsoft.com
            static string endpoint = "YOUR-ENDPOINT";

            // NOTE: Replace this with the ID of your LUIS application
            static string appID = "YOUR-APP-ID";

            // NOTE: Replace this your version number
            static string appVersion = "0.1";

            static string host = String.Format("https://{0}/luis/authoring/v3.0-preview/apps/{1}/versions/{2}/", endpoint, appID, appVersion);

            // GET request with authentication
            async static Task<HttpResponseMessage> SendGet(string uri)
            {
                using (var client = new HttpClient())
                using (var request = new HttpRequestMessage())
                {
                    request.Method = HttpMethod.Get;
                    request.RequestUri = new Uri(uri);
                    request.Headers.Add("Ocp-Apim-Subscription-Key", authoringKey);
                    return await client.SendAsync(request);
                }
            }
            // POST request with authentication
            async static Task<HttpResponseMessage> SendPost(string uri, string requestBody)
            {
                using (var client = new HttpClient())
                using (var request = new HttpRequestMessage())
                {
                    request.Method = HttpMethod.Post;
                    request.RequestUri = new Uri(uri);

                    if (!String.IsNullOrEmpty(requestBody))
                    {
                        request.Content = new StringContent(requestBody, Encoding.UTF8, "text/json");
                    }

                    request.Headers.Add("Ocp-Apim-Subscription-Key", authoringKey);
                    return await client.SendAsync(request);
                }
            }
            // Add utterances as string with POST request
            async static Task AddUtterances(string utterances)
            {
                string uri = host + "examples";

                var response = await SendPost(uri, utterances);
                var result = await response.Content.ReadAsStringAsync();
                Console.WriteLine("Added utterances.");
                Console.WriteLine(JsonFormatter.Format(result));
            }
            // Train app after adding utterances
            async static Task Train()
            {
                string uri = host  + "train";

                var response = await SendPost(uri, null);
                var result = await response.Content.ReadAsStringAsync();
                Console.WriteLine("Sent training request.");
                Console.WriteLine(JsonFormatter.Format(result));
            }
            // Check status of training
            async static Task Status()
            {
                var response = await SendGet(host  + "train");
                var result = await response.Content.ReadAsStringAsync();
                Console.WriteLine("Requested training status.");
                Console.WriteLine(JsonFormatter.Format(result));
            }
            // Add utterances, train, check status
            static void Main(string[] args)
            {
                string utterances = @"
                [
                    {
                    'text': 'go to Seattle today',
                    'intentName': 'BookFlight',
                    'entityLabels': [
                        {
                        'entityName': 'Location::LocationTo',
                        'startCharIndex': 6,
                        'endCharIndex': 12
                        }
                    ]
                    },
                    {
                        'text': 'a barking dog is annoying',
                        'intentName': 'None',
                        'entityLabels': []
                    }
                ]
                ";
                AddUtterances(utterances).Wait();
                Train().Wait();
                Status().Wait();
            }
        }
    }
    ```

1. Ersetzen Sie die Werte, die mit `YOUR-` beginnen, durch Ihre eigenen Werte.

    |Information|Zweck|
    |--|--|
    |`YOUR-KEY`|Ihr Erstellungsschlüssel mit 32 Zeichen.|
    |`YOUR-ENDPOINT`| Ihr URL-Endpunkt für die Erstellung. Beispiel: `replace-with-your-resource-name.api.cognitive.microsoft.com`. Sie haben Ihren Ressourcennamen festgelegt, als Sie die Ressource erstellt haben.|
    |`YOUR-APP-ID`| Ihre LUIS-App-ID. |

    Zugewiesene Schlüssel und Ressourcen werden im LUIS-Portal auf der Seite **Azure-Ressourcen** im Abschnitt „Verwalten“ angezeigt. Die App-ID wird auf der Seite **Anwendungseinstellungen** ebenfalls im Abschnitt „Verwalten“ angezeigt.

1. Erstellen Sie die Konsolenanwendung.

    ```console
    dotnet build
    ```

1. Führen Sie die Konsolenanwendung aus. In der Konsolenausgabe wird das gleiche JSON-Ergebnis wie zuvor im Browserfenster angezeigt.

    ```console
    dotnet run
    ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Datei aus dem Dateisystem, nachdem Sie diese Schnellstartanleitung durchgearbeitet haben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bewährte Methoden für eine App](../luis-concept-best-practices.md)