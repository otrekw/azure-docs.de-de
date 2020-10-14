---
title: 'Schnellstart: Erkennen von Anomalien in Zeitreihendaten mithilfe der Anomalieerkennungs-REST-API und Java'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie die Anomalieerkennungs-API verwenden, um Anomalien in Ihren Datenreihen in Batchform oder in Streamingdaten zu erkennen.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 09/03/2020
ms.custom: devx-track-java
ms.author: mbullwin
ms.openlocfilehash: e23e9016b53a1d9f99809d792d710f865918a098
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92019680"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-java"></a>Schnellstart: Erkennen von Anomalien in Zeitreihendaten mithilfe der Anomalieerkennungs-REST-API und Java

Dieser Schnellstart unterstützt Sie bei den ersten Schritten in den zwei Erkennungsmodi der Anomalieerkennungs-API, um Anomalien in Ihren Zeitreihendaten zu erkennen. Diese Java-Anwendung sendet API-Anforderungen mit Zeitreihendaten im JSON-Format und erhält entsprechende Antworten.

| API-Anforderung                                        | Anwendungsausgabe                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Erkennen von Anomalien als Batch                        | Die JSON-Antwort enthält den Anomaliestatus (und weitere Daten) für jeden Datenpunkt in den Zeitreihendaten sowie die Positionen aller erkannten Anomalien. |
| Erkennen des Anomaliestatus des letzten Datenpunkts | Die JSON-Antwort enthält den Anomaliestatus (und weitere Daten) für den letzten Datenpunkt in den Zeitreihendaten.   |
| Erkennen von Änderungspunkten, die auf neue Datentrends hindeuten | Die JSON-Antwort, die die erkannten Änderungspunkte in den Zeitreihendaten enthält. |

Diese Anwendung ist in Java geschrieben, die API ist jedoch ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist. Den Quellcode für diesen Schnellstart finden Sie auf [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/java-detect-anomalies.java).

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services)
- Sobald Sie über Ihr Azure-Abonnement verfügen, erstellen Sie über <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title="Erstellen einer Anomalieerkennungsressource"  target="_blank"> eine Anomalieerkennungsressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. Warten Sie ihre Bereitstellung ab, und klicken Sie auf die Schaltfläche **Zu Ressource wechseln**.
    - Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Anomalieerkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.
- Das [Java&trade; Development Kit (JDK) 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher
- Importieren dieser Bibliotheken aus dem Maven-Repository
    - Paket [JSON in Java](https://mvnrepository.com/artifact/org.json/json)
    - Paket [Apache HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient)

- Eine JSON-Datei mit Datenpunkten auf einer Zeitreihe. Die Beispieldaten für diesen Schnellstart finden Sie auf [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [anomaly-detector-environment-variables](../includes/environment-variables.md)]

## <a name="create-a-new-application"></a>Erstellen einer neuen Anwendung

1. Erstellen Sie ein neues Java-Projekt, und importieren Sie die folgenden Bibliotheken:

    [!code-java[Import statements](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=imports)]

2. Erstellen Sie Variablen für Ihren Abonnementschlüssel und Ihren Endpunkt. Unten sehen Sie die URIs, die Sie für die Anomalieerkennung verwenden können. Diese werden später dem Dienstendpunkt angefügt, um URLs für API-Anforderungen zu erstellen.

    |Erkennungsmethode  |URI  |
    |---------|---------|
    |Batcherkennung    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |Erkennung am letzten Datenpunkt     | `/anomalydetector/v1.0/timeseries/last/detect`        |
    | Erkennen von Änderungspunkten | `/anomalydetector/v1.0/timeseries/changepoint/detect`   |

    [!code-java[Initial key and endpoint variables](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=vars)]

## <a name="create-a-function-to-send-requests"></a>Erstellen einer Funktion zum Senden von Anforderungen

1. Erstellen Sie eine neuen Funktion namens `sendRequest()`, die die oben erstellten Variablen verwendet. Führen Sie anschließend die folgenden Schritte aus.

2. Erstellen Sie ein `CloseableHttpClient`-Objekt, das Anforderungen an die API senden kann. Senden Sie die Anforderung an ein `HttpPost`-Anforderungsobjekt, indem Sie Ihren Endpunkt und eine URL der Anomalieerkennung kombinieren.

3. Verwenden Sie die Funktion `setHeader()` der Anforderung, um den Header `Content-Type` auf `application/json` festzulegen, und fügen Sie Ihren Abonnementschlüssel dem Header `Ocp-Apim-Subscription-Key` hinzu.

4. Verwenden Sie die Funktion `setEntity()` der Anforderung für die zu sendenden Daten.

5. Verwenden Sie die Funktion `execute()` des Clients, um die Anforderung zu senden, und speichern Sie sie in einem `CloseableHttpResponse`-Objekt.

6. Erstellen Sie ein `HttpEntity`-Objekt, um den Inhalt der Antwort zu speichern. Rufen Sie den Inhalt mit `getEntity()` ab. Wenn die Antwort nicht leer ist, geben Sie sie zurück.

    [!code-java[API request method](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=request)]

## <a name="detect-anomalies-as-a-batch"></a>Erkennen von Anomalien als Batch

1. Erstellen Sie eine Methode namens `detectAnomaliesBatch()`, um Anomalien in allen Daten als Batch zu erkennen. Rufen Sie die oben erstellte Methode `sendRequest()` mit dem Endpunkt, der URL, dem Abonnementschlüssel und den JSON-Daten auf. Rufen Sie das Ergebnis ab, und geben Sie es in der Konsole aus.

2. Wenn die Antwort das Feld `code` enthält, geben Sie den Fehlercode und die Fehlermeldung aus.

3. Andernfalls suchen Sie nach den Positionen der Anomalien im Dataset. Das Feld `isAnomaly` der Antwort enthält einen booleschen Wert, der angibt, ob es sich bei einem bestimmten Datenpunkt um eine Anomalie handelt. Rufen Sie das JSON-Array ab und durchlaufen Sie es, indem Sie den Index aller `true`-Werte ausgeben. Diese Werte stimmen mit dem Index der anomalen Datenpunkte überein, sofern welche gefunden wurden.

    [!code-java[Method for batch detection](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=detectBatch)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>Erkennen des Anomaliestatus des letzten Datenpunkts

Erstellen Sie eine Methode namens `detectAnomaliesLatest()`, um den Anomaliestatus des letzten Datenpunkts im Dataset zu erkennen. Rufen Sie die oben erstellte Methode `sendRequest()` mit dem Endpunkt, der URL, dem Abonnementschlüssel und den JSON-Daten auf. Rufen Sie das Ergebnis ab, und geben Sie es in der Konsole aus.

[!code-java[Latest point detection method](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=detectLatest)]


## <a name="detect-change-points-in-the-data"></a>Erkennen von Änderungspunkten in den Daten

1. Erstellen Sie eine Methode namens `detectChangePoints()`, um Anomalien in allen Daten als Batch zu erkennen. Rufen Sie die oben erstellte Methode `sendRequest()` mit dem Endpunkt, der URL, dem Abonnementschlüssel und den JSON-Daten auf. Rufen Sie das Ergebnis ab, und geben Sie es in der Konsole aus.

2. Wenn die Antwort ein Feld vom Typ `code` enthält, geben Sie den Fehlercode und die Fehlermeldung aus.

3. Suchen Sie andernfalls nach den Positionen der Änderungspunkte im Dataset. Das Feld `isChangePoint` der Antwort enthält einen booleschen Wert, der angibt, ob es sich bei einem bestimmten Datenpunkt um einen Trendänderungspunkt handelt. Rufen Sie das JSON-Array ab und durchlaufen Sie es, um den Index aller Werte vom Typ `true` auszugeben. Diese Werte entsprechen den Indizes der Trendänderungspunkte, sofern welche gefunden wurden.

    [!code-java[detect change points](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=detectChangePoint)]

## <a name="load-your-time-series-data-and-send-the-request"></a>Laden der Zeitreihendaten und Senden der Anforderung

1. Lesen Sie in der Hauptmethode Ihrer Anwendung die JSON-Datei mit den Daten ein, die den Anforderungen hinzugefügt werden.

2. Rufen Sie die beiden oben erstellten Funktionen zur Erkennung von Anomalien auf.

    [!code-java[Main method](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=main)]

### <a name="example-response"></a>Beispielantwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Klicken Sie unten auf die Links, um die JSON-Antwort auf GitHub anzuzeigen:
* [Beispielantwort für die Batcherkennung](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Beispielantwort für die Erkennung des letzten Punkts](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)
* [Beispielantwort für die Erkennung eines Änderungspunkts](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/change-point-sample.json)

[!INCLUDE [anomaly-detector-next-steps](../includes/quickstart-cleanup-next-steps.md)]
