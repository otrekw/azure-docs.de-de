---
title: 'Schnellstart: Erkennen von Anomalien in Zeitreihendaten mithilfe der Anomalieerkennungs-REST-API und C#'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie die Anomalieerkennungs-API verwenden, um Anomalien in Ihren Datenreihen in Batchform oder in Streamingdaten zu erkennen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 06/30/2020
ms.author: aahi
ms.openlocfilehash: 585731212fa31be2757d5b5d4c4e0a2ef1212ca8
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2020
ms.locfileid: "85980214"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-c"></a>Schnellstart: Erkennen von Anomalien in Zeitreihendaten mithilfe der Anomalieerkennungs-REST-API und C# 

Dieser Schnellstart unterstützt Sie bei den ersten Schritten in den zwei Erkennungsmodi der Anomalieerkennungs-API, um Anomalien in Ihren Zeitreihendaten zu erkennen. Diese C#-Anwendung sendet zwei API-Anforderungen mit Zeitreihendaten im JSON-Format und erhält entsprechende Antworten.

| API-Anforderung                                        | Anwendungsausgabe                                                                                                                                         |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Erkennen von Anomalien als Batch                        | Die JSON-Antwort enthält den Anomaliestatus (und weitere Daten) für jeden Datenpunkt in den Zeitreihendaten sowie die Positionen aller erkannten Anomalien. |
| Erkennen des Anomaliestatus des letzten Datenpunkts | Die JSON-Antwort enthält den Anomaliestatus (und weitere Daten) für den letzten Datenpunkt in den Zeitreihendaten.                                        |

 Die Anwendung ist zwar in C# geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist. Den Quellcode für diesen Schnellstart finden Sie auf [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/csharp-detect-anomalies.cs).

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
- Sobald Sie über Ihr Azure-Abonnement verfügen, erstellen Sie über <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title="Erstellen einer Anomalieerkennungsressource"  target="_blank"> eine Anomalieerkennungsressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. Warten Sie ihre Bereitstellung ab, und klicken Sie auf die Schaltfläche **Zu Ressource wechseln**.
    - Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Anomalieerkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.
- Eine beliebige Edition von [Visual Studio 2017 oder höher](https://visualstudio.microsoft.com/downloads/)
- Das [Json.NET](https://www.newtonsoft.com/json)-Framework, das als NuGet-Paket verfügbar ist So installieren Sie Newtonsoft.Json als NuGet-Paket in Visual Studio
    
    1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf Ihr Projekt.
    2. Wählen Sie **NuGet-Pakete verwalten** aus.
    3. Suchen Sie nach *Newtonsoft.Json*, und installieren Sie das Paket.

- Unter Linux/macOS kann diese Anwendung mit [Mono](https://www.mono-project.com/) ausgeführt werden.

- Eine JSON-Datei mit Datenpunkten auf einer Zeitreihe. Die Beispieldaten für diesen Schnellstart finden Sie auf [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [anomaly-detector-environment-variables](../includes/environment-variables.md)]

## <a name="create-a-new-application"></a>Erstellen einer neuen Anwendung

1. Erstellen Sie in Visual Studio eine neue Konsolenprojektmappe, und fügen Sie die folgenden Pakete hinzu. 

    [!code-csharp[using statements](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=usingStatements)]


2. Erstellen Sie Variablen für Ihren Abonnementschlüssel und Ihren Endpunkt. Unten sehen Sie die URIs, die Sie für die Anomalieerkennung verwenden können. Diese werden später dem Dienstendpunkt angefügt, um URLs für API-Anforderungen zu erstellen.

    | Erkennungsmethode                   | URI                                              |
    |------------------------------------|--------------------------------------------------|
    | Batcherkennung                    | `/anomalydetector/v1.0/timeseries/entire/detect` |
    | Erkennung am letzten Datenpunkt | `/anomalydetector/v1.0/timeseries/last/detect`   |
        
    [!code-csharp[initial variables for endpoint, key and data file](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=vars)]

## <a name="create-a-function-to-send-requests"></a>Erstellen einer Funktion zum Senden von Anforderungen

1. Erstellen Sie eine neuen asynchronen Funktion namens `Request`, die die oben erstellten Variablen verwendet.

2. Legen Sie das Sicherheitsprotokoll und die Headerinformationen des Clients mithilfe eines `HttpClient`-Objekts fest. Achten Sie darauf, dass Sie dem Header `Ocp-Apim-Subscription-Key` Ihren Abonnementschlüssel hinzufügen. Erstellen Sie dann ein `StringContent`-Objekt für die Anforderung.

3. Senden Sie die Anforderung mit `PostAsync()`, und geben Sie dann die Antwort zurück.

    [!code-csharp[Request method](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=requestMethod)]

## <a name="detect-anomalies-as-a-batch"></a>Erkennen von Anomalien als Batch

1. Erstellen Sie eine neue Funktion namens `detectAnomaliesBatch()`. Erstellen Sie die Anforderung und senden Sie sie, indem Sie die Funktion `Request()` mit Ihrem Endpunkt, Ihrem Abonnementschlüssel, der URL für die Batcherkennung von Anomalien und den Zeitreihendaten aufrufen.

2. Deserialisieren Sie das JSON-Objekt, und schreiben Sie es in die Konsole.

3. Wenn die Antwort das Feld `code` enthält, geben Sie den Fehlercode und die Fehlermeldung aus. 

4. Andernfalls suchen Sie nach den Positionen der Anomalien im Dataset. Das Feld `isAnomaly` der Antwort enthält ein Array von booleschen Werten, von denen jeder angibt, ob ein Datenpunkt eine Anomalie darstellt. Konvertieren Sie diese in mit der Funktion `ToObject<bool[]>()` des Antwortobjekts in ein Zeichenfolgenarray. Gehen Sie das Array durch, und geben Sie den Index aller `true`-Werte aus. Diese Werte stimmen mit dem Index der anomalen Datenpunkte überein, sofern welche gefunden wurden.

    [!code-csharp[Detect anomalies batch](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=detectAnomaliesBatch)]


## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>Erkennen des Anomaliestatus des letzten Datenpunkts

1. Erstellen Sie eine neue Funktion namens `detectAnomaliesLatest()`. Erstellen Sie die Anforderung und senden Sie sie, indem Sie die Funktion `Request()` mit Ihrem Endpunkt, Ihrem Abonnementschlüssel, der URL für die letzte Erkennung von Punktanomalien und den Zeitreihendaten aufrufen.

2. Deserialisieren Sie das JSON-Objekt, und schreiben Sie es in die Konsole.

    [!code-csharp[Detect anomalies latest](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=detectAnomaliesLatest)]
 
## <a name="load-your-time-series-data-and-send-the-request"></a>Laden der Zeitreihendaten und Senden der Anforderung

1. Laden Sie in der Hauptmethode Ihrer Anwendung Ihre JSON-Zeitreihendaten mit `File.ReadAllText()`. 

2. Rufen Sie die oben erstellten Funktionen zur Erkennung von Anomalien auf. Verwenden Sie `System.Console.ReadKey()`, damit das Konsolenfenster nach dem Ausführen der Anwendung geöffnet bleibt.

    [!code-csharp[Main method](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=main)]

### <a name="example-response"></a>Beispielantwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Klicken Sie unten auf die Links, um die JSON-Antwort auf GitHub anzuzeigen:
* [Beispielantwort für die Batcherkennung](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Beispielantwort für die Erkennung des letzten Punkts](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

[!INCLUDE [anomaly-detector-next-steps](../includes/quickstart-cleanup-next-steps.md)]
